# assembly-1 (reversing, 200pts)

> What does asm1(0xcd) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission
for this question will NOT be in the normal flag format.

You're given the following assembly code:

```asm
.intel_syntax noprefix
.bits 32
	
.global asm1

asm1:
	push	ebp
	mov	ebp,esp
	cmp	DWORD PTR [ebp+0x8],0xde
	jg 	part_a	
	cmp	DWORD PTR [ebp+0x8],0x8
	jne	part_b
	mov	eax,DWORD PTR [ebp+0x8]
	add	eax,0x3
	jmp	part_d
part_a:
	cmp	DWORD PTR [ebp+0x8],0x4e
	jne	part_c
	mov	eax,DWORD PTR [ebp+0x8]
	sub	eax,0x3
	jmp	part_d
part_b:
	mov	eax,DWORD PTR [ebp+0x8]
	sub	eax,0x3
	jmp	part_d
	cmp	DWORD PTR [ebp+0x8],0xee
	jne	part_c
	mov	eax,DWORD PTR [ebp+0x8]
	sub	eax,0x3
	jmp	part_d
part_c:
	mov	eax,DWORD PTR [ebp+0x8]
	add	eax,0x3
part_d:
	pop	ebp
	ret
```

The **Calling Convention** section of https://www.cs.virginia.edu/~evans/cs216/guides/x86.html gives a good introduction
as to how the above code should be interpreted:

* `[ebp+0x8]` refers to the first parameter passed to `asm1`, which is `0xcd`
* The `eax` register contains the return value

Knowing this, we can step through the assembly code line by line:

```
cmp	DWORD PTR [ebp+0x8],0xde
jg 	part_a
// if 0xcd > 0xde, go to part_a
// this is false, so continue to the next instruction

cmp	DWORD PTR [ebp+0x8],0x8
jne	part_b
// if 0xcd != 0x8, go to part_b
// this is true, so move execution to part_b

mov	eax,DWORD PTR [ebp+0x8]
sub	eax,0x3
// move 0xcd to eax
// subtract 0x3 from eax and put the result into eax (i.e. eax = 0xcd - 0x3 = 0xca)

jmp	part_d
// move execution to part_d

pop	ebp
ret
// restore the previous value of ebp and return eax (0xca)
```

The code was simple enough to evaluate by hand, and the flag is the value returned in `eax`:

```
0xca
``` 
