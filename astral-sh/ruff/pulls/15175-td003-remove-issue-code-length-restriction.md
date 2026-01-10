```yaml
number: 15175
title: "TD003: remove issue code length restriction"
type: pull_request
state: merged
author: mdbernard
labels:
  - rule
assignees: []
merged: true
base: main
head: TD003-increase-max-issue-code-length
created_at: 2024-12-29T01:04:29Z
updated_at: 2025-01-03T09:42:05Z
url: https://github.com/astral-sh/ruff/pull/15175
synced_at: 2026-01-10T20:42:27Z
```

# TD003: remove issue code length restriction

---

_Pull request opened by @mdbernard on 2024-12-29 01:04_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

My company has issue codes longer than 6 characters, and I imagine we're not the only ones. The current maximum value of 6 characters seems arbitrary (please correct me if I'm missing something), so I chose the value of 12, since that would easily cover every issue code my company uses (and I sincerely doubt there are very many users using codes longer than this).

## Test Plan

I added some additional snapshot checks which cover the new boundary conditions.

---

_Comment by @github-actions[bot] on 2024-12-29 01:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @mdbernard on 2024-12-29 01:13_

---

_Marked ready for review by @mdbernard on 2024-12-29 01:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_todos/rules/todos.rs`:231 on 2024-12-30 08:51_

I'm leaning towards removing any arbitrary length restrictions and instead simply require that the code has a specific format:

```suggestion
        r"^#\s*[A-Z]+\-?\d+$", // issue code - like "TD003"
```

---

_@MichaReiser approved on 2024-12-30 09:03_

Nice, thank you.

Our current implementation matches the Regex of the upstream flake8-todos implementation. I tried to figure out the reason from the upstream blame but without success, because [the blame](https://github.com/orsinium-labs/flake8-todos/blame/master/flake8_todos/_rules.py) doesn't allow me to go past the most recent revision for the regex line). I also checked the [PR adding](https://github.com/astral-sh/ruff/pull/3921) rule to see if there's any discussion around what issue formats to support, but I couldn't find any.

That said, I think it's fine to remove the length restriction. It helps to remove some false positives, like in your case, and I think it's unlikely to reduce true positives, considering that the format is unlikely to be used in a regular comment. I'd prefer to remove it entirely instead of just bumping it to 12. 

We also need to update the rule's documentation because it mentions *up to 6* which would now be outdated. 

---

_Label `rule` added by @MichaReiser on 2024-12-30 09:03_

---

_Renamed from "TD003: increase max issue code length to 12 characters" to "TD003: remove issue code length restriction" by @MichaReiser on 2025-01-03 09:35_

---

_Merged by @MichaReiser on 2025-01-03 09:42_

---

_Closed by @MichaReiser on 2025-01-03 09:42_

---
