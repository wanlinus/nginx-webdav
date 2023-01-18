FROM ubuntu:22.04 as builder
WORKDIR /data
ENV USERNAME=wanli
ENV PASSWORD=123456
RUN echo "deb http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse" > /etc/apt/sources.list \
        && apt update && apt install -y build-essential libpcre3 libpcre3-dev zlib1g-dev openssl libssl-dev libxml2 libxslt-dev git wget apache2-utils \
        && wget https://nginx.org/download/nginx-1.22.1.tar.gz && git clone https://github.com/arut/nginx-dav-ext-module.git --depth=1 \
        && tar -xzvf nginx-1.22.1.tar.gz && cd nginx-1.22.1 && ./configure --prefix=/nginx --with-http_dav_module --add-module=../nginx-dav-ext-module && make && make install \
        && htpasswd -bc /nginx/password.db ${USERNAME} ${PASSWORD}

FROM ubuntu:22.04
WORKDIR /nginx
RUN echo "deb http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse \
        deb http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-security main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-updates main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-proposed main restricted universe multiverse \
        deb-src http://mirrors.163.com/ubuntu/ jammy-backports main restricted universe multiverse" > /etc/apt/sources.list \
        && apt update && apt install -y libxml2 && apt remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/*

COPY --from=builder /nginx /nginx
CMD ["/nginx/sbin/nginx", "-g", "daemon off;"]
