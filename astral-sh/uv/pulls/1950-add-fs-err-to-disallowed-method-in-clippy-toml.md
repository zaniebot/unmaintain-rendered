```yaml
number: 1950
title: "Add `fs_err` to `disallowed_method` in clippy.toml"
type: pull_request
state: merged
author: yasufumy
labels:
  - internal
assignees: []
merged: true
base: main
head: feature/configure-clippy
created_at: 2024-02-24T12:10:21Z
updated_at: 2024-02-26T14:53:39Z
url: https://github.com/astral-sh/uv/pull/1950
synced_at: 2026-01-12T16:04:48Z
```

# Add `fs_err` to `disallowed_method` in clippy.toml

---

_@yasufumy_

## Summary

Resolve #1916


---

_Marked ready for review by @yasufumy on 2024-02-24 12:15_

---

_Comment by @zanieb on 2024-02-24 16:46_

Thanks for contributing! It looks like this isn't working e.g. this is unflagged

https://github.com/astral-sh/uv/blob/11ed4f7183cc25a00f23e86bd264d4055645eed2/crates/install-wheel-rs/src/wheel.rs#L398

---

_Comment by @charliermarsh on 2024-02-24 19:17_

I think it has to be a list of fully-qualified methods, rather than a module. So we'd have to like... enumerate every method in `fs`, to enforce this? Or is there a more conventional fix for this?

---

_Comment by @yasufumy on 2024-02-25 08:31_

@zanieb Thanks! I fixed my misconfiguration.

@charliermarsh Unfortunately, there seems to be no convenient way to do this like `std::fs::*`, so I enumerated all the methods in `std::fs` that can be replaced in `fs_err`. 

---

_Review comment by @MichaReiser on `clippy.toml`:11 on 2024-02-25 09:13_

What's the motivation for disallowing these types? Are there equivalent types in `fs_err`? I see that you explicitly allow them further down. 

---

_@MichaReiser reviewed on 2024-02-25 09:13_

Nice!

---

_Review requested from @konstin by @MichaReiser on 2024-02-25 09:13_

---

_Label `internal` added by @MichaReiser on 2024-02-25 09:13_

---

_@yasufumy reviewed on 2024-02-25 09:45_

---

_Review comment by @yasufumy on `clippy.toml`:11 on 2024-02-25 09:45_

Yes, there are equivalent types (https://docs.rs/fs-err/2.11.0/fs_err/#structs), but they do not seem to be fully compatible with types in `std::fs` (I got errors when replacing them). So I thought it would be good to use the types in `fs_err` as much as possible and explicitly allow types in `std::fs` otherwise.
But I think it might be a bit annoying to have to explicitly allow it every time we use types in `std::fs`. If so, I'll revise it.

---

_@MichaReiser reviewed on 2024-02-25 10:07_

---

_Review comment by @MichaReiser on `clippy.toml`:11 on 2024-02-25 10:07_

Let's hear what @konstin thinks. He knows best when and where we should use `fs_err`.

---

_@konstin reviewed on 2024-02-26 14:04_

---

_Review comment by @konstin on `crates/uv-git/Cargo.toml`:35 on 2024-02-26 14:04_

```suggestion
fs-err = { workspace = true }
```

---

_@konstin reviewed on 2024-02-26 14:04_

---

_Review comment by @konstin on `crates/uv/Cargo.toml`:101 on 2024-02-26 14:04_

```suggestion
fs-err = { workspace = true }
```

---

_Review comment by @konstin on `clippy.toml`:11 on 2024-02-26 14:05_

`std::fs::File::create` doesn't tell you the filename when an error occurs, while `fs_err::File::create` does. There are case where generics are implemented on std types (e.g. `AutoStream`) so we need to "downcast". I like the current solution.

---

_@konstin approved on 2024-02-26 14:06_

---

_@konstin reviewed on 2024-02-26 14:08_

---

_Review comment by @konstin on `clippy.toml`:11 on 2024-02-26 14:08_

The clippy feature we're missing to ban `std::fs` as a module is https://github.com/rust-lang/rust-clippy/issues/9489

---

_Merged by @konstin on 2024-02-26 14:15_

---

_Closed by @konstin on 2024-02-26 14:15_

---

_Branch deleted on 2024-02-26 14:53_

---
