```yaml
number: 7030
title: "Match Black's formatting of trailing comments containing NBSP"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: nbsp
created_at: 2023-08-31T17:46:43Z
updated_at: 2023-09-01T12:55:39Z
url: https://github.com/astral-sh/ruff/pull/7030
synced_at: 2026-01-12T02:45:38Z
```

# Match Black's formatting of trailing comments containing NBSP

---

_Pull request opened by @cnpryer on 2023-08-31 17:46_

## Summary

I noticed the following isn't aligned between the formatter and `black`:

Before this change:

`type:`
```py
# Input: \u{a0}\u{a0}type
i = ""  #  type: Add space before two leading NBSP

# Ruff: \u{a0}type
i = ""  #  type: Add space before two leading NBSP

# Black: \u{a0}\u{a0}type
i = ""  #   type: Add space before two leading NBSP
```

other
```py
# Input: #\u{a0}\u{a0}noqa
i = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  #  noqa

# Black: # \u{a0}noqa
i = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  #  noqa

# Ruff: # noqa
i = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  # noqa
```

## Test Plan

Current fixtures

---

_Comment by @codspeed-hq[bot] on 2023-08-31 17:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:nbsp)

### Merging #7030 will **degrade performances by 3.38%**

<sub>Comparing <code>cnpryer:nbsp</code> (35071c8) with <code>main</code> (68f605e)</sub>



### Summary

`❌ 1` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cnpryer:nbsp)._

### Benchmarks breakdown

|     | Benchmark | `main` | `cnpryer:nbsp` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 38.2 ms | 39.5 ms | -3.38% |


---

_Marked ready for review by @cnpryer on 2023-09-01 11:03_

---

_@MichaReiser reviewed on 2023-09-01 12:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:488 on 2023-09-01 12:28_

Nit: Github doesn't allows me to add a suggestion for the whole code block, but we can now rewrite this function to not use return

```
if content.starts_with('\u{A0}') {
	if trimmed.trim_start().starts_with("type") {
		...
	} else if trimmed.starts_with(' ') {
		...
	} else {
		...
	}
} else {
	... 
}
```

---

_@cnpryer reviewed on 2023-09-01 12:34_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:488 on 2023-09-01 12:34_

Like this? 1e92b1c01ac8882db0d4d74865cd4c03eb9217df

---

_@MichaReiser approved on 2023-09-01 12:52_

---

_Merged by @MichaReiser on 2023-09-01 12:53_

---

_Closed by @MichaReiser on 2023-09-01 12:53_

---

_Label `formatter` added by @MichaReiser on 2023-09-01 12:53_

---

_Branch deleted on 2023-09-01 12:55_

---
