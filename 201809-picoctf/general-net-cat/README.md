# net cat (general, 75pts)

> Using netcat (nc) will be a necessity throughout your adventure. Can you connect to 2018shell.picoctf.com at port
36356 to get the flag?

Simply use [`netcat`](https://linux.die.net/man/1/nc) to connect to the server, which prints the flag when you connect:

```sh
$ nc 2018shell.picoctf.com 36356
That wasn't so hard was it?
picoCTF{NEtcat_iS_a_NEcESSiTy_9454f3e0}
```
