```yaml
number: 1643
title: "feat: Allow uv pip install to fetch requirements files from a remote URL."
type: pull_request
state: closed
author: ottaviohartman
labels: []
assignees: []
base: main
head: thartman/url-requirements-2
created_at: 2024-02-18T13:43:04Z
updated_at: 2024-03-06T13:40:57Z
url: https://github.com/astral-sh/uv/pull/1643
synced_at: 2026-01-10T14:54:43Z
```

# feat: Allow uv pip install to fetch requirements files from a remote URL.

---

_Pull request opened by @ottaviohartman on 2024-02-18 13:43_

## Summary

Fixes #1481 

Allow `uv pip install` to fetch requirements files from a remote URL. 

## Questions

Apologies for the questions; this is my first Rust code in a long time.

1. Using `reqwest` like this feels wrong and it doesn't work: `::blocking` complains about dropping the current runtime, and I'm not inclined to change the function and all of its calling functions to `async`. Is there a better way?
2. Is it OK to use `String` type for "something that can be a PathBuf or URL"? Is there a better type to use?

## TODO
- [ ] add tests
  - [ ] mock HTTP calls in tests

## Test Plan

1. Add unit tests.
2. Smoke test in CLI with `uv pip install -r <valid_url/to/requirements.txt>`


---

_@ottaviohartman reviewed on 2024-02-18 13:43_

---

_Review comment by @ottaviohartman on `crates/requirements-txt/src/lib.rs`:396 on 2024-02-18 13:43_

This is the section of code that isn't working - calling`::blocking::get` while a tokio runtime is active

---

_Renamed from "feat: HTTP requirements" to "feat: Allow uv pip install to fetch requirements files from a remote URL." by @ottaviohartman on 2024-02-18 13:59_

---

_Comment by @charliermarsh on 2024-03-06 04:24_

Thank you for the PR -- unfortunately I didn't get to this one, and another contributor came in with an async version that was merged: https://github.com/astral-sh/uv/pull/2081. I really apologize that I wasn't able to get you feedback in a sufficiently prompt manner.

---

_Closed by @charliermarsh on 2024-03-06 04:24_

---

_Comment by @ottaviohartman on 2024-03-06 13:40_

@charliermarsh No problem! I've got a lot to learn with Rust. I'll check out the other PR and see how it should have been done 😅 

---
