# Data at Rest \(Files and Databases\)

## Threats

### Loss of Confidentiality

Only the **right data** should be shown to the **right person** \(both users and employees!\)

### Integrity

Ensuring that there are **no unauthorized changes** to the data **without a record** of the change.

Another way of putting it: only data should **only be changed by authorized parties** and a **record made of the change.**

### **Availability**

Data needs to be **where** you need it to be **when** you need it to be there. [**Denial of Service**](https://en.wikipedia.org/wiki/Denial-of-service_attack) attacks are examples of an attack on this front.

## Symmetric Encryption

The data is **encrypted** _before_ storage and **decrypted** when pulled from storage to show to the authorized user.

**Symmetric Encryption** uses **one key** with which it both **encrypts** _and_ **decrypts** the data. This means that without that key, the encrypted data is **unreadable**.

This means that this **one key** must be carefully managed and protected.

### Creating the Cipher

We'll need three things: an **algorithm**, a **key**, and an **initialization vector.**

#### Algorithm

For this example, the **AES-256 Cipher Block Chaining** algorithm will be being used. 

* **AES** stands for the **Advanced Encryption Standard**
* **256** refers to the number of bits
* **Cipher Block Chaining** is the process in which the data is encrypted. The data will be chopped into blocks and encrypted within each block. The output of one block will be the input to the next and so forth. See [this blog article](https://justinboyerwriter.com/2017/07/29/developers-guide-cryptography-basics/) for more information.

Node's crypto module's [createCipheriv ](https://nodejs.org/api/crypto.html#crypto_crypto_createcipheriv_algorithm_key_iv_options)function will want this as a string.

```javascript
const algo = "aes-256-cbc";
```

#### Key

The PBKDF2 password hashing algorithm to generate a salted password to use as the key:

```javascript
const password = 'Good strong password for key';
//? 256 bits being equal to 32 bytes
const salt = crypto.randomBytes(32);
const key = crypto.scryptSync(password, salt, 32);
```

#### Initialization Vector

For the **initialization vector** just needs to be a random 16 bytes created using crypto's [`randomBytes`](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback) function:

```javascript
const iv = crypto.randomBytes(16);
```

Now that the we have all of the parts needed to create the cipher, we need only some data to encrypt and to call the `update` and `final` functions:

```javascript
const cipher = crypto.createCipheriv(algo, key, iv);

//* Now that we have our Cipher prepared, we need data to encrypt...
let socialSecurityNumber = '999-999-9999';

//* First, the update function is used to specify 'utf8' encoding with a 'hex' output
let encrypted = cipher.update(socialSecurityNumber, 'utf8', 'hex');

//* The cipher now has all the data it needs, so
//* the final function will only need which type of output
encrypted += cipher.final("hex");

```

Here's the final results for generating our encrypted data:

```javascript
const crypto = require('crypto');

//* Algorithm - AES-256 Cipher Block Chaining
const algo = "aes-256-cbc";

//* Key
//? In addition to being useful for hashing passwords, the
//? PBKDF2 algo is also useful for generating keys.

const password = 'Good strong password for key';
//? 256 bits being equal to 32 bytes
const salt = crypto.randomBytes(32);
const key = crypto.scryptSync(password, salt, 32);

//* Initialization Vector
//? 16 byte initialization vector  
//! why 16 bytes instead of 32?
const iv = crypto.randomBytes(16);


//* Finally we create our Cipher
const cipher = crypto.createCipheriv(algo, key, iv);

//* Now that we have our Cipher prepared, we need data to encrypt...
let socialSecurityNumber = '999-999-9999';

//* First, the update function is used to specify 'utf8' encoding with a 'hex' output
let encrypted = cipher.update(socialSecurityNumber, 'utf8', 'hex');

//* The cipher now has all the data it needs, so
//* the final function will only need which type of output
encrypted += cipher.final("hex");

console.log(encrypted);
// 1a80967f65731bef8064aab79c7166eb
```

### Creating the Decipher

The Decipher is created in much the same way, except that instead of passing in the data that we want to encrypt as the first argument, we pass in the data we have that is **already encrypted.** 

The `encrypted` variable here is in `hex` form, hence the second argument to `createDecipheriv`.

```javascript
let decrypted = decipher.update(encrypted, 'hex', 'utf8');
decrypted = decipher.final('utf8');
console.log(decrypted);
// 999-999-9999
```

## Protecting the Keys

Often referred to as a **KMS**, a **Key Management Storage System** is a must for proper management and protection of keys. Best practices generally look like this:

* Key store to protect keys
* Encryption keys encrypted by master key
* User requests key when data is needed
* Key store decrypts key and sends to user
* Keys can be rotated regularly for extra security

Both Microsoft Azure \(Key Vault\) and Amazon Web Services \(Key Management Service\) offer paid key management solutions.

### [Vault](https://www.vaultproject.io/)

"Vault" is an open-source KMS solution created by HashiCorp.

