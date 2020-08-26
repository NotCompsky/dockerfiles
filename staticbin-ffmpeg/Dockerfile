# Mostly copied from notcompsky/static-wangle-ffmpeg

FROM notcompsky/static-wangle-ffmpeg:latest AS intermediate
RUN cd / \
	&& git clone --depth 1 https://github.com/FFmpeg/FFmpeg \
	&& cd /FFmpeg \
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
	&& make \
	&& find -name 'ffmpeg'

FROM alpine:latest
COPY --from=intermediate /FFmpeg/ffmpeg /ffmpeg
