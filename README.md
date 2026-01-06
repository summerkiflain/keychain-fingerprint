# keychain-fingerprint

Touch ID 인증으로 macOS Keychain 비밀번호에 안전하고 편리하게 접근하는 CLI 도구

## Why?

macOS Keychain에 저장된 비밀번호에 접근할 때 두 가지 불편함이 있습니다:

### 문제 1: 보안 vs 편의성 딜레마

`security find-generic-password` 명령어로 비밀번호에 접근하면:

```
"security"가 키체인의 "myapp"에 저장된 기밀 정보를 사용하려고 합니다.
[거부] [허용] [항상 허용]
```

- **"허용"**: 매번 Mac 비밀번호를 입력해야 함 → 번거로움
- **"항상 허용"**: 이후 어떤 앱에서든 비밀번호 없이 접근 가능 → 보안 취약

### 문제 2: 비밀번호 입력의 불편함

Mac 비밀번호는 보통 길고 복잡해서 매번 입력하기 번거롭습니다.

### 해결책: Touch ID

이 도구는 **Touch ID**로 인증하여:
- **빠르고 편리함**: 손가락 한 번으로 인증 (비밀번호 입력 불필요)
- **보안 유지**: 다른 앱에서 접근 시 여전히 Mac 비밀번호 필요

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
