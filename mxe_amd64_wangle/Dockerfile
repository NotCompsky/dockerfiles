FROM notcompsky/mxe_amd64:latest

# WARNING: -fpermissive for wangle is temporary.
COPY static-connector.patch /static-connector.patch

RUN cd /mxe \
	\
	&& apt install -y --no-install-recommends rename \
	\
	&& make MXE_TARGETS=x86_64-w64-mingw32.static zlib lz4 openssl libsodium libevent curl zstd boost \
	\
	&& cd / \
	&& git clone --depth 1 https://github.com/libunwind/libunwind \
	&& git clone --depth 1 https://github.com/google/glog \
	&& git clone --depth 1 https://github.com/google/double-conversion \
	&& git clone --depth 1 https://github.com/google/snappy \
	&& git clone --depth 1 https://github.com/fmtlib/fmt \
	&& git clone --depth 1 https://github.com/gflags/gflags \
	\
	&& cd /libunwind \
	&& ./autogen.sh \
	&& ./configure --disable-tests --disable-documentation \
	&& make install \
	&& cd / \
	&& rm -rf libunwind \
	\
	&& cd /glog \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-Wno-dev -DWITH_GFLAGS=OFF -DBUILD_SHARED_LIBS=OFF -DWITH_UNWIND=OFF -DWITH_PKGCONFIG=OFF -DWITH_SYMBOLIZE=OFF \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
		-DCMAKE_BUILD_TYPE=Release \
		. \
	&& make install \
	&& cd / \
	&& rm -rf glog \
	\
	&& cd /double-conversion \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake -DBUILD_TESTING=OFF . \
	&& make install \
	&& cd / \
	&& rm -rf double-conversion \
	\
	&& cd /snappy \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-DBUILD_SHARED_LIBS=OFF \
		-DSNAPPY_BUILD_TESTS=OFF \
		-DSNAPPY_INSTALL=ON \
		-DCMAKE_BUILD_TYPE=Release \
		. \
	&& make install \
	&& cd / \
	&& rm -rf snappy \
	\
	&& cd /fmt \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-DFMT_DOC=OFF -DFMT_INSTALL=ON -DFMT_TEST=OFF -DFMT_FUZZ=OFF -DFMT_CUDA_TEST=OFF \
		-DBUILD_SHARED_LIBS=OFF \
		. \
	&& make install \
	&& cd / \
	&& rm -rf fmt \
	\
	&& cd / \
	&& git clone --depth 1 https://github.com/ARMmbed/mbedtls \
	&& mkdir /mbedtls/build_dir \
	&& cd /mbedtls/build_dir \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-DENABLE_TESTING=OFF \
		-DUSE_SHARED_MBEDTLS_LIBRARY=OFF \
		-DENABLE_PROGRAMS=OFF \
		-DCMAKE_BUILD_TYPE=Release \
		.. \
	&& make install \
	&& cd / \
	&& rm -rf mbedtls \
	\
	&& cd /gflags \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-DBUILD_STATIC_LIBS=ON \
		-DBUILD_SHARED_LIBS=OFF \
		-DINSTALL_HEADERS=ON \
		-DINSTALL_SHARED_LIBS=OFF \
		-DBUILD_gflags_nothreads_LIB=OFF \
		-DGFLAGS_BUILD_TESTING=OFF \
		-DGFLAGS_BUILD_PACKAGING=OFF \
		-DGFLAGS_BUILD_STATIC_LIBS=ON \
		-DGFLAGS_BUILD_SHARED_LIBS=OFF \
		-DBUILD_TESTING=OFF \
		-DCMAKE_BUILD_TYPE=Release \
		. \
	&& make install \
	&& cd / \
	&& rm -rf gflags \
	\
	&& mkdir /wangler \
	&& cd /wangler \
	&& git clone --depth 1 https://github.com/facebook/folly \
	&& git clone --depth 1 https://github.com/facebookincubator/fizz \
	&& git clone --depth 1 https://github.com/facebook/wangle \
	&& git clone https://github.com/mariadb-corporation/mariadb-connector-c \
	\
	&& cd /wangler \
	&& cp -r /mxe/usr/x86_64-w64-mingw32.static/include /mxe/usr/x86_64-w64-mingw32.static/include.case_sensitive \
	&& mv /mxe/usr/x86_64-w64-mingw32.static/include /wangler/include \
	&& find /wangler -type f \( -name '*.h' -o -name '*.hpp' -o -name '*.cpp' \) -exec \
		sed -i -E 's_(#include <(.*/)?)([^/]+[.]h>)_\1\L\3_g' {} \; \
	&& echo 'echo "$(dirname "$1")/$(basename "$1" | tr [:upper:] [:lower:])"' > /usr/bin/path2lower \
	&& echo 'mv "$1" "$(/usr/bin/path2lower "$1")"' > /usr/bin/rename2lower \
	&& chmod +x /usr/bin/path2lower \
	&& chmod +x /usr/bin/rename2lower \
	&& find /wangler -type f \( -name '*.h' -o -name '*.hpp' \) -exec \
		/usr/bin/rename2lower {} \; \
	\
	&& cd /wangler/folly \
	&& sed -i 's/^add_subdirectory(folly)//g' CMakeLists.txt  \
	&& mkdir _build \
	&& cd _build \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake .. \
		-Wno-dev \
		-DBUILD_TESTS=OFF \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
		\
		-DFOLLY_HAVE_UNALIGNED_ACCESS=1 \
		-DFOLLY_HAVE_LINUX_VDSO=0 \
		-DFOLLY_HAVE_WCHAR_SUPPORT=1 \
		-DHAVE_VSNPRINTF_ERRORS=1 \
	&& make install \
	&& rm -rf /wangler/folly \
	\
	&& mkdir /wangler/fizz/build_ \
	&& cd /wangler/fizz/build_ \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-Wno-dev \
		-DBUILD_TESTS=OFF \
		-DBUILD_EXAMPLES=OFF \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
		-DCMAKE_CXX_STANDARD_INCLUDE_DIRECTORIES=/usr/local/include/zlib \
		../fizz \
	&& make install \
	&& cd / \
	&& rm -rf /wangler/fizz \
	\
	&& cd /wangler/wangle/wangle \
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-DCMAKE_CXX_FLAGS_RELEASE='-fpermissive' \
		-Wno-dev -DBUILD_TESTS=OFF -DBUILD_EXAMPLES=OFF \
		-DCMAKE_EXE_LINKER_FLAGS=-static \
		-DCMAKE_BUILD_TYPE=Release \
		. \
	&& make install \
	&& cd / \
	&& rm -rf /wangler/wangle \
	\
	&& cd /wangler/mariadb-connector-c \
	&& for f in service_sha2; do \
		curl -s "https://raw.githubusercontent.com/MariaDB/server/10.5/include/mysql/$f.h" > "include/mysql/$f.h"; \
	done \
	\
	&& git apply /static-connector.patch \
	\
	&& /mxe/usr/bin/${MXE_TARGETS}-cmake \
		-DWITH_MYSQLCOMPAT=ON \
		-DWITH_UNIT_TESTS=OFF \
		-DCMAKE_BUILD_TYPE=Release \
		\
		-DCLIENT_PLUGIN_CLIENT_ED25519=STATIC \
		-DCLIENT_PLUGIN_CACHING_SHA2_PASSWORD=STATIC \
		-DCLIENT_PLUGIN_SHA256_PASSWORD=STATIC \
		-DCLIENT_PLUGIN_AUTH_GSSAPI_CLIENT=STATIC \
		-DCLIENT_PLUGIN_MYSQL_OLD_PASSWORD=STATIC \
		-DCLIENT_PLUGIN_MYSQL_CLEAR_PASSWORD=STATIC \
		\
		-DCLIENT_PLUGIN_AURORA=STATIC \
		-DCLIENT_PLUGIN_DIALOG=STATIC \
		\
		-DCLIENT_PLUGIN_REMOTE_IO=STATIC \
		\
		-DCLIENT_PLUGIN_PVIO_SOCKET=STATIC \
		-DCLIENT_PLUGIN_PVIO_NPIPE=STATIC \
		-DCLIENT_PLUGIN_PVIO_SHMEM=STATIC \
		\
		. \
	&& make install \
	&& cd / \
	&& rm -rf /wangler/mariadb-connector-c \
	\
	&& rm -rf /wangler/include \
	&& mv /mxe/usr/x86_64-w64-mingw32.static/include.case_sensitive /mxe/usr/x86_64-w64-mingw32.static/include
