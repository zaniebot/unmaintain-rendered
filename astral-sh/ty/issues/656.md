```yaml
number: 656
title: "Build failure with maturin 1.8.7: \"unsupported Zip archive: Unsupported compression level\""
type: issue
state: closed
author: gyscos
labels: []
assignees: []
created_at: 2025-06-14T14:28:54Z
updated_at: 2025-06-15T12:10:49Z
url: https://github.com/astral-sh/ty/issues/656
synced_at: 2026-01-10T02:08:20Z
```

# Build failure with maturin 1.8.7: "unsupported Zip archive: Unsupported compression level"

---

_Issue opened by @gyscos on 2025-06-14 14:28_

### Summary

On a fresh checkout (0.0.1-alpha.10, commit 15bae14c22e2bb3673b379fa6367c19c9aa00235), with maturin 1.8.7 installed, running `maturin buid` in this repo results in this error message:

```
...
   Compiling ty_project v0.0.0 (/home/gyscos/fetched/ty/ruff/crates/ty_project)
   Compiling ty_server v0.0.0 (/home/gyscos/fetched/ty/ruff/crates/ty_server)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 25.39s
💥 maturin failed
  Caused by: unsupported Zip archive: Unsupported compression level
```

With maturin 1.8.6, the same command works fine.

This is running on an up-to-date archlinux system with the official maturin package.

Might be related to https://github.com/PyO3/maturin/pull/2625.

### Version

_No response_

---

_Comment by @gyscos on 2025-06-15 02:03_

Looks like it's probably entirely on maturin's fault: https://github.com/PyO3/maturin/pull/2644

I'll close this ticket as I don't think there's much that can be done here.

Using the git version of maturin indeed solves the problem.

---

_Closed by @gyscos on 2025-06-15 02:03_

---

_Comment by @charliermarsh on 2025-06-15 02:24_

(Sorry if unhelpful but is there a specific reason you need a wheel? I'd suggest just running `cargo build` instead.)

---

_Comment by @gyscos on 2025-06-15 11:06_

I don't think I need a wheel. This is happening during the packaging for [the archlinux AUR package](https://aur.archlinux.org/packages/ty).

Do you have recommended build instructions for this? The current PKGBUILD might not be optimal.

[The current build function](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=ty#n30) runs:
```
  maturin build --locked --release --all-features --target "$(rustc -vV | sed -n 's/host: //p')" --strip
```
(Mostly taking [the ruff archlinux package](https://gitlab.archlinux.org/archlinux/packaging/packages/ruff/-/blob/main/PKGBUILD?ref_type=heads#L36) as inspiration.)

---
