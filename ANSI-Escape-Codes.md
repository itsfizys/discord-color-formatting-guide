# ANSI Escape Codes

ANSI escape codes are terminal color control sequences that Discord renders inside an `ansi` code block. They let you color individual words, lines, or characters with full foreground/background control.

> **Platform:** Desktop and Web only. Mobile shows a plain code block with no color.

---

## How It Works

Every ANSI sequence follows this format:

```
\u001b[CODEm
```

- `\u001b` — the ESC character (Unicode U+001B, same as `\x1b`)
- `[` — literal bracket
- `CODE` — one or more numbers separated by `;`
- `m` — ends the sequence

Always close colored text with a reset:

```
\u001b[0m
```

Wrap everything in an `ansi` code block:

````
```ansi
\u001b[0;31m This text is red \u001b[0m
```
````

---

## Foreground (Text) Colors — codes 30–37

Discord uses the **Solarized Dark** palette for ANSI colors — not standard terminal colors.

| Code | Color | Hex |
|------|-------|-----|
| 30 | Dark Gray | #4e5058 |
| 31 | Red | #dc322f |
| 32 | Green | #859900 |
| 33 | Yellow | #b58900 |
| 34 | Blurple | #268bd2 |
| 35 | Pink | #d33682 |
| 36 | Cyan | #2aa198 |
| 37 | White | #ffffff |

```js
const ESC = '\u001b';

const output = [
  '```ansi',
  ESC + '[0;30m  Dark Gray  ' + ESC + '[0m',
  ESC + '[0;31m  Red        ' + ESC + '[0m',
  ESC + '[0;32m  Green      ' + ESC + '[0m',
  ESC + '[0;33m  Yellow     ' + ESC + '[0m',
  ESC + '[0;34m  Blurple    ' + ESC + '[0m',
  ESC + '[0;35m  Pink       ' + ESC + '[0m',
  ESC + '[0;36m  Cyan       ' + ESC + '[0m',
  ESC + '[0;37m  White      ' + ESC + '[0m',
  '```',
].join('\n');
```

---

## Background Colors — codes 40–47

Background colors in Discord do **not** match standard terminal bg codes — they use the Solarized palette.

| Code | Color |
|------|-------|
| 40 | Firefly Dark Blue |
| 41 | Orange |
| 42 | Marble Blue |
| 43 | Greyish Turquoise |
| 44 | Gray |
| 45 | Indigo |
| 46 | Light Gray |
| 47 | White |

```js
const ESC = '\u001b';

const output = [
  '```ansi',
  ESC + '[40m  Firefly Dark Blue   ' + ESC + '[0m',
  ESC + '[41m  Orange              ' + ESC + '[0m',
  ESC + '[42m  Marble Blue         ' + ESC + '[0m',
  ESC + '[43m  Greyish Turquoise   ' + ESC + '[0m',
  ESC + '[44m  Gray                ' + ESC + '[0m',
  ESC + '[45m  Indigo              ' + ESC + '[0m',
  ESC + '[46m  Light Gray          ' + ESC + '[0m',
  ESC + '[47m  White               ' + ESC + '[0m',
  '```',
].join('\n');
```

---

## Style Codes

| Code | Effect |
|------|--------|
| 0 | Reset all (back to default) |
| 1 | Bold |
| 4 | Underline |

```js
const ESC = '\u001b';

const output = [
  '```ansi',
  ESC + '[0m   Normal text',
  ESC + '[1m   Bold text'        + ESC + '[0m',
  ESC + '[4m   Underline text'   + ESC + '[0m',
  ESC + '[1;4m Bold + Underline' + ESC + '[0m',
  '```',
].join('\n');
```

---

## Notes

- `256-color` (`\u001b[38;5;<n>m`) and `true-color` (`\u001b[38;2;<r>;<g>;<b>m`) are **not** supported — Discord ignores them
- In JavaScript, `'\u001b'` and `'\x1b'` are identical — both produce the ESC character
- Always end colored text with `\u001b[0m` or colors bleed into the next line
- ANSI does **not** render on Discord mobile
