# General Warmup 2 (general, 50pts)

> Can you convert the number 27 (base 10) to binary (base 2)?

Python has a [`bin`](https://docs.python.org/2/library/functions.html#bin) function to convert an integer to a binary
string:

```sh
$ python2 -c "print bin(27)"
0b11011
```

The `0b` prefix indicates the number that follows is in binary, so remove it to get the binary result:
```sh
$ python2 -c "print bin(27)[2:]"
11011
```

Using the `picoCTF{...}` format, the flag becomes:

```
picoCTF{11011}
```
