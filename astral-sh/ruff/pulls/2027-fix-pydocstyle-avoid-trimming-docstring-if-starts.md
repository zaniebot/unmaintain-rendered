```yaml
number: 2027
title: "fix(pydocstyle): Avoid trimming docstring if starts with leading quote"
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: d200/leadind-quote
created_at: 2023-01-20T13:00:24Z
updated_at: 2023-01-20T14:57:49Z
url: https://github.com/astral-sh/ruff/pull/2027
synced_at: 2026-01-12T04:52:00Z
```

# fix(pydocstyle): Avoid trimming docstring if starts with leading quote

---

_Pull request opened by @spaceone on 2023-01-20 13:00_

Fixes: #2017

looks like the other way round is also possible to break:

```""" "foo"""`

---

_Comment by @spaceone on 2023-01-20 13:00_

I don't know how to update the signature files, yet.

---

_Comment by @charliermarsh on 2023-01-20 13:10_

Ah great! You can run `cargo test --lib` and then `cargo insta review` (after `cargo install cargo-insta`). If you don't have those tools setup, I can also help.

---

_Comment by @spaceone on 2023-01-20 13:25_

@charliermarsh thanks, done.

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/no_surrounding_whitespace.rs`:36 on 2023-01-20 14:29_

Though it’s not intuitive, I think this should also use last and next. Last will always be the quote char, but next could be a kind prefix (like r, if this is a raw string). Same in the other file.

---

_@charliermarsh reviewed on 2023-01-20 14:29_

---

_@spaceone reviewed on 2023-01-20 14:35_

---

_Review comment by @spaceone on `src/rules/pydocstyle/rules/no_surrounding_whitespace.rs`:36 on 2023-01-20 14:35_

oh right, but why last and next? I would say only last. If next() is `r` it would ignore a string starting with `r`..

---

_@charliermarsh reviewed on 2023-01-20 14:37_

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/no_surrounding_whitespace.rs`:36 on 2023-01-20 14:37_

Sorry, typo — should read “last, not next”.

---

_Review comment by @charliermarsh on `src/rules/pydocstyle/rules/no_surrounding_whitespace.rs`:36 on 2023-01-20 14:38_

As in, we shouldn’t use “next” here, just “last” for both condition checks.

---

_@charliermarsh reviewed on 2023-01-20 14:38_

---

_@spaceone reviewed on 2023-01-20 14:40_

---

_Review comment by @spaceone on `src/rules/pydocstyle/rules/no_surrounding_whitespace.rs`:36 on 2023-01-20 14:40_

done

---

_Merged by @charliermarsh on 2023-01-20 14:57_

---

_Closed by @charliermarsh on 2023-01-20 14:57_

---
