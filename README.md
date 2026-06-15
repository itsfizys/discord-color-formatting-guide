# Discord Color Formatting — Complete Guide

> Guide by **itsfizys** • Support Server: [discord.gg/aerox](https://discord.gg/aerox)

Discord doesn't have a built-in color picker for text, but there are two solid ways to get colored output from your bot: **ANSI escape codes** inside an `ansi` code block, and **syntax-highlighted code blocks** using specific language tags.

---

## At a Glance

| Method | How it works | Platform |
|--------|-------------|----------|
| `ansi` block + escape codes | Terminal color sequences | Desktop & Web only |
| `diff` block | `+` green, `-` red | All platforms |
| `css` block | `[text]` = orange | All platforms |
| `ini` block | `[text]` = cyan | All platforms |
| `yaml` block | `key:` = pink | All platforms |
| `fix` block | full block orange | All platforms |
| `ml` block | `** text **` = orange | All platforms |

---

## Table of Contents

| # | Guide | Description |
|---|-------|-------------|
| 1 | [ANSI Escape Codes](ANSI-Escape-Codes.md) | Full foreground, background, and style reference |
| 2 | [Syntax Blocks](Syntax-Blocks.md) | diff, css, ini, yaml, fix, ml — no escape codes needed |
| 3 | [Combining Styles](Combining-Styles.md) | Layering colors, backgrounds, bold, and underline |
| 4 | [Real-World Examples](Real-World-Examples.md) | Ready-to-use patterns for bot commands |
| 5 | [Tips & Gotchas](Tips-And-Gotchas.md) | Platform limits, CV2 notes, common mistakes |

---

## Quick Cheat Sheet

```
What you want          How to get it
--------------------   --------------------------------------------------
Red text               \u001b[0;31m text \u001b[0m        (ansi block)
Green text             \u001b[0;32m text \u001b[0m        (ansi block)
Yellow text            \u001b[0;33m text \u001b[0m        (ansi block)
Blurple text           \u001b[0;34m text \u001b[0m        (ansi block)
Pink text              \u001b[0;35m text \u001b[0m        (ansi block)
Cyan text              \u001b[0;36m text \u001b[0m        (ansi block)
Bold                   \u001b[1m text \u001b[0m           (ansi block)
Underline              \u001b[4m text \u001b[0m           (ansi block)
Bold red               \u001b[1;31m text \u001b[0m        (ansi block)
Orange background      \u001b[41m text \u001b[0m          (ansi block)
Green line             + text                             (diff block)
Red line               - text                             (diff block)
Orange text            [text]                             (css block)
Cyan text              [text]                             (ini block)
Pink label             label: value                       (yaml block)
All orange             any text                           (fix block)
```
