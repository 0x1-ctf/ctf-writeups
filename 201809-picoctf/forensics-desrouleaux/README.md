# Desrouleaux (forensics, 150pts)

> Our network administrator is having some trouble handling the tickets for all of of our incidents. Can you help him
> out by answering all the questions? Connect with nc 2018shell.picoctf.com 14079.
>
> [incidents.json](./assets/incidents.json)

When you connect to the server you'll be prompted to answer questions based on data in the `incidents.json` file.

```
$ nc 2018shell.picoctf.com 14079
You'll need to consult the file `incidents.json` to answer the following questions.

What is the most common source IP address? If there is more than one IP address that is the most common, you may give
any of the most common ones.
```

We can use [`jq`](https://stedolan.github.io/jq/) to parse the JSON and print the `src_ip` field, then
[`uniq`](https://linux.die.net/man/1/uniq) to only show duplicates:

```
$ cat incidents.json | jq '.tickets[].src_ip' | uniq -c -d
      2 "122.231.138.129"
```

```
How many unique destination IP addresses were targeted by the source IP address 122.231.138.129?
```

Again, use `jq` but add a filter to only list destination IP addresses with `src_ip` set to `122.231.138.129`:

```
$ cat incidents.json | jq '.tickets[] | select(.src_ip == "122.231.138.129") | .dst_ip' | uniq | wc -l
2
```

```
What is the number of unique destination ips a file is sent, on average? Needs to be correct to 2 decimal places.
```

Use `jq` to group by the file hash, filter out unique destination IPs a file is sent to, and then print the number of
times a file is sent somewhere unique (which is now the length of each array):

```
cat incidents.json | jq '.tickets | group_by(.file_hash)[] | unique_by(.dst_ip) | length'
1
2
1
1
1
2
2
```

Sum this up and divide by the total number of lines to get the average:

```
totalDestinations = 1+2+1+1+1+2+2 = 10
totalFiles = 7
averageDestinationsPerFile = 10 / 7 = 1.43
```

Submit `1.43` to get the flag:

```
Great job. You've earned the flag: picoCTF{J4y_s0n_d3rUUUULo_4f3aae0d}
```
