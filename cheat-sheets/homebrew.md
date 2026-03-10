# Homebrew Cheat Sheet

A practical, minimal reference for everyday Homebrew usage on macOS.

## Basics

```bash
brew --version
brew help
```

## Installing

### Install a formula (CLI tool)

```bash
brew install git
```

### Install a cask (GUI app)

```bash
brew install --cask firefox
```

## Listing Installed Packages

### Everything (formulas + dependencies)

```bash
brew list
```

### Explicitly installed formulas (no deps)

```bash
brew leaves
```

### Explicit formulas only (tracked intent)

```bash
brew leaves --installed-on-request
```

### Installed casks (apps)

```bash
brew list --cask
```

## Searching & Info

```bash
brew search node
brew info node
brew info --cask firefox

# Find what package(s) depend on another package
brew uses --installed <package name>
```

## Updating & Upgrading

### Update Homebrew metadata

```bash
brew update
```

### Upgrade all packages

```bash
brew upgrade

# Dry run
brew upgrade --dry-run
```

### Upgrade a single package

```bash
brew upgrade git
```

## Cleanup & Maintenance

### Preview unused dependencies

```bash
brew autoremove --dry-run
```

### Remove unused dependencies

```bash
brew autoremove
```

### Clean old versions

```bash
brew cleanup
```

### Check for issues

```bash
brew doctor
```

### Maintenance Commands to Run Occassionally

```bash
brew update
brew upgrade
brew autoremove
brew cleanup
brew doctor
```

## Uninstalling

```bash
brew uninstall git
brew uninstall --cask firefox
```

## Pinning (Prevent Upgrades)

```bash
brew pin node
brew unpin node
```

## Services (if applicable)

```bash
brew services list
brew services start redis
brew services stop redis
```

## Paths & Debugging

```bash
which brew
brew config
brew --prefix
```

## Export Installed Packages (Backup)

```bash
brew bundle dump --file Brewfile
```

Reinstall later with:

```bash
brew bundle
```

## Homebrew Config Settings

```bash
# Disable analytics
brew analytics off
```

## Common Gotchas

- `brew list` includes dependencies — use `brew leaves`
- Prefer **either** Homebrew Node **or** nvm, not both
- Run `brew cleanup` occasionally
- Always run `brew update` before `brew upgrade`