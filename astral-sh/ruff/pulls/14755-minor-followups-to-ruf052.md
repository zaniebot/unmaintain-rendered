```yaml
number: 14755
title: Minor followups to RUF052
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/ruf052-nits
created_at: 2024-12-03T12:24:30Z
updated_at: 2025-08-03T11:16:02Z
url: https://github.com/astral-sh/ruff/pull/14755
synced_at: 2026-01-12T15:55:48Z
```

# Minor followups to RUF052

---

_@AlexWaygood_

## Summary

Just some minor followups to the recently merged RUF052 rule, that was added in bf0fd04:
- Some small tweaks to the docs
- A minor code-style nit
- Some more tests for my peace of mind, just to check that the new methods on the semantic model are working correctly

I'm adding the "internal" label as this doesn't deserve a changelog entry. RUF052 is a new rule that hasn't been released yet.

## Test Plan

`cargo test -p ruff_linter`


---

_Label `internal` added by @AlexWaygood on 2024-12-03 12:24_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-03 12:24_

---

_Comment by @github-actions[bot] on 2024-12-03 12:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-12-03 13:32_

---

_Merged by @AlexWaygood on 2024-12-03 13:33_

---

_Closed by @AlexWaygood on 2024-12-03 13:33_

---

_Branch deleted on 2024-12-03 13:33_

---

_Comment by @alessio-locatelli on 2025-08-03 11:16_

@AlexWaygood @MichaReiser Searching https://github.com/search?q=repo%3Aastral-sh%2Fruff+RUF052&type=issues does not reveal previous discussions on detecting used dummy variables inside FOR loops and list comprehensions. To avoid extra noise from rushing into opening a new issue, I would like to clarify:  Was this rejected somewhere, or was this just not discussed and not implemented yet?

Example:

```py
def no_ruff_warnings():
    my_list = [{"foo": 1}, {"foo": 2}]

    i = 0
    for _ in my_list:
        i += _["foo"]  # A dummy variable is used, but no warnings. 

    for _item in my_list:
        i += _item["foo"]  # A dummy variable is used, but no warnings. 

    [_["foo"] for _ in my_list]  # A dummy variable is used, but no warnings. 

    [_item["foo"] for _item in my_list]  # A dummy variable is used, but no warnings. 
```

---
