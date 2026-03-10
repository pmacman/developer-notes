# Git Configuration Using Identities

## Overview

This document explains how to configure Git to use **Identity Routing**. One use case is for switching between different version control platforms, or switching between different accounts (personal and work). The following structure will be used to demonstrate this process.

### Code directory structure:

```
~/Code/Personal/
  github/
    repo1/
    repo2/
  gitlab/
    repo/
~/Code/Work/
  github/
    repo1/
    repo2/
  gitlab/
    repo/
```

### .gitconfig structure:

```
~/.gitconfig
~/.gitconfig-github-personal
~/.gitconfig-gitlab-personal
~/.gitconfig-github-work
~/.gitconfig-gitlab-work
```

## Create .gitconfig files

### .gitconfig

These can be set with the following command or by editing the file in a text editor:

`git config --global user.useConfigOnly true`

```bash
[user]
  useConfigOnly = true

[init]
  defaultBranch = main

[includeIf "gitdir/i:~/Code/Personal/github/"]
  path = ~/.gitconfig-github-personal

[includeIf "gitdir/i:~/Code/Personal/gitlab/"]
  path = ~/.gitconfig-gitlab-personal

[includeIf "gitdir/i:~/Code/Work/github/"]
  path = ~/.gitconfig-github-work

[includeIf "gitdir/i:~/Code/Work/gitlab/"]
  path = ~/.gitconfig-gitlab-work
```

The optional `useConfigOnly` setting is used for the following reason:

- **Consistency**: Ensures that the name and email used in commits are always the ones specified in your configuration files.
- **Avoids Confusion**: Prevents Git from using any default values that might be set elsewhere, which can lead to mistakes in commit history.

Note that directory paths contain a trailing slash. Without the trailing slash, Git may match unintended directories such as:

```
~/Code/Personal/github-old
~/Code/Personal/github_backup
```

The slash restricts the match to that directory tree.

### .gitconfig-github-personal

```bash
[user]
  name = my-github-personal-username
  email = my-github-personal-email@example.com
  signingkey = my-github-personal-signing-key

[commit]
  gpgsign = true
```

### .gitconfig-gitlab-personal

```bash
[user]
  name = my-gitlab-personal-username
  email = my-gitlab-personal-email@example.com
```

### .gitconfig-github-work

```bash
[user]
  name = my-github-work-username
  email = my-github-work-email@example.com
  signingkey = my-github-work-signing-key

[commit]
  gpgsign = true
```

### .gitconfig-gitlab-work

```bash
[user]
  name = my-gitlab-work-username
  email = my-gitlab-work-email@example.com
```

## Testing

You can confirm the active identity in any repo with:

```bash
git config --show-origin --get-regexp '^user\.'

# Type "Q" to quit
```

Example output:

```
file:/Users/username/.gitconfig-github-personal user.name Your Name
file:/Users/username/.gitconfig-github-personal user.email my-github-personal-email@example.com
```

Now when switching between repositories, the appropriate config file will be selected

## Optional: Configure SSH to connect to repositories

SSH is the recommended authentication method as it avoids passwords and personal access tokens.

### Check for an existing SSH key

```bash
ls ~/.ssh
```

### Create a new SSH key

This example uses the following naming convention.

```bash
ssh-keygen -t ed25519 \
  -f ~/.ssh/id_ed25519_github_personal_repos \
  -C "github-personal-repos"

ssh-keygen -t ed25519 \
  -f ~/.ssh/id_ed25519_gitlab_personal_repos \
  -C "gitlab-personal-repos"

ssh-keygen -t ed25519 \
  -f ~/.ssh/id_ed25519_github_work_repos \
  -C "github-work-repos"

ssh-keygen -t ed25519 \
  -f ~/.ssh/id_ed25519_gitlab_work_repos \
  -C "gitab-work-repos"
```

- Press **Enter** to accept the default file location
- Optionally set a passphrase

This creates:

- `~/.ssh/id_ed25519_github_personal_repos` (private key)
- `~/.ssh/id_ed25519_github_personal_repos.pub` (public key)
- `~/.ssh/id_ed25519_gitlab_personal_repos` (private key)
- `~/.ssh/id_ed25519_gitlab_personal_repos.pub` (public key)
- `~/.ssh/id_ed25519_github_work_repos` (private key)
- `~/.ssh/id_ed25519_github_work_repos.pub` (public key)
- `~/.ssh/id_ed25519_gitlab_work_repos` (private key)
- `~/.ssh/id_ed25519_gitlab_work_repos.pub` (public key)

### Add the SSH key to version control platforms

Copy the public key:

```bash
cat ~/.ssh/id_ed25519_{descriptor}.pub
```

Paste it into the **SSH** section of each version control platform.

### Modify ~/.ssh/config

Note: There are multiple approaches to handling the config file. This example attaches multiple SSH keys to each version control platform. SSH offers the keys in order until one works.

Add the following to `~/.ssh/config`:

```bash
Host github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_personal_repos  
  IdentityFile ~/.ssh/id_ed25519_github_work_repos
  IdentitiesOnly yes

Host gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_gitlab_personal_repos
  IdentityFile ~/.ssh/id_ed25519_gitlab_work_repos
  IdentitiesOnly yes
```

Alternatively, **host aliases** can be used.

```bash
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_personal_repos
  IdentitiesOnly yes

Host gitlab-personal
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_gitlab_personal_repos
  IdentitiesOnly yes

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_github_work_repos
  IdentitiesOnly yes

Host gitlab-work
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_gitlab_work_repos
  IdentitiesOnly yes
```

Then the remotes can be customized as follows:

```bash
git@github-personal:user/repo.git
git@gitlab-personal:user/repo.git
git@github-work:org/repo.git
git@gitlab-work:user/repo.git
```

### Test SSH access

```bash
ssh -T git@github.com
ssh -T git@gitlab.com
```

Or if using **host aliases**:

```bash
ssh -T git@github-personal
ssh -T git@gitlab-personal
ssh -T git@github-work
ssh -T git@gitlab-work
```

You should see a successful authentication message.