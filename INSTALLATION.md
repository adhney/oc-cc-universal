# Installation

## Homebrew (macOS & Linux)

```bash
brew tap samueltuyizere/tap
brew install claudepass
```

## Scoop (Windows)

```powershell
scoop bucket add claudepass https://github.com/samueltuyizere/scoop-bucket
scoop install claudepass
```

## Build from Source

```bash
git clone https://github.com/samueltuyizere/claudepass.git
cd claudepass
make build

# Binary is at bin/claudepass
# Optionally install to $GOPATH/bin
make install
```

## Download a Release Binary

Download the latest release for your platform from the [Releases page](https://github.com/samueltuyizere/claudepass/releases):

| Platform              | File                         |
| --------------------- | ---------------------------- |
| macOS (Apple Silicon) | `claudepass_darwin-arm64`      |
| macOS (Intel)         | `claudepass_darwin-amd64`      |
| Linux (x86_64)        | `claudepass_linux-amd64`       |
| Linux (ARM64)         | `claudepass_linux-arm64`       |
| Windows (x86_64)      | `claudepass_windows-amd64.exe` |
| Windows (ARM64)       | `claudepass_windows-arm64.exe` |

```bash
# macOS Apple Silicon
curl -L -o claudepass https://github.com/samueltuyizere/claudepass/releases/latest/download/claudepass_darwin-arm64
chmod +x claudepass
sudo mv claudepass /usr/local/bin/

# Windows (PowerShell)
Invoke-WebRequest -Uri "https://github.com/samueltuyizere/claudepass/releases/latest/download/claudepass_windows-amd64.exe" -OutFile "claudepass.exe"
Move-Item -Path "claudepass.exe" -Destination "$env:LOCALAPPDATA\Microsoft\WindowsApps\claudepass.exe"
```

## Requirements

- An [OpenCode Go](https://opencode.ai/auth) subscription and API key
- Go 1.21+ (only needed if building from source)
