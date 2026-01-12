```yaml
number: 16034
title: Start improving detection of syntax errors
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
draft: true
base: main
head: brent/syntax-errors
created_at: 2025-02-08T00:46:46Z
updated_at: 2025-05-01T15:32:23Z
url: https://github.com/astral-sh/ruff/pull/16034
synced_at: 2026-01-12T15:55:53Z
```

# Start improving detection of syntax errors

---

_@ntBre_

## Summary

This is a first prototype of syntax error detection meant to accompany an internal design document (coming soon!).

This PR introduces the `ruff_python_syntax_errors` crate, which contains a `check_syntax` function for traversing the parsed AST and detecting `SyntaxError`s, which can be converted into `Diagnostic`s in both ruff and red-knot.

Only one new syntax error is currently detected, `MatchBeforePy310`, the use of `match` statements in Python 3.9 or earlier. However, `check_syntax` is wired into ruff and red-knot, so all future syntax errors should only require additions to `ruff_python_syntax_errors`, with a caveat I'll note as a review comment below.

## Test Plan

There are two new, very simple test cases in `ruff_python_syntax_errors` itself, as well as a couple of new diagnostics in existing ruff tests. I think it would be nice to add at least one red-knot test that triggers the new diagnostic before merging.


---

_Label `rule` added by @ntBre on 2025-02-08 00:46_

---

_@ntBre reviewed on 2025-02-08 00:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/syntax/mod.rs`:1 on 2025-02-08 00:56_

I initially tried just implementing `From<SyntaxError> for ruff_diagnostics::Diagnostic` like in red-knot, but this caused problems in [`code_to_rule`](https://github.com/astral-sh/ruff/blob/3a806ecaa1d34e00c35a2ac2b0ed93ed3739fade/crates/ruff_linter/src/codes.rs#L63), which maps code names like `match-before-python-310` back to rule codes. As it stands, every new `SyntaxErrorKind` will also need an entry in the `syntax_errors` macro and an arm in the `match` here.

This macro should pass along the documentation, though, so I think this will integrate well with the way other lint rules are documented, at least.

---

_Comment by @github-actions[bot] on 2025-02-08 01:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L405'>chameleon/viewer/backend/models/chameleon_distributed.py:405:13:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L818'>chameleon/viewer/backend/models/chameleon_distributed.py:818:17:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_local.py#L633'>chameleon/viewer/backend/models/chameleon_local.py:633:13:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L131'>chameleon/viewer/backend/models/service.py:131:21:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L154'>chameleon/viewer/backend/models/service.py:154:17:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L226'>chameleon/viewer/backend/models/service.py:226:25:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SYN001 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L405'>chameleon/viewer/backend/models/chameleon_distributed.py:405:13:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L818'>chameleon/viewer/backend/models/chameleon_distributed.py:818:17:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_local.py#L633'>chameleon/viewer/backend/models/chameleon_local.py:633:13:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L131'>chameleon/viewer/backend/models/service.py:131:21:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L154'>chameleon/viewer/backend/models/service.py:154:17:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
+ <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L226'>chameleon/viewer/backend/models/service.py:226:25:</a> SYN001 Cannot use `match` statement on Python 3.9 (syntax was new in Python 3.10)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SYN001 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-02-08 07:55_

It would be great to have a design discussion first on how we plan on integrating this into red know and ruff from a ux perspective because this approach already implies some important decisions 

---

_Comment by @AlexWaygood on 2025-02-08 11:29_

> It would be great to have a design discussion first on how we plan on integrating this into red know and ruff from a ux perspective because this approach already implies some important decisions

Brent's been writing up a design doc and he's shared an early draft with me. He will be sharing it with the rest of the team as well. I believe the idea of this (draft) PR is for it to be a prototype to share alongside the design doc.

---

_Comment by @ntBre on 2025-02-08 13:52_

Yes, sorry about that. The intention is definitely to have a design discussion first.

---

_Comment by @ntBre on 2025-02-24 14:56_

Closing in favor #16090 and #16106.

---

_Closed by @ntBre on 2025-02-24 14:56_

---

_Branch deleted on 2025-05-01 15:32_

---
