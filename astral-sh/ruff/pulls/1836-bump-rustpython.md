```yaml
number: 1836
title: Bump RustPython
type: pull_request
state: merged
author: bluetech
labels: []
assignees: []
merged: true
base: main
head: bump-rustpython
created_at: 2023-01-12T22:21:10Z
updated_at: 2023-01-14T13:03:27Z
url: https://github.com/astral-sh/ruff/pull/1836
synced_at: 2026-01-12T15:55:07Z
```

# Bump RustPython

---

_@bluetech_

~~Draft pending merge of https://github.com/RustPython/RustPython/pull/4443, currently pointing at my fork.~~

This bumps RustPython so we can use the new `NonLogicalNewline` token.
A couple of rules needed a fix due to the new token. There might be more that are not caught by tests (anything working with tokens directly with lookaheads), I hope not.

---

_Comment by @charliermarsh on 2023-01-12 22:40_

Cool, I can also do an audit of the sites that rely on tokenization before merging. 

There's also a TODO in `src/flake8_implicit_str_concat/rules.rs`:

```rs
// TODO(charlie): The RustPython tokenization doesn't differentiate between
// continuations and newlines, so we have to detect them manually.
```

Feel free to resolve that here, or I can do it later -- either way!

---

_Marked ready for review by @bluetech on 2023-01-14 12:05_

---

_Comment by @bluetech on 2023-01-14 12:08_

The RustPython fix was merged, so this should be good now.

> Cool, I can also do an audit of the sites that rely on tokenization before merging.

I tried to find places which look at tokens in chunks, I think they are good, but definitely worth an extra look.

> There's also a TODO in src/flake8_implicit_str_concat/rules.rs:

Yes this can be simplified now, done.

---

_Comment by @charliermarsh on 2023-01-14 12:11_

Awesome, thanks! Will merge this today.

---

_Merged by @charliermarsh on 2023-01-14 13:03_

---

_Closed by @charliermarsh on 2023-01-14 13:03_

---
