```yaml
number: 14893
title: Also have zizmor check for low-severity security issues
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - security
assignees: []
merged: true
base: main
head: more-zizmor
created_at: 2024-12-10T17:30:19Z
updated_at: 2025-02-06T18:59:55Z
url: https://github.com/astral-sh/ruff/pull/14893
synced_at: 2026-01-10T19:57:22Z
```

# Also have zizmor check for low-severity security issues

---

_Pull request opened by @AlexWaygood on 2024-12-10 17:30_

## Summary

This PR changes our zizmor configuration to also flag low-severity security issues in our GitHub Actions workflows. It's a followup to https://github.com/astral-sh/ruff/pull/14844. The issues being fixed here were all flagged by [zizmor's `template-injection` rule](https://woodruffw.github.io/zizmor/audits/#template-injection):

> Detects potential sources of code injection via template expansion.
>
> GitHub Actions allows workflows to define template expansions, which occur within special `${{ ... }}` delimiters. These expansions happen before workflow and job execution, meaning the expansion of a given expression appears verbatim in whatever context it was performed in.
>
> Template expansions aren't syntax-aware, meaning that they can result in unintended shell injection vectors. This is especially true when they're used with attacker-controllable expression contexts, such as `github.event.issue.title` (which the attacker can fully control by supplying a new issue title).

[...]

> To fully remediate the vulnerability, you should not use `${{ env.VARNAME }}`, since that is still a template expansion. Instead, you should use `${VARNAME}` to ensure that the shell itself performs the variable expansion.

## Test Plan

I tested that this passes all zizmore warnings by running `pre-commit run -a zizmor` locally. The other test is obviously to check that the workflows all still run correctly in CI 😄


---

_Label `ci` added by @AlexWaygood on 2024-12-10 17:30_

---

_Label `security` added by @AlexWaygood on 2024-12-10 17:30_

---

_Marked ready for review by @AlexWaygood on 2024-12-10 18:42_

---

_@dhruvmanila approved on 2024-12-12 07:27_

Looks good

---

_Merged by @AlexWaygood on 2024-12-12 07:43_

---

_Closed by @AlexWaygood on 2024-12-12 07:43_

---

_Branch deleted on 2025-02-06 18:59_

---
