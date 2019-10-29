# What's My Name? (forensics, 250pts)

> Say my name, say [my name](./assets/myname.tar.gz).

We're given a `pcap` (packet capture) file which can be opened in [Wireshark](https://www.wireshark.org/).

From the challenge text we can guess that `name` relates to the solution, so we can search for packets containing
the literal string `name` (`Edit > Find Packet` and change the search type to `String`).

Two DNS packets match our query:
* The first is a DNS query for `thisismyname.com`
* The second is the DNS response which contains a TXT record with the flag

```
picoCTF{w4lt3r_wh1t3_033ad0f914a0b9d213bcc3ce5566038b}
```
