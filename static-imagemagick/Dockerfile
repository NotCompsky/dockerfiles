FROM notcompsky/static-wangle-ffmpeg-opencv:latest AS intermediate
RUN cd / \
	&& git clone git@github.com:ImageMagick/ImageMagick.git \
	&& cd ImageMagick \
	&& ./configure \
		--disable-dependency-tracking \
		--disable-shared \
		--enable-static \
		--disable-fast-install \
		--disable-deprecated \
		--disable-cipher \
		--enable-zero-configuration \
		--enable-hdri \
		--enable-pipes \
		--disable-legacy-support \
		--disable-assert \
		--disable-docs \
		--with-utilities \
		--without-magick-plus-plus \
		--without-perl \
		--without-modules \
	&& CFLAGS="${CFLAGS} /mimalloc-override.o" make

FROM alpine:latest
COPY --from=intermediate /ImageMagick/utilities/magick /magick
