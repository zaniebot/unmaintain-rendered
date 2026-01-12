```yaml
number: 1703
title: Short Alias for --no-ignore
type: issue
state: closed
author: ghost
labels:
  - doc
assignees: []
created_at: 2020-10-15T07:54:17Z
updated_at: 2020-10-16T13:52:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1703
synced_at: 2026-01-12T16:13:24Z
```

# Short Alias for --no-ignore

---

_@ghost_

`--no-ignore` is, in my experience, one of the most common flags one would want to pass to ripgrep, but unfortunately it doesn't have a single character alias for its option. Obviously you can just add it by creating an alias under a different name, but it's annoying to do that on every system. It would be great if ripgrep supported such an option out of the box.

---

_Comment by @okdana on 2020-10-15 07:59_

`-u` is basically the short option for `--no-ignore`. It's documented separately because it behaves differently when used more than once, but `-u` by itself is the same as `--no-ignore`

---

_Label `doc` added by @BurntSushi on 2020-10-15 14:26_

---

_Comment by @BurntSushi on 2020-10-15 14:28_

Aye, yeah, the short flag for `--no-ignore` is `-u`. And in particular, `-u` can be used repeatedly to progressively disabled smart filtering. `-u` disables ignore file filtering. `-uu` disables hidden file filtering. And `-uuu` disables binary file filtering.

The docs for `--no-ignore` should definitely mention the `-u` flag here.

---

_Closed by @BurntSushi on 2020-10-16 13:52_

---
