FROM ubuntu:20.04
ENV MXE_TARGETS='x86_64-w64-mingw32.static'
# The CMake wrapper to use is then ${MXE_TARGET}-cmake
ENV TZ="Europe/London"
ENV DEBIAN_FRONTEND="noninteractive"
RUN apt update \
	&& apt install -y --no-install-recommends \
		autoconf \
		automake \
		autopoint \
		bash \
		bison \
		bzip2 \
		flex \
		g++ \
		g++-multilib \
		gettext \
		git \
		gperf \
		libc6-dev \
		libgdk-pixbuf2.0-dev \
		libltdl-dev \
		libssl-dev \
		libtool-bin \
		libxml-parser-perl \
		lzip \
		make \
		openssl \
		p7zip-full \
		patch \
		perl-base \
		python \
		ruby \
		sed \
		unzip \
		wget \
		xz-utils \
		intltool \
	\
	&& git clone --depth 1 https://github.com/mxe/mxe.git \
	&& cd mxe \
	&& make cc openssl curl autotools libpng zlib sqlite \
	&& unset $(env | \
		grep -vi '^EDITOR=\|^HOME=\|^LANG=\|MXE\|^PATH=' | \
		grep -vi 'PKG_CONFIG\|PROXY\|^PS1=\|^TERM=' | \
		cut -d '=' -f1 | tr '\n' ' ' \
	)
ENV PATH="/mxe/usr/x86_64-w64-mingw32.static/bin:/mxe/usr/bin:/mxe/usr/i686-w64-mingw32.static/bin:/mxe/usr/x86_64-pc-linux-gnu/bin:$PATH"
