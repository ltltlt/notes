# dns

## dns resource record types

* A: ipv4 address
* AAAA: ipv6 address
* PTR: used for pointer queries. The IP address is represented as a domain name in the in-addr.arpa domain
* CNAME: canonical name. Also known as an alias name.
* HINFO: host info, not used oftenly.
* MX: mail exchange records, define a mail address of a host
* NS: name server record. These specify the authoritative name server for a domain
* SOA: start of authority. Specifies authoritative information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.
* TXT: descriptive text
* SRV: location of service. Generalized service location record, used for newer protocols instead of creating protocol-specific records such as MX. Generic of MX.
