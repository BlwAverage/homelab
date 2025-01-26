# Bind9 DNS

------

## Install & Initial config

1. Install the latest packages.
```bash
sudo apt update -y && apt upgrade -y
```
2. Install bind9 and additional packages.
* bind9 - DNS.
* bind9utils - Utilities for bind.
* bind9-doc - Documentation package for bind.
```bash
sudo apt install bind9 bind9utils bind9-doc -y
```
3. Check the bind server is running.
```bash
systemctl status bind9
```
4. Update /etc/bind/named.conf.options.
```
example:
// allow only LAN traffic from 192.168.20.*
acl LAN {
192.168.20.0/24;
};
options {
        directory "/var/cache/bind"; // default directory
        allow-query { localhost; LAN; }; // allow queries from localhost and 192.168.20.*
        forwarders { 1.1.1.1; }; // use CloudFlare 1.1.1.1 DNS as a forwarder
        recursion yes;  // allow recursive queries
};
```
5. Check updated conf file is fine with named-checkconf.
```bash
named-checkconf /etc/bind/named.conf.options
```
6. Update named.conf.local with new DNS zone.
```
zone "example.domain" IN { \\ define the forward zone
        type master;
        file "/etc/bind/zones/example.domain";
};
zone "0.20.168.in-addr.arpa" IN { \\ define the reverse zone
        type master;
        file "/etc/bind/zones/example.domain.rev";
};
```
7. Check updated conf file is fine with named-checkconf.
```bash
named-checkconf /etc/bind/named.conf.options
```
8. Create zones directory.
```bash
mkdir -p /etc/bind/zones
```
7. Create zone files
```bash
touch /etc/bind/zones/example.domain
touch /etc/bind/zones/example.domain.rev
```
8. Update primary domain zone file
```
example:
$TTL    604800
; SOA record with MNAME and RNAME udpated
@       IN      SOA     example.domain. root.example.domain. (
                                26012025        ; Serial
                                604800          ; Refresh
                                86400           ; Retry
                                2419200         ; Expire
                                604800 )        ; Negative Cache TTL
; Name server record
@       IN      NS      pi401p.example.domain.
; A record for name server
pi401p  IN      A       192.168.20.11
; A record for clients
pi402p  IN      A       192.168.20.12
pi403p  IN      A       192.168.20.13
pi404p  IN      A       192.168.20.14
pi405p  IN      A       192.168.20.15
ms01ap  IN      A       192.168.20.16
ms01bp  IN      A       192.168.20.19
junk01p IN      A       192.168.20.21
```
9. Check the configuration with named-checkzone
```bash
named-checkzone example.domain /etc/bind/zone/example.domain
```
10. Update reverse zone file
```
example:
$TTL    604800
; SOA record with MNAME and RNAME updated
@       IN      SOA     example.domain. root.example.domain. (
                                26012025        ; Serial
                                604800          ; Refresh
                                86400           ; Retry
                                2419200         ; Expire
                                604800  )       ; Negative Cache TTL
; Name server record
@       IN      NS      pi401p.example.domain.
; A record for name server
pi401p  IN      A       192.168.20.11
; PTR record for name server
11      IN      PTR     pi401p.example.domain
; PTR record for clients
12      IN      PTR     pi402p.example.domain
13      IN      PTR     pi403p.example.domain
14      IN      PTR     pi404p.example.domain
15      IN      PTR     pi405p.example.domain
16      IN      PTR     ms01ap.example.domain
19      IN      PTR     ms01bp.example.domain
21      IN      PTR     junk01p.example.domain
```
11. Check the configuration with named-checkzone
```bash
named-checkzone example.domain /etc/bind/zone/example.domain
```
12. Restart bind server
```bash
systemctl restart bind9

or 

systemctl restart named
```
