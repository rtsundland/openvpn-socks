FROM ubuntu:latest

RUN apt-get update && \
    apt-get install openvpn wget traceroute inetutils-ping dnsutils net-tools tcpdump -y

COPY openvpn-updown /etc/openvpn/openvpn-updown

RUN chmod +X "/etc/openvpn/openvpn-updown"

ENTRYPOINT openvpn --config /config/service.ovpn \
  		--script-security 2 \
		--auth-user-pass /config/credentials.txt \
		--up /etc/openvpn/openvpn-updown \
		--down /etc/openvpn/openvpn-updown \
		--setenv LOCAL_SUBNETS ${LOCAL_SUBNETS} \
		--setenv PREVENT_DNS_LEAKS ${PREVENT_DNS_LEAKS}

