```yaml
number: 883
title: Add -z, --null-data param
type: issue
state: closed
author: kenorb
labels:
  - duplicate
assignees: []
created_at: 2018-04-14T23:59:29Z
updated_at: 2018-04-15T00:50:33Z
url: https://github.com/BurntSushi/ripgrep/issues/883
synced_at: 2026-01-12T16:13:22Z
```

# Add -z, --null-data param

---

_@kenorb_

#### What version of ripgrep are you using?

`ripgrep 0.8.1`

#### What operating system are you using ripgrep on?

macOS High Sierra

#### Describe your feature request

GNU `grep` has the following option `-z`/`--null-data` to treat input and output data as sequences of lines, each terminated by a zero byte. This is especially useful when looking for patterns across multiple lines (read complete file into a single string).

- [Example 1](https://stackoverflow.com/a/152755/55075): `grep -Pzo '_name.*\n.*_description'
`
- [Example 2](https://stackoverflow.com/a/49763153/55075): `grep -qzP '(?s)(?=.*\bstring1\b)(?=.*\bstring2\b)(?=.*\bstring3\b)' file`

---

_Comment by @BurntSushi on 2018-04-15 00:50_

Dupe of #176.

---

_Closed by @BurntSushi on 2018-04-15 00:50_

---

_Label `duplicate` added by @BurntSushi on 2018-04-15 00:50_

---
