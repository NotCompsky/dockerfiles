# Create a portable version of KeePassXC to ensure always able to retrieve passwords

FROM ubuntu:19.04
RUN apt install --no-install-recommends -y keepassxc \
	&& curl -sL https://github.com/probonopd/linuxdeployqt/releases/download/7/linuxdeployqt-7-x86_64.AppImage > linuxdeployqt \
	&& chmod a+x linuxdeployqt \
	&& ./linuxdeployqt \
		/usr/bin/keepassxc \
		-appimage \
		-unsupported-bundle-everything \
		-no-copy-copyright-files \
		-no-translations

FROM alpine:latest
COPY --from=intermediate /usr/bin/keepassxc /keepassxc
ENTRYPOINT /bin/sh
