```yaml
number: 2198
title: "--ignore-dot does not ignore .rgignore"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - rollup
assignees: []
created_at: 2022-04-29T16:40:25Z
updated_at: 2023-07-08T22:53:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2198
synced_at: 2026-01-12T16:13:24Z
```

# --ignore-dot does not ignore .rgignore

---

_@BurntSushi_

### Discussed in https://github.com/BurntSushi/ripgrep/discussions/2197

<div type='discussions-op-text'>

<sup>Originally posted by **balazser** April 29, 2022</sup>
Why `--ignore-dot` switch ignores `.ignore` but not `.rgignore`? Although if `.rgignore` file presents in the parent dictionary it can be ignored with `--ignore-parent`.</div>

Here is a test case:

```
$ mkcd /tmp/rg-2197
$ touch a b c
$ echo a > .ignore
$ echo b > .rgignore
$ rg --files
c
$ rg --files --no-ignore-dot
c
a
```

---

_Label `bug` added by @BurntSushi on 2022-04-29 16:40_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 13:46_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
