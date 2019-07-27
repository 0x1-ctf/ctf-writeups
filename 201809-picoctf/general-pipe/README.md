# pipe (general, 110pts)

> During your adventure, you will likely encounter a situation where you need to process data that you receive over the
> network rather than through a file. Can you find a way to save the output from this program and search for the flag?
> Connect with 2018shell.picoctf.com 44310.

Use [`grep`](https://linux.die.net/man/1/grep) to filter the output so only lines containing `picoCTF` are printed:

```sh
$ nc 2018shell.picoctf.com 44310 | grep "picoCTF"
picoCTF{almost_like_mario_a13e5b27}
```
