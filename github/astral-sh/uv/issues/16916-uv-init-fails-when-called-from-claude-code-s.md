---
number: 16916
title: "`uv init` fails when called from Claude Code's sandbox on macOS"
type: issue
state: open
author: buth
labels:
  - bug
  - external
assignees: []
created_at: 2025-12-01T21:43:14Z
updated_at: 2025-12-01T22:06:37Z
url: https://github.com/astral-sh/uv/issues/16916
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv init` fails when called from Claude Code's sandbox on macOS

---

_Issue opened by @buth on 2025-12-01 21:43_

### Summary

The `uv init` panics when run within [Claude Code's](https://www.claude.com/product/claude-code) [sandbox](https://code.claude.com/docs/en/sandboxing). I'm guessing this is a similar problem to #16664.

```

╭─── Claude Code v2.0.55 ───────────────────────────────────────────────────────────────────╮
│                                    │ Tips for getting started                             │
│           Welcome back!            │ Ask Claude to create a new app or clone a repository │
│                                    │ ──────────────────────────────────────────────────── │
│              ▗ ▗   ▖ ▖             │ Recent activity                                      │
│                                    │ No recent activity                                   │
│                ▘▘ ▝▝               │                                                      │
│                                    │                                                      │
│   Sonnet 4.5 · API Usage Billing   │                                                      │
│        /private/tmp/claude        │                                                      │
╰───────────────────────────────────────────────────────────────────────────────────────────╯

  Your bash commands will be sandboxed. Disable with /sandbox.

> can you create a new python cli project using uv? 

⏺ I'll help you create a new Python CLI project using uv. Let me start by checking if uv is installed and then create the
  project.

⏺ Bash(uv --version)
  ⎿  uv 0.9.13 (Homebrew 2025-11-26)

⏺ Great! uv is installed. Now I'll create a new Python CLI project. Let me initialize a new project:

⏺ Bash(uv init my-cli-app --app)
  ⎿  Error: Exit code 101

     thread 'main2' (35729760) panicked at /Users/brew/Library/Caches/Homebrew/cargo_cache/registry/src/index.crates.io-1949cf8c6b5
     b557f/system-configuration-0.6.1/src/dynamic_store.rs:154:1:
     Attempted to create a NULL object.
     note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

     thread 'main' (35729758) panicked at /private/tmp/uv-20251126-7963-rnw244/uv-0.9.13/crates/uv/src/lib.rs:2483:10:
     Tokio executor failed, was there a panic?: Any { .. }
```

### Platform

Darwin 25.1.0 arm64

### Version

uv 0.9.13 (Homebrew 2025-11-26)

### Python version

Python 3.14.0

---

_Label `bug` added by @buth on 2025-12-01 21:43_

---

_Comment by @zanieb on 2025-12-01 22:05_

This is fixed upstream but unreleased

- https://github.com/mullvad/system-configuration-rs/pull/59
- https://github.com/mullvad/system-configuration-rs/issues/71
- https://github.com/mullvad/system-configuration-rs/issues/72

See also

- https://github.com/openai/codex/issues/1457
- https://github.com/openai/codex/issues/5914

---

_Label `external` added by @zanieb on 2025-12-01 22:06_

---
