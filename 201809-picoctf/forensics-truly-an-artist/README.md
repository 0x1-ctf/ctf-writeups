# Truly an Artist (forensics, 200pts)

> Can you help us find the flag in this [Meta-Material](./assets/2018.png)?

Open the file in a hex viewer like [`xxd`](https://linux.die.net/man/1/xxd):

```
$ xxd -c 48 hex_editor.jpg | less
```

The `-c` flag lets you specify how many octets to show per line (more octets = more characters).  Scroll to the bottom
(hotkey `G`) to find the flag:

```
picoCTF{look_in_image_788a182e}
```
