# @belocal/cli

The official command-line interface for [belocal.dev](https://belocal.dev).

This CLI tool is designed for translating i18n interfaces and managing your static localization files directly from your terminal. It automates the process of localizing your application's UI into multiple languages.

## Usage

You can run the CLI directly using `npx` without installing it globally.

### 1. Login

Authenticate with your belocal.dev account:

```bash
npx @belocal/cli@latest login %api_key%
```

### 2. Initialize Configuration

Create a `belocal.config.json` file in your project root:

```bash
npx @belocal/cli@latest init
```

This command will guide you through setting up your source language, target languages, and the file patterns for your localization files.

### 3. Translate

Translate your localization files based on your configuration:

```bash
npx @belocal/cli@latest translate
```

### Other Commands

- `npx @belocal/cli@latest whoami` - Check your current authentication status.
- `npx @belocal/cli@latest logout` - Log out from your account.
