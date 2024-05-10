### 1. Установка базового образа
FROM  --platform=$TARGETOS/$TARGETARCH ubuntu:22.04

LABEL org.opencontainers.image.source https://github.com/HOZAMA/NERV-docker-game-servers
### 2. Настройка переменных окружения
ENV   DEBIAN_FRONTEND noninteractive

### 3. Создание пользователя контейнера
RUN   useradd -m -d /home/container -s /bin/bash container


### 4. Установка необходимых пакетов
RUN         dpkg --add-architecture i386 \
            && apt update \
            && apt upgrade -y \
			&& apt install -y libcurl4-gnutls-dev:i386 libssl3:i386 libcurl4:i386 lib32tinfo6 libtinfo6:i386 lib32z1 lib32stdc++6 libncurses5:i386 libcurl3-gnutls:i386 libsdl2-2.0-0:i386 \
 				gcc g++ libgcc1 libc++-dev gdb libc6 curl tar iproute2 net-tools libatomic1 libsdl1.2debian libsdl2-2.0-0 \
        		libfontconfig locales libcurl3-gnutls libpulse-dev libpulse0 libnss-wrapper gettext

### 5. Настройка локали
RUN   update-locale lang=en_US.UTF-8 \
        &&   dpkg-reconfigure --frontend noninteractive locales

### 6. Настройка рабочего каталога и переменных окружения пользователя
ENV         USER=container HOME=/home/container
WORKDIR     /home/container


### 7. Подготовка NSS Wrapper (https://github.com/pelican-eggs/yolks)
### Используется для обеспечения сопоставления пользовательских UID/GID в контейнерах с внешними системами. 
### Это может быть необходимо для некоторых приложений, требующих определенных пользовательских идентификаторов.
ENV         NSS_WRAPPER_PASSWD=/tmp/passwd NSS_WRAPPER_GROUP=/tmp/group
RUN         touch ${NSS_WRAPPER_PASSWD} ${NSS_WRAPPER_GROUP} \
            && chgrp 0 ${NSS_WRAPPER_PASSWD} ${NSS_WRAPPER_GROUP} \
            && chmod g+rw ${NSS_WRAPPER_PASSWD} ${NSS_WRAPPER_GROUP}
ADD         passwd.template /passwd.template

### 8. Копирование и выполнение скрипта входа
COPY  ./entrypoint.sh /entrypoint.sh
CMD   [ "/bin/bash", "/entrypoint.sh" ]
