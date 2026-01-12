```yaml
number: 7932
title: "Fix handling of `Applicability::Display` fixes when generating summary messages"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/fix-display-only-applicability
created_at: 2023-10-12T22:30:28Z
updated_at: 2023-10-13T01:33:33Z
url: https://github.com/astral-sh/ruff/pull/7932
synced_at: 2026-01-12T15:55:25Z
```

# Fix handling of `Applicability::Display` fixes when generating summary messages

---

_@zanieb_

We were including `Display` fixes in the summary counts for unapplicable fixes resulting in incorrect prompts to the user that a fix could be enabled.

## Example

```python
def add_to_list(item, some_list=[]):
    ...
```

Before

```
❯ ruff check example.py --no-cache --select B
example.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
1 hidden fix can be enabled with the `--unsafe-fixes` option.
❯ ruff check example.py --no-cache --select B --unsafe-fixes
example.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
[*] 0 fixable with the --fix option.
❯ ruff check example.py --no-cache --select B --unsafe-fixes --diff
No errors would be fixed (1 fix available with `--unsafe-fixes`).
❯ ruff check example.py --no-cache --select B --unsafe-fixes --fix
example.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
[*] 0 fixable with the --fix option.
```

After

```
❯ ruff check example.py --no-cache --select B --unsafe-fixes
example.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
❯ ruff check example.py --no-cache --select B
example.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
❯ ruff check example.py --no-cache --select B --unsafe-fixes --diff
❯ ruff check example.py --no-cache --select B --unsafe-fixes --fix
example.py:1:33: B006 Do not use mutable data structures for argument defaults
Found 1 error.
```

---

_Comment by @github-actions[bot] on 2023-10-12 22:46_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

<pre>
+ [*] 16066 fixable with the `--fix` option (6165 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 16066 fixable with the `--fix` option (6343 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
+ [*] 17800 fixable with the `--fix` option (4369 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 17800 fixable with the `--fix` option (4727 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>latch (+1, -1)</summary>
<p>

<pre>
+ [*] 205 fixable with the `--fix` option (33 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 205 fixable with the `--fix` option (38 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>pymilvus (+1, -1)</summary>
<p>

<pre>
- [*] 233 fixable with the `--fix` option (132 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 233 fixable with the `--fix` option (98 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>rotki (+1, -1)</summary>
<p>

<pre>
- [*] 11 fixable with the `--fix` option (20 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 11 fixable with the `--fix` option (8 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>zulip (+1, -1)</summary>
<p>

<pre>
+ [*] 7882 fixable with the `--fix` option (12139 hidden fixes can be enabled with the `--unsafe-fixes` option).
- [*] 7882 fixable with the `--fix` option (12629 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>



---

_@charliermarsh approved on 2023-10-13 00:21_

---

_Merged by @zanieb on 2023-10-13 01:33_

---

_Closed by @zanieb on 2023-10-13 01:33_

---

_Branch deleted on 2023-10-13 01:33_

---
