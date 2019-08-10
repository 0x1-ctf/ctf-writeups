# hertz 2 (crypto, 200pts)

> This flag has been encrypted with some kind of cipher, can you decrypt it?
> Connect with nc 2018shell.picoctf.com 39961.

Connecting to the server returns the ciphertext:

```
Gwr ioaek fdvbq cvl sohym vxrd gwr jnzt pvu. A enq'g frjarxr gwam am moew nq rnmt ydvfjrh aq Yaev. Ag'm njhvmg nm ac A
mvjxrp n ydvfjrh njdrnpt! Vknt, caqr. Wrdr'm gwr cjnu: yaevEGC{mofmgagogavq_eaywrdm_ndr_gvv_rnmt_pqunvbhxtr}
```

This is easily solved using an [online tool](https://www.guballa.de/substitution-solver) to bruteforce the plaintext:

```
The quick brown fox jumps over the lazy dog. I can't believe this is such an easy problem in Pico. It's almost as if I
solved a problem already! Okay, fine. Here's the flag: picoCTF{substitution_ciphers_are_too_easy_dngaowmvye}
```
