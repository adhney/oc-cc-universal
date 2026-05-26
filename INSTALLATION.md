# Installation

## Homebrew (macOS & Linux)

```bash
brew tap samueltuyizere/tap
brew install oc-cc-universal
```

## Scoop (Windows)

```powershell
scoop bucket add oc-cc-universal https://github.com/samueltuyizere/scoop-bucket
scoop install oc-cc-universal
```

## Build from Source

```bash
git clone https://github.com/samueltuyizere/oc-cc-universal.git
cd oc-cc-universal
make build

# Binary is at bin/oc-cc-universal
# Optionally install to $GOPATH/bin
make install
```

## Download a Release Binary

Download the latest release for your platform from the [Releases page](https://github.com/samueltuyizere/oc-cc-universal/releases):

| Platform              | File                         |
| --------------------- | ---------------------------- |
| macOS (Apple Silicon) | `oc-cc-universal_darwin-arm64`      |
| macOS (Intel)         | `oc-cc-universal_darwin-amd64`      |
| Linux (x86_64)        | `oc-cc-universal_linux-amd64`       |
| Linux (ARM64)         | `oc-cc-universal_linux-arm64`       |
| Windows (x86_64)      | `oc-cc-universal_windows-amd64.exe` |
| Windows (ARM64)       | `oc-cc-universal_windows-arm64.exe` |

```bash
# macOS Apple Silicon
curl -L -o oc-cc-universal https://github.com/samueltuyizere/oc-cc-universal/releases/latest/download/oc-cc-universal_darwin-arm64
chmod +x oc-cc-universal
sudo mv oc-cc-universal /usr/local/bin/

# Windows (PowerShell)
Invoke-WebRequest -Uri "https://github.com/samueltuyizere/oc-cc-universal/releases/latest/download/oc-cc-universal_windows-amd64.exe" -OutFile "oc-cc-universal.exe"
Move-Item -Path "oc-cc-universal.exe" -Destination "$env:LOCALAPPDATA\Microsoft\WindowsApps\oc-cc-universal.exe"
```

## Requirements

- An [OpenCode Go](https://opencode.ai/auth) subscription and API key
- Go 1.21+ (only needed if building from source)
