# Inspect me (web, 125pts)

> Inpect this code! http://2018shell.picoctf.com:35349

View the source code of the HTML, CSS and JS files to find comments with parts of the flag:

```
index.html: I learned HTML! Here's part 1/3 of the flag: picoCTF{ur_4_real_1nspe
mycss.css:  I learned CSS! Here's part 2/3 of the flag: ct0r_g4dget_098df0d0}
myjs.js:    I learned JavaScript! Here's part 3/3 of the flag:
```

Despite what it says, there is only a flag component in the HTML and CSS files.  Combining them, the flag becomes:
```
picoCTF{ur_4_real_1nspect0r_g4dget_098df0d0}
```
