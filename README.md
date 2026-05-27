# claudepass

A universal Go proxy that lets you use **any OpenAI-compatible backend** with [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

`claudepass` sits between Claude Code and your backend, intercepting Anthropic API requests, transforming them to OpenAI format, and forwarding them upstream. Claude Code thinks it's talking to Anthropic — but your requests go to whatever backend you choose.

Works with: **OpenAI-compatible**, **Ollama**, **OpenRouter**, **vLLM**, **LiteLLM**, **Azure OpenAI**, **GCP Vertex AI**, or any `/v1/chat/completions` endpoint.

## Why?

Claude Code only speaks Anthropic's API format. This proxy translates everything — requests, responses, streaming, tool calls — so you can use any OpenAI-compatible model behind Claude Code's interface. No patches, no forks, just set a few environment variables and go.

## Features

- **Any OpenAI-compatible Backend** — Point at any `/v1/chat/completions` endpoint
- **Transparent Proxy** — Claude Code sends Anthropic-format requests, proxy transforms to OpenAI format and back
- **Model Routing** — Automatically routes to different models based on context (default, thinking, long context, background)
- **Fallback Chains** — If a model fails, automatically tries the next one in your configured chain
- **Circuit Breaker** — Tracks model health and skips failing models to avoid latency spikes
- **Real-time Streaming** — Full SSE streaming with live OpenAI → Anthropic format transformation
- **Tool Calling** — Proper Anthropic tool_use/tool_result ↔ OpenAI function calling translation
- **Thinking/Reasoning** — Handles thinking fields, reasoning_effort, and provider-specific quirks (DeepSeek, o1/o3)
- **Cache Token Math** — Correct Anthropic-spec cache token subtraction from input_tokens
- **Token Counting** — Uses tiktoken (cl100k_base) for accurate token counting and context threshold detection
- **JSON Configuration** — Flexible config file with environment variable overrides and `${VAR}` interpolation
- **Hot Reload** — Watch config file for changes and reload automatically (off by default)
- **Background Mode** — Run as daemon detached from terminal
- **Auto-start on Login** — Launch on system startup via launchd (macOS)

## Quick Start

### 1. Build from Source

```bash
git clone https://github.com/adhney/claudepass.git
cd claudepass
go build -o claudepass ./cmd/claudepass/
sudo mv claudepass /usr/local/bin/
```

### 2. Initialize Configuration

```bash
claudepass init
```

Creates a default config at `~/.config/claudepass/config.json`. Edit it to set your backend URL and API key:

```bash
export CLAUDEPASS_API_KEY=your-api-key
export CLAUDEPASS_BASE_URL=https://your-backend.example.com/v1/chat/completions
```

### 3. Start the Proxy

```bash
claudepass serve
```

### 4. Configure Claude Code

```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:3456
export ANTHROPIC_AUTH_TOKEN=unused
```

### 5. Run Claude Code

```bash
claude
```

## Environment Variables

| Variable | Description | Default |
| -------- | ----------- | ------- |
| `CLAUDEPASS_API_KEY` | API key for your backend (**required**) | — |
| `CLAUDEPASS_BASE_URL` | Full URL to your `/v1/chat/completions` endpoint | `https://api.openai.com/v1/chat/completions` |
| `CLAUDEPASS_HOST` | Proxy listen host | `127.0.0.1` |
| `CLAUDEPASS_PORT` | Proxy listen port | `3456` |
| `CLAUDEPASS_CONFIG` | Custom config file path | `~/.config/claudepass/config.json` |
| `CLAUDEPASS_LOG_LEVEL` | Log level: `debug`, `info`, `warn`, `error` | `info` |

## CLI Commands

```
claudepass serve              Start the proxy server
claudepass serve -b           Start in background (detached from terminal)
claudepass serve --port 8080  Start on a custom port
claudepass stop               Stop the running proxy server
claudepass status             Check if the proxy is running
claudepass init               Create default configuration file
claudepass validate           Validate configuration file
claudepass models             List configured model scenarios
claudepass autostart enable   Enable auto-start on login
claudepass autostart disable  Disable auto-start on login
claudepass autostart status   Check autostart status
claudepass --version          Show version
```

## Example: GCP Vertex AI

```bash
export CLAUDEPASS_API_KEY=your-vertex-key
export CLAUDEPASS_BASE_URL=https://your-project.endpoints.your-region.cloud.goog/v1/chat/completions
export ANTHROPIC_BASE_URL=http://127.0.0.1:3456
export ANTHROPIC_AUTH_TOKEN=unused
claudepass serve
claude
```

## Example: Ollama (Local)

```bash
export CLAUDEPASS_API_KEY=unused
export CLAUDEPASS_BASE_URL=http://localhost:11434/v1/chat/completions
export ANTHROPIC_BASE_URL=http://127.0.0.1:3456
export ANTHROPIC_AUTH_TOKEN=unused
claudepass serve
claude
```

## Documentation

| Document | Description |
| -------- | ----------- |
| [CONFIGURATION.md](CONFIGURATION.md) | Config file reference, env vars, model routing, fallback chains |
| [MODELS.md](MODELS.md) | Model capabilities and routing recommendations |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Development setup, architecture, how it works |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Common issues and debug mode |

## Credits

Forked from [samueltuyizere/claudepass](https://github.com/samueltuyizere/claudepass) and generalized to work with any OpenAI-compatible backend.

## License

[AGPL-3.0](LICENSE)
