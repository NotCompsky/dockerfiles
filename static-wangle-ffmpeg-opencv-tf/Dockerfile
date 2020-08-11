FROM notcompsky/static-wangle-ffmpeg-opencv:latest
ARG TENSORFLOW_VERSION=2.3.0
RUN curl -L https://raw.githubusercontent.com/davido/bazel-alpine-package/master/david@ostrovsky.org-5a0369d6.rsa.pub > /etc/apk/keys/david@ostrovsky.org-5a0369d6.rsa.pub \
	&& curl -L https://github.com/davido/bazel-alpine-package/releases/download/0.26.1/bazel-0.26.1-r0.apk > /tmp/bazel-0.26.1-r0.apk \
	&& apk add /tmp/bazel-0.26.1-r0.apk \
	\
	&& mkdir /tensorflow \
	&& curl -L https://github.com/tensorflow/tensorflow/archive/v${TENSORFLOW_VERSION}.tar.gz | tar xz -C /tensorflow --strip-components 1 \
	&& /tensorflow/tensorflow/lite/tools/make/download_dependencies.sh \
	&& ( \
		make CFLAGS='-O3 -DNDEBUG -static' LIBS='/usr/lib/libpthread.a /usr/lib/libm.a /usr/local/lib64/opencv4/3rdparty/libzlib.a' -f /tensorflow/tensorflow/lite/tools/make/Makefile \
		|| echo "FAILED" \
	)


#	&& cd /tensorflow \
#	&& cd "tensorflow-${TENSORFLOW_VERSION}" \
##	&& sed -i -e '/define TF_GENERATE_BACKTRACE/d' tensorflow/core/platform/default/stacktrace.h \
#	&& sed -i -e '/define TF_GENERATE_STACKTRACE/d' tensorflow/core/platform/stacktrace_handler.cc \
#	&& bazel build -c opt --local_resources "${LOCAL_RESOURCES}" //tensorflow/tools/pip_package:build_pip_package
