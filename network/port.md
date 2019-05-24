# port number

## Port 0

This isn’t in use for network traffic, but it’s sometimes used in communications taking place between different programs on the same computer.

## Ports 1-1023

Referred to as system ports, also known as reserved port number, or sometimes as “well-known ports.” These ports represent the official ports for most well-known network services. In an earlier video, we talked about how HTTP normally communicates over port 80, while FTP usually communicates over port 21. In most operating systems, administrator-level access is needed to start a program that listens on a system port.

## Ports 1024-49151

Known as registered ports. These ports are used for lots of other network services that might not be quite as common as the ones that are on system ports. A good example of a registered port is 3306, which is the port that many databases listen on. Registered ports are sometimes officially registered and acknowledged by the IANA, but not always. On most operating systems, any user of any access level can start a program listening on a registered port.

## Ports 49152-65535

These are known as private or ephemeral ports. Ephemeral ports can’t be registered with the IANA and are generally used for establishing outbound connections. You should remember that all TCP traffic uses a destination port and a source port. When a client wants to communicate with a server, the client will be assigned an ephemeral port to be used for just that one connection, while the server listens on a static system or registered port.