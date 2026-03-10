# NVM Cheat Sheet

A quick reference for managing Node.js versions using **nvm**.

## Installation & Setup

```bash
nvm --version        # Verify nvm is installed
command -v nvm       # Check that nvm is available
```

## Installing Node Versions

```bash
nvm install node     # Install latest Node.js
nvm install --lts    # Install latest LTS version
nvm install 20       # Install a specific major version
nvm install 20.11.1  # Install a specific exact version
```

## Listing Versions

```bash
nvm ls               # List installed Node versions
nvm ls-remote        # List all available remote versions
nvm ls-remote --lts  # List all available LTS versions
```

## Using Node Versions

```bash
nvm use 20           # Use Node 20 in current shell
nvm use --lts        # Use latest LTS
nvm use node         # Use latest installed Node
```

Make a version the default for new shells:

```bash
nvm alias default 20
nvm alias default --lts
```

## Checking Current Version

```bash
node -v              # Show current Node version
nvm current          # Show current nvm-managed version
```

## Aliases

```bash
nvm alias             # List aliases
nvm alias mynode 20   # Create alias
nvm use mynode        # Use alias
```

## Uninstalling Node Versions

```bash
nvm uninstall 18
nvm uninstall 18.19.0
```

## Project-Level Version Control

Create a `.nvmrc` file in your project:

```bash
echo 20 > .nvmrc
```

Then run:

```bash
nvm use
nvm install          # Installs version from .nvmrc if missing
```

## npm Version Management

Each Node version has its **own npm version**.

```bash
npm -v               # Check npm version
npm install -g npm   # Update npm for current Node version
```

## Global Packages per Node Version

Global npm packages are **isolated per Node version**.

```bash
npm list -g --depth=0
```

Reinstall globals after switching versions if needed.

## Environment & Cleanup

```bash
nvm deactivate       # Remove Node from PATH
nvm cache dir        # Show nvm cache
nvm cache clear      # Clear downloaded Node archives
```

## Common Pitfalls

- `npm install -g` installs packages **only for the active Node version**
- `brew install node` can conflict with nvm-managed Node
- Always restart your shell after installing nvm

## Recommended Workflow

```bash
nvm install --lts
nvm alias default --lts
echo lts/* > .nvmrc
```

## References

- https://github.com/nvm-sh/nvm