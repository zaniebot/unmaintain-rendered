---
number: 14594
title: Tests failed for ruff 0.8.0
type: issue
state: closed
author: eli-schwartz
labels:
  - testing
  - ty
assignees: []
created_at: 2024-11-25T23:19:29Z
updated_at: 2024-11-26T14:40:04Z
url: https://github.com/astral-sh/ruff/issues/14594
synced_at: 2026-01-07T13:12:16-06:00
---

# Tests failed for ruff 0.8.0

---

_Issue opened by @eli-schwartz on 2024-11-25 23:19_

I attempted to package ruff 0.8.0 as a distro package update for Gentoo Linux. The new version fails to run tests.

Possibly relevant environment information:

```
$ rustc --version
rustc 1.81.0 (eeb90cda1 2024-09-04) (gentoo)
```
I notice that the CI/releases seem to be using 1.82 since ruff 0.7.1 but that version hasn't been stabilized in Gentoo yet...


Failed assertion:

```
 * /usr/lib/rust/1.81.0/bin/cargo test --release --target-dir /var/tmp/portage/dev-util/ruff-0.8.0/work/ruff-0.8.0/tested-target/

[... lots of building ...]

failures:

---- types::infer::tests::basic_async_comprehension stdout ----
thread 'types::infer::tests::basic_async_comprehension' panicked at crates/red_knot_python_semantic/src/types/infer.rs:5998:9:
assertion `left == right` failed
  left: "@Todo"
 right: "@Todo(async iterables/iterators)"

---- types::infer::tests::invalid_async_comprehension stdout ----
thread 'types::infer::tests::invalid_async_comprehension' panicked at crates/red_knot_python_semantic/src/types/infer.rs:5968:9:
assertion `left == right` failed
  left: "@Todo"
 right: "@Todo(async iterables/iterators)"


failures:
    types::infer::tests::basic_async_comprehension
    types::infer::tests::invalid_async_comprehension

test result: FAILED. 431 passed; 2 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.68s
```

Exhaustively exhaustive build + test logs: [build.log](https://github.com/user-attachments/files/17910023/build.log)


---

_Comment by @MichaReiser on 2024-11-26 08:03_

@sharkdp would you mind taking a look at this. It seems that our mdtests now fail if the tests are run in release mode. I suspect that we need to roll-back the change to assert on specific `@Todo(source)` comments 

---

_Label `testing` added by @MichaReiser on 2024-11-26 08:04_

---

_Label `red-knot` added by @MichaReiser on 2024-11-26 08:04_

---

_Assigned to @sharkdp by @sharkdp on 2024-11-26 08:37_

---

_Referenced in [astral-sh/ruff#14604](../../astral-sh/ruff/pulls/14604.md) on 2024-11-26 09:34_

---

_Closed by @sharkdp on 2024-11-26 14:40_

---
