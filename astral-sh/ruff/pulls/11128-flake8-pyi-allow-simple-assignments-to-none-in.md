```yaml
number: 11128
title: "[`flake8-pyi`] Allow simple assignments to `None` in enum class scopes (`PYI026`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
assignees: []
merged: true
base: main
head: pyi-enums
created_at: 2024-04-24T13:46:01Z
updated_at: 2024-04-24T14:13:56Z
url: https://github.com/astral-sh/ruff/pull/11128
synced_at: 2026-01-10T22:37:01Z
```

# [`flake8-pyi`] Allow simple assignments to `None` in enum class scopes (`PYI026`)

---

_Pull request opened by @AlexWaygood on 2024-04-24 13:46_

Fixes #11117

---

_Comment by @AlexWaygood on 2024-04-24 13:47_

I also deduplicated the two `is_enum` functions between `flake8-slots` and `flake8-pyi`, since the `flake8-pyi` one was way more complex than it needed to be, and was also missing an enum class that the `flake8-slots` function included.

---

_@charliermarsh approved on 2024-04-24 13:49_

Thanks.

---

_Label `bug` added by @charliermarsh on 2024-04-24 13:49_

---

_Comment by @github-actions[bot] on 2024-04-24 13:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/d0f2be92aba2e007841eec073cb47d7f86722792/stdlib/uuid.pyi#L12'>stdlib/uuid.pyi:12:5:</a> PYI042 Type alias `unknown` should be CamelCase
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI042 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-04-24 13:59_

> ```
> - [stdlib/uuid.pyi:12:5:](https://github.com/python/typeshed/blob/d0f2be92aba2e007841eec073cb47d7f86722792/stdlib/uuid.pyi#L12) PYI042 Type alias `unknown` should be CamelCase
> ```

hmmm, seems like a good change but not the one I expected lol

---

_Comment by @AlexWaygood on 2024-04-24 14:07_

> hmmm, seems like a good change but not the one I expected lol

Well, I confirmed locally that this fixes the false positive I was trying to fix, so I'll just add another test for the other fixed bug that the ecosystem check highlighted

---

_Comment by @AlexWaygood on 2024-04-24 14:13_

And, I can't repro the change the ecosystem _is_ reporting locally either. Weird. Anyway, I'm pretty confident in this change, so I'll merge...

---

_Merged by @AlexWaygood on 2024-04-24 14:13_

---

_Closed by @AlexWaygood on 2024-04-24 14:13_

---

_Branch deleted on 2024-04-24 14:13_

---
