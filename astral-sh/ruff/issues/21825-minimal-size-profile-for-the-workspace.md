```yaml
number: 21825
title: "`minimal-size` profile for the workspace"
type: issue
state: closed
author: lmmx
labels:
  - release
  - wish
assignees: []
created_at: 2025-12-06T16:58:56Z
updated_at: 2025-12-06T19:29:51Z
url: https://github.com/astral-sh/ruff/issues/21825
synced_at: 2026-01-12T15:54:58Z
```

# `minimal-size` profile for the workspace

---

_@lmmx_

Can we add a `minimal-size` profile for the workspace like `uv` has? I just built with it and it was very effective (2x smaller uv than the prebuilt via `uv tool install`)!

I think adding this might enable me to build a smaller ty via maturin, which has a symlink to this repo's crates in its repo that is used to get ty's rust source

- See related issue https://github.com/astral-sh/ty/issues/1792

---

_Label `wish` added by @Gankra on 2025-12-06 17:22_

---

_Label `release` added by @Gankra on 2025-12-06 17:23_

---

_Comment by @Gankra on 2025-12-06 17:29_

Yes I think it would be reasonable to include a version of uv's minimal-size profile:

https://github.com/astral-sh/uv/blob/28a8194a67726b93524e3ade42d0db884519ec73/Cargo.toml#L321

In our workspace Cargo.toml:

https://github.com/astral-sh/ruff/blob/main/Cargo.toml#L271

However the panic=abort setting is slightly dubious as we do catch and recover from panics in ty to make the software more reliable (the code "shouldn't ever" panic but sometimes it does so, we don't want that to break everything). The minimal-size profile is used by uv only for uv-build which had a lot of concerns from people about bloat, and doesn't have as much concern about graceful error recovery (either your build works or it doesn't).

Recovering from panics is especially important for the LSP which must run continuously.


---

_Comment by @lmmx on 2025-12-06 17:29_

I tried it out with the same section as the uv workspace has, and built smaller versions of both ruff and ty:

- ruff: 11M ⇒ 6.3M (-42%)
- ty: 8.1M ⇒ 5.6M (-31%)

I'll open a PR, and sure I will remove the panic=abort setting in it 👍 

---

_Comment by @lmmx on 2025-12-06 17:41_

After removing the `panic = "abort"` they got slightly bigger, so updated size reductions for reference:

- ruff 11M ⇒ 7.1M (-35%)
- ty: 8.1M ⇒ 6.1M (-24%)

(Note that these are measured on `upx`-compressed, stripped, binaries built on Linux)

---

_Closed by @MichaReiser on 2025-12-06 19:29_

---
