
# Git Configuration for GPG Signing

## Edit / Create GPG Key

### List existing keys

```bash
gpg --list-secret-keys --keyid-format=long
```

### Generating a new key

If no keys exist or you wish to generate a new one:

```bash
gpg --full-generate-key
```

Recommended choices:

```
Key type: ECC  
Curve: ed25519  
Usage: sign only
```

Then provide:

```
Name: Your Name
Email: your-commit-email@example.com
```

> **NOTE:**
> 
> **GitHub** noreply email address can be accessed here:
> `Settings > Emails` (email address is listed at the top of the page)
> 
> **GitLab** noreply email addresses can be accessed here:
> `Preferences > Profile` (Commit Email section)

### Edit Existing Key


> **IMPORTANT:**
> Whenever you edit a key, it changes the public key value and you must export it again and add that value to the version control platforms as shown below.

Get Key ID:

```bash
gpg --list-secret-keys --keyid-format=long
```

Example output where `0000XXXXXX` is the KEYID:

```bash
sec   ed25519/0000XXXXXX 2026-03-09 [SC]
      8A80000XXXX0000XXXXXXXX
uid                 [ultimate] Your Name <email1@example.com>
uid                 [ultimate] Your Name <email2@example.com>
```

Adding new UID:

```bash
gpg --edit-key <KEYID>
adduid

# Name: Your Name
# Email: your-commit-email@example.com
```

Deleting or Revoking an ID

```bash
gpg --edit-key <KEYID>

# Example Output:
# [ultimate] (1). John Doe <john@example.com>
# [ultimate] (2) John Doe #2 <john.doe@example.com>

# Selecting ID 2 to revoke
uid 2

# Example Output:
# NOTE: As you select UIDs, they will be indicated with an asterisk.
# [ultimate] (1). John Doe <john@doe.tld>
# [ultimate] (2)* John Doe #2 (Corp) <john.doe@example.com>

# Delete
deluid

# Revoke
revuid
```

## Export the public key

```bash
gpg --armor --export <KEYID>
```

This prints something like:

```
-----BEGIN PGP PUBLIC KEY BLOCK-----  
...  
-----END PGP PUBLIC KEY BLOCK-----
```

## Add the key to version control platform

Paste the exported public key.

The commit will be marked as **Verified** if:

- the commit email matches your account
- the commit is signed with that key

## Configure Git to use the key

Add settings to global `.gitconfig`:

```bash
git config --global user.signingkey 0000XXXXXX

git config --global commit.gpgsign true
```

Equivalent to `.gitconfig` section:

```bash
[user]  
  signingkey = 0000XXXXXX  
  
[commit]  
  gpgsign = true  
  
[gpg]  
  program = gpg
```

If using multiple config files, enter these settings in the respective files instead of the global config.

## Verify locally

Create a signed commit using the `-S` flag:

```bash
git commit --allow-empty -S -m "Test signed commit"
```

Inspect it:

```bash
git log --show-signature -1
```

You should see output similar to:

```
gpg: Good signature from "Your Name <your-commit-email@example.com>"
```

## Optional: Sign tags too

```bash
git config --global tag.gpgsign true

git tag -s <tag-name>
```
