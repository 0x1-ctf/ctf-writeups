# Forensics Warmup 2 (forensics, 50pts)

> Hmm for some reason I can't open this [PNG](./assets/flag.png)? Any ideas?

Run `file` on the PNG to check if it recognised as a PNG:

```sh
$ file flag.png
flag.png: JPEG image data, JFIF standard 1.01, resolution (DPI), density 75x75, segment length 16, baseline, precision
8, 909x190, frames 3
```

Looks like this is a JPEG - simply change the extension and open the image to get the flag:

```sh
$ mv flag.png flag.jpg
$ eog flag.jpg
```

```
picoCTF{extensions_are_a_lie}
```
