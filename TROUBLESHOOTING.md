# Troubleshooting

## Windows Scoop Background Mode

On Windows, `claudepass serve -b` uses the native Windows process APIs and keeps
the Scoop shim path intact. This means background mode does not require `nohup`
or a Unix-like shell, and Scoop-provided environment variables continue to work.

## "invalid request body" Error

This means the proxy couldn't parse the request from Claude Code. Enable debug logging to see the raw request:

```json
{ "logging": { "level": "debug" } }
```

Or set the environment variable:

```bash
export CLAUDEPASS_LOG_LEVEL=debug
```

## "all models failed" Error

All models in the fallback chain returned errors. Check:

1. Your API key is valid: `claudepass validate`
2. You haven't exceeded your [usage limits](https://platform.openai.com/)
3. The OpenAI-compatible service is reachable: `curl -H "Authorization: Bearer $CLAUDEPASS_API_KEY" https://api.openai.com/zen/go/v1/models`

## Connection Refused

Make sure the proxy is running:

```bash
claudepass status
```

And Claude Code is pointing to the right address:

```bash
echo $ANTHROPIC_BASE_URL  # Should be http://127.0.0.1:3456
```

## Streaming Not Working

The proxy transforms OpenAI SSE to Anthropic SSE in real-time. If streaming appears broken:

1. Set log level to `debug` to see the raw SSE chunks
2. Check that no proxy or firewall is buffering the connection
3. Try a non-streaming request first to verify the model works

## Debug Mode

For maximum logging, run with debug level:

```bash
CLAUDEPASS_LOG_LEVEL=debug claudepass serve
```

This logs:

- Raw Anthropic request body from Claude Code
- Transformed OpenAI request sent to OpenAI-compatible
- Raw OpenAI response received
- SSE stream events during streaming
