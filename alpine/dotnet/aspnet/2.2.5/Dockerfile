FROM alpine:3.9

RUN set -xe && \
    apk update && apk upgrade


# # # # # # # # # # # # # # # # # # # # # # #
# INSTALL SUPERVISORD
RUN set -xe && \
    apk add --no-cache --purge -uU supervisor && \
    rm -rf /var/cache/apk/* /tmp/*


# # # # # # # # # # # # # # # # # # # # # # #
# INSTALL WKHTMLTOPDF 
RUN apk add --update --no-cache \
    libstdc++ libx11 glib libxrender libxext 
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.8/main' >> /etc/apk/repositories && \
    apk add --no-cache libcrypto1.0 libssl1.0 ttf-liberation ttf-freefont ttf-ubuntu-font-family && \
    rm -rf /var/cache/apk/* /tmp/*

# Dependencies folder
COPY deps/wkhtmltopdf /bin


# # # # # # # # # # # # # # # # # # # # # # #
# INSTALL .NET CORE 
RUN apk add --no-cache \
    ca-certificates \
    \
    # .NET Core dependencies
    krb5-libs \
    libgcc \
    libintl \
    libssl1.1 \
    libstdc++ \
    lttng-ust \
    tzdata \
    userspace-rcu \
    zlib && \
    rm -rf /var/cache/apk/* /tmp/*

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 \
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true \
    # Set the invariant mode since icu_libs isn't included (see https://github.com/dotnet/announcements/issues/20)
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true

# Install ASP.NET Core
ENV ASPNETCORE_VERSION 2.2.5

RUN wget -O aspnetcore.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$ASPNETCORE_VERSION/aspnetcore-runtime-$ASPNETCORE_VERSION-linux-musl-x64.tar.gz \
    && aspnetcore_sha512='6dc1058c218a8ac70c4ea8ce0c62a0ce275374c282ff87fcac940c9e472297effaab94fb7e1738d68e61f1f021166e9f565e676a0a9ddcf2aeb0762144bf7b92' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf aspnetcore.tar.gz -C /usr/share/dotnet \
    && rm aspnetcore.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet


# # # # # # # # # # # # # # # # # # # # # # #
# INSTALL JAVA
# A few reasons for installing distribution-provided OpenJDK:
#
#  1. Oracle.  Licensing prevents us from redistributing the official JDK.
#
#  2. Compiling OpenJDK also requires the JDK to be installed, and it gets
#     really hairy.
#
#     For some sample build times, see Debian's buildd logs:
#       https://buildd.debian.org/status/logs.php?pkg=openjdk-8

# Default to UTF-8 file.encoding
ENV LANG C.UTF-8

# add a simple script that can auto-detect the appropriate JAVA_HOME value
# based on whether the JDK or only the JRE is installed
RUN { \
    echo '#!/bin/sh'; \
    echo 'set -e'; \
    echo; \
    echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
    } > /usr/local/bin/docker-java-home \
    && chmod +x /usr/local/bin/docker-java-home
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk/jre
ENV PATH $PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin

ENV JAVA_VERSION 8u212
ENV JAVA_ALPINE_VERSION 8.212.04-r0

RUN set -x \
    && apk add --no-cache \
    openjdk8-jre="$JAVA_ALPINE_VERSION" \
    && [ "$JAVA_HOME" = "$(docker-java-home)" ] \
    && rm -rf /var/cache/apk/* /tmp/*


# If you're reading this and have any feedback on how this image could be
# improved, please open an issue or a pull request so we can discuss it!
#
#   https://github.com/docker-library/openjdk/issues