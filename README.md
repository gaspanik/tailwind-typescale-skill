# tailwind-typescale — Type Scale Skill for Tailwind CSS v4

A Claude Code skill that applies a harmonious typography scale to any Tailwind CSS v4 project. Picks a base font size and a named ratio, calculates all 13 steps, writes them into `@theme`, and maps `h1`–`h6` in `@layer base` — all in one command.

---

## What this is

Typography scales get messy fast — arbitrary `text-[17px]` values, heading sizes set inline, no consistent rhythm. This skill replaces all of that with a single mathematically consistent scale derived from typescale.com ratios.

```
Project  →  tailwind-typescale  →  @theme with --text-xs … --text-9xl
                                →  @layer base with h1–h6 font-size mapping
                                →  explicit text-* removed from heading elements
                                →  DESIGN.md typography section synced (if present)
```

---

## Supported scales

| Scale name | Ratio | Character |
|---|---|---|
| **Perfect Fourth** | 1.333 | Balanced and readable — recommended for most projects |
| **Major Third** | 1.250 | Gentle, compact |
| **Golden Ratio** | 1.618 | Dynamic contrast |
| **Minor Third** | 1.200 | Subtle, refined |

Base font sizes: **14px / 16px / 18px / 20px** (or any custom value).

---

## How it works

1. **Pre-flight** — Locates the Tailwind CSS entry file (any `.css` file containing `@import "tailwindcss"`), reads `DESIGN.md` if present, detects existing `--text-*` variables, and warns if a `tailwind.config.js` (v3) is found.
2. **Parameters** — Asks for base size and scale ratio via `AskUserQuestion` (or reads from arguments).
3. **Preview** — Shows a table of all 13 steps with rem and px values before applying anything.
4. **Apply** — Writes `--text-xs` through `--text-9xl` into `@theme`, then maps `h1`–`h6` in `@layer base`.
5. **Clean up** — Scans HTML/JSX/Vue files for heading elements with explicit `text-*` size classes that would override the new scale, and offers to remove them. Also syncs the `DESIGN.md` typography section if the file is present.
6. **Done** — Reports what changed and reminds you of `leading-*` utilities for line-height control.

---

## Repo structure

```
skills/
  tailwind-typescale/
    SKILL.md          — skill definition loaded by Claude Code
```

---

> **⚠️ Backup before applying**
>
> The skill overwrites your CSS entry file directly. Always make sure you can revert before running.
>
> - If you use Git: run `git status` to check for uncommitted changes, then `git stash` or `git commit` before proceeding.
> - If you don't use Git: make a manual backup of your CSS entry file.
>
> ---
>
> **⚠️ 適用前にバックアップを**
>
> このスキルは CSS エントリファイルを直接書き換えます。実行前に必ず元に戻せる状態にしておいてください。
>
> - Git を使っている場合は `git status` で未コミットの変更を確認し、`git stash` または `git commit` で保存してから実行してください。
> - Git を使っていない場合は、CSS エントリファイルを手動でバックアップしておいてください。

---

## Getting started

**1. Clone this repo**

```bash
git clone https://github.com/gaspanik/tailwind-typescale-skill
```

**2. Install the skill into Claude Code**

```bash
cp -r skills/tailwind-typescale ~/.claude/skills/
```

**3. Run the skill**

Invoke with a slash command or describe what you want — both work:

```
/tailwind-typescale
```

```
タイプスケールを整えたい
```

```
/tailwind-typescale 16px perfect-fourth
```

Arguments are optional. Without them the skill will ask interactively.

---

## When the skill is triggered automatically

The skill description instructs Claude to use it whenever:

- Asked to set up or change a type scale (`"タイプスケール変えたいんだけど"`, `"apply a type scale"`)
- Asked to tidy up font sizes (`"フォントサイズを整えたい"`, `"set up typography scale"`)
- Asked to fix heading sizes (`"見出しのサイズをちゃんとしたい"`)

---

## Requirements

- **Tailwind CSS v4** — uses `@theme` directive and `--text-*` CSS variables.
  For v3 projects, configure font sizes under `theme.fontSize` in `tailwind.config.js`.
- A CSS entry file containing `@import "tailwindcss"` somewhere in the project (any path).

---

## Line-height

The skill sets font sizes only — line-height is intentionally left to Tailwind's `leading-*` utilities so you can tune it per element.

| Step | Suggested utility |
|---|---|
| `text-xs` / `text-sm` | `leading-relaxed` |
| `text-base` / `text-lg` | `leading-normal` |
| `text-xl` / `text-2xl` | `leading-snug` |
| `text-3xl` | `leading-snug` |
| `text-4xl` / `text-5xl` | `leading-tight` |
| `text-6xl` and above | `leading-none` |

---

Built by Masaaki Komori - [@cipher](https://x.com/cipher) · Skill for [Claude Code](https://claude.ai/code)
