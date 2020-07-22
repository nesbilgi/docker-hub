docker hub
---

## Why we do this image
We use wkhtmltopdf on our project to get pdf preview for html files

## When you need this image

if you want to run  dotnet and wkhtmltox in same container

## How to build docker image

Go to root folder of repository then run command;

**Asp .Net core**
```sh
docker build -t nesbilgi/alpine:aspnetcore-2.2.5  -f alpine/aspnetcore/2.2.5/Dockerfile .

docker build -t nesbilgi/debian:aspnet-3.1.4 -f debian/dotnet/aspnet/3.1.4/Dockerfile .
```

**.Net core**
```sh
docker build -t nesbilgi/alpine:netcore-2.2.5  -f alpine/netcore/2.2.5/Dockerfile .

docker build -t nesbilgi/debian:runtime-3.1.4 -f debian/dotnet/runtime/3.1.4/Dockerfile .
```

