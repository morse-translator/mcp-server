---
name: morse-code-translator
description: Use when a question needs Morse code - translating text to Morse, decoding dots and dashes to text, or showing the International Morse code alphabet. Applies to the Morse Code Translator MCP tools text_to_morse, morse_to_text and morse_alphabet.
metadata:
  author: Morse Code Translator
  homepage: https://morsecodetranslator.com/
  documentation: https://morsecodetranslator.com/mcp/
  repository: https://github.com/morse-translator/mcp-server
---

# Morse Code Translator

The Morse Code Translator MCP tools translate between text and
International Morse code. Use them for each Morse result that the
user will rely on. The tools are read-only and need no account.

If the tools are not available, tell the user to connect the Morse
Code Translator MCP server. The setup steps for each client are at
https://morsecodetranslator.com/mcp/.

## Before you call a tool

- **Alphabet:** International Morse only: letters A-Z, accented
  Latin letters, digits 0-9 and common punctuation. Do not use
  these tools for Japanese or Russian Morse, binary or the NATO
  alphabet. Point the user to
  https://morsecodetranslator.com/translators/ instead.
- **How to write Morse:** Use "." for a dot and "-" for a dash.
  Separate letters with one space and words with " / ".
- **Length:** `text_to_morse` accepts at most 1,000 characters.
  `morse_to_text` accepts at most 5,000. Split longer input into
  parts and combine the results.
- **Sound:** The tools return text only. They cannot play or
  generate audio. Always include the translator link from the
  result, so the user can hear the code or download it.

## Choose the call

| Question | Call |
| --- | --- |
| Text to Morse code | `text_to_morse` with `text` |
| Morse code to text | `morse_to_text` with `morse` |
| The Morse code chart, or the codes for letters, digits or punctuation | `morse_alphabet` (no arguments) |

## Answer the user

1. Give the result first: the Morse code, or the decoded text.
2. If the result lists characters that were left out, or Morse
   groups shown as `#`, include that in your answer.
3. Add the translator link from the result, so the user can open
   the same code on morsecodetranslator.com and play or download
   the sound.
4. Decoded text is returned in capital letters. Keep it that way
   unless the user asks you to change the case.

Each Morse code, decoded text and alphabet entry in your answer
comes from a tool result. Do not invent codes from memory when
the tools are available. For a longer text, make more than one
call and join the parts. Characters that have no Morse code are
left out of the code and listed in the result; they are not
guessed.

## When a call fails

- **`isError: true`**, or a message that an argument is missing
  or too long: Fix the arguments and try again. Split text over
  1,000 characters or Morse code over 5,000 characters into
  parts.
- **HTTP 429:** The limit is 60 requests a minute per IP address.
  Wait the number of seconds in the `Retry-After` header, then
  try again. Tell the user if you had to wait.
- **This is not Morse code** or **none of these characters is in
  International Morse code:** Tell the user, and explain the
  writing rules or the characters that the tools support. Do not
  invent a translation.

## Not supported

Do not use these tools to play sound, to create an audio file, or
to translate Japanese Morse, Russian Morse, binary or the NATO
alphabet. Tell the user that this server does not do these tasks,
and point them to the translator or the other translators on
morsecodetranslator.com where that helps.
