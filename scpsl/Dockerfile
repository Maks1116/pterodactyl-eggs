FROM debian:bookworm

USER root

RUN apt update; apt upgrade -y
RUN apt install -y dirmngr gnupg ca-certificates curl wget
RUN gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/mono-official-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN echo "deb [signed-by=/usr/share/keyrings/mono-official-archive-keyring.gpg] https://download.mono-project.com/repo/debian stable-buster main" | tee /etc/apt/sources.list.d/mono-official-stable.list
RUN wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O /tmp/packages-microsoft-prod.deb
RUN dpkg -i /tmp/packages-microsoft-prod.deb
RUN rm /tmp/packages-microsoft-prod.deb
RUN apt update
RUN apt install -y dotnet-sdk-8.0 mono-complete
RUN apt clean

RUN adduser --home /home/container container --disabled-password --gecos "" --uid 999
RUN usermod -a -G container container
RUN chown -R container:container /home/container
RUN mkdir /mnt/server

ARG CACHBUST=1
USER container

ENV USER=container HOME=/home/container
WORKDIR /home/container
COPY ./entrypoint.sh /entrypoint.sh

CMD ["/bin/bash", "/entrypoint.sh"]
