# How to Sign Executables with GPG

This guide explains how to sign an executable file with a GPG key using a **detached signature** and verify the signature.

---

## **Why Use Detached Signatures?**

Detached signatures are stored in a separate file, leaving the original file unchanged. This approach is ideal for signing sensitive files like executables, where modification can break functionality or checksums.

---

## **Steps to Sign an Executable**

### **1. List Your GPG Keys**
Identify the GPG key you want to use for signing:
```bash
gpg --list-secret-keys --keyid-format LONG
```

Example output:
```
/home/user/.gnupg/pubring.kbx
-----------------------------------
sec   rsa4096/ABCD1234EFGH5678 2024-01-01 [SC]
      1234567890ABCDEF1234567890ABCDEF12345678
uid           [ultimate] Your Name <your-email@example.com>
```

- **Key ID**: The portion after `rsa4096/` (e.g., `ABCD1234EFGH5678`).

---

### **2. Create a Detached Signature**
Generate a detached signature for your executable:
```bash
gpg --armor --detach-sign --local-user ABCD1234EFGH5678 --output your_executable.sig your_executable
```

**Explanation**:
- `--armor`: Outputs the signature in a readable ASCII format (`.asc` file).
- `--detach-sign`: Creates a detached signature in a separate file.
- `--local-user`: Specifies the key to use for signing (replace with your key ID).
- `--output`: Defines the name of the signature file.
- `your_executable`: The file to be signed.

This command creates `your_executable.sig`, which is the detached signature for `your_executable`.

---

## **Steps to Verify the Signature**

To verify the executable using the signature file and your public key:
```bash
gpg --verify your_executable.sig your_executable
```

**Expected Output**:
- **If the signature is valid**:
  ```
  gpg: Signature made 2024-12-04 14:00 PKT using RSA key ABCD1234EFGH5678
  gpg: Good signature from "Your Name <your-email@example.com>"
  ```
- **If the file has been tampered with**:
  ```
  gpg: BAD signature from "Your Name <your-email@example.com>"
  ```

---

## **Sharing Your Public Key**

For others to verify your signatures, share your public key:

1. Export your public key:
   ```bash
   gpg --armor --export ABCD1234EFGH5678 > public_key.asc
   ```

2. Recipients can import your public key:
   ```bash
   gpg --import public_key.asc
   ```

---
