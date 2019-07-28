# admin panel (forensics, 150pts)

> We captured some [traffic](./assets/data.pcap) logging into the admin panel, can you find the password?

This task can be solved quickly using [`grep`](https://linux.die.net/man/1/grep) to search for the flag (the `-a` flag
will process binary files as if they were text):

```
$ grep -a "picoCTF" data.pcap 
user=admin&password=picoCTF{n0ts3cur3_df598569}
```

The intended solution is to use a PCAP analyzer like [Wireshark](https://www.wireshark.org/).

Open the PCAP file, set the filter to `http` and look at the POST requests - the very last one contains the password:

```
POST /login HTTP/1.1
Host: 192.168.3.128
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:59.0) Gecko/20100101 Firefox/59.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.168.3.128/
Content-Type: application/x-www-form-urlencoded
Content-Length: 53
Connection: keep-alive
Upgrade-Insecure-Requests: 1

user=admin&password=picoCTF{n0ts3cur3_df598569}
```
