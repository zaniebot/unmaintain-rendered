```yaml
number: 8659
title: "Treat `class C: ...` and `class C(): ...` equivalently"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comparable
created_at: 2023-11-13T17:57:12Z
updated_at: 2023-11-13T18:25:19Z
url: https://github.com/astral-sh/ruff/pull/8659
synced_at: 2026-01-12T15:55:26Z
```

# Treat `class C: ...` and `class C(): ...` equivalently

---

_@charliermarsh_

## Summary

These should be seen as identical from the `ComparableAst` perspective.

---

_Label `internal` added by @charliermarsh on 2023-11-13 17:57_

---

_Merged by @charliermarsh on 2023-11-13 18:03_

---

_Closed by @charliermarsh on 2023-11-13 18:03_

---

_Branch deleted on 2023-11-13 18:03_

---

_Comment by @github-actions[bot] on 2023-11-13 18:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/07622daa4ef0fde41b527d27eed95ef16028f8f5/rotkehlchen/tests/api/test_balances.py#L953'>rotkehlchen/tests/api/test_balances.py:953:135:</a> PIE807 [*] Prefer `dict` over useless lambda
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE807 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/79b3c261d5ff72d9a367a01772b078523ea8f472/Packs/RubrikPolaris/Integrations/RubrikPolaris/RubrikPolaris_test.py#L61'>Packs/RubrikPolaris/Integrations/RubrikPolaris/RubrikPolaris_test.py:61:45:</a> PIE807 [*] Prefer `dict` over useless lambda
+ <a href='https://github.com/demisto/content/blob/79b3c261d5ff72d9a367a01772b078523ea8f472/Packs/VMware/Integrations/VMware/VMware_test.py#L593'>Packs/VMware/Integrations/VMware/VMware_test.py:593:48:</a> PIE807 [*] Prefer `dict` over useless lambda
+ <a href='https://github.com/demisto/content/blob/79b3c261d5ff72d9a367a01772b078523ea8f472/Packs/VMware/Integrations/VMware/VMware_test.py#L649'>Packs/VMware/Integrations/VMware/VMware_test.py:649:48:</a> PIE807 [*] Prefer `dict` over useless lambda
+ <a href='https://github.com/demisto/content/blob/79b3c261d5ff72d9a367a01772b078523ea8f472/Packs/VMware/Integrations/VMware/VMware_test.py#L677'>Packs/VMware/Integrations/VMware/VMware_test.py:677:48:</a> PIE807 [*] Prefer `dict` over useless lambda
+ <a href='https://github.com/demisto/content/blob/79b3c261d5ff72d9a367a01772b078523ea8f472/Packs/VMware/Integrations/VMware/VMware_test.py#L721'>Packs/VMware/Integrations/VMware/VMware_test.py:721:48:</a> PIE807 [*] Prefer `dict` over useless lambda
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/07622daa4ef0fde41b527d27eed95ef16028f8f5/rotkehlchen/tests/api/test_balances.py#L953'>rotkehlchen/tests/api/test_balances.py:953:135:</a> PIE807 [*] Prefer `dict` over useless lambda
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE807 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2023-11-13 18:25_

Ecosystem changes must be a result of stale upstream.

---
