```yaml
number: 14072
title: "Add `rustfmt.toml`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/rustfmt
created_at: 2025-06-16T13:49:15Z
updated_at: 2025-06-16T14:05:58Z
url: https://github.com/astral-sh/uv/pull/14072
synced_at: 2026-01-12T16:11:00Z
```

# Add `rustfmt.toml`

---

_@BurntSushi_

We've gotten away without this file for a while. In particular, we
explicitly use its default settings.

However, this is occasionally problematic in certain contexts where
`rustfmt` is invoked directly. Or in contexts where the Rust Edition is
otherwise not specified. At least, this happens when using the Rust vim
plugin. When an edition isn't explicitly specified, it defaults back to
the 2015 edition.

I think that there aren't a lot of rustfmt changes, and so we've been
able to get away with this for a while. But it looks like something in
the 2024 edition changes how imports are ordered. So to make it explicit
that we want to use the 2024 edition of rustfmt, we opt into it.

This is analogous to a change made to the Ruff repository somewhat
recently: https://github.com/astral-sh/ruff/pull/18197


---

_Review requested from @charliermarsh by @BurntSushi on 2025-06-16 13:51_

---

_Review requested from @zanieb by @BurntSushi on 2025-06-16 13:51_

---

_@zanieb approved on 2025-06-16 13:52_

---

_Label `internal` added by @zanieb on 2025-06-16 13:52_

---

_Merged by @BurntSushi on 2025-06-16 14:05_

---

_Closed by @BurntSushi on 2025-06-16 14:05_

---

_Branch deleted on 2025-06-16 14:05_

---
