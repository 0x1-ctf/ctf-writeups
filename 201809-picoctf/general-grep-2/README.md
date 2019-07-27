# grep 2 (general, 125pts)

> This one is a little bit harder. Can you find the flag in /problems/grep-2_2_413a577106278d0711d28a98f4f6ac28/files on
> the shell server? Remember, grep is your friend.

Use [`grep`](https://linux.die.net/man/1/grep) with the `-r` flag to recursively search for the flag:

```sh
$ grep -r "picoCTF" .
./files5/file24:picoCTF{grep_r_and_you_will_find_8eb84049}
```
