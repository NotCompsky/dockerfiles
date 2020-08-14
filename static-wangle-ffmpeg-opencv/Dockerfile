FROM notcompsky/static-wangle-ffmpeg:latest
ARG OPENCV_VERSION=4.4.0
RUN git clone --depth 1 https://github.com/glennrp/libpng \
	&& cd libpng \
	&& cmake \
		-DPNG_SHARED=OFF \
		-DPNG_STATIC=ON \
		-DPNG_EXECUTABLES=OFF \
		-DPNG_TESTS=OFF \
		\
		-DPNG_HARDWARE_OPTIMIZATIONS=ON \
		\
		-DCMAKE_C_STANDARD_INCLUDE_DIRECTORIES=/usr/local/include/zlib \
		\
		. \
	&& make install \
	&& cd / \
	&& rm -rf libpng \
	\
	&& mkdir /opencv \
	&& curl -L https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz | tar xz -C /opencv --strip-components 1 \
	&& mkdir /opencv/build \
	&& cd /opencv/build \
	&& echo "Building OpenCV without multithreading (to enable static linking)" \
	&& cmake \
		-DCMAKE_BUILD_TYPE=Release \
		\
		-DOPENCV_ENABLE_NONFREE=OFF \
		-DENABLE_PIC=OFF \
		-DBUILD_SHARED_LIBS=OFF \
		-DOPENCV_FORCE_3RDPARTY_BUILD=ON \
		-DBUILD_ZLIB=OFF \
		-DWITH_PNG=ON \
		-DBUILD_PNG=OFF \
		-DWITH_FFMPEG=ON \
		-DBUILD_PACKAGE=OFF \
		-DBUILD_TESTS=OFF \
		-DBUILD_PERF_TESTS=OFF \
		-DBUILD_opencv_apps=OFF \
		\
		-DWITH_IPP=OFF \
		-DWITH_TBB=OFF \
		-DWITH_OPENMP=OFF \
		-DWITH_PTHREADS_PF=OFF \
		.. \
	&& make install \
	&& cd / \
	&& rm -rf opencv
