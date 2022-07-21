# Docker Networking

## Linux Firewall

- Docker makes use of the Linux Kernel's built-in firewall NETFILTER.
  
## Exploring a Container

```sh
docker dun -dit -p 8080:80 php:apache
```

```sh
iptables -nL "DOCKER"
#output
tartget prot opt source destination
ACCEPT tcp -- 0.0.0.0/0 172.17.0.2 tcp dpt:80
```

dpt means destination port. So docker is applyin a local, non-routable on the Internet, IP Address to our running container.

```sh
curl http://172.17.0.2
```

## Docker Networking Model

```sh
curl http://172.17.0.1
curl http://127.0.0.1:8080
```

The output will be the HTML response from the web server. Docker's internal routing is seamlessly forwarding traffic.

