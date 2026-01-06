# keychain-fingerprint

A CLI tool for secure and convenient macOS Keychain access using Touch ID authentication.

📖 **Blog post**: [Secure and Convenient Keychain Access with Touch ID](https://dss99911.github.io/tools/mac/2026/01/06/keychain-fingerprint.html)

## Why?

When accessing passwords stored in macOS Keychain, you face a security vs convenience dilemma:

### The Problem

Running `security find-generic-password` shows this dialog:

```
"security" wants to use your confidential information stored in "myapp" in your keychain.
[Deny] [Allow] [Always Allow]
```

- **"Allow"**: Requires typing Mac password every time → Inconvenient
- **"Always Allow"**: Any app can access without authentication → Insecure

### The Solution: Touch ID

This tool uses **Touch ID** for authentication:
- **Fast and convenient**: One touch to authenticate (no password typing)
- **Secure**: Other apps still require Mac password to access

## Installation

```bash
# Clone
git clone https://github.com/dss99911/keychain-fingerprint.git
cd keychain-fingerprint

# Compile
swiftc -o keychain-fingerprint main.swift -framework LocalAuthentication -framework Security

# Install (optional)
sudo cp keychain-fingerprint /usr/local/bin/
```

## Usage

```bash
# Save password (Touch ID → secure input)
keychain-fingerprint set myapp user@example.com

# Get password (Touch ID → stdout)
keychain-fingerprint get myapp user@example.com

# List saved items (Touch ID)
keychain-fingerprint list

# Delete password (Touch ID)
keychain-fingerprint delete myapp user@example.com
```

### Shell Variable (Recommended)

```bash
# Capture password in variable (not displayed on screen)
PASSWORD=$(keychain-fingerprint get myapp user@example.com)

# Use the password
echo "Using password..."

# Clear the variable when done
unset PASSWORD
```

## Security

| Access Method | Authentication Required |
|---------------|------------------------|
| This app | Touch ID |
| Other apps / `security` command | Mac password |

### How it works

```
┌─────────────────────────────────────────┐
│         keychain-fingerprint            │
├─────────────────────────────────────────┤
│  1. Touch ID authentication             │
│  2. Access Keychain (auto-authorized)   │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│         Other apps / terminal           │
├─────────────────────────────────────────┤
│  Keychain access → Mac password prompt  │
└─────────────────────────────────────────┘
```

### Features

- All commands require Touch ID
- Passwords stored encrypted in macOS Keychain
- Password input is hidden (no echo)
- Passwords only output to stdout (for variable capture)
- Device-only access (`kSecAttrAccessibleWhenUnlockedThisDeviceOnly`)

## Requirements

- macOS with Touch ID
- Xcode Command Line Tools (`xcode-select --install`)

## License

MIT
