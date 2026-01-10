```yaml
number: 9929
title: Fixed nextest install line in CONTRIBUTING.md
type: pull_request
state: merged
author: owenlamont
labels:
  - documentation
assignees: []
merged: true
base: main
head: update_nextest_contrib
created_at: 2024-02-11T12:12:51Z
updated_at: 2024-02-11T15:22:18Z
url: https://github.com/astral-sh/ruff/pull/9929
synced_at: 2026-01-10T22:57:09Z
```

# Fixed nextest install line in CONTRIBUTING.md

---

_Pull request opened by @owenlamont on 2024-02-11 12:12_

## Summary

I noticed the example line in CONTRIBUTING.md:

```shell
cargo install nextest
```

Didn't appear to install the intended package cargo-nextest.

![nextest](https://github.com/astral-sh/ruff/assets/12672027/7bbdd9c3-c35a-464a-b586-3e9f777f8373)

So I checked what it [should be](https://nexte.st/book/installing-from-source.html) and replaced the line:

```shell
cargo install cargo-nextest --locked
```

## Test Plan

Just checked the cargo install appeared to give sane looking results


---

_@charliermarsh approved on 2024-02-11 15:18_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-02-11 15:18_

---

_Merged by @charliermarsh on 2024-02-11 15:22_

---

_Closed by @charliermarsh on 2024-02-11 15:22_

---
