```yaml
number: 4695
title: "Add `tool dir` and `toolchain dir` commands"
type: pull_request
state: merged
author: blueraft
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: dir-tool-toolchain
created_at: 2024-07-01T13:48:14Z
updated_at: 2024-07-01T20:43:21Z
url: https://github.com/astral-sh/uv/pull/4695
synced_at: 2026-01-10T13:48:28Z
```

# Add `tool dir` and `toolchain dir` commands

---

_Pull request opened by @blueraft on 2024-07-01 13:48_

## Summary

Resolves #4483 
Resolves #4484 

## Test Plan

`cargo test`

```sh
❯ cargo run -- toolchain dir
warning: `uv toolchain dir` is experimental and may change without warning.
/Users/ahmedilyas/Library/Application Support/uv/toolchains

❯ cargo run -- tool dir
warning: `uv tool dir` is experimental and may change without warning.
/Users/ahmedilyas/Library/Application Support/uv/tools
```

---

_@charliermarsh approved on 2024-07-01 13:49_

Thank you! That's perfect.

---

_Label `cli` added by @charliermarsh on 2024-07-01 13:49_

---

_Label `preview` added by @charliermarsh on 2024-07-01 13:49_

---

_Renamed from "Add tool dir and toolchain dir cmds" to "Add `tool dir` and `toolchain dir` commands" by @charliermarsh on 2024-07-01 13:49_

---

_Comment by @blueraft on 2024-07-01 13:56_

Uh tests failed I'll check 🤔

---

_Comment by @charliermarsh on 2024-07-01 14:13_

It looks like something frustrating with the filters. Maybe try running without `context.filters()` to see what's happening?

---

_Comment by @blueraft on 2024-07-01 14:31_

It's an issue with `path.user_display()` function stripping off the temp dir path, what do you think of using `path.simplified_display()` function here instead? The test passes on both MacOS and Ubuntu then (haven't tested on Windows yet). 

---

_Comment by @charliermarsh on 2024-07-01 14:42_

That seems ok to me... `user_display` just truncates if it's relative, which is rare for these cases (and probably not desired anyway).

---

_Comment by @charliermarsh on 2024-07-01 14:42_

Can you change `cache dir` as well for consistency?

---

_Comment by @blueraft on 2024-07-01 14:44_

Done

---

_Merged by @charliermarsh on 2024-07-01 14:51_

---

_Closed by @charliermarsh on 2024-07-01 14:52_

---

_Branch deleted on 2024-07-01 20:43_

---
