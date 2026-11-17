# Morse Code Translator MCP

A free, hosted MCP server for International Morse code from [morsecodetranslator.com](https://morsecodetranslator.com). Connect it to your AI assistant, and the assistant can translate text to Morse code, decode Morse code, make the Morse code sound as a WAV file and show the Morse code alphabet, with the same rules as the translator on the site.

| | |
| --- | --- |
| **Server URL** | `https://morsecodetranslator.com/mcp/` |
| **Transport** | Streamable HTTP (JSON-RPC 2.0 over HTTP POST, no session) |
| **Sign-in** | None: no account, no API key |
| **Access** | Read-only |
| **Limit** | 60 requests a minute |
| **Version** | 1.1.0 |
| **Documentation** | [morsecodetranslator.com/mcp/](https://morsecodetranslator.com/mcp/) |

This repository holds the documentation, the [MCP Registry](https://registry.modelcontextprotocol.io) metadata (`server.json`), a [Gemini CLI](https://geminicli.com) extension (`gemini-extension.json`) and an [Agent Skill](https://agentskills.io) for the server. The server itself runs on morsecodetranslator.com; there is nothing to install or run.

## Tools

| Tool | Arguments | What it does |
| --- | --- | --- |
| `text_to_morse` | `text` (string, required): at most 1,000 characters | Translates text into International Morse code. Returns the code with "." and "-", one space between letters and " / " between words, lists any characters that have no Morse code, and gives a link to the translator with the code filled in. |
| `morse_to_text` | `morse` (string, required): at most 5,000 characters | Decodes International Morse code into text. Write dots as "." and dashes as "-"; separate letters with a space and words with " / ", a line break or three spaces. Returns the text in capital letters, lists any groups that are not Morse characters (shown as `#` in the text), and gives a translator link. |
| `morse_audio` | `text` (string, required): at most 200 characters<br>`wpm` (integer): speed, 5 to 40 words per minute, default 20<br>`frequency` (integer): tone, 300 to 1000 Hz, default 600 | Translates text into International Morse code and returns the sound as a WAV audio file (8 kHz, 8-bit, mono), with the code as text and a translator link. The sound can be at most 60 seconds long. |
| `morse_alphabet` | None | Returns the International Morse code chart: the letters A-Z, the digits 0-9 and punctuation, each with its code, and a link to the chart with sound on the site. |

All four tools are marked read-only, idempotent and closed-world (`readOnlyHint`, `idempotentHint`, `openWorldHint: false`).

International Morse code covers the letters A-Z, accented Latin letters, the digits 0-9 and common punctuation. Morse code is written with "." and "-", one space between letters and " / " between words.

Each translation ends with a link to the translator on morsecodetranslator.com with the code filled in. There you can play the code, change the speed and the tone, and download it. `morse_audio` also returns the sound itself, as a base64 `audio` content block (`audio/wav`) in the tool result. Your assistant app decides if it plays the file: some apps show only the text.

## Connect

Every client below uses the server URL `https://morsecodetranslator.com/mcp/`. The name `morse` is only a label: you can choose another one.

### Claude

On claude.ai and in the Claude apps (Free, Pro and Max plans):

1. Open **Customize**, then **Connectors**.
2. Select **+ Add**, then **Add custom connector**.
3. Enter **Morse Code Translator** as the name and `https://morsecodetranslator.com/mcp/` as the URL, then select **Continue**.
4. Review the detected settings, then select **Continue**.
5. Under **Authentication**, choose **No sign in**, then select **Add**.

To use it in a chat, select **+** at the lower left, then **Connectors**, and turn it on.

On Team and Enterprise plans, an owner adds the connector first, in **Organization settings** > **Connectors** (**Add** > **Custom** > **Web**). Members then select **Connect** on it in **Customize** > **Connectors**. See Anthropic's [custom connector instructions](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp).

### Claude Code

```bash
claude mcp add --transport http morse https://morsecodetranslator.com/mcp/
```

Add `--scope user` before the name to use the server in all your projects.

### ChatGPT

Where your plan and workspace allow custom MCP servers:

1. Open [chatgpt.com/plugins](https://chatgpt.com/plugins).
2. Select the plus button, then **Add custom MCP server**.
3. Enter **Morse Code Translator** as the name and `https://morsecodetranslator.com/mcp/` as the **Server URL**.
4. Choose **No authentication**.
5. Review the warning, select **I understand and want to continue**, then **Create as a plugin**.
6. Install the plugin from your personal plugins, then use it in a conversation.

See OpenAI's [developer mode guide](https://developers.openai.com/api/docs/guides/developer-mode).

### Cursor

Add this entry to `~/.cursor/mcp.json` for all your projects, or to `.cursor/mcp.json` in one project:

```json
{
  "mcpServers": {
    "morse": {
      "url": "https://morsecodetranslator.com/mcp/"
    }
  }
}
```

### VS Code (GitHub Copilot)

Add this entry to `.mcp.json` in the workspace root, or to `~/.copilot/mcp-config.json` for all your workspaces. You can also run **MCP: Add Server** from the Command Palette.

```json
{
  "mcpServers": {
    "morse": {
      "type": "http",
      "url": "https://morsecodetranslator.com/mcp/"
    }
  }
}
```

The older `.vscode/mcp.json` file uses a top-level `servers` object instead of `mcpServers`, with the same `type` and `url`. VS Code reads it only for the Local harness, which is deprecated. See the [VS Code MCP configuration reference](https://code.visualstudio.com/docs/agents/reference/mcp-configuration).

### Gemini CLI

Install this repository as an extension. It adds the server and the [Morse code skill](skills/morse-code-translator/SKILL.md):

```bash
gemini extensions install https://github.com/morse-translator/mcp-server
```

To add only the server:

```bash
gemini mcp add --transport http morse https://morsecodetranslator.com/mcp/
```

### Grok

1. Open [Grok Connectors](https://grok.com/connectors).
2. Select **New Connector**, then **Custom**.
3. Enter `https://morsecodetranslator.com/mcp/` as the MCP server URL. This server needs no authentication.
4. Complete the setup to make the tools available in conversations.

On Grok Business and Enterprise, a team admin must first add the connector for the organization. See Grok's [connector documentation](https://docs.x.ai/grok/connectors).

### Other MCP clients

Where custom remote MCP servers are available, use `https://morsecodetranslator.com/mcp/` with Streamable HTTP and no authentication. A client that can only start local servers can reach this one through `mcp-remote`, which needs Node.js:

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
- "Play “SOS” in Morse code at 15 words per minute."

## Call it without an assistant

The server speaks JSON-RPC 2.0 over HTTP POST and needs no session, so one request can call a tool:

```bash
curl -X POST https://morsecodetranslator.com/mcp/ \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"text_to_morse","arguments":{"text":"SOS"}}}'
```

The answer:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "content": [
      {
        "type": "text",
        "text": "Morse code:\n... --- ...\n\nOpen it in the Morse code translator to play the sound or download it: https://morsecodetranslator.com/?c=...%20---%20..."
      }
    ],
    "isError": false
  }
}
```

Use `tools/list` to get each tool's input schema. The server also answers `initialize` from clients that start a session, and the stateless 2026-07-28 protocol (`server/discover`). It does not open an event stream (an MCP client's GET gets HTTP 405) and does not accept JSON-RPC batches. Browser-based clients can call it from any origin, without credentials.

## Limits and errors

- Each IP address (for IPv6, each /64 network) can send 60 requests a minute. Over the limit, the server answers with HTTP 429 and a `Retry-After` header that says how many seconds to wait.
- `text_to_morse` takes up to 1,000 characters in one call, `morse_to_text` up to 5,000 and `morse_audio` up to 200, for a sound of at most 60 seconds. At the default 20 words per minute, 60 seconds holds about 20 five-letter words. Your assistant can split longer text into parts.
- A request body can be at most 64 KiB. A larger body gets HTTP 413.
- Characters that have no Morse code are left out of the code and listed in the result. Morse code groups that are not a character show as `#` in the text.
- When a tool cannot do the call (an argument is missing, too long or out of range, the sound would be longer than 60 seconds, or the input has no Morse character in it), the tool result has `isError: true` and a message that says what to change.

For Japanese and Russian Morse code, binary and the NATO alphabet, use the [other translators](https://morsecodetranslator.com/translators/) on the site.

## Privacy

The server needs no account and sets no cookie. It translates the text or Morse code that it gets and does not store it. Your assistant sends only the tool arguments, not your conversation. Like every request to the site, a request can appear in the server logs. See the [Privacy Policy](https://morsecodetranslator.com/privacy-policy/#mcp).

## Agent skill

[`skills/morse-code-translator/SKILL.md`](skills/morse-code-translator/SKILL.md) is an [Agent Skill](https://agentskills.io/specification) that tells an assistant when to use each tool, how to write Morse code, how to show the results and what to do when a call fails. The Gemini CLI extension loads it. In Claude Code, copy the `morse-code-translator` folder into `~/.claude/skills/` or into `.claude/skills/` in your project. The skill only helps when the server is connected.

## Repository contents

| File | Purpose |
| --- | --- |
| `server.json` | Server metadata for the [MCP Registry](https://registry.modelcontextprotocol.io) |
| `gemini-extension.json` | Gemini CLI extension manifest |
| `skills/morse-code-translator/SKILL.md` | Agent Skill for the four tools |
| `CHANGELOG.md` | Changes in each version |

## Support

For bug reports and questions, [open an issue](https://github.com/morse-translator/mcp-server/issues).

- [MCP documentation](https://morsecodetranslator.com/mcp/)
- [Privacy Policy](https://morsecodetranslator.com/privacy-policy/)
- [Terms of Use](https://morsecodetranslator.com/terms-of-use/)
