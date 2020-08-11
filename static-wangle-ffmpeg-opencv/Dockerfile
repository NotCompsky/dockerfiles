FROM notcompsky/wangle-static-and-ffmpeg:latest
ARG OPENCV_VERSION=4.4.0
RUN mkdir /opencv \
	&& curl -L https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz | tar xz -C /opencv --strip-components 1 \
	&& mkdir /opencv/build \
	&& cd /opencv/build \
	&& cmake \
		-DCMAKE_BUILD_TYPE=Release \
		\
		-DOPENCV_ENABLE_NONFREE=OFF \
		-DENABLE_PIC=OFF \
		-DBUILD_SHARED_LIBS=OFF \
		-DOPENCV_FORCE_3RDPARTY_BUILD=ON \
		-DBUILD_ZLIB=OFF \
		-DWITH_FFMPEG=ON \
		-DBUILD_PACKAGE=OFF \
		-DBUILD_TESTS=OFF \
		-DBUILD_PERF_TESTS=OFF \
		-DBUILD_opencv_apps=OFF \
		.. \
	&& make install
