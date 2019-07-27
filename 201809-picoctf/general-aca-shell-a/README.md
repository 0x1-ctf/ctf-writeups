# Aca-Shell-A (general, 150pts)

> It's never a bad idea to brush up on those linux skills or even learn some new ones before you set off on this
> adventure! Connect with nc 2018shell.picoctf.com 58422.

Connecting to the server, you're presented with the following message:

```
Sweet! We have gotten access into the system but we aren't root.
It's some sort of restricted shell! I can't see what you are typing
but I can see your output. I'll be here to help you along.
If you need help, type "echo 'Help Me!'" and I'll see what I can do
There is not much time left!
```

Look around with `ls -l`:

```
~/$ ls
drwxr-xr-x 2 aca-shell-a_0 aca-shell-a_0 4096 Jul  1  2018 blackmail
drwxr-xr-x 2 aca-shell-a_0 aca-shell-a_0 4096 Jul  1  2018 executables
drwxrwxr-x 2 aca-shell-a_0 aca-shell-a_0 4096 Apr 13  2018 passwords
drwxrwxr-x 2 aca-shell-a_0 aca-shell-a_0 4096 Apr 13  2018 photos
drwxr-xr-x 2 aca-shell-a_0 aca-shell-a_0 4096 Jul  1  2018 secret
```

The `secret` folder looks most interesting, so navigate there to start off:

```
~/$ cd secret
Now we are cookin'! Take a look around there and tell me what you find!

~/secret$ ls -l
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  86 Jul  1  2018 intel_1
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0 100 Jul  1  2018 intel_2
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  95 Jul  1  2018 intel_3
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  40 Jul  1  2018 intel_4
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  85 Jul  1  2018 intel_5
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  44 Jul  1  2018 profile_ahqueith5aekongieP4ahzugi
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  44 Jul  1  2018 profile_ahShaighaxahMooshuP1johgo
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  90 Jul  1  2018 profile_aik4hah9ilie9foru0Phoaph0
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  95 Jul  1  2018 profile_AipieG5Ua9aewei5ieSoh7aph
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  30 Jul  1  2018 profile_bah9Ech9oa4xaicohphahfaiG
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  34 Jul  1  2018 profile_ie7sheiP7su2At2ahw6iRikoe
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  75 Jul  1  2018 profile_of0Nee4laith8odaeLachoonu
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  34 Jul  1  2018 profile_poh9eij4Choophaweiwev6eev
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  89 Jul  1  2018 profile_poo3ipohGohThi9Cohverai7e
-rw-r--r-- 1 aca-shell-a_0 aca-shell-a_0  50 Jul  1  2018 profile_Xei2uu5suwangohceedaifohs
Sabatoge them! Get rid of all their intel files!
```

Remove the intel files as instructed:


```
~/secret$ rm intel*
Nice! Once they are all gone, I think I can drop you a file of an exploit!
Just type "echo 'Drop it in!' " and we can give it a whirl!
```

Request the exploit:

```
~/secret$ echo 'Drop it in!'
Drop it in!
I placed a file in the executables folder as it looks like the only place we can execute from!
Run the script I wrote to have a little more impact on the system!
```

Navigate back to the `executables` folder and run the script:

```
~/secret$ cd ..
~/$ cd executables
~/executables$ ls -l 
-rwxr-xr-x 1 aca-shell-a_0 aca-shell-a_0 611 Jul  1  2018 dontLookHere

~/executables$ ./dontLookHere
b2c5 8f67 a3e6 f55c 6559...
Looking through the text above, I think I have found the password. I am just having trouble with a username.
Oh drats! They are onto us! We could get kicked out soon!
Quick! Print the username to the screen so we can close are backdoor and log into the account directly!
You have to find another way other than echo!
```

Print the username of the current user with [`whoami`](https://linux.die.net/man/1/whoami):

```
~/executables$ whoami
l33th4x0r
Perfect! One second!
Okay, I think I have got what we are looking for. I just need to to copy the file to a place we can read.
Try copying the file called TopSecret in tmp directory into the passwords folder.
```

For whatever reason copying directly using `cp /tmp/TopSecret ~/passwords` doesn't work, so copy it from the home
folder:
```
~/executables$ cd ..
~/$ cp /tmp/TopSecret passwords
Server shutdown in 10 seconds...
Quick! go read the file before we lose our connection!
```

Navigate to the `passwords` folder and read the file:

```
~/$ cd passwords
~/passwords$ ls
TopSecret

~/passwords$ cat TopSecret
Major General John M. Schofield's graduation address to the graduating class of 1879 at West Point is as follows: The
discipline which makes the soldiers of a free country reliable in battle is not to be gained by harsh or tyrannical
treatment.On the contrary, such treatment is far more likely to destroy than to make an army.It is possible to impart
instruction and give commands in such a manner and such a tone of voice as to inspire in the soldier no feeling butan
intense desire to obey, while the opposite manner and tone of voice cannot fail to excite strong resentment and a desire
to disobey.The one mode or other of dealing with subordinates springs from a corresponding spirit in the breast of the
commander.He who feels the respect which is due to others, cannot fail to inspire in them respect for himself, while he
who feels,and hence manifests disrespect towards others, especially his subordinates, cannot fail to inspire hatred
against himself.
picoCTF{CrUsHeD_It_4e355279}
``` 
