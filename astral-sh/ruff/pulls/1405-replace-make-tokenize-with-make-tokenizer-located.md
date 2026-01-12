```yaml
number: 1405
title: "Replace `make_tokenize` with `make_tokenizer_located`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: use-make_tokenizer_located
created_at: 2022-12-27T14:36:40Z
updated_at: 2023-04-20T15:40:33Z
url: https://github.com/astral-sh/ruff/pull/1405
synced_at: 2026-01-12T15:55:06Z
```

# Replace `make_tokenize` with `make_tokenizer_located`

---

_@harupy_

This PR replaces `make_tokenize` with `make_tokenizer_located` to simplify the code.

---

_@harupy reviewed on 2022-12-27 14:38_

---

_Review comment by @harupy on `Cargo.toml`:50 on 2022-12-27 14:38_

https://github.com/RustPython/RustPython/pull/4369 fixed the issue where RustPython's lexer always starts from (row=1, col=0).

---

_Merged by @charliermarsh on 2022-12-27 15:07_

---

_Closed by @charliermarsh on 2022-12-27 15:07_

---

_Comment by @charliermarsh on 2022-12-27 15:07_

This is great!

---

_Branch deleted on 2022-12-27 15:08_

---

_Comment by @harupy on 2022-12-27 15:09_

@charliermarsh I found `locate_cmpops` also uses `make_tokenizer`. Feel free to replace it if you think it's necessary.

---

_Comment by @charliermarsh on 2022-12-27 15:23_

I wonder if RustPython could leverage this to fix locations within f-strings.

---

_Comment by @youknowone on 2023-04-20 15:40_

@charliermarsh does f-strings one also resolved? (to check if RustPython side tasks exist)

---
