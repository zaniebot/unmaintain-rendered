---
number: 13638
title: "`cargo install --git https://github.com/astral-sh/uv uv` is not working"
type: issue
state: closed
author: fromdot
labels:
  - bug
assignees: []
created_at: 2025-05-24T15:59:02Z
updated_at: 2025-05-25T11:11:08Z
url: https://github.com/astral-sh/uv/issues/13638
synced_at: 2026-01-10T01:25:35Z
---

# `cargo install --git https://github.com/astral-sh/uv uv` is not working

---

_Issue opened by @fromdot on 2025-05-24 15:59_

### Summary

**Issue Description:**

When attempting to install `uv` directly from the GitHub repository using the command `cargo install --git https://github.com/astral-sh/uv uv`, the installation fails with the error:

```
error: could not find `uv` in https://github.com/astral-sh/uv with version `*`
```

Additionally, attempts to specify a version or tag (e.g., using --tag, --version, or @[version_or_tag]) did not resolve this issue.

**Workaround:**

The current workaround is to clone the repository locally and then install using the `--path` flag:

```bash
git clone https://github.com/astral-sh/uv.git
cd uv
cargo install --path crates/uv
```

### Platform

macOS 15.1 M4

### Version

uv 0.7.8, 0.7.7

### Python version

_No response_

---

_Label `bug` added by @fromdot on 2025-05-24 15:59_

---

_Comment by @Gankra on 2025-05-24 17:36_

Hmm I just ran `cargo install --git https://github.com/astral-sh/uv uv` on my M4 macbook pro and it went off without a hitch. Maybe github was throwing errors that cargo was responding weirdly to?

---

_Comment by @Gankra on 2025-05-24 17:40_

In case relevant, my current `cargo -vV`:

```
cargo 1.86.0 (adf9b6ad1 2025-02-28)
release: 1.86.0
commit-hash: adf9b6ad14cfa10ff680d5806741a144f7163698
commit-date: 2025-02-28
host: aarch64-apple-darwin
libgit2: 1.9.0 (sys:0.20.0 vendored)
libcurl: 8.7.1 (sys:0.4.79+curl-8.12.0 system ssl:(SecureTransport) LibreSSL/3.3.6)
ssl: OpenSSL 1.1.1w  11 Sep 2023
os: Mac OS 15.4.1 [64-bit]
```

---

_Comment by @fromdot on 2025-05-25 11:11_

Thanks for the feedback! @Gankra 

Quick update: I was previously on rustc 1.80.1 (via the stable toolchain) and encountering this issue. 
After seeing @Gankra 's success with cargo 1.86.0, I switched my default toolchain to 1.86-aarch64-apple-darwin using `rustup default 1.86-aarch64-apple-darwin`.

With the 1.86.0 toolchain active, `cargo install --git https://github.com/astral-sh/uv uv` now works correctly for me.

This suggests the problem was likely related to my previous toolchain setup (either `stable` being in a bad state or an issue with Cargo `1.80.1`) rather than an issue with `uv` itself or the `cargo install --git` command for workspaces in general. 

My `stable` toolchain was also failing to update correctly, which further points to a local environment issue.

Hope this info is helpful for anyone else who might run into something similar.

---

_Closed by @fromdot on 2025-05-25 11:11_

---
