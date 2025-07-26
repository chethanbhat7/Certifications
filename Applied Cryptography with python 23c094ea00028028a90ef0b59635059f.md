# Applied Cryptography with python

> Cryptography is the science of securing information by transforming it into an unreadable format — only authorized parties can decode it.
> 

---

### ✅ **Why is Cryptography Required?**

1. **Confidentiality** – hides data from unauthorized users
2. **Integrity** – ensures data isn't changed
3. **Authentication** – verifies sender/receiver identity
4. **Non-repudiation** – prevents denial of actions

---

### 🔧 **Types of Cryptography**

- **Symmetric** (same key for encryption & decryption)
- **Asymmetric** (public & private keys)
- **Hashing** (one-way encryption, e.g., passwords)

---

### 💡 **Applied Cryptography with Python**

Use libraries like:

- `cryptography` – encrypt/decrypt with Fernet
- `hashlib` – hash strings (e.g., SHA256)
- `pycryptodome` – advanced encryption (AES, RSA)

## `cryptography` Library

The `cryptography` library in Python is used for:

- Encrypting and decrypting data
- Generating secure keys
- Safeguarding sensitive information

It supports **Fernet encryption**, which is:

- Symmetric (same key for encryption & decryption)
- Easy to use
- Secure (uses AES-128 under the hood)

---

## ✅ Installation

```bash

pip install cryptography

```

---

## 🧪 Example: Encrypt & Decrypt Message Using `Fernet`

```python

from cryptography.fernet import Fernet

# Step 1: Generate a key (store this securely)
key = Fernet.generate_key()

# Step 2: Create a Fernet cipher object
cipher = Fernet(key)

# Step 3: Define a message and convert it to bytes
message = "This is a secret".encode()

# Step 4: Encrypt the message
encrypted = cipher.encrypt(message)
print("Encrypted:", encrypted.decode())

# Step 5: Decrypt the message
decrypted = cipher.decrypt(encrypted)
print("Decrypted:", decrypted.decode())

```

---

### 📘 Output:

```

Encrypted: gAAAAABk...
Decrypted: This is a secret

```

### Asymmetric Ecryption

**Asymmetric Encryption** uses **two keys**:

- **Public key** — used to **encrypt** the data.
- **Private key** — used to **decrypt** the data.

It ensures **confidentiality** because only the owner of the private key can decrypt the message.

---

### 🛠 Why Use Asymmetric Encryption?

- You can **share the public key openly**.
- **Only you (with private key)** can decrypt the data.
- Common in secure communication (e.g., HTTPS, emails, authentication).

---

### 🔑 Popular Algorithm: **RSA (Rivest–Shamir–Adleman)**

Used in:

- Secure websites (SSL/TLS)
- JWT authentication
- Digital signatures

---

### ✅ Python Example Using `cryptography` Library

### 🔧 Step-by-step RSA encryption and decryption

```bash

pip install cryptography

```

```python
cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.primitives import serialization, hashes
from cryptography.hazmat.primitives.asymmetric import padding

# Step 1: Generate RSA key pair
private_key = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
)

# Step 2: Get public key from private key
public_key = private_key.public_key()

# Step 3: Message to encrypt
message = b"This is a secret message"

# Step 4: Encrypt using public key
encrypted = public_key.encrypt(
    message,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        algorithm=hashes.SHA256(),
        label=None
    )
)

print("🔐 Encrypted message:", encrypted)

# Step 5: Decrypt using private key
decrypted = private_key.decrypt(
    encrypted,
    padding.OAEP(
        mgf=padding.MGF1(algorithm=hashes.SHA256()),
        algorithm=hashes.SHA256(),
        label=None
    )
)

print("🔓 Decrypted message:", decrypted.decode())

```

### **Digital Signature**

A **Digital Signature** is like an **electronic seal** that verifies:

- ✅ **Authenticity** – the message really came from the sender.
- ✅ **Integrity** – the message was **not changed** in transit.

---

### 🔐 How it works (in simple steps):

1. **Sender signs** the message using their **private key**.
2. The **receiver verifies** it using the sender's **public key**.
3. If verification succeeds → message is **authentic and unmodified**.

---

### 🛠 Why Digital Signatures Are Important

| Feature | Description |
| --- | --- |
| **Non-repudiation** | Sender **cannot deny** they sent it |
| **Integrity** | Detects if message was **tampered** |
| **Authentication** | Proves the **identity** of the sender |

---

### ✅ Python Example Using `cryptography` Library

```bash

pip install cryptography

```

```python

from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.primitives import hashes

# Step 1: Generate keys
private_key = rsa.generate_private_key(public_exponent=65537, key_size=2048)
public_key = private_key.public_key()

# Step 2: Original message
message = b"Hello, this is a signed message!"

# Step 3: Create a digital signature using private key
signature = private_key.sign(
    message,
    padding.PSS(
        mgf=padding.MGF1(hashes.SHA256()),
        salt_length=padding.PSS.MAX_LENGTH
    ),
    hashes.SHA256()
)

print("✅ Digital Signature created.")

# Step 4: Receiver verifies using public key
try:
    public_key.verify(
        signature,
        message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )
    print("✅ Signature verified. Message is authentic.")
except Exception:
    print("❌ Signature invalid. Message may be tampered.")

```