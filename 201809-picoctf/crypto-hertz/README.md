# hertz (crypto, 150pts)

> Here's another simple cipher for you where we made a bunch of substitutions. Can you decrypt it? Connect with
> nc 2018shell.picoctf.com 43324.

Connecting to the server returns the ciphertext:

```
zvcqfdex lwfw yx uvsf podq - xsmxeyeseyvc_zyjlwfx_dfw_xvoadmow_fwbjpuwbzf
-------------------------------------------------------------------------------
"kwoo, jfyczw, xv qwcvd dcb oszzd dfw cvk hsxe pdryou wxedewx vp elw
msvcdjdfewx. mse y kdfc uvs, yp uvs bvce ewoo rw elde elyx rwdcx kdf,
yp uvs xeyoo efu ev bwpwcb elw ycpdrywx dcb lvffvfx jwfjwefdewb mu elde
dceyzlfyxe-y fwdoou mwoywaw lw yx dceyzlfyxe-y kyoo ldaw cvelycq
rvfw ev bv kyel uvs dcb uvs dfw cv ovcqwf ru pfywcb, cv ovcqwf ru
'pdyelpso xodaw,' dx uvs zdoo uvsfxwop! mse lvk bv uvs bv? y xww y
ldaw pfyqlewcwb uvs-xye bvkc dcb ewoo rw doo elw cwkx."

ye kdx yc hsou, 1805, dcb elw xjwdnwf kdx elw kwoo-ncvkc dccd jdaovacd
xzlwfwf, rdyb vp lvcvf dcb pdavfyew vp elw wrjfwxx rdfud pwbvfvacd.
kyel elwxw kvfbx xlw qfwwewb jfyczw adxyoy nsfdqyc, d rdc vp lyql
fdcn dcb yrjvfedczw, klv kdx elw pyfxe ev dffyaw de lwf fwzwjeyvc. dccd
jaovacd ldb ldb d zvsql pvf xvrw bdux. xlw kdx, dx xlw xdyb, xsppwfycq
pfvr od qfyjjw; qfyjjw mwycq elwc d cwk kvfb yc xe. jwewfxmsfq, sxwb
vcou mu elw woyew.

doo lwf ycayedeyvcx kyelvse wizwjeyvc, kfyeewc yc pfwczl, dcb bwoyawfwb
mu d xzdfowe-oyawfywb pvverdc elde rvfcycq, fdc dx pvoovkx:

"yp uvs ldaw cvelycq mweewf ev bv, zvsce (vf jfyczw), dcb yp elw
jfvxjwze vp xjwcbycq dc wawcycq kyel d jvvf ycadoyb yx cve evv ewffymow,
y xldoo mw awfu zldfrwb ev xww uvs evcyqle mwekwwc 7 dcb 10 dccweew
xzlwfwf."

"lwdawcx! klde d ayfsowce deedzn!" fwjoywb elw jfyczw, cve yc elw
owdxe byxzvczwfewb mu elyx fwzwjeyvc. lw ldb hsxe wcewfwb, kwdfycq dc
wrmfvybwfwb zvsfe scypvfr, ncww mfwwzlwx, dcb xlvwx, dcb ldb xedfx vc
lyx mfwdxe dcb d xwfwcw wijfwxxyvc vc lyx pode pdzw. lw xjvnw yc elde
fwpycwb pfwczl yc klyzl vsf qfdcbpdelwfx cve vcou xjvnw mse elvsqle, dcb
kyel elw qwceow, jdefvcygycq ycevcdeyvc cdesfdo ev d rdc vp yrjvfedczw
klv ldb qfvkc vob yc xvzyweu dcb de zvsfe. lw kwce sj ev dccd jaovacd,
nyxxwb lwf ldcb, jfwxwceycq ev lwf lyx mdob, xzwcewb, dcb xlycycq lwdb,
dcb zvrjodzwceou xwdewb lyrxwop vc elw xvpd.

"pyfxe vp doo, bwdf pfywcb, ewoo rw lvk uvs dfw. xwe uvsf pfywcb'x
rycb de fwxe," xdyb lw kyelvse doewfycq lyx evcw, mwcwdel elw
jvoyewcwxx dcb dppwzewb xurjdelu vp klyzl ycbyppwfwczw dcb wawc yfvcu
zvsob mw byxzwfcwb.
```

This is easily solved using an [online tool](https://quipqiup.com/) to bruteforce the plaintext.  The first line
contains the flag:

```
congrats here is your flag - substitution_ciphers_are_solvable_redpfyedcr
```
