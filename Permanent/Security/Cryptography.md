gen2026-04-23 22:35

Status:  #new

Tags: [[pure math]] [[security]]
# Cryptography

**Cryptography** is the study of methods to hide information in plain sight. The message that conveys information is understood only by the intended recipients.

Cryptography is where security engineer and applied mathematics meet: it is the key component to make distributed systems secure. 

- The message that needs to be sent is called *plaintext*
- The disguised message is the *cipher text*.

The process of converting a plaintext to a cypher text is called **encryption**. The reverse process, of converting a cypher text to a plaintext, is called **decryption**.

[![What Is Encryption? Explanation and Types - Cisco](https://www.cisco.com/content/dam/cisco-cdc/site/images/legacy/assets/swa/img/1100/symmetric-encryption-1256x706.png)
The *enciphering transformation* is a function that takes a plaintext as a parameter and *maps* it to a cypher text. The inverse function is called *deciphering transformation*.
$$
P \to^fC \to^{f^{-1}} P
$$
This setup is called **crypto system**. The science of breaking crypto systems is called **cryptanalysis**.
There are two sensitive information in a crypto system:
1. Algorithm of the enciphering/deciphering transformation
2. Choice of certain parameters. This selection of parameters is called **key**.
### Key

The values of the parameters passed to the crypto system are called **key**. Depending on the crypto system, the *enciphering key* $K_e$ and the *deciphering key* $K_d$ may be different.  

In the older crypto systems, one who possess the deciphering key or the enciphering key could easily compute the other one. This is clearly a security problem, which was tackled by the introduction of a revolutionary family of crypto systems: **public key crypto systems**.

## Public key crypto-systems

### Trapdoors and OWFs

By definition, a public key crypto-system has a property that someone who knows the enciphering key cannot compute the deciphering one in a *reasonable* amount of time. So, generally speaking:
- One who possess $K_e$ can easily compute $f: P \to C$, but not $f^{-1}: C \to P$
- One who possess $K_d$ can easily compute $f^{-1}: C \to P$, but not $f: P \to C$

> That is, the function $f$ is not invertible unless some other information is obtained, in this case the other key. This type of functions are called **trapdoors**.

Trapdoors are similar to *one-way functions*: the difference is that the latter still remains hard to inverse after some extra information is obtained.

Trapdoors and OWF have this title because *as of today* those functions are hard to invert. Those families of functions are indeed affected by the advances in computational power and the discovery of new algorithms.

> There is now crypto system that is *proved* to be public key.

### Why "public"?

The reason for the name "public key" is that encryption keys ($K_e$) can be shared with the world. In fact, someone who wants to send us a message *must* use our public key to encrypt the message.
This means that a secure communication between two subjects can be made without prior handshaking. 

[![What is Public Key Cryptography? | Twilio](https://www.twilio.com/content/dam/twilio-com/global/en/blog/legacy/2018/what-is-public-key-cryptography/19DfiKodi3T25Xz7g9EDTyvF9di2SzvJo6JebRJaCN-1P_c1fMqGtrAyZzxGGucG0bcmR8UwNes-gS.png)

### Digital signature

One who receives a message surely wants to know the identity of the sender. An easy way for someone who sends a message to be *authenticated* is via a mechanism called **digital signature**.
The process is fairly trivial: 
- the *signature* is a portion of the message that first gets encrypted with Alice private key $d_a$, and then with Bob's public key $e_b$. 
- This signature is sent at the beginning or at the end of the transmission (so a protocol must be used).
- Bob deciphers the whole message with its private key $d_b$. The signature segment is the decrypted again with Alice's public key $e_a$, which is of public domain. Bob finally obtains the signature that he was expecting, a thereby authenticates the sender of the message as Alice.

> The signature is a portion of "gibberish" that is generated through a OWF *hash function*. 

If Alice gives a portion of the text as input of the hash function, Bob can then verify not only that Alice was the sender of the message, but that the message itself was not manipulated during the transmission.
This mechanism works because only Alice and Bob know what portion of the plaintext message was used when generating the signature.
### A problem...and a solution

Public key algorithms are indeed very powerful and secure. Features like encryption, authentication and message integrity comes at a cost, though: **crypto systems tend to be slower**.

A sweet spot between public-key encryption and traditional symmetric-key encryption is using the former only for the **key exchange**, and then migrate to symmetric crypto systems. 

> The **Diffie-Hellman** key exchange is the most famous algorithm to securely generating symmetric keys over a public (insecure) channel using hardcore number theory.

