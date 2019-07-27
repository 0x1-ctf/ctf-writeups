# strings (general, 100pts)

> Can you find the flag in this [file](./assets/strings) without actually running it?

Use [`strings`](https://linux.die.net/man/1/strings) to list printable character sequences in the binary and get the
flag:

```sh
$ strings strings | grep "picoCTF"
picoCTF{sTrIngS_sAVeS_Time_3f712a28}
```
