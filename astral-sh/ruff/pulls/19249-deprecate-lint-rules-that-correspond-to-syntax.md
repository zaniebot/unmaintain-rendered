```yaml
number: 19249
title: Deprecate lint rules that correspond to syntax errors 
type: pull_request
state: closed
author: CodeMan62
labels:
  - rule
  - breaking
assignees: []
draft: true
base: main
head: codeman/19122
created_at: 2025-07-10T02:05:49Z
updated_at: 2025-09-05T13:09:10Z
url: https://github.com/astral-sh/ruff/pull/19249
synced_at: 2026-01-12T15:56:35Z
```

# Deprecate lint rules that correspond to syntax errors 

---

_@CodeMan62_

## Summary
partially closes #19122 

## Test Plan
followed the contributing.md

Question:- in this issues we have something around 7-8 rules we have to deprecate so should we add them in this PR or different different PR's ?? 


---

_Review requested from @MichaReiser by @CodeMan62 on 2025-07-10 02:05_

---

_Review requested from @dhruvmanila by @CodeMan62 on 2025-07-10 02:05_

---

_Comment by @github-actions[bot] on 2025-07-10 02:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of deprecated rule `F404` is not allowed when preview is enabled.
```

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of deprecated rule `F404` is not allowed when preview is enabled.
```

</p>
</details>




---

_Label `rule` added by @dhruvmanila on 2025-07-10 06:17_

---

_Label `breaking` added by @dhruvmanila on 2025-07-10 06:17_

---

_Added to milestone `v0.13` by @dhruvmanila on 2025-07-10 06:17_

---

_Review requested from @ntBre by @dhruvmanila on 2025-07-10 06:17_

---

_Comment by @dhruvmanila on 2025-07-10 06:18_

This might not be a breaking change as deprecation can go into patch releases as per our versioning policy.

---

_Comment by @CodeMan62 on 2025-07-10 06:21_

make sense but in this issue there are so many rules we have to deprecate so i think doing them all together can be a breaking change  and can be release in 0.13 as i have questioned too in description

---

_Converted to draft by @dhruvmanila on 2025-07-10 06:22_

---

_Comment by @MichaReiser on 2025-07-10 06:30_

> Question:- in this issues we have something around 7-8 rules we have to deprecate so should we add them in this PR or different different PR's ??

Let's wait with opening PRs for more rules. It only creates PRs that sit for a very long time and then need to be rebased.

---

_Comment by @ntBre on 2025-09-05 13:09_

It feels a bit premature to me to do this, I've moved the corresponding issue to the 0.14 release.

---

_Closed by @ntBre on 2025-09-05 13:09_

---
