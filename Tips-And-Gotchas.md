# Tips & Gotchas

Common mistakes, limits, and things to know before you ship colored output.

---

## ANSI Does Not Work on Mobile

ANSI escape codes inside `ansi` blocks only render on **Discord Desktop and Web**. On mobile, the user sees a plain uncolored code block. There is no workaround for this — it is a Discord client limitation.

Use syntax blocks (`diff`, `css`, `ini`, `yaml`, `fix`) if you need color on all platforms.

---

## Always Reset After Colored Text

If you open a color without closing it, the color bleeds into everything that follows:

```js
const ESC = '\u001b';

// Wrong — bleeds into the next line
ESC + '[0;31m Red text'

// Right — always close with reset
ESC + '[0;31m Red text ' + ESC + '[0m'
```

---

## Triple Backticks Inside Code Blocks Will Break the Block

If you embed triple backticks (` ``` `) inside the content of a code block, it closes the block early. This affects both regular messages and Components V2 `TextDisplayBuilder`.

```js
// Wrong — the ``` inside breaks the outer block
'```diff\n+ green (in ```diff block)\n```'

// Right — describe without backticks
'```diff\n+ green (in a diff block)\n```'
```

---

## Background + Foreground Must Be Chained, Not Merged

Discord's ANSI parser treats `[31;41m` as foreground only. To combine foreground and background you need two separate escape sequences:

```js
const ESC = '\u001b';

// Wrong — only foreground applies
ESC + '[31;41m text ' + ESC + '[0m'

// Right — chain two sequences
ESC + '[0;31m' + ESC + '[41m text ' + ESC + '[0m'
```

---

## 256-Color and True-Color Are Ignored

Discord only supports the 8-color ANSI palette (codes 30–37 fg, 40–47 bg). Extended codes are silently ignored:

```
\u001b[38;5;200m   ← 256-color — NOT supported
\u001b[38;2;255;0;0m ← true-color — NOT supported
\u001b[0;31m        ← standard 8-color — supported
```

---

## cv2 — ANSI Works the Same Way Inside TextDisplayBuilder

ANSI color blocks behave identically inside a Components V2 `TextDisplayBuilder` as they do in a regular message. Same platform restriction applies — Desktop and Web only.

```js
new TextDisplayBuilder().setContent(
  '```ansi\n' + ESC + '[0;32m Success ' + ESC + '[0m\n```'
)
```

---

## Syntax Blocks Work Inside TextDisplayBuilder on All Platforms

`diff`, `css`, `ini`, `yaml`, `fix`, and `ml` blocks render correctly inside `TextDisplayBuilder` on Desktop, Web, and Mobile.

---

## \u001b Must Be the Actual ESC Character in Your Code

In JavaScript source code, `'\u001b'` and `'\x1b'` are both interpreted as the actual ESC byte (0x1B) at runtime. They are identical and both work correctly. Do not type the literal characters `\`, `u`, `0`, `0`, `1`, `b` as a string in a config file or external input — only in source code or template literals.

```js
const ESC = '\u001b'; // correct — interpreted as ESC byte
const ESC = '\x1b';   // correct — same thing

// Do NOT do this:
const ESC = '\\u001b'; // wrong — produces literal backslash-u-0-0-1-b
```

---

## Colors Reset at the End of the Block

You do not need to reset at the very last line of an `ansi` block — Discord resets automatically when the block closes. But resetting explicitly is still good practice for readability.

---

## Platform Compatibility Summary

| Method | Desktop | Web | Mobile |
|--------|---------|-----|--------|
| ANSI escape codes | YES | YES | NO |
| `diff` block | YES | YES | YES |
| `css` block | YES | YES | YES |
| `ini` block | YES | YES | YES |
| `yaml` block | YES | YES | YES |
| `fix` block | YES | YES | YES |
| `ml` block | YES | YES | YES |
