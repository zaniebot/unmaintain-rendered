```yaml
number: 1478
title: Clarify multiple glob flag behavior in help message
type: issue
state: closed
author: casey
labels:
  - doc
  - rollup
assignees: []
created_at: 2020-02-06T03:03:11Z
updated_at: 2020-03-15T17:19:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1478
synced_at: 2026-01-12T16:13:23Z
```

# Clarify multiple glob flag behavior in help message

---

_@casey_

I was unsure of how conflicting globs are handled and was unable to figure it out from the help message, so it might be nice to include this information. I'm not entirely sure of the wording, but something like:

```
Include or exclude files and directories for searching that match the given
glob. This always overrides any other ignore logic. Multiple glob flags may be
used. If multiple globs match a file or directory, the glob given later in
the command line takes precedence.
```

---

_Label `doc` added by @BurntSushi on 2020-03-15 15:24_

---

_Label `rollup` added by @BurntSushi on 2020-03-15 15:24_

---

_Closed by @BurntSushi on 2020-03-15 17:19_

---
