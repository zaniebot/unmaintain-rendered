```yaml
number: 1792
title: "How to build a tiny `ty`?"
type: issue
state: closed
author: lmmx
labels:
  - question
assignees: []
created_at: 2025-12-06T16:47:57Z
updated_at: 2025-12-06T17:28:09Z
url: https://github.com/astral-sh/ty/issues/1792
synced_at: 2026-01-12T15:54:25Z
```

# How to build a tiny `ty`?

---

_@lmmx_

### Question

I just built my own `uv` with the `minimal-size` profile and saw over 50% disk size saving (I am also stripping the binary and compressing with upx) from 17MB down to just 7.8 MB (🥳!!) and I was wondering if/how I could achieve the same with ty

```sh
git clone --depth 1 https://github.com/astral-sh/uv.git "$temp_dir/uv"
cargo build --manifest-path "$temp_dir/uv/Cargo.toml" --profile minimal-size -p uv
```

ty is a Python package not a pure Rust crate, but the underlying crate is built with maturin so my hunch is this should be possible...

https://github.com/astral-sh/ty/blob/84a1881169fe0d5c0e012d405ed1c0c8a4dcb7c6/pyproject.toml#L88-L99

Pre-built `ty` as installed via `uv tool` is now bigger than the minimally sized `uv`!

```
8.1M	ty
7.8M	uv
```

- [Source](https://github.com/lmmx/just-pre-commit/blob/master/refresh_binaries.sh#L37-L38)

### Version

Latest on trunk

---

_Label `question` added by @lmmx on 2025-12-06 16:47_

---

_Comment by @Gankra on 2025-12-06 17:13_

`uv` and `ty` have the same basic structure, they're both pure Rust that we optionally bundle as python packages for ease of use. The actual rust code for `ty` is in https://github.com/astral-sh/ruff/ (which this repo includes as a submodule).

`cargo build -p ty` builds the pure rust binary version of ty

---

_Comment by @lmmx on 2025-12-06 17:27_

Aha aha, I'm sorry for some reason the repo here made me think I needed to go via maturin. I checked and the `ty` I have is indeed a binary, so can't be Python. I added the same profile section that uv has to the ruff workspace TOML and built smaller versions of both ty and ruff 🎉 I'll close this one and see if there's any interest in adding that over in the other repo.

before:

```
11M	ruff
8.1M	ty
```

after:

```
6.3M	ruff
5.6M	ty
```

---

_Closed by @lmmx on 2025-12-06 17:27_

---
