```yaml
number: 8604
title: "Remove unneeded `return` from Maturin project template"
type: pull_request
state: merged
author: vivienm
labels:
  - enhancement
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-27T10:00:44Z
updated_at: 2024-10-29T12:34:03Z
url: https://github.com/astral-sh/uv/pull/8604
synced_at: 2026-01-12T16:08:23Z
```

# Remove unneeded `return` from Maturin project template

---

_@vivienm_

## Summary

This PR removes an unnecessary `return` statement from Rust code in the Maturin template. This makes the generated code more idiomatic and prevents a Clippy warning.

## Test Plan

This change was tested both in unit tests and by manually creating a Maturin project.


---

_Comment by @zanieb on 2024-10-27 13:49_

I don't quite follow, why should we remove it?

Isn't this just mirroring our typical library example function? https://github.com/astral-sh/uv/blob/0b02a8c28ba935cd5e6b76c2cab18be775f5050a/crates/uv/tests/it/init.rs#L368-L369

---

_Review requested from @konstin by @zanieb on 2024-10-27 13:50_

---

_Comment by @vivienm on 2024-10-27 14:11_

> I don't quite follow, why should we remove it?
> 
> Isn't this just mirroring our typical library example function? 

This is a needless return, which I believe is an anti-pattern in Rust. It does trigger a Clippy warning on a newly instantiated project:

```
warning: unneeded `return` statement
 --> src/lib.rs:5:5
  |
5 |     return "Hello from uvrs!".to_string();
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#needless_return
  = note: `#[warn(clippy::needless_return)]` on by default
help: remove `return`
  |
5 -     return "Hello from uvrs!".to_string();
5 +     "Hello from uvrs!".to_string()
  |
```

That said, it may be more important to closely match the Python code. You be the judge :)

---

_Comment by @zanieb on 2024-10-27 14:15_

Ohh I see what you're saying 🤦‍♀️ yes this makes sense, thanks!

---

_@zanieb approved on 2024-10-27 14:15_

---

_Label `enhancement` added by @zanieb on 2024-10-27 14:15_

---

_Merged by @zanieb on 2024-10-27 14:15_

---

_Closed by @zanieb on 2024-10-27 14:15_

---

_Comment by @samypr100 on 2024-10-29 12:34_

Thanks! @vivienm 

---
