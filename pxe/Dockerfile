FROM alpine
ENV MODE proxy-dhcp
ENV DEV eth0
RUN apk add --no-cache dnsmasq iptables
RUN mkdir /tftp
WORKDIR /tftp
RUN wget http://boot.ipxe.org/undionly.kpxe && ln -s undionly.kpxe undionly.kpxe.0
CMD \
    if [ "$MODE" = "" ]; then MODE=proxy-dhcp ; fi &&\
    if [ "$DEV" = "" ]; then DEV=eth0 ; fi &&\
    iptables -t nat -A POSTROUTING -j MASQUERADE &&\
    ip=$(ip -4 addr show dev $DEV | awk -F '[ /]+' '/global/ {print $3}') &&\
    subnet=$(echo $ip | cut -d '.' -f 1,2,3) &&\
    case "$MODE" in \
     dhcp) dhcpRange="$subnet.101,$subnet.199,255.255.255.0,1h" ;; \
     proxy-dhcp) dhcpRange="$ip,proxy" ;; \
     *) echo "Set MODE=(proxy-dhcp|dhcp)"; exit 1 ;; \
    esac && \
    dnsmasq --interface=$DEV \
        --dhcp-range=$dhcpRange \
        --dhcp-userclass=set:iPXE,iPXE \
        --dhcp-boot=tag:!iPXE,undionly.kpxe \
        --dhcp-boot=tag:iPXE,http://$ip:9090/boot.txt \
	    --enable-tftp --tftp-root=/tftp/ --no-daemon \
	    --pxe-service=tag:!iPXE,x86PC,"Boot to iPXE",undionly.kpxe \
	    --pxe-service=tag:iPXE,,"Boot Host Config",http://$ip:9090/boot.txt
