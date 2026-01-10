```yaml
number: 8796
title: Virtual environments created by uv are backed up by Time Machine
type: issue
state: open
author: mikepqr
labels:
  - enhancement
assignees: []
created_at: 2024-11-04T03:05:13Z
updated_at: 2025-02-28T13:44:31Z
url: https://github.com/astral-sh/uv/issues/8796
synced_at: 2026-01-10T03:50:30Z
```

# Virtual environments created by uv are backed up by Time Machine

---

_Issue opened by @mikepqr on 2024-11-04 03:05_

`uv` creates [`CACHEDIR.TAG`](https://bford.info/cachedir/) in `.venv`, indicating that backup tools need not make copies of this directory. Unfortunately, as far as I can tell, the builtin macOS backup tool Time Machine does not respect this file, meaning that the virtual environment is backed up incrementally.

While I appreciate that the right way to fix this is for Apple to teach Time Machine to respect CACHEDIR.TAG, the probability of that seems remote.

Is there any interest in handling this in `uv`? One way to do this would be to shell out to `tmutil addexclusion .venv/` when the environment is created. This is what e.g. https://github.com/stevegrunwell/asimov/ does and it is supported by macOS.

For what it's worth, the performance impact of this would be measurable, but perhaps tolerable since this would only need to run each time `.venv` is created:

```
$ hyperfine "tmutil addexclusion .venv"
Benchmark 1: tmutil addexclusion .venv
  Time (mean ± σ):      18.0 ms ±   6.3 ms    [User: 4.3 ms, System: 4.3 ms]
  Range (min … max):     9.6 ms …  39.1 ms    95 runs
```

---

_Comment by @edmorley on 2025-02-26 19:24_

Cargo's solution to this was to use the `core-foundation` crate to set the relevant extended attributes on macOS to exclude their target directory from Time Machine:
- https://github.com/rust-lang/cargo/issues/3884
- https://github.com/rust-lang/cargo/pull/4386
- https://github.com/rust-lang/cargo/blob/392d68bff462015e8e7d8588333a09b08b4a10a6/crates/cargo-util/src/paths.rs#L840-L861
- https://developer.apple.com/documentation/foundation/nsurlisexcludedfrombackupkey

---

_Comment by @zanieb on 2025-02-26 21:47_

Thanks @edmorley! That looks pretty interesting.

---

_Label `enhancement` added by @konstin on 2025-02-28 13:44_

---
