# GPG CLI Cheat Sheet

This cheat sheet covers common **PGP / GnuPG (`gpg`) CLI commands** for encrypting and decrypting files and folders on **macOS**, assuming **no GUI** and a working `gpg` installation.

## Quick Start

```bash
# Encrypt (Symmetric Encryption - Passphrase Only)
gpg --symmetric file.txt

# Decrypt
gpg --decrypt file.txt.gpg > file.txt

# Encrypt a directory by zipping/compressing it and then encrypt the file
```

## Verify Installation

```bash
gpg --version
```

## Key Management

### List Keys

```bash
# Public keys
gpg --list-keys

# Secret (private) keys
gpg --list-secret-keys --keyid-format=long
```

### Generate a Key Pair

```bash
gpg --full-generate-key
```

Recommended:

- Type: RSA and RSA
- Key size: 4096
- Expiration: optional but recommended
- Strong passphrase

### Export Public Key (Safe to Share)

```bash
gpg --armor --export you@example.com > publickey.asc
```

### Export Private Key (⚠️ Sensitive)

```bash
gpg --armor --export-secret-keys you@example.com > privatekey.asc
```

### Import a Key

```bash
gpg --import publickey.asc
```

### Delete Keys

```bash
# Delete secret key first
gpg --delete-secret-key KEYID

# Then delete public key
gpg --delete-key KEYID
```

## Encrypting Files

### Symmetric Encryption (Passphrase Only)

```bash
gpg --symmetric file.txt
```

Specify cipher:

```bash
gpg --symmetric --cipher-algo AES256 file.txt
```

### Encrypt for a Recipient (Public Key Encryption)

```bash
gpg --encrypt --recipient you@example.com file.txt
```

Creates:

```
file.txt.gpg
```

ASCII armored version:

```bash
gpg --armor --encrypt --recipient you@example.com file.txt
```

## Decrypting Files

### Decrypt to File

```bash
gpg --decrypt file.txt.gpg > file.txt
```

### Decrypt to STDOUT

```bash
gpg --decrypt file.txt.gpg
```

## Encrypting & Decrypting Folders

PGP does **not** encrypt directories directly. Archive first.

### Encrypt a Folder

```bash
tar -czf secrets.tar.gz secrets/
gpg --encrypt --recipient you@example.com secrets.tar.gz
```

### Decrypt a Folder

```bash
gpg --decrypt secrets.tar.gz.gpg > secrets.tar.gz
tar -xzf secrets.tar.gz
```

One-liner:

```bash
gpg --decrypt secrets.tar.gz.gpg | tar -xz
```

## Signing & Verification

### Create Detached Signature

```bash
gpg --detach-sign file.txt
```

### Verify Signature

```bash
gpg --verify file.txt.sig file.txt
```

### Encrypt + Sign

```bash
gpg --encrypt --sign --recipient you@example.com file.txt
```

## Key Trust

After importing someone’s public key:

```bash
gpg --edit-key KEYID
```

Inside prompt:

```
trust
```

⚠️ Only assign **ultimate trust** to **your own keys**.

## Inspecting Files & Keys

### Show Key Fingerprint

```bash
gpg --fingerprint you@example.com
```
### Inspect Encrypted File

```bash
gpg --list-packets file.txt.gpg
```

## macOS GPG Paths

| Item         | Path                      |
| ------------ | ------------------------- |
| GPG home     | `~/.gnupg/`               |
| Config       | `~/.gnupg/gpg.conf`       |
| Agent config | `~/.gnupg/gpg-agent.conf` |

Permissions:

```bash
chmod 700 ~/.gnupg
chmod 600 ~/.gnupg/*
```

## Recommended gpg.conf

```conf
use-agent
cipher-algo AES256
digest-algo SHA512
cert-digest-algo SHA512
default-preference-list SHA512 SHA384 SHA256 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed
```

Reload agent:

```bash
gpgconf --kill gpg-agent
```

## Why GPG Sometimes Doesn’t Prompt for a Password

### Short Answer

Because **gpg-agent caches your passphrase**.

### What Happens

1. You enter your passphrase once (encrypting, signing, etc.)
2. gpg-agent caches it in memory
3. Subsequent decrypts succeed **without prompting**

This is **normal and expected behavior**.

### Force a Prompt Immediately

```bash
gpgconf --kill gpg-agent
```

### Disable Passphrase Caching (Advanced)

Edit:

```bash
~/.gnupg/gpg-agent.conf
```

Add:

```conf
default-cache-ttl 0
max-cache-ttl 0
```

Restart agent:

```bash
gpgconf --kill gpg-agent
```

⚠️ This will prompt for every operation.

## Quick Reference

| Task              | Command                 |
| ----------------- | ----------------------- |
| Encrypt file      | `gpg -e -r user file`   |
| Decrypt file      | `gpg -d file.gpg`       |
| Symmetric encrypt | `gpg -c file`           |
| Encrypt folder    | `tar + gpg`             |
| Sign file         | `gpg --sign file`       |
| Verify signature  | `gpg --verify sig file` |

## Mental Model

> **GPG does cryptography**  
> **gpg-agent manages secrets**

Once unlocked, the agent stays unlocked until:

- TTL expires
- Agent restarts
- You kill it manually