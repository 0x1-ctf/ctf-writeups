# hex editor (forensics, 150pts)

> This [cat](./assets/hex_editor.jpg) has a secret to teach you.

This task can be solved quickly using [`grep`](https://linux.die.net/man/1/grep) to search for the flag (the `-a` flag
will process binary files as if they were text):

```
$ grep -a "picoCTF" hex_editor.jpg
Your flag is: "picoCTF{and_thats_how_u_edit_hex_kittos_3E03e57d}"
```

The intended solution is to teach you about analyzing the bytes of a file using a hex editor/viewer.  You can do this
using [`xxd`](https://linux.die.net/man/1/xxd):

```
$ xxd -c 48 hex_editor.jpg | less
```

The `-c` flag lets you specify how many octets to show per line (more octets = more characters).  Scroll to the bottom
(hotkey `G`) to find the flag:

```
Your flag is: "picoCTF{and_thats_how_u_edit_hex_kittos_3E03e57d}"
```
