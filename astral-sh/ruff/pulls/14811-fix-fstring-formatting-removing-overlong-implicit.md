```yaml
number: 14811
title: Fix fstring formatting removing overlong implicit concatenated string in expression part
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/fix-overlong-single-line-fstring-implicit-concatenated-string
created_at: 2024-12-06T10:27:36Z
updated_at: 2024-12-06T12:01:07Z
url: https://github.com/astral-sh/ruff/pull/14811
synced_at: 2026-01-12T15:55:49Z
```

# Fix fstring formatting removing overlong implicit concatenated string in expression part

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/14778


The formatter incorrectly removed the inner implicitly concatenated string for following single-line f-string:

```py
f"{'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa' 'a' if True else ""}"

# formatted
f"{ if True else ''}"
```

This happened because I changed the `RemoveSoftlinesBuffer` in https://github.com/astral-sh/ruff/pull/14489 to remove any content wrapped in `if_group_breaks`. After all,  it emulates an *all flat* layout. This works fine when `if_group_breaks` is only used to **add** content if the gorup breaks. It doesn't work if the same content is rendered differently depending on if the group fits using `if_group_breaks` and `if_groups_fits` because the enclosing `group` might still *break* if the entire content exceeds the line-length limit. 

This PR fixes this by unwrapping any `if_group_fits` content by removing the `if_group_fits` start and end tags. 


## Test Plan

added test


---

_Label `bug` added by @MichaReiser on 2024-12-06 10:27_

---

_Label `formatter` added by @MichaReiser on 2024-12-06 10:27_

---

_Label `preview` added by @MichaReiser on 2024-12-06 10:27_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-12-06 10:27_

---

_Comment by @github-actions[bot] on 2024-12-06 10:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila approved on 2024-12-06 11:56_

Thanks

---

_Merged by @MichaReiser on 2024-12-06 12:01_

---

_Closed by @MichaReiser on 2024-12-06 12:01_

---

_Branch deleted on 2024-12-06 12:01_

---
