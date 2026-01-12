```yaml
number: 1883
title: impl Default for GlobSet
type: pull_request
state: closed
author: sourcefrog
labels:
  - rollup
assignees: []
base: master
head: globset-default
created_at: 2021-05-31T23:04:40Z
updated_at: 2021-06-01T01:51:27Z
url: https://github.com/BurntSushi/ripgrep/pull/1883
synced_at: 2026-01-12T18:23:14Z
```

# impl Default for GlobSet

---

_@sourcefrog_

This makes it easier to use in structs that derive Default

The empty value seems like a reasonable default "zero" value?

---

_Label `rollup` added by @BurntSushi on 2021-05-31 23:28_

---

_Comment by @sourcefrog on 2021-06-01 00:30_

Fixed rustfmt, sorry.

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
