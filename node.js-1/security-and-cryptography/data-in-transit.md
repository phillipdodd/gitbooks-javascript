# Data in Transit

"In Transit" means _traveling between two computers on a network._ Or between services. 

## Threats

### Lost Confidentiality or Integrity

Plainly put, an attacker could read or change the data before it arrives at its destination.

### Impersonation

In this scenario, the attacker impersonates you or another party. A common attack of this nature is called the [**man-in-the-middle attack**](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)**.**

## Asymmetric Encryption

Unlike the **symmetric encryption** useful for data at rest, _**a**_**symmetric encryption** uses _two_ key: one key to encrypt the data and another key to decrypt it.

## HMAC

**"Hash-based message authentication code"** is useful for ensuring that the **data has not been changed without your knowledge** while in transit. A **keyed hash** of the message is created and sent along with the message. Upon receipt, the recipient will hash the message on its own to see if the two values match as a way to verify no changes have occured. 

This is because if there were any changes to the content of the message, it would result in a different hash. 

## Digital Signatures

These use **asymmetric encryption** and **hashing** in combination to ensure the identity of the sending party **and** that the message has not been changed.

The document is first hashed then encrypted using **one of the two keys**. Assuming the recipient has the **second key**, it will be used to decrypt the message and then **recalculate** the hash it resulted in to determine if changes were made.

**Signing** is a major part of the **PKI**, **Public Key Infrastructure --** the backbone of the **HTTPS protocol.**

