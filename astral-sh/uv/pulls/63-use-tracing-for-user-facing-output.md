```yaml
number: 63
title: "Use `tracing` for user-facing output"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/tracing
created_at: 2023-10-08T19:42:45Z
updated_at: 2023-10-08T19:46:08Z
url: https://github.com/astral-sh/uv/pull/63
synced_at: 2026-01-12T16:03:43Z
```

# Use `tracing` for user-facing output

---

_@charliermarsh_

The setup is now as follows:

- All user-facing logging goes through `tracing` at an `info` leve. (This excludes messages that go to `stdout`, like the compiled `requirements.txt` file.)
- We have `--quiet` and `--verbose` command-line flags to set the tracing filter and format defaults. So if you use `--verbose`, we include timestamps and targets, and filter at `puffin=debug` level.
- However, we always respect `RUST_LOG`. So you can override the _filter_ via `RUST_LOG`.

For example: the standard setup filters to `puffin=info`, and doesn't show timestamps or targets:

<img width="1235" alt="Screen Shot 2023-10-08 at 3 41 22 PM" src="https://github.com/astral-sh/puffin/assets/1309177/54ca4db6-c66a-439e-bfa3-b86dee136e45">

If you run with `--verbose`, you get debug logging, but confined to our crates:

<img width="1235" alt="Screen Shot 2023-10-08 at 3 41 57 PM" src="https://github.com/astral-sh/puffin/assets/1309177/c5c1af11-7f7a-4038-a173-d9eca4c3630b">

If you want verbose logging with _all_ crates, you can add `RUST_LOG=debug`:

<img width="1235" alt="Screen Shot 2023-10-08 at 3 42 39 PM" src="https://github.com/astral-sh/puffin/assets/1309177/0b5191f4-4db0-4db9-86ba-6f9fa521bcb6">

I think this is a reasonable setup, though we can see how it feels and refine over time.

Closes https://github.com/astral-sh/puffin/issues/57.

---

_Merged by @charliermarsh on 2023-10-08 19:46_

---

_Closed by @charliermarsh on 2023-10-08 19:46_

---

_Branch deleted on 2023-10-08 19:46_

---
