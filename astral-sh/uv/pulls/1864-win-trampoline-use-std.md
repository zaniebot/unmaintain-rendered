```yaml
number: 1864
title: "Win-Trampoline: Use Std"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: std-trampoline
created_at: 2024-02-22T13:10:09Z
updated_at: 2024-02-23T15:30:02Z
url: https://github.com/astral-sh/uv/pull/1864
synced_at: 2026-01-12T16:04:45Z
```

# Win-Trampoline: Use Std

---

_@MichaReiser_

## Summary

This PR isn't intended to be merged. I mainly created it to share how far I got migrating the trampoline code to std. 

You can build the code with `cargo build  --target x86_64-pc-windows-msvc`

## Size 

This version enables the `panic_immediate_abort` in the `.cargo/config.toml` which gives us std without increasing the binary size (16.5KB). However, it means that rust aborts without printing any message whenever it encounters a panic. I don't think that's horrible per se, we can use our own `panic` function (similar to `eprintln`) but it is an unstable feature and requires us being extra careful to always use our custom panic handler (don't use `.unwrap` or `expect`)

The binary size increases to `~90KB` when removing said feature because rust pulls in the formatting code of (all?) std types.

## Open Questions

I don't understand why this needs the std-feature `"compiler-builtins-mem"` but linking fails without it.



## Test Plan

<!-- How was it tested? -->


---

_Comment by @MichaReiser on 2024-02-23 15:29_

Closing, because this is not an immediate priority. I opened https://github.com/astral-sh/uv/issues/1917 to track it.

---

_Closed by @MichaReiser on 2024-02-23 15:29_

---
