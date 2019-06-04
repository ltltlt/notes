- [DNS](#dns)
  - [Domain name](#domain-name)
  - [DNS resource record types](#dns-resource-record-types)
    - [A](#a)
    - [AAAA](#aaaa)
    - [PTR](#ptr)
    - [CNAME](#cname)
    - [HINFO](#hinfo)
    - [MX](#mx)
    - [NS](#ns)
    - [SOA](#soa)
    - [TXT](#txt)
    - [SRV](#srv)
  - [Domain Name Server Types](#domain-name-server-types)
    - [Cache Name Server](#cache-name-server)
    - [Recursive Name Server](#recursive-name-server)
    - [Root Name Server](#root-name-server)
    - [TLD(top level domain) name server](#tldtop-level-domain-name-server)
    - [authoritative name server](#authoritative-name-server)
  - [TTL](#ttl)
  - [zone file](#zone-file)

# DNS

## Domain name

subdomain.domain.tld

example: www.google.com, here: com is a tld, google is a domain, and www is a subdomain

## DNS resource record types

### A

ipv4 address

### AAAA

ipv6 address

### PTR

used for pointer queries. The IP address is represented as a domain name in the in-addr.arpa domain

### CNAME

canonical name. Also known as an alias name.

### HINFO

host info, not used oftenly.

### MX

mail exchange records, define a mail address of a host

### NS

name server record. These specify the authoritative name server for a domain

### SOA

start of authority. Specifies authoritative information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.

### TXT

descriptive text

### SRV

location of service. Generalized service location record, used for newer protocols instead of creating protocol-specific records such as MX. Generic of MX.

## Domain Name Server Types

### Cache Name Server

### Recursive Name Server

Recursive name servers are ones that perform full DNS resolution requests.
Most local cache name server is recursive name server.

### Root Name Server

There are 13 root name server.

Root name servers don't do recursive domain name lookup.

They response with TLD name servers.

In the past, these 13 root servers were distributed to very specific geographic regions, but today, they're mostly distributed across the globe via anycast. Anycast is a technique that's used to route traffic to different destinations depending on factors like location, congestion, or link health. Using anycast, a computer can send a data gram to a specific IP but could see it routed to one of many different actual destinations depending on a few factors. This should also make it clear that there aren't really only 13 physical route name servers anymore. It's better to think of them as 13 authorities that provide route name lookups as a service.

### TLD(top level domain) name server

example: com, org name server

The TLD name servers will respond again with a redirect, this time informing the computer performing the name lookup with what authoritative name server to contact.

### authoritative name server

example: google.com name server

Authoritative name servers are responsible for the last two parts of any domain name which is the resolution at which a single organization may be responsible for DNS lookups.

## TTL

All domain names in the global DNS system have a TTL or time to live.
This is a value in seconds, that can be configured by the owner of a domain
name for how long a name server is allowed to cache in entry before it should
discard it and perform a full resolution again.

## zone file

Zones are configured through what are known as zone files, simple configuration files that declare all resource records for a particular zone. So a zone file has to contain an SOA, or a Start of Authority resource record declaration. This SOA record declares the zone and the name of the name server that is authoritative for it. Along with the SOA record, you'll usually find NS records which indicate other name servers that may also be responsible for this zone.

A Domain Name System (DNS) zone file is a text file that describes a DNS zone. A DNS zone is a subset, often a single domain, of the hierarchical domain name structure of the DNS. The zone file contains mappings between domain names and IP addresses and other resources, organized in the form of text representations of resource records (RR).

```txt
$ORIGIN example.com.     ; designates the start of this zone file in the namespace
$TTL 1h                  ; default expiration time of all resource records without their own TTL value
example.com.  IN  SOA   ns.example.com. username.example.com. ( 2007120710 1d 2h 4w 1h )
example.com.  IN  NS    ns                    ; ns.example.com is a nameserver for example.com
example.com.  IN  NS    ns.somewhere.example. ; ns.somewhere.example is a backup nameserver for example.com
example.com.  IN  MX    10 mail.example.com.  ; mail.example.com is the mailserver for example.com
@             IN  MX    20 mail2.example.com. ; equivalent to above line, "@" represents zone origin
@             IN  MX    50 mail3              ; equivalent to above line, but using a relative host name
example.com.  IN  A     192.0.2.1             ; IPv4 address for example.com
              IN  AAAA  2001:db8:10::1        ; IPv6 address for example.com
ns            IN  A     192.0.2.2             ; IPv4 address for ns.example.com
              IN  AAAA  2001:db8:10::2        ; IPv6 address for ns.example.com
www           IN  CNAME example.com.          ; www.example.com is an alias for example.com
wwwtest       IN  CNAME www                   ; wwwtest.example.com is another alias for www.example.com
mail          IN  A     192.0.2.3             ; IPv4 address for mail.example.com
mail2         IN  A     192.0.2.4             ; IPv4 address for mail2.example.com
mail3         IN  A     192.0.2.5             ; IPv4 address for mail3.example.com
```
