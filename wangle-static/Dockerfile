FROM alpine:latest
RUN apk update \
	&& apk add --no-cache \
		binutils cmake make musl-dev g++ \
		mariadb-dev \
		libsodium-dev \
		boost-dev \
		libevent-dev \
		double-conversion-dev \
		libunwind-dev \
		gflags-dev \
		openssl-dev \
		curl-dev \
		lz4-dev zstd-dev snappy-dev libdwarf-dev fmt-dev \
		git
RUN git clone https://github.com/google/glog \
	&& cd glog \
	&& cmake . \
		-Wno-dev -DWITH_GFLAGS=OFF -DBUILD_SHARED_LIBS=OFF -DWITH_UNWIND=OFF -DWITH_PKGCONFIG=OFF -DWITH_SYMBOLIZE=OFF \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
	&& make install
RUN git clone https://github.com/facebook/folly \
	&& cd folly \
	&& sed -i 's/^add_subdirectory(folly)//g' CMakeLists.txt  \
	&& mkdir _build \
	&& cd _build \
	&& cmake .. \
		-Wno-dev -DBUILD_TESTS=OFF -DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
	&& make install
RUN git clone https://github.com/facebookincubator/fizz \
	&& mkdir fizz/build_ \
	&& cd fizz/build_ \
	&& cmake ../fizz \
		-Wno-dev -DBUILD_TESTS=OFF -DBUILD_EXAMPLES=OFF -DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
	&& make install
RUN git clone https://github.com/facebook/wangle \
	&& cd wangle/wangle \
	&& cmake . \
		-Wno-dev -DBUILD_TESTS=OFF -DBUILD_EXAMPLES=OFF \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
	&& make \
	&& make install

RUN git clone https://github.com/NotCompsky/libcompsky \
	&& mkdir libcompsky/build \
	&& cd libcompsky/build \
	&& cmake .. -DCMAKE_BUILD_TYPE=Release \
	&& make install
