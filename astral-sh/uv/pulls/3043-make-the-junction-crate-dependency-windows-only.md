```yaml
number: 3043
title: Make the junction crate dependency Windows-only
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: junction-windows-only
created_at: 2024-04-15T20:46:20Z
updated_at: 2024-04-15T21:01:24Z
url: https://github.com/astral-sh/uv/pull/3043
synced_at: 2026-01-12T16:05:23Z
```

# Make the junction crate dependency Windows-only

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Since the [`junction` crate](https://crates.io/crates/junction) implements Windows-only functionality, and since the only place it is used is guarded by `#[cfg(windows)]`,

https://github.com/astral-sh/uv/blob/1f626bfc7300a700257348e1890af90837c740e6/crates/uv-fs/src/lib.rs#L65-L86

it makes sense not to depend on this crate at all on non-Windows platforms.

If nothing else, this makes Linux distribution packagers’ lives just a *tiny* bit easier.

## Test Plan

<!-- How was it tested? -->

On Fedora Linux 39:

```
# To avoid an error when /tmp and the working directory are on different filesystems:
$ mkdir _tmp
$ TMPDIR="${PWD}/tmp" cargo run -p uv-dev -- fetch-python
$ cargo test
```

I don’t have access to a Windows system.

---

_@charliermarsh approved on 2024-04-15 20:47_

(Cool with me as long as it passes CI.)

---

_Label `internal` added by @zanieb on 2024-04-15 20:47_

---

_Renamed from "Make the junction crate dependency Windows-only" to "[DRAFT] Make the junction crate dependency Windows-only" by @musicinmybrain on 2024-04-15 20:50_

---

_Comment by @musicinmybrain on 2024-04-15 20:50_

Hold on, I’m still getting the `junction` crate pulled in. Let me double-check this.

---

_Comment by @musicinmybrain on 2024-04-15 20:56_

> Hold on, I’m still getting the `junction` crate pulled in. Let me double-check this.

Looks like that’s a downstream issue in my experimental RPM spec file. I think a dependencies is getting generated unconditionally from 

https://github.com/astral-sh/uv/blob/f6b1580d8bd5388d80592701a02d0e77a199ddd5/Cargo.toml#L95

but when I `cargo build` or `cargo test` directly, the `junction` crate isn’t compiled, so I’m going to say that’s a downstream issue and this PR is still good.

---

_Renamed from "[DRAFT] Make the junction crate dependency Windows-only" to "Make the junction crate dependency Windows-only" by @musicinmybrain on 2024-04-15 20:56_

---

_Comment by @zanieb on 2024-04-15 21:01_

Thanks!

---

_Merged by @zanieb on 2024-04-15 21:01_

---

_Closed by @zanieb on 2024-04-15 21:01_

---
