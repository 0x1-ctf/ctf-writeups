# Buttons (web, 250pts)

> There is a website running at http://2018shell.picoctf.com:44730. Try to see if you can push their buttons.

The website contains a single button that when clicked sends a POST request to `/button1.php`. This page contains a
single link that when clicked makes a GET request to `button2.php`.  This redirects to `boo.html` which outputs
`ACCESS DENIED`.

Taking into consideration the word `buttons` in the title and the challenge text, we should focus on the requests that
are made to the button pages. The first request is a POST and the second a GET.

Changing the second request to a POST gets the flag:

```
$ curl -XPOST 'http://2018shell.picoctf.com:44730/button2.php'
Well done, your flag is: picoCTF{button_button_whose_got_the_button_dfe8b73c}
``` 
