# Passwords

## Hashing Algorithms

Also known as **digests**, these result in **fixed length output** by transforming an **input of arbitrary length**.

First, two examples of **bad algorithms:**

### MD5

MD5 stands for **Message Digest 5** and was created in 1992.

{% hint style="info" %}
This method is **no longer thought to be secure**.
{% endhint %}

```javascript
const crypto = require('crypto');
const hash = crypto.createHash('md5');
hash.update('password1');
console.dir(hash.digest('hex'));
//* 7c6a180b36896a0a8c02787eeafb0e4c
```

{% hint style="info" %}
the [**update** ](https://nodejs.org/api/crypto.html#crypto_hash_update_data_inputencoding)and [**digest** ](https://nodejs.org/api/crypto.html#crypto_hash_digest_encoding)method can only be called on the same object **once** without an **error being thrown.**
{% endhint %}

MD5 hashes, however, are not very secure and can easily be decrypted. Our hashed password above can be taken to [https://hashtoolkit.com/](https://hashtoolkit.com/) and decrypted back to our original password string, `password1`.

#### Rainbow Table

A rainbow table refers to databases of known hashes for a given algorithm and their text counterparts and is used in the process of checking unsalted hashes. 

MD5 also has the potential for collisions -- different inputs producing the same output.

### SHA-256

This hash is **256 bytes,** as opposed to the small **128 bytes** of the MD5 hash. See how much larger it is:

```javascript
const crypto = require('crypto');
const hash = crypto.createHash('sha256');
hash.update('password1');
console.dir(hash.digest('hex'));
//* 0b14d501a594442a01c6859541bcb3e8164d183d32937b851835442f69d5c94e
```

Despite **brute force** and **reverse output** being much more difficult than MD5, the **Rainbow Table** problem still exists. This too can be found on [https://hashtoolkit.com/](https://hashtoolkit.com/) and is simply not enough to provide real security for a stored password.

Now, let's get to some **good hash algorithms** that are considered best to use in new applications:

### Argon2

Argon2 won a [password hashing competition in 2013](https://password-hashing.net/) and is now the **recommended algorithm** for new applications.

### PBKDF2

Stands for **Password-Based key Derivation Function 2** and words by applying a **hash-based message authentication code** repeatedly on a password _and_ salt to generate the output.

A benefit to this algorithm is that it can be **tuned to use specific amounts of computation time** towards generating each password.

Its recommended for enterprise level applications that require support in compliance with federal standards like the [Federal Information Processing Standard](https://securebox.comodo.com/ssl-sniffing/what-is-fips-compliance/).

### scrypt

An algorithm much like PBKDF2 but it is optimized to protect against **hardware based attacks** by greatly increasing the amount of **memory** required to generate the password.

### bcrypt

A popular method of hashing based on the [Blowfish algorithm](https://www.geeksforgeeks.org/blowfish-algorithm-with-examples/) that is supported across many languages.

{% hint style="info" %}
These "good' algorithms can be found listed on the [OWASP cheatsheet ](https://github.com/OWASP/CheatSheetSeries)as recommended algorithms for new applications.
{% endhint %}

## Salting the Hash

The term **salt** refers to a **random value added to the password entered** and must be different for every login credential. If two users use the same password with the same algorithm, the salt ensures the output for each will be different than one another, rendering **rainbow tables ineffective.**

**Salt** values are considered the **bare minimum** for password storage in the modern world.

```javascript
const crypto = require('crypto');

//* First we need a password
const password = "password1";

//* Next, a salt value.
const salt = crypto.randomBytes(256).toString('hex');

const hashedPassword = crypto.pbkdf2Sync(
    password,
    salt,
    100000,  //? The number of iterations to calculate
    512,     //? How many bytes to return from the function
    'sha512' //? The hash function to use as the basis
);

console.log(hashedPassword.toString('hex'))

//* fe76134d986a739f8122cabf6555ba3358a056758b1ab80bd25214565af4586a9b2da453942136f154edda121a26ad9e799e2539db8e9c8438190a35ba73035e118bd83fcd7fda40b2958dc2fff547ffcb22a519d3dae1759b7974d1cb0e35c036c9b75b4ba9bc32190065386218062dec897d3fe9c19f37ebcfc5055721479222938a2f4e4101e1fd2d6c515a23fc86e25b02b8b3540871eac9b75988c9978ec2b824a26f464782c15eaab3369dd13791950e625fe61b3c7a42da1a07ea3115c291c516d14621a1706af0a50c2d61e1b51865d1fc951f8ce6dafc6e111a3050fd6627f0398c645d26df2b3713f5c0f97c15c8465d959847ce372164625126eb55f94fc43194a398368c408ac60e024efcf8365446f5097a53e62796be8b6418e3ac5c2a2f7ec7f62957a34336ed7e464b53c1c8229f594e6828344f6e9de803da1cef5c4f39b7212885bfddb977d1b632f59c2ae41fb18388a54ae1d170df2caf635576d2849b1b0748743dc2984003e5fec5dbfeb3b3cecd828bce4d28910654e64848683ccbb3f5a4e5f97d2a9f2eaa60cd9d2ec3e6d022e8e26d3e23a6e76621ab3a4bd92272d7ca73591e764d56a3953dc944c00fcbbd6a1b8f21d558c6ba7173e6bfab8668172de697c5728b82440466028f5a16321184791675ee3f9ce70dd5056b8bea8d77501c14d05015c9ed880959a7a5b95051f84f438fca6f06;
```

{% hint style="info" %}
On **line 7,**  the `.toString('hex')` is required due to the Crypto module's [**randomBytes** ](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback)function returning a **buffer.**
{% endhint %}



