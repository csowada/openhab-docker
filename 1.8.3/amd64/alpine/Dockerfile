# openhab image
#
# ------------------------------------------------------------------------------
#               NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
#
#                       PLEASE DO NOT EDIT IT DIRECTLY.
# ------------------------------------------------------------------------------
#

FROM multiarch/alpine:amd64-latest-stable

MAINTAINER openHAB <info@openhabfoundation.org>

# Set download urls
ENV JAVA_URL="https://www.azul.com/downloads/zulu/zdk-8-ga-linux_x64.tar.gz"
ENV OPENHAB_URL="https://bintray.com/artifact/download/openhab/bin/distribution-1.8.3-runtime.zip"
ENV OPENHAB_VERSION="1.8.3"

# Set variables
ENV \
    APPDIR="/openhab" \
    EXTRA_JAVA_OPTS="" \
    OPENHAB_HTTP_PORT="8080" \
    OPENHAB_HTTPS_PORT="8443"

# Basic build-time metadata as defined at http://label-schema.org
ARG BUILD_DATE
ARG VCS_REF
ARG VERSION
LABEL org.label-schema.build-date=$BUILD_DATE \
    org.label-schema.docker.dockerfile="/Dockerfile" \
    org.label-schema.license="EPL" \
    org.label-schema.name="openHAB" \
    org.label-schema.vendor="penHAB Foundation e.V." \
    org.label-schema.version=$VERSION \
    org.label-schema.description="An open source, technology agnostic home automation platform" \
    org.label-schema.url="http://www.openhab.com/" \
    org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.vcs-type="Git" \
    org.label-schema.vcs-url="https://github.com/openhab/openhab-docker.git" \
    maintainer="openHAB <info@openhabfoundation.org>"

# Set locales
ENV \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8

# Install basepackages
RUN apk update && apk add \
    ca-certificates \
    fontconfig \
    libpcap-dev \
    unzip \
    dpkg \
    gnupg \
    wget \
    bash \
    shadow \
    openjdk8 \
    su-exec && \
    rm -rf /var/cache/apk/*

# Install openhab
RUN wget -nv -O /tmp/openhab.zip ${OPENHAB_URL} && \
    unzip -q /tmp/openhab.zip -d ${APPDIR} && \
    rm /tmp/openhab.zip && \
    cp -a ${APPDIR}/configurations ${APPDIR}/configurations.dist && \
    echo "export TERM=dumb" | tee -a ~/.bashrc

# Expose volume with configuration and userdata dir
VOLUME ${APPDIR}/configurations ${APPDIR}/addons

# Set working directory, expose and entrypoint
WORKDIR ${APPDIR}
EXPOSE 8080 8443 5555
COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# Execute command
CMD ["su-exec", "openhab", "./start.sh"]
