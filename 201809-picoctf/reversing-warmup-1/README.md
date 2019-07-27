# Reversing Warmup 1 (reversing, 50pts)

> Throughout your journey you will have to run many programs. Can you run this [program](./assets/run) to retrieve the
> flag?

Check the binary type using [`file`](https://linux.die.net/man/1/file):

```sh
$ file run
run: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-, for
GNU/Linux 2.6.32, BuildID[sha1]=61cbf2f4b656a244038775ad262ac1f0f143fda1, not stripped
```

Mark the binary as executable and run it to get the flag:

```sh
$ chmod +x run && ./run
picoCTF{welc0m3_t0_r3VeRs1nG}
```

Alternatively, you could use [`strings`](https://linux.die.net/man/1/strings) to get the flag without running the binary:

```sh
$ strings run | grep "picoCTF"
picoCTF{welc0m3_t0_r3VeRs1nG}
```
