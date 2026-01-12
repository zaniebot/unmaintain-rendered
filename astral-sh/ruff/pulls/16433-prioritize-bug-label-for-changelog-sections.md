```yaml
number: 16433
title: "Prioritize \"bug\" label for changelog sections"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/changelog-label
created_at: 2025-02-28T08:41:37Z
updated_at: 2025-02-28T08:47:27Z
url: https://github.com/astral-sh/ruff/pull/16433
synced_at: 2026-01-12T15:55:55Z
```

# Prioritize "bug" label for changelog sections

---

_@dhruvmanila_

## Summary

This PR updates the ordering of changelog sections to prioritize `bug` label such that any PRs that has that label is categorized in "Bug fixes" section in when generating the changelog irrespective of any other labels present on the PR.

I think this works because I've seen PRs with both `server` and `bug` in the "Server" section instead of the "Bug fixes" section. For example, https://github.com/astral-sh/ruff/pull/16262 in https://github.com/astral-sh/ruff/releases/tag/0.9.7.

On that note, this also changes the ordering such that any PR with both `server` and `bug` labels are in the "Bug fixes" section instead of the "Server" section. This is in line with how "Formatter" is done. I think it makes sense to instead prefix the entries with "Formatter:" and "Server:" if they're bug fixes. But, I'm happy to change this such that any PRs with `formatter` and `server` labels are always in their own section irrespective of other labels.

---

_Label `internal` added by @dhruvmanila on 2025-02-28 08:41_

---

_@dhruvmanila reviewed on 2025-02-28 08:42_

---

_Review comment by @dhruvmanila on `pyproject.toml`:124 on 2025-02-28 08:42_

This change is just to group the "Rule changes" section

---

_@MichaReiser reviewed on 2025-02-28 08:43_

---

_Review comment by @MichaReiser on `pyproject.toml`:116 on 2025-02-28 08:43_

Do I understand this correctly that an issue labeled with both `bug` and `preview` would still be categorized as `preview` (which I think is the correct behavior)


Site note: Sort of funny that we rely on the ordering of elements in a TOML table -- which is not specified.

---

_@MichaReiser approved on 2025-02-28 08:44_

Thanks

---

_@dhruvmanila reviewed on 2025-02-28 08:44_

---

_Review comment by @dhruvmanila on `pyproject.toml`:116 on 2025-02-28 08:44_

Yes, that's correct.

---

_Comment by @dhruvmanila on 2025-02-28 08:47_

(I cancelled the build workflow as the changes are not going to affect them.)

---

_Merged by @dhruvmanila on 2025-02-28 08:47_

---

_Closed by @dhruvmanila on 2025-02-28 08:47_

---

_Branch deleted on 2025-02-28 08:47_

---
