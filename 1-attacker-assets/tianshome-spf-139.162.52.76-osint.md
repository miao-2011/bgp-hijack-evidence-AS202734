# OSINT: tianshome.com SPF Record and Associated Linode IP

## Summary

A manual DNS query of `tianshome.com` (domain registered to Junqi Tian / Tianshome.net, operator of AS202734) revealed an SPF record that includes a Linode Singapore IP address (`139.162.52.76`). Further OSINT of this IP shows active open ports, including a publicly accessible Netdata monitoring dashboard.

## DNS Query

```bash
$ dig tianshome.com TXT

; <<>> DiG 9.18.39-0ubuntu0.24.04.5-Ubuntu <<>> tianshome.com TXT
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21994
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;tianshome.com.                 IN      TXT

;; ANSWER SECTION:
tianshome.com.          1       IN      TXT     "v=spf1 ip4:45.63.6.183/32 ip4:45.77.42.196/32 ip4:139.162.52.76/32 ip4:45.76.155.167/32 include:_spf.google.com include:icloud.com ~all"
tianshome.com.          1       IN      TXT     "apple-domain=TY6fPyttaG6qghRp"

;; Query time: 76 msec
;; SERVER: 223.6.6.6#53(223.6.6.6) (UDP)
;; WHEN: Wed Jun 10 13:38:24 UTC 2026
;; MSG SIZE  rcvd: 232
```

IP: 139.162.52.76

Field	Value
Provider	Linode (Akamai Connected Cloud)
Location	Singapore (sg-04)
City	Singapore
ASN	AS63949
Organization	139.162.0.0/16
ISP	Akamai Connected Cloud
Last Seen	June 9, 2026
Tags	cloud

Hostnames
sgp.139-162-52-76.linode.abayip.cf

Domains
abayip.cf

Open Ports
21/tcp	Pure-FTPd	Private system, no anonymous login
123/udp	NTP	Public NTP service (stratum 3)
443/tcp	HTTPS	Active
9999/tcp	HTTP	Returns 503 Service Unavailable
19999/tcp	Netdata Dashboard	Publicly accessible (HTTP 200)
