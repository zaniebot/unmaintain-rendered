```yaml
number: 4138
title: "Implement `Toolchain::find_or_fetch` and use in `uv venv --preview`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-v
created_at: 2024-06-07T17:53:57Z
updated_at: 2024-06-10T15:45:08Z
url: https://github.com/astral-sh/uv/pull/4138
synced_at: 2026-01-10T13:54:02Z
```

# Implement `Toolchain::find_or_fetch` and use in `uv venv --preview`

---

_Pull request opened by @zanieb on 2024-06-07 17:53_

Extends https://github.com/astral-sh/uv/pull/4121
Part of #2607 

Adds support for managed toolchain fetching to `uv venv`, e.g.

```
❯ cargo run -q -- venv --python 3.9.18 --preview -v
DEBUG Searching for Python 3.9.18 in search path or managed toolchains
DEBUG Searching for managed toolchains at `/Users/zb/Library/Application Support/uv/toolchains`
DEBUG Found CPython 3.12.3 at `/opt/homebrew/bin/python3` (search path)
DEBUG Found CPython 3.9.6 at `/usr/bin/python3` (search path)
DEBUG Found CPython 3.12.3 at `/opt/homebrew/bin/python3` (search path)
DEBUG Requested Python not found, checking for available download...
DEBUG Using registry request timeout of 30s
INFO Fetching requested toolchain...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240224/cpython-3.9.18%2B20240224-aarch64-apple-darwin-pgo%2Blto-full.tar.zst to temporary location /Users/zb/Library/Application Support/uv/toolchains/.tmpgohKwp
DEBUG Extracting cpython-3.9.18%2B20240224-aarch64-apple-darwin-pgo%2Blto-full.tar.zst
DEBUG Moving /Users/zb/Library/Application Support/uv/toolchains/.tmpgohKwp/python to /Users/zb/Library/Application Support/uv/toolchains/cpython-3.9.18-macos-aarch64-none
Using Python 3.9.18 interpreter at: /Users/zb/Library/Application Support/uv/toolchains/cpython-3.9.18-macos-aarch64-none/install/bin/python3
Creating virtualenv at: .venv
INFO Removing existing directory
Activate with: source .venv/bin/activate
```

The preview flag is required. The fetch is performed if we can't find an interpreter that satisfies the request. Once fetched, the toolchain will be available for later invocations that include the `--preview` flag. There will be follow-ups to improve toolchain management in general, there is still outstanding work from the initial implementation.
 

---

_Label `preview` added by @zanieb on 2024-06-07 17:53_

---

_@zanieb reviewed on 2024-06-07 17:56_

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:126 on 2024-06-07 17:56_

This whole thing is no longer necessary, we've been discovering managed toolchains via their directories instead of executables for a while now

---

_Marked ready for review by @zanieb on 2024-06-07 18:15_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-07 23:32_

---

_Review requested from @konstin by @zanieb on 2024-06-07 23:32_

---

_@charliermarsh reviewed on 2024-06-09 17:49_

Will review, but while it's on my mind: should there be a way to force a project to use managed toolchains? Like, fetch and install even if you have some non-managed thing on your PATH?

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/downloads.rs`:56 on 2024-06-09 18:09_

It looks like all the individual parts implement display. Consider just knocking it out in this PR?

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/downloads.rs`:144 on 2024-06-09 18:09_

Not: use a single match arm for these cases?

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/downloads.rs`:135 on 2024-06-09 18:09_

Stray newline.

---

_@charliermarsh approved on 2024-06-09 18:13_

---

_Comment by @zanieb on 2024-06-09 21:50_

> Will review, but while it's on my mind: should there be a way to force a project to use managed toolchains? Like, fetch and install even if you have some non-managed thing on your PATH?

There's a hidden environment variable right now (`UV_FORCE_MANAGED_PYTHON`) but yeah I think more holistically we're going to have to expose some enum option that more clearly describes the allowed toolchain. Managed toolchains are grouped in with "system" Python installations right now which isn't very clear.

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:132 on 2024-06-10 08:34_

Nit: `result` does not need to be mutable, either with

```rust
        let result = Self::default();
        Ok(match request {
```

or with 

```
        let result = Self::default();
        let result = match request {
```

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:174 on 2024-06-10 08:35_

You can `#[derive(Default)]` this

---

_Review comment by @konstin on `crates/uv-toolchain/src/managed.rs`:57 on 2024-06-10 08:39_

Nit: Should this be pub (will there be future usages where there are outside calls to `InstalledToolchains::from_path` directly)?

---

_Review comment by @konstin on `crates/uv-toolchain/src/managed.rs`:65 on 2024-06-10 08:40_

With the `#[error(transparent)]` on `Error`, i think the callsites of `from_settings` should have context that the error comes from toolchain discovery.

---

_@konstin approved on 2024-06-10 08:43_

---

_@zanieb reviewed on 2024-06-10 13:59_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:65 on 2024-06-10 13:59_

I'm not sure what the action item is here.

---

_Merged by @zanieb on 2024-06-10 14:10_

---

_Closed by @zanieb on 2024-06-10 14:10_

---

_Branch deleted on 2024-06-10 14:10_

---

_@charliermarsh reviewed on 2024-06-10 14:27_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/managed.rs`:65 on 2024-06-10 14:27_

I think the suggestion is that if you're using transparent IO errors, there will be no indication of why the IO error occurred. Like, if we fail to read or write a file, the entire error trace would just be "Failed to write the file". Using context, you can say that we failed during toolchain discovery.

---

_@zanieb reviewed on 2024-06-10 14:29_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:65 on 2024-06-10 14:29_

Ah interesting, thanks. I'll look into that.

---

_@konstin reviewed on 2024-06-10 15:40_

---

_Review comment by @konstin on `crates/uv-toolchain/src/managed.rs`:65 on 2024-06-10 15:40_

There's two kinds of adding context to errors: One would be replacing something like
```rust
#[error(transparent)]
ManagedToolchain(#[from] crate::managed::Error),
```
with
```rust
#[error("Something about what this error is, maybe a {python version}")]
ManagedToolchain(#[from] crate::managed::Error),
```
The other is replacing `InstalledToolchains::from_settings()?.init()?` with
```rust
let toolchains = InstalledToolchains::from_settings()?.init()?;
```

```rust
let toolchains = InstalledToolchains::from_settings()
    .context("Failed to load installed toolchains")?
    .init()
    .context("Failed to initialize toolchain discovery")?;
```

This requires some manual auditing of who adds the context, e.g. if you're asking for a specific python version, either the caller or the callee should be responsible for reporting the python version.

---

_@zanieb reviewed on 2024-06-10 15:45_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:65 on 2024-06-10 15:45_

Thanks again!

---
