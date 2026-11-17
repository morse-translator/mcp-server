# Morse Code Translator MCP

An MCP server for International Morse code, using the same translator on [morsecodetranslator.com](https://morsecodetranslator.com).

This repository contains documentation and registration metadata for the hosted service.

- **Endpoint:** `https://morsecodetranslator.com/mcp/`
- **Transport:** Streamable HTTP
- **Authentication:** None
- **Documentation:** [morsecodetranslator.com/mcp/](https://morsecodetranslator.com/mcp/)

## Tools

| Tool | What it does |
| --- | --- |
| `text_to_morse` | Translates text into International Morse code. The required `text` argument can be at most 1,000 characters. Returns the code with "." and "-", one space between letters and " / " between words, lists any characters that have no Morse code, and gives a link to the translator with the code filled in. |
| `morse_to_text` | Decodes International Morse code into text. The required `morse` argument can be at most 5,000 characters. Write dots as "." and dashes as "-"; separate letters with a space and words with " / ", a line break or three spaces. Returns the text in capital letters, lists any groups that are not Morse characters (shown as # in the text), and gives a translator link. |
| `morse_alphabet` | Returns the International Morse code chart: the letters A-Z, the digits 0-9 and punctuation, each with its code. This tool takes no input. |

International Morse code covers the letters A-Z, accented Latin letters, the digits 0-9 and common punctuation. Morse code is written with "." and "-", one space between letters and " / " between words.

The tools are read-only and need no account or API key. Each translation ends with a link to the translator on morsecodetranslator.com, where you can play the code as sound or download it. The tools themselves answer with text only.

## Connect

### Claude

1. Open **Customize**, then **Connectors**.
2. Select **+ Add**, then **Add custom connector**.
3. Enter **Morse Code Translator** as the name and `https://morsecodetranslator.com/mcp/` as the URL, then select **Continue**.
4. Choose **No sign in**, then select **Add**.

On Team and Enterprise plans, an organization owner adds the connector first; members then connect it. See Anthropic's [custom connector instructions](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).

### Claude Code

```bash
claude mcp add --transport http morse https://morsecodetranslator.com/mcp/
```

Add `--scope user` before the name to use the server in all your projects.

### Cursor

Add this entry to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "morse": {
      "url": "https://morsecodetranslator.com/mcp/"
    }
  }
}
```

### VS Code

Add this entry to `.vscode/mcp.json`:

```json
{
  "servers": {
    "morse": {
      "type": "http",
      "url": "https://morsecodetranslator.com/mcp/"
    }
  }
}
```

If the configuration file already contains other servers, add the entry to the existing object.

### ChatGPT

Where your account and workspace allow custom MCP connections:

1. Open Settings, then **Apps & Connectors**, and turn on **Developer mode** under Advanced settings.
2. Create a connector with the name **Morse Code Translator** and the server URL `https://morsecodetranslator.com/mcp/`.
3. Choose no authentication, then create it.

### Grok

1. Open [Grok Connectors](https://grok.com/connectors).
2. Select **New Connector**, then **Custom**.
3. Enter `https://morsecodetranslator.com/mcp/` as the MCP server URL. No authentication is required by this server.
4. Complete the setup to make the tools available in conversations.

For Grok Business and Enterprise, a team admin must first add the connector for the organization.

See Grok's [connector documentation](https://docs.x.ai/grok/connectors).

### Other MCP clients

Where custom remote MCP connections are available, use `https://morsecodetranslator.com/mcp/` with Streamable HTTP and no authentication. See the [connection instructions](https://morsecodetranslator.com/mcp/). A client that can only start local servers can reach this one through `mcp-remote`, which needs Node.js:

```json
{
  "mcpServers": {
    "morse": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://morsecodetranslator.com/mcp/"]
    }
  }
}
```

## Example prompts

- "Translate “Happy birthday” into Morse code."
- "What does .. / .-.. --- ...- . / -.-- --- ..- mean?"
- "Show me the Morse code for the digits 0 to 9."
- "Write my name in Morse code and give me a link to hear it."

## Limits

Each IP address (for IPv6, each /64 network) can send 60 requests a minute. Over the limit, the server answers with HTTP 429 and a `Retry-After` header that says how many seconds to wait.

`text_to_morse` takes up to 1,000 characters in one call and `morse_to_text` up to 5,000. Your assistant can split longer text into parts.

Characters that have no Morse code are left out of the code and listed in the result. Morse code groups that are not a character show as `#` in the text. When an argument is missing or too long, the tool result has `isError: true` and a message that says what to change.

The server does not play sound. Use the translator link in the result to hear the code or download it. For Japanese and Russian Morse code, binary and the NATO alphabet, use the [other translators](https://morsecodetranslator.com/translators/) on the site.

The server needs no account and does not store the text or Morse code it translates. See the [Privacy Policy](https://morsecodetranslator.com/privacy-policy/#mcp).

## Support

For bug reports and questions, [open an issue](https://github.com/morse-translator/mcp-server/issues).

- [MCP Documentation](https://morsecodetranslator.com/mcp/)
- [Privacy Policy](https://morsecodetranslator.com/privacy-policy/)
- [Terms of Use](https://morsecodetranslator.com/terms-of-use/)
