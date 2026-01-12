```yaml
number: 17214
title: "panic: AmbiguousAuthority when using proxy url with git source + branch"
type: issue
state: closed
author: b1ankcat
labels:
  - bug
assignees: []
created_at: 2025-12-22T08:47:08Z
updated_at: 2025-12-25T11:42:22Z
url: https://github.com/astral-sh/uv/issues/17214
synced_at: 2026-01-12T16:02:46Z
```

# panic: AmbiguousAuthority when using proxy url with git source + branch

---

_@b1ankcat_

### Summary

### Description
When using a Git dependency via tool.uv.sources with a https://githubproxy.cc/https://github.com/... URL, uv panics during URL parsing with AmbiguousAuthority.

This happens regardless of whether branch or rev is used.

### Infomation
The command that you ran : uv run python
The output of the command with the --verbose flag:
```bash
thread 'main2' (317540) panicked at crates/uv-pypi-types/src/parsed_url.rs:535:14:
Git URL is invalid: AmbiguousAuthority("git+https:***@33d1d5b150363ff64e13418814d3de2e067b565a")
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

thread 'main' (317539) panicked at /home/runner/work/uv/uv/crates/uv/src/lib.rs:2540:10:
Tokio executor failed, was there a panic?: Any { .. }
```

### Minimal reproduction

pyproject.toml
```toml
[project]
name = "uv-ghproxy-bug"
version = "0.1.0"

[tool.uv.sources]
alpaca-eval = {
  git = "https://githubproxy.cc/https://github.com/nelson-liu/alpaca_eval.git",
  branch = "forward_kwargs_to_vllm"
}
```

### Root cause analysis
uv internally constructs a URL similar to:
```rust
git+https://githubproxy.cc/https://github.com/...@branch
```
Because the Git URL itself contains a nested https://, the Rust URL parser misinterprets the authority boundary, causing the @branch suffix to be parsed as userinfo, triggering AmbiguousAuthority.

This is a structured URL parsing issue, not a Git or repository problem.

### Platform

Linux 5.4.0-144-generic #161-Ubuntu SMP  x86_64 GNU/Linux

### Version

uv  0.9.18

### Python version

cpython-3.12.12-linux-x86_64-gnu

---

_Label `bug` added by @b1ankcat on 2025-12-22 08:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-24 14:02_

---

_Closed by @charliermarsh on 2025-12-25 11:42_

---
