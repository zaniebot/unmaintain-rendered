```yaml
number: 10484
title: "Add a formatting step using `mdformat` as part of `generate_mkdocs.py`"
type: pull_request
state: merged
author: augustelalande
labels:
  - documentation
assignees: []
merged: true
base: main
head: mdformat-rule-docs
created_at: 2024-03-20T01:30:48Z
updated_at: 2024-03-21T00:48:18Z
url: https://github.com/astral-sh/ruff/pull/10484
synced_at: 2026-01-10T22:47:02Z
```

# Add a formatting step using `mdformat` as part of `generate_mkdocs.py`

---

_Pull request opened by @augustelalande on 2024-03-20 01:30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The purpose of this change is mainly to address one of the issues outlined in #10427. Namely, some lists in the docs were not rendering properly when preceded by a text block without a newline character. This PR adds `mdformat` as a final step to the rule documentation script, so that any missing newlines will be added.

NB: The default behavior of `mdformat` is to escape markdown special characters found in text such as `<`. This resulted in some misformatted docs. To address this I implemented an ad-hoc mdformat plugin to override the behavior. This may be considered a bit 'hacky', but I think it's a good solution. Nevertheless, if someone has a better idea, let me know.

## Test Plan

This change is hard to test systematically, however, I tried my best to look at the before and after diffs to ensure no unwanted changes were made to the docs.


---

_Assigned to @zanieb by @zanieb on 2024-03-20 01:32_

---

_@zanieb reviewed on 2024-03-20 01:38_

---

_Review comment by @zanieb on `scripts/_mdformat_utils.py`:14 on 2024-03-20 01:38_

Could you say a bit more about why this is necessary?

---

_Review comment by @zanieb on `scripts/generate_mkdocs.py`:148 on 2024-03-20 01:39_

Probably ought to be cross-platform complaint here
```suggestion
    for rule_doc in (Path("docs") / "rules").glob("*.md"):
```

---

_@zanieb reviewed on 2024-03-20 01:39_

---

_Label `documentation` added by @zanieb on 2024-03-20 01:39_

---

_@augustelalande reviewed on 2024-03-20 01:40_

---

_Review comment by @augustelalande on `scripts/generate_mkdocs.py`:148 on 2024-03-20 01:40_

pathlib should resolve this fine, and I was merely copying line 115 above

---

_@augustelalande reviewed on 2024-03-20 01:43_

---

_Review comment by @augustelalande on `scripts/generate_mkdocs.py`:148 on 2024-03-20 01:43_

https://github.com/astral-sh/ruff/blob/f7740a8a20e528725daf4fc4f34fa00aec8e4e1e/scripts/generate_mkdocs.py#L111

---

_@zanieb reviewed on 2024-03-20 01:50_

---

_Review comment by @zanieb on `scripts/generate_mkdocs.py`:148 on 2024-03-20 01:50_

Really it automatically parses names with Unix separators into parts on Windows? 🤯 

---

_@zanieb reviewed on 2024-03-20 01:50_

---

_Review comment by @zanieb on `scripts/generate_mkdocs.py`:148 on 2024-03-20 01:50_

We don't generate documentation on win32 anyway so if it's consistent with the rest of the script sgtm.

---

_Comment by @github-actions[bot] on 2024-03-20 02:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-03-21 00:30_

Thanks

---

_Merged by @charliermarsh on 2024-03-21 00:37_

---

_Closed by @charliermarsh on 2024-03-21 00:37_

---

_Branch deleted on 2024-03-21 00:39_

---
