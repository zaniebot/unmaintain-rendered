```yaml
number: 467
title: "-o but with groups"
type: issue
state: closed
author: passcod
labels: []
assignees: []
created_at: 2017-04-29T01:18:47Z
updated_at: 2017-04-29T01:53:58Z
url: https://github.com/BurntSushi/ripgrep/issues/467
synced_at: 2026-01-12T16:13:22Z
```

# -o but with groups

---

_@passcod_

Grep doesn't have this feature either.

Something I do fairly frequently is try to extract patterns within a file which may occur one or more times per line, and output them one per line for further processing. For example, I have files with lines like this:

```
- {SW Legends} foo bar {33k + 2k + 11k words}
- {SW} blah blah {160k words atow}
```

and I try to extract just the word counts, individually.

Currently I use multiple invocations and xargs:

```
$ rg -o '[\d\s+.k]+ words' file.md | xargs | ag -o '[\d.]+'
33
2
11
160
```

But I'd really like to be able to run something like this:

```
$ rg -o '$n' '\{((?P<n>[\d.]+)k[\s+]*)+ words[^}]*\}' file.md
```

and obtain the same result.

---

_Comment by @passcod on 2017-04-29 01:25_

The example above uses `ag` on the second invocation because of #468.

---

_Comment by @BurntSushi on 2017-04-29 01:33_

This seems like a complex feature. I don't think you need `xargs` in your pipeline. If I remove it, then your example still works. Having to use a simple pipeline seems fine?

---

_Comment by @passcod on 2017-04-29 01:53_

Hmm. In this case, yes. I've had other cases where the multiple invocation approach wouldn't work. I'll reopen if/when I find those?

---

_Closed by @passcod on 2017-04-29 01:53_

---
