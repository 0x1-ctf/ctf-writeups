# you can't see me (general, 200pts)

> '...reading transmission... Y.O.U. .C.A.N.'.T. .S.E.E. .M.E. ...transmission ended...' Maybe something lies in
> /problems/you-can-t-see-me_0_8fc4b46df0f4dd36b87a28877fcf9ea2.

Use [`ls`](https://linux.die.net/man/1/ls) with the `-a` flag to show all files:

```
$ ls -lah
total 60K
drwxr-xr-x   2 root       root       4.0K Mar 25 19:57 .
-rw-rw-r--   1 hacksports hacksports   57 Mar 25 19:57 .  
drwxr-x--x 556 root       root        52K Mar 25 19:58 ..
```

There's a file named `.` that we can't see by using [`cat`](https://linux.die.net/man/1/cat) as we normally would
because there's a directory with the same name that is matched first.  We can leverage the
[bash](https://linux.die.net/man/1/bash) pathname expansion character `*` to match any file/directory starting with a
`.`:

```
$ cat .*
cat: .: Is a directory
picoCTF{j0hn_c3na_paparapaaaaaaa_paparapaaaaaa_e3d80588}
cat: ..: Permission denied
```
