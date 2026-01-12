```yaml
number: 2702
title: "[`flake8-simplify`]: combine-if-conditions"
type: pull_request
state: closed
author: colin99d
labels:
  - rule
assignees: []
base: main
head: combine-if-conditions
created_at: 2023-02-10T01:52:19Z
updated_at: 2023-02-12T19:56:15Z
url: https://github.com/astral-sh/ruff/pull/2702
synced_at: 2026-01-12T04:52:00Z
```

# [`flake8-simplify`]: combine-if-conditions

---

_Pull request opened by @colin99d on 2023-02-10 01:52_

Ref: #998 

Still very much a WIP. @charliermarsh, I am thinking I will not add an autofixer on this one, because there are a lot of different ways the if statements could be formatted. I think it would be hard to do well. If you disagree, let me know.

---

_Comment by @charliermarsh on 2023-02-10 01:55_

@colin99d - All good, no objections on my end.

---

_Label `rule` added by @charliermarsh on 2023-02-10 04:09_

---

_Comment by @colin99d on 2023-02-10 23:17_

<img width="1243" alt="Screenshot 2023-02-10 at 6 16 38 PM" src="https://user-images.githubusercontent.com/72827203/218220254-d66983e9-d300-4502-b015-bb0feeb3853b.png">
FYI. Our pre-commit linters are currently fighting over how to format the markdown. Let me know if I should open an issue.

---

_Closed by @colin99d on 2023-02-10 23:52_

---

_Reopened by @colin99d on 2023-02-11 00:02_

---

_Marked ready for review by @colin99d on 2023-02-11 00:02_

---

_@spaceone reviewed on 2023-02-11 18:47_

---

_Review comment by @spaceone on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:545 on 2023-02-11 18:47_

maybe write the reason directly here, if the link becomes unavailable somewhen

---

_@spaceone reviewed on 2023-02-11 18:48_

---

_Review comment by @spaceone on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:113 on 2023-02-11 18:48_

```suggestion
    /// ```python
    /// if x = 1:
    ///     print("Hello")
    /// elif x = 2:
    ///     print("Hello")
    /// ```
    ///
    /// Use instead:
    /// ```python
    /// if x = 1 or x = 2
```

---

_Comment by @charliermarsh on 2023-02-12 05:31_

For some reason I'm having trouble pushing edits... Maybe related to the HEAD being deleted and the PR closed then re-opened. Do you mind closing this and reopening as a completely new PR?

---

_Closed by @colin99d on 2023-02-12 19:56_

---
