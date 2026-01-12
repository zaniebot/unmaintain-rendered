```yaml
number: 21395
title: "[`flake8-simplify`] Apply `SIM113` when index variable is of type `int`"
type: pull_request
state: merged
author: njhearp
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: SIM113-apply-all-int
created_at: 2025-11-12T03:57:39Z
updated_at: 2025-11-12T17:54:39Z
url: https://github.com/astral-sh/ruff/pull/21395
synced_at: 2026-01-12T15:57:23Z
```

# [`flake8-simplify`] Apply `SIM113` when index variable is of type `int`

---

_@njhearp_

## Summary

Fixes #21393

Now the rule checks if the index variable is initialized as an `int` type rather than only flagging if the index variable is initialized to `0`. I used `ResolvedPythonType` to check if the index variable is an `int` type.

## Test Plan

Updated snapshot test for `SIM113`.

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 04:08_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d3afc02b022c19c4a85984d758e28a9f0dfa1fea/rotkehlchen/tests/unit/decoders/test_dripsv1.py#L79'>rotkehlchen/tests/unit/decoders/test_dripsv1.py:79:9:</a> SIM113 Use `enumerate()` for index variable `idx` in `for` loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/62f4fc501cb5ecf1a9e99d2bb74ce60ba219b059/zerver/lib/export.py#L1962'>zerver/lib/export.py:1962:9:</a> SIM113 Use `enumerate()` for index variable `dump_file_id` in `for` loop
+ <a href='https://github.com/zulip/zulip/blob/62f4fc501cb5ecf1a9e99d2bb74ce60ba219b059/zerver/lib/export.py#L2892'>zerver/lib/export.py:2892:9:</a> SIM113 Use `enumerate()` for index variable `dump_file_id` in `for` loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM113 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>





---

_@ntBre reviewed on 2025-11-12 14:07_

Thank you! 

Would you mind making this a [`preview`](https://github.com/astral-sh/ruff/blob/c0fb235a703684bca1cc1767015bb7e19ab1c22f/crates/ruff_linter/src/preview.rs#L16-L19) change? It seems to be in the spirit of the rule and also matches the upstream linter for this test case, but I see there are some ecosystem hits.

Preview will also be nice in case we want to iterate further by inferring the type of the argument, for example with [`is_int`](https://github.com/astral-sh/ruff/blob/19326a7b5e953041cee0ed184ff991b21d2df359/crates/ruff_python_semantic/src/analyze/typing.rs#L1056). I think `ResolvedPythonType` only handles literals and simple operations on them like `1 + 2` rather than using type annotations. (I'm not saying we need to or should do that, just an idea)

---

_Comment by @dscorbett on 2025-11-12 14:41_

SIM113’s recommendation is only safe when the index variable’s initial value is a strict instance of `int`, whereas something annotated `int` could be an instance of e.g. `bool` or a custom subclass, which would make the recommendation unsafe because the program’s behavior could change.

---

_Comment by @ntBre on 2025-11-12 15:01_

Ah, thanks for the clarification! Disregard my last paragraph then, but I'd still lean toward making it a preview change.

---

_Comment by @njhearp on 2025-11-12 16:14_

Thanks for the feedback! I updated the rule to be gated behind preview for strict instances of `int`. I also updated the documentation and comments. It looks like some actions failed due to timeouts, so I suspect some GitHub services are temporarily down and they will pass later.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/enumerate_for_loop.rs`:16 on 2025-11-12 17:48_

```suggestion
/// In [preview], this rule checks for index variables initialized with any integer rather than only
/// a literal zero.
```

Having to say this is making me rethink the preview suggestion 😅 But I guess it's better to be safe. Thanks for updating the docs!

---

_@ntBre approved on 2025-11-12 17:49_

Thank you!

---

_Label `rule` added by @ntBre on 2025-11-12 17:50_

---

_Label `preview` added by @ntBre on 2025-11-12 17:50_

---

_Renamed from "[`flake8-simplify`] Apply `SIM113` when index variable is type `int`" to "[`flake8-simplify`] Apply `SIM113` when index variable is of type `int`" by @ntBre on 2025-11-12 17:50_

---

_Merged by @ntBre on 2025-11-12 17:54_

---

_Closed by @ntBre on 2025-11-12 17:54_

---
