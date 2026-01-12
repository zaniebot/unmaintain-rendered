```yaml
number: 910
title: Ignore Crate Case Insensitive
type: issue
state: closed
author: gsquire
labels:
  - doc
assignees: []
created_at: 2018-05-03T21:29:44Z
updated_at: 2018-05-06T23:03:12Z
url: https://github.com/BurntSushi/ripgrep/issues/910
synced_at: 2026-01-12T16:13:22Z
```

# Ignore Crate Case Insensitive

---

_@gsquire_

This is not directly related to ripgrep, hence the template is missing.

When using the `ignore` crate, I wanted to add a set of overrides using the `OverrideBuilder` type. When testing case-insensitivity, I noticed that it only worked when I called `case_insensitive(true)` before adding any overrides. I don't know if this is an issue or just something that should be documented in `OverrideBuilder`.

If this is a simple doc fix, I would be happy to submit a patch. Thanks!

---

_Comment by @BurntSushi on 2018-05-03 21:34_

Yeah, that is the intended behavior. A doc fix would be most welcome. :-)

---

_Label `doc` added by @BurntSushi on 2018-05-03 21:34_

---

_Closed by @BurntSushi on 2018-05-06 23:03_

---
