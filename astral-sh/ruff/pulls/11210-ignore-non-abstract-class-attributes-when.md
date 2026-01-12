```yaml
number: 11210
title: Ignore non-abstract class attributes when enforcing B024
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/b
created_at: 2024-04-30T03:40:08Z
updated_at: 2024-04-30T16:01:09Z
url: https://github.com/astral-sh/ruff/pull/11210
synced_at: 2026-01-12T15:55:37Z
```

# Ignore non-abstract class attributes when enforcing B024

---

_@charliermarsh_

## Summary

I think the check included here does make sense, but I don't see why we would allow it if a value is provided for the attribute -- since, in that case, isn't it _not_ abstract?

Closes: https://github.com/astral-sh/ruff/issues/11208.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-04-30 03:40_

---

_Label `rule` added by @charliermarsh on 2024-04-30 03:40_

---

_Marked ready for review by @charliermarsh on 2024-04-30 03:40_

---

_Comment by @github-actions[bot] on 2024-04-30 03:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/disnake/app_commands.py#L468'>disnake/app_commands.py:468:7:</a> B024 `ApplicationCommand` is an abstract base class, but it has no abstract methods
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B024 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a1ff945177b7bf9acb3cdcbcf8f813cd7c86f41b/disnake/app_commands.py#L468'>disnake/app_commands.py:468:7:</a> B024 `ApplicationCommand` is an abstract base class, but it has no abstract methods
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B024 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@AlexWaygood approved on 2024-04-30 09:35_

LGTM! I pushed to your branch to fix a couple of micro-nits I had ;)

---

_Merged by @charliermarsh on 2024-04-30 16:01_

---

_Closed by @charliermarsh on 2024-04-30 16:01_

---

_Branch deleted on 2024-04-30 16:01_

---
