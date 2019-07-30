# environ (general, 150pts)

> Sometimes you have to configure environment variables before executing a program. Can you find the flag we've hidden
> in an environment variable on the shell server?

Use [`env`](https://linux.die.net/man/1/env) to print all environment variables:

```
$ env
SECRET_FLAG=picoCTF{eNv1r0nM3nT_v4r14Bl3_fL4g_3758492}
FLAG=Finding the flag wont be that easy...
...
PICOCTF_FLAG=Nice try... Keep looking!
```

The real flag is in the `SECRET_FLAG` environment variable - the other flag-related variables are there to steer you
away from trying to guess the environment variable name.
