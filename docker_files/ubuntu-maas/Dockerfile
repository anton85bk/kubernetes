FROM ubuntu:18.10

ENV DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install -y locales maas
RUN locale-gen ru_RU.UTF-8 && dpkg-reconfigure locales

