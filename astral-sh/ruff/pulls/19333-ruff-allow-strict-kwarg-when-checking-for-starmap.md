```yaml
number: 19333
title: "[`ruff`] Allow `strict` kwarg when checking for `starmap-zip` (`RUF058`) in Python 3.14+"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - fixes
  - python314
assignees: []
merged: true
base: main
head: starmarp-py314
created_at: 2025-07-14T16:15:26Z
updated_at: 2025-07-15T12:04:23Z
url: https://github.com/astral-sh/ruff/pull/19333
synced_at: 2026-01-10T17:58:13Z
```

# [`ruff`] Allow `strict` kwarg when checking for `starmap-zip` (`RUF058`) in Python 3.14+

---

_Pull request opened by @dylwil3 on 2025-07-14 16:15_

In Python 3.14 the keyword-argument `strict` was [added to `map`](https://docs.python.org/3.14/library/functions.html#map). This PR adds support for this when replacing a starmap-zip call with map in [starmap-zip (RUF058)](https://docs.astral.sh/ruff/rules/starmap-zip/#starmap-zip-ruf058).

Progress towards #15506 


---

_Label `rule` added by @dylwil3 on 2025-07-14 16:15_

---

_Label `fixes` added by @dylwil3 on 2025-07-14 16:15_

---

_Label `python314` added by @dylwil3 on 2025-07-14 16:15_

---

_Comment by @github-actions[bot] on 2025-07-14 16:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:89 on 2025-07-15 08:52_

Nit: This reduces the branching by moving most logic into a shared branch

```suggestion
    let keywords = &iterable_call.arguments.keywords;
    
    if keywords.len() > 2 {
    	return;
  	}
  	
  	let strict = iterable_call.arguments.find_keyword("strict");
  	
    // Only supported keyword argument is `strict` starting in 3.14
  	if strict.is_some() and checker.target_version() < PythonVersion::PY314 {
  		return;
	}
```

---

_@MichaReiser approved on 2025-07-15 08:53_

---

_@dylwil3 reviewed on 2025-07-15 12:04_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:89 on 2025-07-15 12:04_

I don't think these are equivalent.

I could flatten somewhat but I think the only thing that would be merged would be the case where there is more than one keyword argument supplied.

---

_Merged by @dylwil3 on 2025-07-15 12:04_

---

_Closed by @dylwil3 on 2025-07-15 12:04_

---
