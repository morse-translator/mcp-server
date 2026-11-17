---
name: morse-code-translator
description: Translates text to International Morse code and Morse code to text, makes the Morse code sound as a WAV audio file, and shows the Morse code alphabet, with the Morse Code Translator MCP tools text_to_morse, morse_to_text, morse_audio and morse_alphabet. Use when a question needs Morse code - encoding text, decoding dots and dashes, playing or hearing Morse code, or looking up the code for a letter, digit or punctuation mark.
metadata:
  author: Morse Code Translator
  homepage: https://morsecodetranslator.com/
  documentation: https://morsecodetranslator.com/mcp/
  repository: https://github.com/morse-translator/mcp-server
---

# Morse Code Translator

The Morse Code Translator MCP tools translate between text and
International Morse code, and make the sound of the code. Use them
for each Morse result that the user will rely on. The tools are
read-only and need no account.

If the tools are not available, tell the user to connect the Morse
Code Translator MCP server. The server URL and the setup steps
for each client are at https://morsecodetranslator.com/mcp/.

## Before you call a tool

- **Alphabet:** International Morse only: letters A-Z, accented
  Latin letters, digits 0-9 and common punctuation. Do not use
  these tools for Japanese or Russian Morse, binary or the NATO
  alphabet. Point the user to
  https://morsecodetranslator.com/translators/ instead.
- **How to write Morse:** Use "." for a dot and "-" for a dash.
  Separate letters with one space and words with " / ".
- **Length:** `text_to_morse` accepts at most 1,000 characters.
  `morse_to_text` accepts at most 5,000. `morse_audio` accepts at
  most 200, and the sound can be at most 60 seconds long. Split
  longer input into parts and combine the results.
- **Sound:** `morse_audio` returns the code as text and the sound
  as an `audio/wav` content block (8 kHz, 8-bit, mono). The
  optional `wpm` argument sets the speed: a whole number from 5 to
  40 words per minute (default 20). The optional `frequency`
  argument sets the tone: a whole number from 300 to 1000 Hz
  (default 600). At 20 wpm, 60 seconds holds about 20 five-letter
  words.

## Choose the call

| Question | Call |
| --- | --- |
| Text to Morse code | `text_to_morse` with `text` |
| Morse code to text | `morse_to_text` with `morse` |
| The sound of a text in Morse code | `morse_audio` with `text`; add `wpm` or `frequency` only if the user asks for a speed or a tone |
| The Morse code chart, or the codes for letters, digits or punctuation | `morse_alphabet` (no arguments) |

`morse_audio` takes text, not dots and dashes. To play Morse code
that the user wrote with dots and dashes, decode it with
`morse_to_text` first, then send the text to `morse_audio`.

## Answer the user

1. Give the result first: the Morse code, the decoded text or the
   sound.
2. If the result lists characters that were left out, or Morse
   groups shown as `#`, include that in your answer.
3. Add the translator link from the result, so the user can open
   the same code on morsecodetranslator.com and play or download
   the sound. Some apps do not play audio content blocks, so give
   the link also after `morse_audio`.
4. Decoded text is returned in capital letters. Keep it that way
   unless the user asks you to change the case.
5. Do not copy the base64 audio data into your answer.

Each Morse code, decoded text and alphabet entry in your answer
comes from a tool result. Do not invent codes from memory when
the tools are available. For a longer text, make more than one
call and join the parts. Characters that have no Morse code are
left out of the code and listed in the result; they are not
guessed.

## When a call fails

- **`isError: true`**, or a message that an argument is missing,
  too long or out of range: Fix the arguments and try again. Split
  text over 1,000 characters or Morse code over 5,000 characters
  into parts. For `morse_audio`, split text over 200 characters,
  and use a higher `wpm` or a shorter text when the sound would be
  longer than 60 seconds.
- **HTTP 429:** The limit is 60 requests a minute per IP address
  (for IPv6, per /64 network). Wait the number of seconds in the
  `Retry-After` header, then try again. Tell the user if you had
  to wait.
- **This is not Morse code** or **none of these characters is in
  International Morse code:** Tell the user, and explain the
  writing rules or the characters that the tools support. Do not
  invent a translation.

## Not supported

Do not use these tools to translate Japanese Morse, Russian Morse,
binary or the NATO alphabet. Tell the user that this server does
not do these tasks, and point them to the other translators on
morsecodetranslator.com where that helps.
