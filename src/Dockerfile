FROM alpine:latest

LABEL \
    maintainer="Morgan AUCHEDE <morgan.auchede@gmail.com>"

ENV \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_KEEP_ENV=1 \
    S6_READ_ONLY_ROOT=1

RUN set -e -u -x \
\
    # Install packages
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS curl \
    && apk add --no-cache --no-progress bash openssh-server python socat \
\
    # Install s6-overlay
\
    && curl --location "https://gitlab.com/timonier/dumb-entrypoint/raw/master/src/s6-overlay/installer" | bash -s -- install \
\
    # Generate ssh keys
\
    && ssh-keygen -A \
\
    # Clean
\
    && apk del --no-progress BUILD_DEPS

COPY rootfs/ /

ENTRYPOINT [ "/init" ]
