# OpenVPN + SOCKS5 Gateway Service

Simply, this docker-compose configuration creates two services.  The first service creates an OpenVPN tunnel using the
OpenVPN-based service/provider of your choice.  The second service is a SOCKS5 proxy (using dante) that forwards traffic out
through the VPN created by the openvpn service.

ANY client that supports SOCKS5 can use this proxy to forward connections over the VPN, whether it's another Docker container
or a host somewhere else on your network.  For other containers, you may choose to assign them to the network created by this
package, and subsequently specify the SOCKS5 proxy as 172.19.32.2.  Or, if you choose to run your container with network_mode:
host or connect from another system on your network, you would use the IP address of the system running Docker.

You need to edit settings.env to configure the service; it allows configuration of the following:

* Timezone configuration
* OpenVPN *.ovpn and credentials path
* SOCKS5 port customization (default: 1080/tcp)
* Specify any local subnets that require access to the VPN
* Configure DNS leak protection (on by default).
* Number of SOCKS5 child processes, if you need more connection capabilities

## Required Stuff

* An OpenVPN-based service provider.  I use ProtonVPN
* A SOCKS5-capable client (curl can be used for testing)

## DNS Leak Protection

Just a quick word on this.  With DNS Leak Protection enabled, and assuming your VPN provider supplies DNS servers to you, any
DNS request made will be forwarded over the VPN instead of using Docker's internal resolver (and therefore your system's
resolver).  This prevents your ISP from sniffing the DNS traffic, whether you use your ISP's DNS servers or 3rd party DNS 
servers (e.g. Google or Cloudflare)

## Docker Network Configuration

The service creates a new network within Docker using the 172.19.32.0/24 subnet.  You may change this by editing
docker-compose.yaml, but usually not necessary.

## Launching the Service

After properly configuring settings.env, you need to use run.sh (without any parameters) to properly launch everything
using settings.env and your *.ovpn configurations.

## Updating the Services

When the services are running, you can edit the files within the config/ subfolder and just restart the containers.. you
don't need to run run.sh again, but once you change settings in settings.env or docker-compose.yaml, you will have to
launch again with run.sh.

## Testing

Curl is your best option to test, assuming you're connecting from the host running Docker and you're using port 1080:

```
curl -x socks5://localhost:1080/ github.com
```
