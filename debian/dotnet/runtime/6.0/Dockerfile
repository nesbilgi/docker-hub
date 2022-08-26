FROM mcr.microsoft.com/dotnet/runtime:6.0

# # # # # # # # # # # # # # # # # # # # # # #
# INSTALL WKHTMLTOPDF
ENV WKHTMLTOX wkhtmltox_0.12.5-1.buster_amd64.deb
ENV BUILD_PACKAGES build-essential
ENV MAIN_PACKAGES fontconfig libfreetype6 libjpeg62-turbo libxext6 libpng16-16 libx11-6 libxcb1  libxrender1 xfonts-75dpi xfonts-base 

COPY deps/$WKHTMLTOX /

RUN rm -f /etc/localtime \
&& ln -sv /usr/share/zoneinfo/Europe/Istanbul /etc/localtime \
&& echo "Europe/Istanbul" > /etc/timezone

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