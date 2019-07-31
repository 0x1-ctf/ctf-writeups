# ssh-keyz (general, 150pts)

> As nice as it is to use our webshell, sometimes its helpful to connect directly to our machine. To do so, please add
> your own public key to ~/.ssh/authorized_keys, using the webshell. The flag is in the ssh banner which will be
> displayed when you login remotely with ssh to with your username.

If you need to generate a key, use [`ssh-keygen`](https://linux.die.net/man/1/ssh-keygen) and it it to the ssh-agent:

```
$ ssh-keygen -t rsa -b 4096
$ ssh-add
```

Copy the public key to the server using [`ssh-copy-id`](https://linux.die.net/man/1/ssh-copy-id):

```
$ ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@2018shell.picoctf.com
```

SSH to the server to receive the flag when you connect:

```
$ ssh <username>@2018shell.picoctf.com
picoCTF{who_n33ds_p4ssw0rds_38dj21}
```
