FROM ubuntu:latest

RUN apt-get update && \
    apt-get install dante-server wget traceroute inetutils-ping dnsutils net-tools tcpdump -y

RUN useradd -c 'dante daemon' -U -s /bin/false dante

ENTRYPOINT danted -f /etc/danted.conf -N${PROXY_INSTANCES}
