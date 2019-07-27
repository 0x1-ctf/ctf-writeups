# Client Side is Still Bad (web, 150pts)

> I forgot my password again, but this time there doesn't seem to be a reset, can you help me?
> http://2018shell.picoctf.com:53990

View the source code of the page in a browser and you'll see a script tag with this JavaScript code:

```js
function verify() {
  checkpass = document.getElementById("pass").value;
  split = 4;
  if (checkpass.substring(split*7, split*8) == '}') {
    if (checkpass.substring(split*6, split*7) == '0594') {
      if (checkpass.substring(split*5, split*6) == 'd_04') {
       if (checkpass.substring(split*4, split*5) == 's_ba') {
        if (checkpass.substring(split*3, split*4) == 'nt_i') {
          if (checkpass.substring(split*2, split*3) == 'clie') {
            if (checkpass.substring(split, split*2) == 'CTF{') {
              if (checkpass.substring(0,split) == 'pico') {
                alert("You got the flag!")
                }
              }
            }
    
          }
        }
      }
    }
  }
  else {
    alert("Incorrect password");
  }
}
```

The code checks the password from right-to-left, so you just need to construct the password from the last check to the
first:

```
picoCTF{client_is_bad_040594}
```
