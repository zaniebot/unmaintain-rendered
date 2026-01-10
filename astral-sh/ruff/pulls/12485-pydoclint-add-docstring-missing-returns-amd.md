```yaml
number: 12485
title: "[`pydoclint`] Add `docstring-missing-returns` amd `docstring-extraneous-returns` (`DOC201`, `DOC202`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - docstring
  - preview
assignees: []
merged: true
base: main
head: doc2xx
created_at: 2024-07-24T04:43:55Z
updated_at: 2024-08-02T17:08:00Z
url: https://github.com/astral-sh/ruff/pull/12485
synced_at: 2026-01-10T21:47:02Z
```

# [`pydoclint`] Add `docstring-missing-returns` amd `docstring-extraneous-returns` (`DOC201`, `DOC202`)

---

_Pull request opened by @augustelalande on 2024-07-24 04:43_

## Summary

Add `docstring-missing-returns` amd `docstring-extraneous-returns` (`DOC201`, `DOC202`). These rules check that the returns defined (or not) in the docstring of a function match its implementation.

Part of #12434.

## Test Plan

Test cases added.

---

_Comment by @codspeed-hq[bot] on 2024-07-24 04:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:doc2xx)

### Merging #12485 will **not alter performance**

<sub>Comparing <code>augustelalande:doc2xx</code> (456db23) with <code>augustelalande:doc2xx</code> (16e30f2)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Label `rule` added by @MichaReiser on 2024-07-24 13:23_

---

_Label `docstring` added by @MichaReiser on 2024-07-24 13:23_

---

_Label `preview` added by @MichaReiser on 2024-07-24 13:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:18 on 2024-07-24 13:24_

I'm not entirely sure how to phrase it but should the documentation mention that it only applies to functions that have a body with a return statement?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:405 on 2024-07-24 13:26_

This is not specific to your change but won't this visitor traverse into nested functions?

```python
def test():
	def nested():
		return 5

	print("I never return")
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:500 on 2024-07-24 13:28_

Does emitting a diagnostic for each return statement match the behavior of the upstream rule? It feels a bit verbose to me. What do you think, would it be enough to just flag the first return statement?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs`:100 on 2024-07-24 13:30_

As a user, I think it wouldn't be entirely clear to me **why** the docstring doesn't need to contain a return statement. Could we add some explanation around *because the function contains no return statement*.

---

_@MichaReiser approved on 2024-07-24 13:31_

Thanks. This overall looks good to me. I think there's a pre-existing issue around nested functions. I'm okay with merging the PR as is and doing a follow up pr to fix it or fix it in this pr.

---

_Comment by @augustelalande on 2024-07-26 04:06_

All done

---

_Merged by @MichaReiser on 2024-07-26 06:36_

---

_Closed by @MichaReiser on 2024-07-26 06:36_

---

_Branch deleted on 2024-07-26 15:04_

---
