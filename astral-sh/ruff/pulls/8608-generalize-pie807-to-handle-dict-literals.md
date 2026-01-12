```yaml
number: 8608
title: Generalize PIE807 to handle dict literals
type: pull_request
state: merged
author: alanhdu
labels:
  - rule
assignees: []
merged: true
base: main
head: alan/container-builtins
created_at: 2023-11-10T19:42:41Z
updated_at: 2023-11-16T02:33:19Z
url: https://github.com/astral-sh/ruff/pull/8608
synced_at: 2026-01-12T15:55:26Z
```

# Generalize PIE807 to handle dict literals

---

_@alanhdu_

## Summary

PIE807 will rewrite `lambda: []` to `list` -- AFAICT though, the same rationale also applies to dicts, so I've modified the code to also rewrite `lambda: {}` to `dict`.

Two things I'm not sure about:
* Should this go to a new rule? This no longer actually matches the behavior of flake8-pie, and while I think thematically it makes sense to be part of the same rule, we could make it a standalone rule (but if so, where should I put it and what error code should I use)?
* If we want a single rule, are there backwards compatibility concerns with the rule name change (from `reimplemented_list_builtin` to `reimplemented_container_builtin`?

## Test Plan

Added snapshot tests of the functionality.

---

_Comment by @github-actions[bot] on 2023-11-10 20:00_

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




---

_Comment by @charliermarsh on 2023-11-10 20:51_

👍 I think it's fine to use a single rule, and even to rename it (I believe our versioning policy allows this), but we should probably gate the change in behavior to preview-only.

---

_Comment by @charliermarsh on 2023-11-10 20:51_

You can grep for `fn preview_rules` for an example of testing preview rules, and to find rules that currently deviate based on the preview flag.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-11-10 20:51_

---

_Label `rule` added by @charliermarsh on 2023-11-10 20:51_

---

_@zanieb reviewed on 2023-11-10 21:01_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:41 on 2023-11-10 21:01_

Maybe just `ReimplementedBuiltin` would suffice?

---

_@zanieb reviewed on 2023-11-10 21:03_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:49 on 2023-11-10 21:03_

You could store the type on the `Violation` then say "Prefer `list` over lambda" or "Prefer `dict` over lambda" depending on the case. It seems confusing to present both to the user when we know which they meant.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:41 on 2023-11-10 22:23_

I think we have some other rules that also detect re-implemented builtins though -- like the simplify rules that detect re-implemented `any` and `all` loops.

---

_@charliermarsh reviewed on 2023-11-10 22:23_

---

_@charliermarsh reviewed on 2023-11-10 22:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:49 on 2023-11-10 22:24_

Agreed! Can grep for `DeferralKeyword` as an example of this pattern.

---

_@zanieb reviewed on 2023-11-10 22:32_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:41 on 2023-11-10 22:32_

Hm yes `ReimplementedBuiltin` is used for `SIM110` and `SIM111` — that's such a generic name for those though...

I think `ReimplementedContainerBuiltin` is fine but separately we should consider changing that other one to be more specific :D


---

_Comment by @Skylion007 on 2023-11-11 14:59_

I remember reviewing a PR for a more general rule about any lambda that didn't pass any args to a function call. Would that be a better rule to enable? I think it was a pylint one.

---

_Comment by @charliermarsh on 2023-11-11 15:04_

I think it's https://github.com/astral-sh/ruff/pull/7953, although I think that rule may not catch this case because it's not aware that (e.g.) `[]` is equivalent to `list`.

---

_Comment by @Skylion007 on 2023-11-11 15:25_

Ah, you are right. PIE807 doesn't actually even catch
```
doc_string_dict = defaultdict(lambda: list())
```
but unnecessary lambda does.

---

_@alanhdu reviewed on 2023-11-13 15:50_

---

_Review comment by @alanhdu on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:46 on 2023-11-13 15:50_

I could make this an `enum` instead, but not sure it's worthwhile compared to just storing the builtin inline.

---

_@charliermarsh approved on 2023-11-13 17:46_

Thanks!

---

_@charliermarsh reviewed on 2023-11-13 17:46_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/reimplemented_container_builtin.rs`:46 on 2023-11-13 17:46_

I decided to make it an enum to follow the pattern we use elsewhere, even though it's really unimportant.

---

_Comment by @charliermarsh on 2023-11-13 17:46_

(The Rotki ecosystem change is expected as they use preview mode by default.)

---

_Merged by @charliermarsh on 2023-11-13 17:55_

---

_Closed by @charliermarsh on 2023-11-13 17:55_

---

_Branch deleted on 2023-11-16 02:33_

---
