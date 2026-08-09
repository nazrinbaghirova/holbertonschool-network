# Networking basics #1

Three small scripts about the loopback interface, name resolution and opening a
socket by hand.

## Notes I took while doing this

**localhost / 127.0.0.1.** The loopback address. Anything sent to it never
touches a cable, the kernel loops it straight back to the same machine. The
whole `127.0.0.0/8` block is reserved for this, which is why `127.0.0.2` works
just as well as `127.0.0.1`.

**0.0.0.0.** Not a real destination. When a server binds to it, it means "every
IPv4 address on this machine", so the service answers on the loopback and on the
LAN interface at once. As a destination it means "unspecified" and is used, for
example, by a machine that does not have an address yet.

**/etc/hosts.** A plain text file mapping addresses to hostnames. The resolver
reads it before asking DNS, so an entry here overrides whatever the DNS server
would have answered. Each line is an IP, then whitespace, then the names it
answers to. That is the whole trick behind task 0: adding `8.8.8.8 facebook.com`
means the name never reaches a DNS server at all.

**Seeing my interfaces.** `ifconfig` prints every interface with its addresses.
The IPv4 ones sit on the lines starting with `inet` (`inet6` lines are IPv6), so
filtering on those and cutting out the second field leaves just the addresses.

**Netcat.** `nc -l <host> <port>` opens a listening socket and prints whatever
arrives on it. Handy for checking whether a port is actually reachable before
blaming the application.

## Files

| File | Description |
| ---- | ----------- |
| `0-change_your_home_IP` | Points `localhost` at `127.0.0.2` and `facebook.com` at `8.8.8.8` |
| `1-show_attached_IPs` | Prints the active IPv4 address of every interface |
| `2-port_listening_on_localhost` | Listens on port 98 of localhost |

## Running them

Task 0 edits `/etc/hosts`, so it needs root:

```
$ sudo ./0-change_your_home_IP
```

It writes through a temporary file instead of `sed -i`, because inside a
container `/etc/hosts` is a bind mount and renaming over it fails.

Careful: if you run this on a machine you keep using, put `localhost` back to
`127.0.0.1` afterwards, otherwise a lot of local services stop working.

Task 2 binds a privileged-ish port, so run it with `sudo` in one terminal and
connect from another:

```
$ sudo ./2-port_listening_on_localhost
```

```
$ telnet localhost 98
Trying 127.0.0.2...
Connected to localhost.
Escape character is '^]'.
Hello world
```

Whatever you type in the telnet session shows up in the terminal running the
script.

## Environment

Ubuntu 22.04, Bash. All three scripts are executable and pass `shellcheck`
clean.
