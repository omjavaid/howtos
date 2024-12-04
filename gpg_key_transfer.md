# How to Transfer GPG Keys Between Computers

This guide will walk you through securely transferring your GPG keys (both public and private) from one computer to another.

---

## **Steps to Transfer GPG Keys**

### **1. Export All Secret Keys from the Source Computer**
On the source computer, export all your secret keys into a single ASCII-armored file:
```bash
gpg --export-secret-keys --armor > all_private_keys.asc
```

- `--export-secret-keys`: Exports your private (secret) keys.
- `--armor`: Outputs the keys in a text-readable format for easier transfer.

The `all_private_keys.asc` file will contain all your private keys. **Keep this file secure** because anyone with access to it can use your keys.

---

### **2. Securely Copy the File to the New Computer**
Transfer the `all_private_keys.asc` file to the new computer using a secure method:

- **Encrypted USB drive**: Save the file on a secure drive and physically transfer it.
- **SSH/SCP**: Use a secure file transfer protocol:
  ```bash
  scp all_private_keys.asc user@new-computer:/path/to/destination
  ```
- **Avoid insecure methods**: Do not email the file or use public file-sharing services.

---

### **3. Import the Secret Keys on the New Computer**
On the new computer, import the secret keys using the following command:
```bash
gpg --import all_private_keys.asc
```

This will add the keys to the new computer’s GPG keyring.

---

### **4. Verify the Imported Keys**
List the secret keys on the new computer to ensure the import was successful:
```bash
gpg --list-secret-keys --keyid-format LONG
```

The output will display your secret keys, similar to this:
```
/home/user/.gnupg/pubring.kbx
-----------------------------------
sec   rsa4096/ABCD1234EFGH5678 2024-01-01 [SC]
      1234567890ABCDEF1234567890ABCDEF12345678
uid           [ultimate] Your Name <your-email@example.com>
ssb   rsa4096/IJKL5678MNOP9012 2024-01-01 [E]
```

---

## **Important Notes**

- **Passphrase Reminder**: Ensure you remember the passphrase for each private key. The passphrase is required to use the keys for signing or decryption.
- **Backup Your Keys**: Keep a secure backup of your keys in a trusted location, like an encrypted drive or a password-protected archive.
- **Delete the `all_private_keys.asc` File**: After the import, delete the key file from both computers to prevent accidental exposure:
  ```bash
  rm all_private_keys.asc
  ```

---

## **Troubleshooting**

- **Forgot the Passphrase?**
  Unfortunately, without the passphrase, the private keys cannot be used. You’ll need to revoke the keys and generate new ones.

- **Revoke Old Keys (If Needed)**:
  If you are no longer using the keys on the old computer, consider revoking them to prevent misuse.

---

By following this guide, you can securely transfer your GPG keys between computers. Let me know if you encounter any issues!
