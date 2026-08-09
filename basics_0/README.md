# Networking basics #0

First look at how machines actually talk to each other: the layers a message
goes through, the kinds of network it travels over, the addresses that identify
a machine, and the two transport protocols that carry the data.

Most of the tasks are multiple choice, so the answer files hold one number per
line. The last two are short Bash scripts.

## Notes I took while doing this

**OSI model.** Seven layers, numbered from the bottom up. Layer 1 is the
physical medium (copper, fibre, radio), layer 7 is the application talking to
the user (HTTP, SMTP, ...). It is only a mental model. Nothing on the wire is
"doing OSI", it just gives us a shared vocabulary for the pieces.

**LAN / WAN / Internet.** A LAN is what my laptop joins at home or in a
classroom. A WAN stitches several LANs together, typically across a city or a
country. The Internet is the global network the WANs run over, and it is what my
phone uses on mobile data.

**MAC vs IP.** The MAC address is burned into the network interface and
uniquely identifies that piece of hardware. The IP address is assigned by the
network and works like a postal address: it says where to deliver, not what the
device is.

**TCP vs UDP.** Both sit on top of IP. TCP sets up a connection, numbers the
segments and asks for the missing ones again, so nothing is lost but it costs
round trips. UDP just fires the datagrams off, which is faster but a lost packet
stays lost. Video calls and DNS prefer UDP, file transfers and web pages prefer
TCP.

**Ports.** A device has 65535 of them per protocol. An IP plus a port is a
socket. The three to remember:

| Port | Service |
| ---- | ------- |
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |

**ICMP.** Not a transport protocol, it carries control and error messages.
`ping` uses it to check whether a host answers and how long the round trip
takes.

## Files

| File | Description |
| ---- | ----------- |
| `0-OSI_model` | What the OSI model is and how it is organized |
| `1-types_of_network` | Telling LAN, WAN and the Internet apart |
| `2-MAC_and_IP_address` | What a MAC address and an IP address each identify |
| `3-UDP_and_TCP` | Filling in the TCP/UDP drawing |
| `4-TCP_and_UDP_ports` | Bash script listing the listening sockets with their PID and program |
| `5-is_the_host_on_the_network` | Bash script pinging an IP address 5 times |

## Running the scripts

`4-TCP_and_UDP_ports` needs root to be able to read the program name behind
every socket:

```
$ sudo ./4-TCP_and_UDP_ports
```

`5-is_the_host_on_the_network` takes the address to ping as its only argument,
and prints a usage line if you forget it:

```
$ ./5-is_the_host_on_the_network 8.8.8.8
$ ./5-is_the_host_on_the_network
Usage: 5-is_the_host_on_the_network {IP_ADDRESS}
```

## Environment

Ubuntu 22.04, Bash. Both scripts are executable and pass `shellcheck` clean.
