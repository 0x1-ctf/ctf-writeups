# Secret Agent (web, 200pts)

> Here's a little website that hasn't fully been finished. But I heard google gets all your info anyway.
> http://2018shell.picoctf.com:3827

The word `Agent` in the title hints that this challenge is related to the
[User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) header. If you navigate to `/flag`,
you get the message:

```
You're not google! <your_current_user_agent_here>
```

[Lookup](https://support.google.com/webmasters/answer/1061943) the user agent Google uses for its web crawler
(Googlebot), then make a request with this user agent - the flag is in the HTML output:

```
$ curl -H 'User-Agent:Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)' http://2018shell.picoctf.com:3827/flag
...
<b>Flag</b>: <code>picoCTF{s3cr3t_ag3nt_m4n_12387c22}</code></p>
```
