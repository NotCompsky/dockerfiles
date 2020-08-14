FROM alpine:latest

RUN apk update \
	&& apk add --no-cache \
		git \
		cmake make \
		gcc g++ \
		musl-dev musl-utils \
		boost-static boost-dev \
		libdwarf-static libdwarf-dev \
		patch \
		curl gettext glib \
		autoconf automake libtool perl \
	\
	&& git clone --depth 1 https://github.com/richfelker/musl-cross-make \
	&& cd musl-cross-make \
	&& printf "%s\n%s\n%s\n%s\n" \
		'TARGET = x86_64-linux-musl' \
		'OUTPUT = /usr/local' \
		'DL_CMD = curl -C - -L -o' \
		'COMMON_CONFIG += --disable-nls CFLAGS="-g0 -O3 -flto" CXXFLAGS="-g0 -O3 -flto" LDFLAGS="-s"' \
		> config.mak \
	&& make install \
	&& cd .. \
	&& rm -rf musl-cross-make \
	\
	&& git clone --depth 1 https://github.com/microsoft/mimalloc \
	&& mkdir mimalloc/build \
	&& cd mimalloc/build \
	&& cmake \
		-DCMAKE_BUILD_TYPE=Release \
		\
		-DMI_PADDING=OFF \
		-DMI_OVERRIDE=ON \
		-DMI_BUILD_SHARED=OFF \
		-DMI_BUILD_STATIC=OFF \
		-DMI_BUILD_OBJECT=ON \
		-DMI_BUILD_TESTS=OFF \
		.. \
	&& make \
	&& mv ./CMakeFiles/mimalloc-obj.dir/src/static.c.o /mimalloc-override.o \
	&& cd .. \
	&& rm -rf mimalloc

ENV PKG_CONFIG_PATH="/usr/local/lib/pkgconfig"
ENV PATH=$PATH:/usr/local/x86_64-linux-musl/lib
ENV CC=/usr/local/bin/x86_64-linux-musl-gcc
ENV CXX=/usr/local/bin/x86_64-linux-musl-g++
ENV C_INCLUDE_PATH=/usr/local/x86_64-linux-musl/include
ENV LDFLAGS="-Wl,-Bstatic"
ENV CFLAGS="-static"
ENV CXXFLAGS="-static"
