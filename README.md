# SecureFile Recovery System

## Overview

SecureFile Recovery System is a Java-based cybersecurity project that demonstrates how hybrid encryption, RSA, AES, digital signatures and client-server communication can be used to securely encrypt and recover files.

The project is an educational simulation of ransomware-style file encryption. A file is encrypted using AES, while the AES encryption key is protected using RSA. A recovery server then authenticates the user and securely returns the AES key so that the original file can be recovered.

## What the Project Does

The system consists of four main components:

### 1. RSA Key Generation

`RSAKeyGenerator.java` generates a 2048-bit RSA public/private key pair for a user.

For example:

```text
alice.pub
alice.prv
```

The public key can be shared, while the private key must be kept secure.

### 2. File Encryption

`FileEncryptor.java`:

- Reads `test.txt`
- Generates an AES-256 encryption key
- Encrypts the file using AES
- Creates an encrypted file called `test.txt.cry`
- Encrypts the AES key using the recovery server's RSA public key
- Stores the encrypted AES key in `aes.key`
- Deletes the original `test.txt`

This demonstrates hybrid encryption, where AES is used for the file because it is efficient for encrypting data, while RSA is used to protect the AES key.

### 3. Secure Key Recovery Server

`FileRecoveryServer.java` acts as the recovery server.

The server:

- Receives the user's ID
- Receives the encrypted AES key
- Receives a digital signature
- Loads the user's public key
- Verifies that the request came from the correct user
- Uses the server's private RSA key to decrypt the AES key
- Sends the recovered AES key back to the user

### 4. File Decryption

`FileDecryptor.java`:

- Loads the user's private key
- Signs the recovery request
- Connects to the recovery server
- Sends the encrypted AES key and digital signature
- Receives the decrypted AES key
- Uses the AES key to decrypt `test.txt.cry`
- Recreates the original `test.txt`

---

## The Purpose of `server-b64.prv`

`server-b64.prv` is the **private RSA key belonging to the recovery server**.

It has an important role in the encryption and recovery process.

When `FileEncryptor.java` encrypts the file, it generates an AES key. Instead of storing this AES key in plain text, the AES key is encrypted using the recovery server's RSA **public key**.

The matching RSA private key is stored in:

```text
server-b64.prv
```

The recovery server uses this private key to decrypt the encrypted AES key when a valid recovery request is received.



## Why Is This Project Useful?

This project demonstrates several important cybersecurity and software engineering concepts:

- **Hybrid encryption** using AES and RSA
- **AES-256 symmetric encryption**
- **RSA public/private key cryptography**
- **Digital signatures**
- **User authentication**
- **Secure key recovery**
- **Client-server communication**
- **Cryptographic key management**
- **Confidentiality and data protection**
- **Java networking using sockets**

It also demonstrates how different cryptographic techniques can work together rather than relying on a single encryption algorithm.

---

## Technologies Used

- Java
- RSA
- AES-256
- SHA-256
- Digital signatures
- Java Cryptography Architecture (JCA)
- Java sockets
- IntelliJ IDEA
- Git/GitHub

## Getting Started

### Prerequisites

You will need:

- Java JDK installed
- IntelliJ IDEA or another Java IDE
- Git
- A terminal/command prompt

### 1. Clone the Repository

```bash
git clone https://github.com/ac957/CO3099_Assignment.git
```

Open the project in IntelliJ IDEA.

### 2. Generate a User Key Pair

Run:

```bash
java RSAKeyGenerator alice
```

This creates:

```text
alice.pub
alice.prv
```

Keep `alice.prv` private.

### 3. Create a Test File

Create a file called:

```text
test.txt
```

Add some text to the file so that you can test the encryption and recovery process.

### 4. Encrypt the File

Run:

```bash
java FileEncryptor
```

The program will:

- Encrypt `test.txt`
- Create `test.txt.cry`
- Create `aes.key`
- Remove the original `test.txt`

### 5. Start the Recovery Server

Open a second terminal and run:

```bash
java FileRecoveryServer 9999
```

The server should start listening on port `9999`.

### 6. Recover the File

In another terminal, run:

```bash
java FileDecryptor localhost 9999 alice
```

The client will contact the recovery server and authenticate the recovery request.

If successful, the server will decrypt the AES key using `server-b64.prv` and return it to the client.

The client will then use the AES key to decrypt:

```text
test.txt.cry
```

and recreate:

```text
test.txt
```

---

## Running the Project in IntelliJ IDEA

The programs require different command-line arguments.

| Program | Program Arguments |
|---|---|
| `RSAKeyGenerator` | `alice` |
| `FileEncryptor` | No arguments |
| `FileRecoveryServer` | `9999` |
| `FileDecryptor` | `localhost 9999 alice` |

These arguments can be added under **Run → Edit Configurations → Program arguments** in IntelliJ IDEA.

### Recommended Run Order

Run the programs in this order:

```text
1. RSAKeyGenerator
2. FileEncryptor
3. FileRecoveryServer
4. FileDecryptor
```

The recovery server should be running before `FileDecryptor` attempts to connect to it.

---

## Cybersecurity Concepts Demonstrated

### Hybrid Encryption

The project combines two encryption methods:

**AES**

Used to encrypt the actual file because symmetric encryption is efficient for large amounts of data.

**RSA**

Used to encrypt the AES key because RSA allows the key to be securely protected using public-key cryptography.

### Digital Signatures

`FileDecryptor` creates a digital signature using the user's private RSA key.

The recovery server uses the corresponding public key to verify the signature.

This helps ensure that the recovery request came from the expected user.

### Client-Server Communication

The project uses Java sockets to allow the file recovery client and recovery server to communicate.

The client sends:

```text
User ID
Encrypted AES key
Digital signature
```

The server validates the request and returns the decrypted AES key.

---


## Maintainers and Contributors

This project was developed collaboratively by:

**Anetta Chibangula**  
**Tonia Fru**

Both contributors collaborated on the development of the project and its implementation of encryption, key management, authentication and secure file recovery.


## Disclaimer

This project is intended for **educational and cybersecurity learning purposes only**.

It demonstrates concepts associated with ransomware-style file encryption and recovery in a controlled environment. It should not be used to encrypt, modify or interfere with files belonging to other users or systems without permission.
