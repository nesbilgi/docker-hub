FROM mcr.microsoft.com/dotnet/aspnet:8.0

# # # # # # # # # # # # # # # # # # # # # # #
# INSTALL WKHTMLTOPDF
ENV WKHTMLTOX wkhtmltox_0.12.5-1.buster_amd64.deb
ENV BUILD_PACKAGES build-essential
ENV MAIN_PACKAGES fontconfig libfreetype6 libjpeg62-turbo libxext6 libpng16-16 libx11-6 libxcb1  libxrender1 xfonts-75dpi xfonts-base p7zip-full 

COPY deps/$WKHTMLTOX /

RUN rm -f /etc/localtime \
&& ln -sv /usr/share/zoneinfo/Europe/Istanbul /etc/localtime \
&& echo "Europe/Istanbul" > /etc/timezone

COPY deps/libssl1.1_1.1.1f-1ubuntu2_amd64.deb /
RUN dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb

RUN set -xe \
	&& apt-get update -qq \
	&& apt-get install --no-install-recommends -yq $BUILD_PACKAGES $MAIN_PACKAGES \
	&& dpkg -i ${WKHTMLTOX} \
	&& apt-get remove -y $BUILD_PACKAGES \
	&& apt-get autoremove -y \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
	&& rm -rf ${WKHTMLTOX} \
	&& truncate -s 0 /var/log/*log