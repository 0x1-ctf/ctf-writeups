# assembly-0 (reversing, 150pts)

> What does asm0(0xb6,0xc6) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission
for this question will NOT be in the normal flag format.

You're given the following assembly code:

```asm
.intel_syntax noprefix
.bits 32
	
.global asm0

asm0:
	push	ebp
	mov	ebp,esp
	mov	eax,DWORD PTR [ebp+0x8]
	mov	ebx,DWORD PTR [ebp+0xc]
	mov	eax,ebx
	mov	esp,ebp
	pop	ebp	
	ret
```

The **Calling Convention** section of https://www.cs.virginia.edu/~evans/cs216/guides/x86.html gives a good introduction
as to how the above code should be interpreted:

* `[ebp+0x8]` refers to the first parameter passed at `asm0`, which is `0xb6`
* `[ebp+0x12]` refers to the second parameter passed at `asm0`, which is `0xc6`
* The `eax` register contains the return value

Knowing this, we can translate the above code into JavaScript:

```js
function asm0(a, b) {
  let eax = a;
  let ebx = b;
  eax = ebx;

  return eax;
}

console.log(asm0(0xb6, 0xc6));
```

Reading this, we can see the function just sets `eax` to the value of `ebx`, which is `0xc6`. Running the above JS
code confirms this - it outputs 198, which is `c6` in hexadecimal. Our flag is therefore:

```
0xc6
``` 
