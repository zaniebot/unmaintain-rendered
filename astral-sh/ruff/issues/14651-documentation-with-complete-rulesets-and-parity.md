```yaml
number: 14651
title: Documentation with complete rulesets and parity information
type: issue
state: closed
author: kaddkaka
labels:
  - documentation
assignees: []
created_at: 2024-11-28T06:26:02Z
updated_at: 2024-11-28T08:50:04Z
url: https://github.com/astral-sh/ruff/issues/14651
synced_at: 2026-01-10T11:09:56Z
```

# Documentation with complete rulesets and parity information

---

_Issue opened by @kaddkaka on 2024-11-28 06:26_

I would like to propose that the rule documentation or rule page (or a new subpage?) lists all existing rules from the different tools (flake8, pycodestyle, pylint, etc...) and which ones currently are and are not implemented in ruff.

This would make it easier for developers to understand if they can stop using some tool and adopt ruff instead. 

Related issues:
- https://github.com/astral-sh/ruff/issues/2322
- https://github.com/astral-sh/ruff/issues/1593
  - This issue also asks for this information but was closed without much discussion as at that time there was a section into the README called "How does ruff compare to flake8".

The list could include:
- all rules of each set and which rules are implemented: 
  - in ruff (stable default)
  - in ruff (stable opt-in)
  - in ruff (preview)
  - in ruff (not yet)
  - in ruff (not planned/low priority because of reason X)
  - what version of ruff the rule was implemented in 

---

_Comment by @kaddkaka on 2024-11-28 06:32_

Existing issues I could find that tries to reach parity and implement remaining rules of a specific ruleset:
- #970 
- #2402

---

_Comment by @MichaReiser on 2024-11-28 06:36_

Thanks. Something very similar has come up before in https://github.com/astral-sh/ruff/issues/13897 Please see my response in that issue (and feel free to continue the conversation over there)

---

_Closed by @MichaReiser on 2024-11-28 06:36_

---

_Label `documentation` added by @AlexWaygood on 2024-11-28 08:50_

---
