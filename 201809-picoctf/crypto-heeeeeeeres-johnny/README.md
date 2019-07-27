# HEEEEEEERE'S Johnny! (crypto, 100pts)

> Okay, so we found some important looking files on a linux computer. Maybe they can be used to get a password to the
> process. Connect with nc 2018shell.picoctf.com 42165. Files can be found here: [passwd](./assets/passwd)
> [shadow](./assets/shadow).

Use [John the Ripper](https://www.openwall.com/john/) to crack the password:

```sh
$ john --show shadow
root:password1:17770:0:99999:7:::

1 password hash cracked, 0 left
```

Connect to the server using the credentials from above to get the flag:

```sh
$ nc 2018shell.picoctf.com 42165
Username: root
Password: password1
picoCTF{J0hn_1$_R1pp3d_5f9a67aa}
```
