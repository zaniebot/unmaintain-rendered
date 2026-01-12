```yaml
number: 649
title: incorrect gitignore behaviour
type: issue
state: closed
author: Kartiku
labels: []
assignees: []
created_at: 2017-10-23T14:58:50Z
updated_at: 2020-04-15T12:58:24Z
url: https://github.com/BurntSushi/ripgrep/issues/649
synced_at: 2026-01-12T16:13:22Z
```

# incorrect gitignore behaviour

---

_@Kartiku_

Steps

1. Clone nim project `git clone https://github.com/nim-lang/Nim.git`
2. In nim folder run `rg uint64`.  It gives no results which is incorrect.  Silver searcher gives correct results
3. Used `git check-ignore lib\system.nim` to verify git doesn't ignore the file
4.  `rg --debug` output [here](https://gist.github.com/kartiku/130d3c63c98fcb3304cc649d23838e2b).  Lib folder is being ignored

Using 0.7.1 release from [here](https://github.com/BurntSushi/ripgrep/releases/download/0.7.1/ripgrep-0.7.1-x86_64-pc-windows-msvc.zip)

---

_Comment by @lambda on 2017-10-24 01:56_

That is a strange `.gitignore`. Ignoring everything with `*` then unignoring all directories with `!**/`, and all files with extensions with `!*.*`, and finally listing a bunch of more specific ignore rules.

Anyhow, regardless of how strange a gitignore it is, looks like the issue is with the conversion of `!**/` into `**/**/*` before converting it into a regex. That looks like a clear bug.

---

_Comment by @lambda on 2017-10-24 15:04_

This should be fixed by PR #652, but that's currently held up by an odd, seemingly unrelated test failure on the Mac build. Should be merged once we've sorted that out.

---

_Closed by @BurntSushi on 2017-11-01 11:10_

---
