```yaml
number: 9037
title: Make diagnostic methods dynamic on error
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/gen
created_at: 2024-11-12T02:01:15Z
updated_at: 2024-11-12T15:01:39Z
url: https://github.com/astral-sh/uv/pull/9037
synced_at: 2026-01-12T16:08:37Z
```

# Make diagnostic methods dynamic on error

---

_@charliermarsh_

## Summary

I need these to accept "any error".


---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-12 02:01_

---

_Label `internal` added by @charliermarsh on 2024-11-12 02:01_

---

_@konstin approved on 2024-11-12 11:45_

Looks good to me (i'll let andrew have the final word), i just wanted to note that this sounds a lot like the abstraction `anyhow::Error` builds.

---

_@BurntSushi approved on 2024-11-12 12:24_

This LGTM. (What is the motivation?) This is the same way `std::io::Error::other` works: https://doc.rust-lang.org/std/io/struct.Error.html#method.other

You could use an `anyhow::Error` here, but the main advantage of that type is to build up a causal error chain. I think we're mostly doing that via `thiserror`, so I don't see much advantage to using it here.

---

_Comment by @charliermarsh on 2024-11-12 14:35_

The motivation here is that I want to be able to pass in either `Box<uv_diagnostic::Error>` or `Arc<uv_diagnostic::Error>`... I can't clone, and I can't take a reference because I then need the type to be a field on the inner `struct Error` I create within these methods, and using a non-static lifetime gives me: `non-static lifetimes are not allowed in the source of an error, because std::error::Error requires the source is dyn Error + 'static`


---

_Merged by @charliermarsh on 2024-11-12 15:01_

---

_Closed by @charliermarsh on 2024-11-12 15:01_

---

_Branch deleted on 2024-11-12 15:01_

---
