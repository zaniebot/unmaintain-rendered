```yaml
number: 2009
title: "Change AsRef<str> impl for Rule to kebab-case"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: kebab-case
created_at: 2023-01-20T02:05:40Z
updated_at: 2023-01-20T02:37:11Z
url: https://github.com/astral-sh/ruff/pull/2009
synced_at: 2026-01-12T04:52:00Z
```

# Change AsRef<str> impl for Rule to kebab-case

---

_Pull request opened by @not-my-profile on 2023-01-20 02:05_

As we surface rule names more to users we want
them to be easier to type than PascalCase.

Prior art:

Pylint and ESLint also use kebab-case for their rule names. Clippy uses snake_case but only for syntactical reasons (so that the argument to e.g. #![allow(clippy::some_lint)] can be parsed as a path[1]).

[1]: https://doc.rust-lang.org/reference/paths.html

---

_Comment by @charliermarsh on 2023-01-20 02:07_

Yes!

---

_Comment by @not-my-profile on 2023-01-20 02:12_

My next planned steps are:

1. improving the `--explain` output
2. implementing the many to many mapping for rule codes and rules

---

_Merged by @charliermarsh on 2023-01-20 02:37_

---

_Closed by @charliermarsh on 2023-01-20 02:37_

---
