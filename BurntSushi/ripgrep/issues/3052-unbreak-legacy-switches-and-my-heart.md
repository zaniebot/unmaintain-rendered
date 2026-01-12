```yaml
number: 3052
title: Unbreak legacy switches (and my heart)
type: issue
state: closed
author: forthrin
labels:
  - wontfix
assignees: []
created_at: 2025-05-19T09:40:25Z
updated_at: 2025-07-01T00:45:42Z
url: https://github.com/BurntSushi/ripgrep/issues/3052
synced_at: 2026-01-12T16:13:25Z
```

# Unbreak legacy switches (and my heart)

---

_@forthrin_

When comparing the command line switches of `grep` and `rg`, two breaks with legacy stand out in particular, ie. `-h` and `-L`. These are used all the time, and it's a pain to type them out in full.

1. Question: What were rationales for breaking compatibility with one most fundamental UNIX tool in the first place?
1. Proposal: Look into ways to make them comply with legacy `grep` switches.

Preliminary suggestion:

1. Make `rg` display short help
2. Make `rg -h` do as `grep -h`

`-L` may require some more consideration, since `grep` has at least three related switches for links (`-O`, `-p` and `-S`).

PS! There may be more breaking than emphasized in the table, which only highlights on explicit changes in the long switches.

| gr | rg | grep | ripgrep
|-|-|-|-
|    | -. |                       | --hidden                 |
|    | -0 |                       | --null                   |
| -a | -a | --text                | --text                   |
| -b | -b | --byte-offset         | --byte-offset            |
| -c | -c | --count               | --count                  |
| -E |    | --extended-regexp     |                          |
| -F | -F | --fixed-strings       | --fixed-strings          |
| -G |    | --basic-regexp        |                          |
| -H | -H |                       | --with-filename          |
| -h | -h | --no-filename         | **--help**               |
| -i | -i | --ignore-case         | --ignore-case            |
| -I | -I |                       | --no-filename            |
| -J |    | --bz2decompress       |                          |
| -L | -L | --files-without-match | **--follow**             |
| -l | -l | --files-with-matches  | --files-with-matches     |
| -M |    | --lzma                |                          |
| -n | -n | --line-number         | --line-number            |
|    | -N |                       | --no-line-number         |
| -o | -o | --only-matching       | --only-matching          |
| -O |    |                       |                          |
| -p | -p |                       | --pretty                 |
|    | -P |                       | --pcre2                  |
| -q | -q | --silent              | **--quiet**              |
| -r |    | --recursive           |                          |
| -s | -s | --no-messages         | **--case-sensitive**     |
| -S | -S |                       | --smart-case             |
| -u | -u |                       | --unrestricted           |
| -U | -U | --binary              | **--multiline**          |
| -V | -V | --version             | --version                |
| -v | -v | --invert-match        | --invert-match           |
| -w | -w | --word-regexp         | --word-regexp            |
| -X |    | --xz                  |                          |
| -x | -x | --line-regexp         | --line-regexp            |
| -y |    |                       |                          |
| -z | -z | --null-data           | **--search-zip**         |
| -Z |    | --decompress          |                          |



---

_Comment by @BurntSushi on 2025-05-19 13:03_

Somewhat related: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever

ripgrep has no promised compatibility with grep. And _not_ breaking existing users of ripgrep is way more important to me than trying to match grep compatibility, particularly at this stage.

> Question: What were rationales for breaking compatibility with one most fundamental UNIX tool in the first place?

I didn't "break compatibility." ripgrep is a new tool that is intentionally and specifically not POSIX grep. Compatibility is not really a factor so much as familiarity. That is, I did try to make a number of options in ripgrep behave the same or roughly the same as they do in GNU grep.

More specifically, for `-h`, you really should flip the question around. Not why does ripgrep break compatibility with one of the most "fundamental UNIX tools," but rather, why does grep break compatibility with one of the most foundational Unix conventions ever?

Righteous indignation cuts both ways.

As for `-L`, that's because ripgrep is recursive by default. There is no non-recursive vs recursive mode. (Although you can achieve non-recursive mode with `--max-depth`.)

> since grep has at least three related switches for links (`-O`, `-p` and `-S`).

None of `-O`, `-p` or `-S` appear in `man grep` on my system (GNU grep) or in the [POSIX grep specification](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/grep.html). So I don't know what you're talking about here.

> AI assisted comparison based on respective man pages (not proof-read).

Your tables are bunk.

---

_Closed by @BurntSushi on 2025-05-19 13:03_

---

_Label `wontfix` added by @BurntSushi on 2025-05-19 13:03_

---

_Comment by @forthrin on 2025-05-19 14:50_

Your unfriendly response just ruined the joy of using this software.

```
$ brew uninstall rg
Uninstalling /opt/homebrew/Cellar/ripgrep
$
```

---

_Comment by @zenspider on 2025-07-01 00:45_

FWIW... I banned the guy for similar behavior.

---
