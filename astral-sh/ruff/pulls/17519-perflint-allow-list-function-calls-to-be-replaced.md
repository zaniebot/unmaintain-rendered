```yaml
number: 17519
title: "[`perflint`] Allow list function calls to be replaced with a comprehension (`PERF401`)"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - rule
assignees: []
merged: true
base: main
head: perf401_call_fix
created_at: 2025-04-21T03:52:19Z
updated_at: 2025-04-21T17:29:24Z
url: https://github.com/astral-sh/ruff/pull/17519
synced_at: 2026-01-12T15:56:02Z
```

# [`perflint`] Allow list function calls to be replaced with a comprehension (`PERF401`)

---

_@w0nder1ng_

This is an implementation of the discussion from #16719. 

This change will allow list function calls to be replaced with comprehensions:

```python
result = list()
for i in range(3):
    result.append(i + 1)
# becomes
result = [i + 1 for i in range(3)]
```

I added a new test to `PERF401.py` to verify that this fix will now work for `list()`. 

---

_Comment by @github-actions[bot] on 2025-04-21 03:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -7 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L350'>tests/integration_tests/charts/api_tests.py:350:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L464'>tests/integration_tests/charts/api_tests.py:464:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/charts/api_tests.py#L515'>tests/integration_tests/charts/api_tests.py:515:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1280'>tests/integration_tests/dashboards/api_tests.py:1280:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1307'>tests/integration_tests/dashboards/api_tests.py:1307:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1441'>tests/integration_tests/dashboards/api_tests.py:1441:13:</a> PERF401 Use a list comprehension to create a transformed list
- <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use `list.extend` to create a transformed list
+ <a href='https://github.com/apache/superset/blob/8c94f9c435f126e3bd30890a61966ee81be0f68f/tests/integration_tests/dashboards/api_tests.py#L1506'>tests/integration_tests/dashboards/api_tests.py:1506:13:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF401 | 14 | 7 | 7 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @dhruvmanila on 2025-04-21 16:49_

---

_Assigned to @ntBre by @ntBre on 2025-04-21 17:12_

---

_@ntBre approved on 2025-04-21 17:28_

Thanks for the follow-up and for the test cases! I don't think this needs to be in preview, as it's just a straightforward extension of the `[]` case to `list()`.

---

_Label `rule` added by @ntBre on 2025-04-21 17:29_

---

_Merged by @ntBre on 2025-04-21 17:29_

---

_Closed by @ntBre on 2025-04-21 17:29_

---
