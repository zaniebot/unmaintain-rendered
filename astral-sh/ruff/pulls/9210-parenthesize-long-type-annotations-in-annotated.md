```yaml
number: 9210
title: Parenthesize long type annotations in annotated assignments
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: parenthesize-long-type-annotations-in-annotated-assigns
created_at: 2023-12-20T10:29:35Z
updated_at: 2023-12-22T03:33:48Z
url: https://github.com/astral-sh/ruff/pull/9210
synced_at: 2026-01-10T23:07:18Z
```

# Parenthesize long type annotations in annotated assignments

---

_Pull request opened by @MichaReiser on 2023-12-20 10:29_

## Summary

This PR implements https://github.com/astral-sh/ruff/issues/8894 for annotated assignments. It intentionally excludes annotations in function definition because we're internally discussing whether we support the changes or not. 


## Test Plan

Ran the ecosystem check and there are.... no changes! Which is rather disappointing. I checked the diff shades output from the Black changes and verified that ruff formats the **one** shade the same as black. 
I ran the similarity index script and verified that the numbers remain unchanged. 




---

_Label `formatter` added by @MichaReiser on 2023-12-20 10:29_

---

_Label `preview` added by @MichaReiser on 2023-12-20 10:29_

---

_Comment by @github-actions[bot] on 2023-12-20 10:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__long_type_annotations.py.snap`:438 on 2023-12-21 02:00_

I'm not a huge fan of this change but it is consistent to how we would format the same assignment when `Decimal`is the assigned value

Input 
```python
safe_age = Decimal #  the user's age....
```

---

_@MichaReiser reviewed on 2023-12-21 02:00_

---

_Marked ready for review by @MichaReiser on 2023-12-21 02:21_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-21 23:53_

---

_@charliermarsh approved on 2023-12-22 02:15_

Nice, this looks good to me. Thanks for all the clear comments around the intended preview behavior. I wonder if we'd see more ecosystem changes by including the function parameter annotations... maybe?

---

_Comment by @MichaReiser on 2023-12-22 03:33_

> Nice, this looks good to me. Thanks for all the clear comments around the intended preview behavior. I wonder if we'd see more ecosystem changes by including the function parameter annotations... maybe?

Yeah, there are more changes if you include the function parameters. It's not a ton but a couple of 100 changed lines.

---

_Merged by @MichaReiser on 2023-12-22 03:33_

---

_Closed by @MichaReiser on 2023-12-22 03:33_

---

_Branch deleted on 2023-12-22 03:33_

---
