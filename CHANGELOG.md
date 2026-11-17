# Changelog

All notable changes to the Morse Code Translator MCP server and to this repository. The version is the server's version, as it reports in `serverInfo` and as `server.json` gives it.

## 1.1.0 - 2026-11-18

### Added

- `morse_audio` tool: translates text (at most 200 characters) into International Morse code and returns the sound as a WAV audio file (8 kHz, 8-bit, mono), with the code as text and a translator link. Optional `wpm` (5 to 40, default 20) and `frequency` (300 to 1000 Hz, default 600). The sound can be at most 60 seconds long.
- Gemini CLI and VS Code (GitHub Copilot) connection steps, a raw JSON-RPC example and a list of the repository files in the README.

### Changed

- The Agent Skill covers `morse_audio`.
- The Claude and ChatGPT connection steps follow the current apps.

## 1.0.0 - 2026-11-17

### Added

- The hosted MCP server at morsecodetranslator.com with the tools `text_to_morse`, `morse_to_text` and `morse_alphabet`.
- `server.json` for the MCP Registry, a Gemini CLI extension and the `morse-code-translator` Agent Skill.
