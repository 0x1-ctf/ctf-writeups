# Ext Super Magic (forensics, 250pts)

> We salvaged a ruined Ext SuperMagic II-class mech recently and pulled the
> [filesystem](./assets/ext-super-magic.tar.gz) out of the black box. It looks a bit corrupted, but maybe there's
> something interesting in there.

The description hints that this is a corrupt Second Extended File System (from `Ext` and `II`).  Let's try to repair it:

```
$ fsck.ext2 ext-super-magic.img
e2fsck 1.42.13 (17-May-2015)
ext2fs_open2: Bad magic number in super-block
fsck.ext2: Superblock invalid, trying backup blocks...
fsck.ext2: Bad magic number in super-block while trying to open ext-super-magic.img

The superblock could not be read or does not describe a valid ext2/ext3/ext4
filesystem.  If the device is valid and it really contains an ext2/ext3/ext4
filesystem (and not swap or ufs or something else), then the superblock
is corrupt, and you might try running e2fsck with an alternate superblock:
    e2fsck -b 8193 <device>
 or
    e2fsck -b 32768 <device>
```

Neither of the suggested commands fix the problem.  Focusing on the error message, we see that there is a bad magic
number in the super-block.  Searching for `ext2 superblock` gets us
[this page](https://www.nongnu.org/ext2-doc/ext2.html).

If you look through at the [superblock table](https://www.nongnu.org/ext2-doc/ext2.html#superblock), you'll notice a
value called `s_magic` that sounds like what we're looking for.  The documentation says it should be located at offset
`1024 + 56 = 1080 (0x438)` with the value `0xEF53`.

If you create a hexdump to check the value in the file system we have using a tool like
[`xxd`](https://linux.die.net/man/1/xxd), you'll see it isn't set:

```
$ xxd -s 1080 -l 2 ext-super-magic.img 
  00000438: 0000
```

Use `xxd` (or a visual tool like [hexed.it](https://hexed.it)) to patch the file system (note that it is little-endian):

```
$ echo "00000438: 53ef" | xxd -r - ext-super-magic.img
$ xxd -s 1080 -l 2 -c 0 backup.img
  00000438: 53ef
```

Running [`file`](https://linux.die.net/man/1/file) shows us that our patch was successful, as the file type can now be
determined:

```
$ file ext-super-magic-img
backup.img: Linux rev 1.0 ext2 filesystem data, UUID=e5197553-0071-4621-9829-b74fe5ac99f5 (large files)
```

Mount the file system and manually look through the images to get the flag:

```
$ sudo mount -t auto ext-super-magic.img /media/ctf
```

```
Your flag is: "picoCTF{a7DB29eCf7dB9960f0A19Fdde9d00Af0}"
```
