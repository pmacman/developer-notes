# NPM Cheat Sheet

## Project Setup

### Initialize a new project

```bash
npm init
```

```bash
npm init -y
```

## Installing Packages

### Install all dependencies

```bash
npm install
```

### Install a dependency

```bash
npm install <package>
```

### Install a dev dependency

```bash
npm install -D <package>
```

### Install a specific version

```bash
npm install <package>@1.2.3
```

### Install globally

```bash
npm install -g <package>
```

### Install a dependency for CI build

```bash
npm ci
```

This will:

- Delete `node_modules/`
- Require `package-lock.json` to exist
- Install _exactly_ the versions in `package-lock.json`
- Fail if `package.json` and `package-lock.json` are out of sync
- Never modify `package.json` or `package-lock.json`

## Updating Packages

### Update within version ranges

```bash
npm update
```

### Check outdated packages

```bash
npm outdated
```

### Update all packages to latest (including majors)

```bash
npx npm-check-updates -u
npm install
```

### Update only minor versions

```bash
npx npm-check-updates -u --target minor
npm install
```

### Update only patch versions

```bash
npx npm-check-updates -u --target patch
npm install
```

## Cleaning & Resetting

### Remove a package

```bash
npm uninstall <package>
```

### Clean reinstall

```bash
rm -rf node_modules package-lock.json
npm install
```

### Deduplicate dependencies

```bash
npm dedupe
```

## Scripts

### Run a script

```bash
npm run <script>
```

### List scripts

```bash
npm run
```

### Run a package binary

```bash
npx <command>
```

## Info & Debugging

```bash
npm list <package>
npm why <package>
npm view <package>
npm view <package> versions
```

## Security

```bash
npm audit
npm audit fix
npm audit fix --force
```

## Mental Model

| Command           | Purpose              |
| ----------------- | -------------------- |
| npm install       | Install dependencies |
| npm update        | Update within ranges |
| npm outdated      | Show outdated        |
| npx               | Run binaries         |
| npm-check-updates | Upgrade versions     |