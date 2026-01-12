```yaml
number: 16364
title: Normalize inconsistent markdown headings in docstrings
type: pull_request
state: merged
author: CNSeniorious000
labels:
  - documentation
assignees: []
merged: true
base: main
head: docstring-consistency
created_at: 2025-02-25T10:07:09Z
updated_at: 2025-02-25T10:18:42Z
url: https://github.com/astral-sh/ruff/pull/16364
synced_at: 2026-01-12T15:55:54Z
```

# Normalize inconsistent markdown headings in docstrings

---

_@CNSeniorious000_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I am working on a project that uses ruff linters' docs to generate a fine-tuning dataset for LLMs.

To achieve this, I first ran the command `ruff rule --all --output-format json` to retrieve all the rules. Then, I parsed the explanation field to get these 3 consistent sections:

- `Why is this bad?`
- `What it does`
- `Example`

However, during the initial processing, I noticed that the markdown headings are not that consistent. For instance:

- In most cases, `Use instead` appears as a normal paragraph within the `Example` section, but in the file `crates/ruff_linter/src/rules/flake8_bandit/rules/django_extra.rs` it is a level-2 heading
- The heading "What it does**?**" is used in some places, while others consistently use "What it does"
- There are 831 `Example` headings and 65 `Examples`. But all of them only have one example case

This PR normalized these across all rules.

## Test Plan

CI are passed.

---

_Review requested from @AlexWaygood by @CNSeniorious000 on 2025-02-25 10:07_

---

_@dhruvmanila approved on 2025-02-25 10:12_

Thank you for doing this!

---

_Label `documentation` added by @dhruvmanila on 2025-02-25 10:12_

---

_Merged by @dhruvmanila on 2025-02-25 10:12_

---

_Closed by @dhruvmanila on 2025-02-25 10:12_

---

_Comment by @github-actions[bot] on 2025-02-25 10:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @CNSeniorious000 on 2025-02-25 10:18_

Thank you for the quick review and merge!

---

_Branch deleted on 2025-02-25 10:18_

---
