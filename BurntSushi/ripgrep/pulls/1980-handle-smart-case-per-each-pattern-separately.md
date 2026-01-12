```yaml
number: 1980
title: Handle smart case per each pattern separately
type: pull_request
state: closed
author: xzfc
labels: []
assignees: []
base: master
head: 1791-smart-case-patterns
created_at: 2021-08-29T11:06:08Z
updated_at: 2023-07-08T22:23:37Z
url: https://github.com/BurntSushi/ripgrep/pull/1980
synced_at: 2026-01-12T18:23:14Z
```

# Handle smart case per each pattern separately

---

_@xzfc_

When `-S`/`--smart-case` is enabled, analyze and apply a case-insensitivity flag to each search pattern separately rather than globally.

For the PCRE2 engine, wrap case-insensitive expressions into `(?i:…)`. I.e. `rg --pcre2 --smart-case -e foo -e bAr` gives `(?i:foo)|bAr`.

For the default engine, produce each expression HIR separately, then combine them using `Hir::alternation`.  Although the PCRE2 approach is also possible, it turned out to be slower on large pattern files.

Fixes #1791

---

As a side effect, each pattern is validated separately in the default engine; thus, this no longer works:
```
$ rg -e "something(" -e ")something"
regex parse error:
    something(
             ^
error: unclosed group
```
Also, this could be considered as a first step in fixing #478.

---

_Comment by @BurntSushi on 2023-07-08 22:23_

Thanks for working on this. Unfortunately, a refactor I did made this PR unmergeable as-is. Actually, the specific kind of change pursued here is probably simpler to do now. With that said, I'm unsure of whether I want to parse every pattern individually like this. It could be a big additional cost when searching a lot of patterns. However, it is something I've been mulling over for making error reporting better. Anyway, I'm going to close this PR out for now but I might re-visit the idea later.

---

_Closed by @BurntSushi on 2023-07-08 22:23_

---
