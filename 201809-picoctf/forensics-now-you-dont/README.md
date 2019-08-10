# now you don't (forensics, 200pts)

> We heard that there is something hidden in this [picture](./assets/nowYouDont.png). Can you find it?

Use [`stegsolve`](http://www.caesum.com/handbook/stego.htm) to iterate over the different bit planes in the image - the
flag is easily visible on `Red plane 0`:

```
picoCTF{n0w_y0u_533_m3}
```
