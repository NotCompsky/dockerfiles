FROM notcompsky/static-mariadb:latest

#	&& git clone https://aomedia.googlesource.com/aom \
#	&& mkdir aom/aom_build \
#	&& cd aom/aom_build \
#	&& git reset --hard d14c5bb4f336ef1842046089849dee4a301fbbf0 \
#	&& cmake \
#		-DCMAKE_BUILD_TYPE=Release \
#		\
#		-DENABLE_SHARED=OFF \
#		-DENABLE_NASM=ON \
#		-DENABLE_EXAMPLES=OFF \
#		-DENABLE_TOOLS=OFF \
#		-DENABLE_DOCS=OFF \
#		-DENABLE_TESTS=OFF \
#		.. \
#	&& make install \
#
# TODO: --enable-libaom

# NOTE: When pkg-config claims it cannot find a package, it probably means that it cannot find a library that package requires. Look at the log: tail ffbuild/config.log

# WARNING: Maybe remove enable-demuxer? Can't remember if I needed that removed to compile it
# --enable-demuxer=mov,m4v,matroska,mp4 \

RUN apk add --no-cache \
		bzip2-static \
		expat-static \
		mercurial \
		yasm \
		nasm \
		diffutils \
		numactl-dev \
	\
	&& mkdir /git \
	&& cd /git \
	\
	&& git clone --depth 1 https://github.com/webmproject/libvpx \
	&& git clone --depth 1 https://github.com/FFmpeg/FFmpeg \
	&& git clone --depth 1 https://code.videolan.org/videolan/x264.git \
	&& hg clone http://hg.videolan.org/x265 \
	\
	&& cd /git/libvpx/build \
	&& LDFLAGS="-static" ../configure \
		--disable-shared \
		--enable-static \
		--disable-examples \
		--enable-small \
		--disable-multi-res-encoding \
		--disable-debug-libs \
		--disable-unit-tests \
		--disable-decode-perf-tests \
		--disable-encode-perf-tests \
		--disable-docs \
		--disable-tools \
		--enable-libs \
		--disable-postproc \
		--enable-vp9-highbitdepth \
		--as=yasm \
	&& make install \
	\
	&& cd /git/x264 \
	&& LDFLAGS="-static" ./configure \
		--disable-cli \
		--enable-static \
		--disable-shared \
		--disable-bashcompletion \
		--disable-opencl \
		--enable-lto \
		--enable-strip \
		--disable-avs \
		--disable-swscale \
		--disable-lsmash \
	&& make install \
	\
	&& cd /git/x265/build \
	&& cmake \
		-DCMAKE_BUILD_TYPE=Release \
		-DSTATIC_LINK_CRT=ON \
		-DENABLE_PIC=OFF \
		-DENABLE_SHARED=OFF \
		-DENABLE_CLI=OFF \
		../source \
	&& make install \
	\
	&& cd /git/FFmpeg \
	&& LDFLAGS="-static" \
		./configure \
			--cc="$CC" \
			--cxx="$CXX" \
			\
			--enable-gpl \
			--enable-version3 \
			--enable-static \
			\
			--disable-shared \
			--enable-small \
			--disable-programs \
			--disable-doc \
			--disable-openssl \
			--disable-libxcb \
			--disable-libxcb-shm \
			--disable-libxcb-xfixes \
			--disable-libxcb-shape \
			--disable-iconv \
			--disable-debug \
			\
			--pkg-config-flags="--static" \
			\
			\
			--disable-everything \
			--disable-network \
			--disable-autodetect \
			--enable-protocol=file \
			\
			--enable-libx264 \
			--enable-libx265 \
			--enable-libvpx \
	&& make install \
	\
	&& curl -s https://raw.githubusercontent.com/dtschump/CImg/master/CImg.h > /usr/include/CImg.h
	\
	&& rm -rf /git
