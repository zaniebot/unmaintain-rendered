```yaml
number: 1893
title: Categorize options in --help and man page
type: issue
state: closed
author: dfabulich
labels:
  - duplicate
assignees: []
created_at: 2021-06-14T19:13:59Z
updated_at: 2021-06-14T19:26:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1893
synced_at: 2026-01-12T16:13:24Z
```

# Categorize options in --help and man page

---

_@dfabulich_

When you run `rg --help` or `man rg` it presents a giant list of 100 options in alphabetical order. Related options like `--heading` and `--no-heading` do not appear together.

By comparison, `man grep` (for GNU grep) presents its options in categories: https://linux.die.net/man/1/grep

* Generic Program Information (`--help`, `--version`)
* Matcher Selection (`--extended--regexp`, `--perl-regexp`)
* Matching Control (`--ignore-case`, `--invert-match`)
* General Output Control (`--files-with-matches`, `--count`, `--silent`)
* Output Line Prefix Control (`--line-number`, `--no-filename`)
* Context Line Control (`--after-context`, `--before-context`)
* File and Directory Selection (`--exclude`, `--include`)

Skimming through the options for ripgrep, I think a similar list of categories would work.

---

_Label `duplicate` added by @BurntSushi on 2021-06-14 19:25_

---

_Comment by @BurntSushi on 2021-06-14 19:26_

Dupe of #1814 

---

_Closed by @BurntSushi on 2021-06-14 19:26_

---
