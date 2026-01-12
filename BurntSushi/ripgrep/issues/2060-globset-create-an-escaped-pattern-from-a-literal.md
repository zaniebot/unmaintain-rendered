```yaml
number: 2060
title: "[globset] Create an escaped pattern from a literal string"
type: issue
state: closed
author: piegamesde
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2021-11-12T14:23:23Z
updated_at: 2023-07-08T22:53:12Z
url: https://github.com/BurntSushi/ripgrep/issues/2060
synced_at: 2026-01-12T16:13:24Z
```

# [globset] Create an escaped pattern from a literal string

---

_@piegamesde_

Basically I'm migrating some code from `glob` and looking for a functional equivalent of https://docs.rs/glob/0.3.0/glob/struct.Pattern.html#method.escape. Maybe put is as `GlobBuilder::new_escaped` or something.

---

_Comment by @BurntSushi on 2021-11-12 14:55_

`GlobBuilder::new_escaped` seems pretty weird to me. I suspect it should either be `Glob::escape` (like `Pattern::escape`), or perhaps just a free function, like in the [regex crate](https://docs.rs/regex/1.5.4/regex/fn.escape.html). I think I have a preference toward the latter.

---

_Comment by @piegamesde on 2021-11-12 15:25_

You are right, I didn't think this through. Since there is no globbing, we also don't need the options and thus don't need to use the `GlobBuilder`.

---

_Comment by @BurntSushi on 2021-11-12 15:34_

A PR would be welcome. Otherwise, it's unlikely I'll work on this any time soon myself.

---

_Label `enhancement` added by @BurntSushi on 2021-11-12 15:34_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 21:25_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
