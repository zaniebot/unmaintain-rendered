```yaml
number: 15850
title: "Add deprecation warning for `ruff-lsp` related settings"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/deprecate-editor-setting
created_at: 2025-01-31T13:09:46Z
updated_at: 2025-02-06T14:42:44Z
url: https://github.com/astral-sh/ruff/pull/15850
synced_at: 2026-01-12T15:55:52Z
```

# Add deprecation warning for `ruff-lsp` related settings

---

_@dhruvmanila_

## Summary

This PR updates the documentation to add deprecated warning for `ruff-lsp` specific settings

### Preview

https://github.com/user-attachments/assets/64e11e4b-7178-43ab-be5b-421e7f4689de

## Test Plan

Build the documentation locally and test out the links. Refer to the preview video above.

---

_Label `documentation` added by @dhruvmanila on 2025-01-31 13:09_

---

_Marked ready for review by @dhruvmanila on 2025-02-03 06:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-03 06:42_

---

_Review comment by @MichaReiser on `docs/editors/settings.md`:940 on 2025-02-03 14:38_

What do you think of moving all deprecated settings to their own section? E.g. Deprecated `ruff-lsp` Settings` 



---

_@MichaReiser approved on 2025-02-03 14:38_

Nice! We could consider grouping all deprecated settings in their own setting so that they appear less prominent. 

---

_@dhruvmanila reviewed on 2025-02-04 10:33_

---

_Review comment by @dhruvmanila on `docs/editors/settings.md`:940 on 2025-02-04 10:33_

That's an interesting idea, I quickly tried it although I think I'd prefer the current version as one might miss the deprecation warning if they're looking at the settings that requires scrolling past the warning.

<img width="1535" alt="Screenshot 2025-02-04 at 4 02 30 PM" src="https://github.com/user-attachments/assets/39a01db7-4208-4397-90ac-7f876b17ee71" />


---

_Merged by @dhruvmanila on 2025-02-06 14:42_

---

_Closed by @dhruvmanila on 2025-02-06 14:42_

---

_Branch deleted on 2025-02-06 14:42_

---
