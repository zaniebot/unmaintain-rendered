```yaml
number: 2458
title: "--trim runs after -M/--max-columns"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - rollup
assignees: []
created_at: 2023-03-15T12:31:44Z
updated_at: 2023-11-25T20:03:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2458
synced_at: 2026-01-12T16:13:24Z
```

# --trim runs after -M/--max-columns

---

_@BurntSushi_

See the discussion below. It seems to me like `--trim` should run _before_ applying the max column length.

I say this without re-contextualizing myself with the code. There may be a reason for the current behavior that I'm forgetting.

### Discussed in https://github.com/BurntSushi/ripgrep/discussions/2456

<div type='discussions-op-text'>

<sup>Originally posted by **misaki-web** March 15, 2023</sup>
Is there a way to make ripgrep apply the `--trim` flag before reducing the line length with `--max-columns-preview -M`?

Here's an example of the current result (the string starts with 5 spaces):

```bash
$ echo "     0123456789abcdefghijklmnopqrstuvwxyz" | rg --trim --max-columns-preview -M 8 abc
012 [... 1 more match]
```

ripgrep reduces the line length, then it trims it. I expected the opposite, like this:

```bash
$ echo "     0123456789abcdefghijklmnopqrstuvwxyz" | rg --trim --max-columns-preview -M 8 abc
01234567 [... 1 more match]
```
</div>

---

_Label `question` added by @BurntSushi on 2023-03-15 12:31_

---

_Label `bug` added by @BurntSushi on 2023-11-22 21:04_

---

_Label `question` removed by @BurntSushi on 2023-11-22 21:04_

---

_Label `rollup` added by @BurntSushi on 2023-11-22 21:25_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
