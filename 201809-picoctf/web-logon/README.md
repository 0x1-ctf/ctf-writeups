# Logon (web, 150pts)

> I made a website so now you can log on to! I don't seem to have the admin password. See if you can get to the flag.
> http://2018shell.picoctf.com:6153

If you try to login with the username `admin`, you'll always be denied.  If you try any other account,
e.g. `test` with password `test`, you'll be logged in successfully.

View the website cookies (e.g. `Chrome Developer Tools > Application tab > Cookies`) and you'll see an admin cookie
with a value `False`:
```
admin       False	2018shell.picoctf.com	/	Session	10				
password    test	2018shell.picoctf.com	/	Session	12				
username    test	2018shell.picoctf.com	/	Session	12				
```

Update this value to `True` and reload the page to get the flag:

```
picoCTF{l0g1ns_ar3nt_r34l_82e795f4}
```
