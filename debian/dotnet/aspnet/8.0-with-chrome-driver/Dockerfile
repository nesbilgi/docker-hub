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


# Gerekli paketleri yükle
RUN apt-get update && apt-get install -y \
    wget \
    unzip \
    libnss3 \
    fonts-liberation \
    libappindicator3-1 \
    libasound2 \
    libatk-bridge2.0-0 \
    libgtk-3-0 \
    --no-install-recommends && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Google Chrome yükle
RUN wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    dpkg -i google-chrome-stable_current_amd64.deb || apt-get -fy install && \
    rm google-chrome-stable_current_amd64.deb

# ChromeDriver'ı indir ve kur
RUN wget -q https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_linux64.zip && \
    unzip chromedriver_linux64.zip -d /usr/local/bin/ && \
    rm chromedriver_linux64.zip && \
    chmod +x /usr/local/bin/chromedriver

