# Combining Styles

ANSI codes can be layered — combine foreground color, background color, bold, and underline in a single sequence.

> **Platform:** Desktop and Web only. Mobile shows plain text.

---

## Combining Style + Foreground Color

Separate codes with a semicolon inside the same sequence:

```
\u001b[STYLE;COLORm
```

```js
const ESC = '\u001b';

const output = [
  '```ansi',
  ESC + '[1;31m  Bold Red     ' + ESC + '[0m',
  ESC + '[1;32m  Bold Green   ' + ESC + '[0m',
  ESC + '[1;33m  Bold Yellow  ' + ESC + '[0m',
  ESC + '[1;34m  Bold Blurple ' + ESC + '[0m',
  ESC + '[1;35m  Bold Pink    ' + ESC + '[0m',
  ESC + '[1;36m  Bold Cyan    ' + ESC + '[0m',
  ESC + '[1;37m  Bold White   ' + ESC + '[0m',
  ESC + '[4;31m  Underline Red   ' + ESC + '[0m',
  ESC + '[4;36m  Underline Cyan  ' + ESC + '[0m',
  ESC + '[1;4m   Bold + Underline (no color)' + ESC + '[0m',
  '```',
].join('\n');
```

---

## Adding a Background Color

Chain two separate sequences back to back — one for foreground, one for background:

```
\u001b[FG_CODEm\u001b[BG_CODEm
```

```js
const ESC = '\u001b';

const output = [
  '```ansi',
  ESC + '[1;31m' + ESC + '[40m  Bold Red on Dark Blue     ' + ESC + '[0m',
  ESC + '[1;33m' + ESC + '[44m  Bold Yellow on Gray        ' + ESC + '[0m',
  ESC + '[1;37m' + ESC + '[41m  Bold White on Orange       ' + ESC + '[0m',
  ESC + '[1;32m' + ESC + '[47m  Bold Green on White        ' + ESC + '[0m',
  ESC + '[1;36m' + ESC + '[45m  Bold Cyan on Indigo        ' + ESC + '[0m',
  ESC + '[1;34m' + ESC + '[43m  Bold Blurple on Teal       ' + ESC + '[0m',
  '```',
].join('\n');
```

---

## Mixing Colors Within a Single Line

Reset mid-line with `\u001b[0m` and open a new color sequence:

```js
const ESC = '\u001b';

// "PASS" in green, then "FAIL" in red, on the same line
const line =
  ESC + '[0;32m PASS ' + ESC + '[0m' +
  ' — ' +
  ESC + '[0;31m FAIL ' + ESC + '[0m';

const output = '```ansi\n' + line + '\n```';
```

---

## Full Reference Grid

```js
const ESC = '\u001b';
const fgCodes = [31, 32, 33, 34, 35, 36, 37];
const bgCodes = [40, 41, 42, 43, 44, 45, 46, 47];

// Build a foreground × background grid
const lines = bgCodes.map(bg =>
  fgCodes.map(fg =>
    ESC + `[1;${fg}m` + ESC + `[${bg}m` + ` ${fg}/${bg} ` + ESC + '[0m'
  ).join('')
);

const output = ['```ansi', ...lines, '```'].join('\n');
```

---

## Code Reference

| Code(s) | Effect |
|---------|--------|
| `0` | Reset all |
| `1` | Bold |
| `4` | Underline |
| `1;4` | Bold + Underline |
| `0;31` | Normal Red |
| `1;31` | Bold Red |
| `4;31` | Underline Red |
| `1;4;31` | Bold + Underline + Red |
| `0;32` + `\u001b[41m` | Green text on Orange bg |

---

## Notes

- Order of codes in a single sequence doesn't matter — `[1;31m` and `[31;1m` both produce bold red
- Codes persist until reset — always end with `\u001b[0m`
- Background and foreground must be separate sequences — `[31;41m` only sets foreground in Discord's parser
