```yaml
number: 12020
title: "Redirect `F509` to `PLE1300`"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - rule
assignees: []
base: ruff-0.5
head: dhruv/redirect-F509-to-PLE1300
created_at: 2024-06-25T04:44:00Z
updated_at: 2024-06-25T16:05:30Z
url: https://github.com/astral-sh/ruff/pull/12020
synced_at: 2026-01-10T21:56:00Z
```

# Redirect `F509` to `PLE1300`

---

_Pull request opened by @dhruvmanila on 2024-06-25 04:44_

## Summary

This PR removes the `F509` rule and redirects it to `PLE1300`

For context, `PLE1300` looks at _both_ `%`-style strings and `.format` strings while `F509` only looks at `%` style strings. Basically, `F509` is a subset of `PLE1300` which means we should remove `F509` instead.

This PR also updates the documentation for `PLE1300` with "Use instead" and "References" section.

resolves: #11403 

## Documentation Preview

### `F509`
<img width="1387" alt="Screenshot 2024-06-25 at 10 13 13" src="https://github.com/astral-sh/ruff/assets/67177269/3662c526-abd1-4572-b1f5-58f4bd37c18b">

### `PLE1300`
<img width="1362" alt="Screenshot 2024-06-25 at 10 14 17" src="https://github.com/astral-sh/ruff/assets/67177269/24d80553-880a-477d-9677-a2cc454e79f7">


## Test Plan

Remove the test and snapshot for `F509`.




---

_Label `rule` added by @dhruvmanila on 2024-06-25 05:51_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-06-25 06:17_

---

_Comment by @github-actions[bot] on 2024-06-25 06:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-25 08:35_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-25 09:36_

---

_Comment by @charliermarsh on 2024-06-25 10:48_

I think this would be problematic because the F rule is part of our defaults. We’d be removing this rule from the default list.

---

_Comment by @dhruvmanila on 2024-06-25 13:45_

> I think this would be problematic because the F rule is part of our defaults. We’d be removing this rule from the default list.

Yeah, good point. I think the other options to keep the `F509` rule is to either:
1. Remove the `%`-style format string support in `PLE1300` but keep the rule itself
2. Remove `PLE1300` completely, move support for `str.format` in `F509`

---

_Comment by @dhruvmanila on 2024-06-25 13:58_

I'm not sure if either of the options are good. This was raised by me but I don't think I realized back then that `PLE1300` also supported `str.format` strings. I think we can avoid doing this now and pick this up post rule re-categorization where these rules can be merged.

---

_Comment by @charliermarsh on 2024-06-25 15:05_

I'm fine with punting it to the rule recategorization effort.

---

_Closed by @dhruvmanila on 2024-06-25 16:02_

---

_Removed from milestone `v0.5.0` by @dhruvmanila on 2024-06-25 16:05_

---
