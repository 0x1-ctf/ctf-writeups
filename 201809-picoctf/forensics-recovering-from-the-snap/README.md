# Recovering From the Snap (forensics, 150pts)

> There used to be a bunch of [animals](./assets/animals.zip) here, what did Dr. Xernon do to them?

Check the file type using [`file`](https://linux.die.net/man/1/file):

```
$ file animals.dd
animals.dd: DOS/MBR boot sector, code offset 0x3c+2, OEM-ID "mkfs.fat", sectors/cluster 4, root entries 512, sectors
20480 (volumes <=32 MB) , Media descriptor 0xf8, sectors/FAT 20, sectors/track 32, heads 64, serial number 0x9b664dde,
unlabeled, FAT (16 bit)
```

You can mount this filesystem and look around using [`mount`](https://linux.die.net/man/8/mount):

```
$ sudo mount -t auto animals.dd /media/ctf
$ cd /media/ctf
$ ls -lh
total 1.7M
-rwxr-xr-x 1 root root 618K Jul 25  2018 dachshund.jpg
-rwxr-xr-x 1 root root 381K Jul 25  2018 frog.jpg
-rwxr-xr-x 1 root root 315K Jul 25  2018 music.jpg
-rwxr-xr-x 1 root root 384K Jul 25  2018 rabbit.jpg
```

If you open these images, you'll see nothing interesting.  Maybe there's something else in the filesystem, e.g. a
deleted file?

Use [`binwalk`](https://github.com/ReFirmLabs/binwalk) to list the files in the filesystem:

```
$ binwalk animals.dd

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
39424         0x9A00          JPEG image data, JFIF standard 1.01
39454         0x9A1E          TIFF image data, big-endian, offset of first image directory: 8
672256        0xA4200         JPEG image data, JFIF standard 1.01
1165824       0x11CA00        JPEG image data, JFIF standard 1.01
1556992       0x17C200        JPEG image data, JFIF standard 1.01
1812992       0x1BAA00        JPEG image data, JFIF standard 1.01
1813022       0x1BAA1E        TIFF image data, big-endian, offset of first image directory: 8
2136576       0x209A00        JPEG image data, JFIF standard 1.01
2136606       0x209A1E        TIFF image data, big-endian, offset of first image directory: 8
2607616       0x27CA00        JPEG image data, JFIF standard 1.01
2607646       0x27CA1E        TIFF image data, big-endian, offset of first image directory: 8
3000832       0x2DCA00        JPEG image data, JFIF standard 1.01
3000862       0x2DCA1E        TIFF image data, big-endian, offset of first image directory: 8
```

There are 8 JPEG images listed in this output, but we only saw 4 of them when we mounted the filesystem.  Extract them:

```
$ binwalk -D 'jpeg' animals.dd
$ cd _animals.dd.extracted/
$ ls -lh
total 68M
-rw-rw-r-- 1 root root 8.9M Jul 28 13:36 11CA00
-rw-rw-r-- 1 root root 8.6M Jul 28 13:36 17C200
-rw-rw-r-- 1 root root 8.3M Jul 28 13:36 1BAA00
-rw-rw-r-- 1 root root 8.0M Jul 28 13:36 209A00
-rw-rw-r-- 1 root root 7.6M Jul 28 13:36 27CA00
-rw-rw-r-- 1 root root 7.2M Jul 28 13:36 2DCA00
-rw-rw-r-- 1 root root  10M Jul 28 13:36 9A00
-rw-rw-r-- 1 root root 9.4M Jul 28 13:36 A4200
```

Looking through each image, you'll find that `2DCA00` contains the flag:

```
picoCTF{th3_5n4p_happ3n3d}
```
