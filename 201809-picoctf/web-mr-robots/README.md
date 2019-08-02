# Mr. Robots (web, 200pts)

> Do you see the same things I see? The glimpses of the flag hidden away? http://2018shell.picoctf.com:10157

The challenge title hints that you should check the
[`robots.txt`](https://en.wikipedia.org/wiki/Robots_exclusion_standard) file.

Accessing `/robots.txt` returns the following:

```
User-agent: *
Disallow: /143ce.html
```

Accessing `/143ce.html` returns the flag:

```
picoCTF{th3_w0rld_1s_4_danger0us_pl4c3_3lli0t_143ce}
```
