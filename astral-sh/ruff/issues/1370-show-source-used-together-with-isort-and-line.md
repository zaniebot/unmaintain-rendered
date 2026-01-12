```yaml
number: 1370
title: "`--show-source` used together with isort and line continuation characters panics"
type: issue
state: closed
author: squiddy
labels:
  - bug
assignees: []
created_at: 2022-12-25T06:15:30Z
updated_at: 2022-12-26T00:52:43Z
url: https://github.com/astral-sh/ruff/issues/1370
synced_at: 2026-01-12T15:54:41Z
```

# `--show-source` used together with isort and line continuation characters panics

---

_@squiddy_

```python
from module import member1, \
     member2
```

```
❯ RUST_BACKTRACE=1 cargo run test.py --show-source --select I --no-cache
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Line index out of bounds: line index 3, Rope/RopeSlice line count 2', /home/squiddy/.cargo/registry/src/github.com-1ecc6299db9ec823/ropey-1.5.0/src/rope.rs:764:41

<snip>

   4: ropey::rope::Rope::line_to_char
             at /home/squiddy/.cargo/registry/src/github.com-1ecc6299db9ec823/ropey-1.5.0/src/rope.rs:764:9
   5: ruff::source_code_locator::SourceCodeLocator::slice_source_code_range
             at ./src/source_code_locator.rs:43:19
   6: ruff::message::Source::from_check
             at ./src/message.rs:58:22
   7: ruff::linter::lint::{{closure}}
             at ./src/linter.rs:353:26

<snip>
```

---

When running `cargo run resources/test/fixtures/isort/* --show-source --select I` I can see some seemingly wrong check locations. The source code slices are cut off, e.g.

```
resources/test/fixtures/isort/combine_as_imports.py:1:1: I001 Import block is un-sorted or un-formatted
  |
1 | / from module import Class as C
2 | | from module import CONSTANT
3 | | from module import function
4 | | from module import function as f
```

---

_Label `bug` added by @charliermarsh on 2022-12-25 06:17_

---

_Comment by @charliermarsh on 2022-12-26 00:37_

(Just clarifying to myself that this requires that the file is missing a trailing newline.)

---

_Closed by @charliermarsh on 2022-12-26 00:52_

---
