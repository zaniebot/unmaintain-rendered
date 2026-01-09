---
number: 17080
title: Add value hints to command line arguments to improve shell completion accuracy
type: pull_request
state: open
author: EliteTK
labels:
  - enhancement
assignees: []
base: main
head: tk/completion-improvements
created_at: 2025-12-10T23:11:38Z
updated_at: 2025-12-11T09:32:33Z
url: https://github.com/astral-sh/uv/pull/17080
synced_at: 2026-01-07T13:12:19-06:00
---

# Add value hints to command line arguments to improve shell completion accuracy

---

_Pull request opened by @EliteTK on 2025-12-10 23:11_

## Summary

This partially addresses #17076 by adding `value_hint` to various arguments.

For cases where an option takes a path to either specifically a file or a directory directory, `ValueHint::FilePath` and `ValueHint::DirPath` are used respectively to try to limit the amount of noise presented by completions in shells which support it.

For cases where a URL (and only a URL, not a path) can be supplied, `ValueHint::Url` is used.

For cases where a python interpreter is to be specified, `ValueHint::CommandName` is used which will tab complete from `$PATH` by default, but will fall back to completing executable filenames if you start typing a path.

Finally, for the many cases where there is no built in completion which would make sense, and where default completion of a path would make no sense (e.g. a package name, or version specifier, or date) `ValueHint::Other` is used to explicitly disable completion.

## Test Plan

Manually tested a bunch of these. These _could_ be automated in the sense that we could snapshot the completion from zsh but I've not thought about how that could be done yet.

---

_Label `enhancement` added by @konstin on 2025-12-11 09:32_

---
