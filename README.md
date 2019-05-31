docker hub
---


## Why we do this image
We use wkhtmltopdf on our project to get pdf preview for html files also we use java **saxon** project to get html preview of xml with xslt transformation


## When you need this image

if you want to run  dotnet and java project in same container with **supervisord**


## How to build docker image

Go to root folder of repository then run command;


**Asp .Net core**
```sh
docker build -t nesbilgi/alpine/netcore:2.2.5  -f alpine/aspnetcore/2.2.5/Dockerfile .
```

**.Net core**
```sh
docker build -t nesbilgi/alpine/netcore:2.2.5  -f alpine/netcore/2.2.5/Dockerfile .
```