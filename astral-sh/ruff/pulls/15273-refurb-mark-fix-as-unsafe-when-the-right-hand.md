```yaml
number: 15273
title: "[`refurb`] Mark fix as unsafe when the right-hand side is a string (`FURB171`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: FURB171
created_at: 2025-01-05T16:49:15Z
updated_at: 2025-01-05T17:54:38Z
url: https://github.com/astral-sh/ruff/pull/15273
synced_at: 2026-01-10T20:34:00Z
```

# [`refurb`] Mark fix as unsafe when the right-hand side is a string (`FURB171`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-05 16:49_

## Summary

Resolves #15268.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-05 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -4 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L637'>tools/lib/template_parser.py:637:29:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L637'>tools/lib/template_parser.py:637:29:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L645'>tools/lib/template_parser.py:645:29:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/669a341b022a1ee13490ad857b0ea2e61950bfd0/tools/lib/template_parser.py#L645'>tools/lib/template_parser.py:645:29:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB171 | 4 | 0 | 0 | 0 | 4 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:109 on 2025-01-05 17:30_

nit

```suggestion
    let fix = Fix::applicable_edit(edit, applicability);
    checker.diagnostics.push(diagnostic.with_fix(fix));
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:31 on 2025-01-05 17:32_

```suggestion
/// This is because `c in "a"` is true both when `c` is `"a"` and when `c` is the empty string,
/// so the fix can change the behavior of your program in these cases.
```

---

_@AlexWaygood reviewed on 2025-01-05 17:33_

Nice, thanks! My only comment is that I wonder if we should even be emitting this lint if the r.h.s. is a string? It's quite possible that the user is deliberately exploiting the way `in` works with regard to string here, in which case it would be annoying to have to `noqa` the lint away.

That _does_ seem unlikely, though, so maybe it's better to continue emitting the lint and just mark the fix as unsafe. Not sure -- what do you think?

---

_Comment by @InSyncWithFoo on 2025-01-05 17:52_

I'm with the latter. In such cases, `c == "" or c == "a"` (or `c in ("", "a")` to avoid double evaluation) would be clearer and I would have suggested that as a secondary fix had it not been for the one-fix-per-diagnostic limit.

---

_@AlexWaygood approved on 2025-01-05 17:53_

Thanks!

---

_Label `bug` added by @AlexWaygood on 2025-01-05 17:54_

---

_Label `fixes` added by @AlexWaygood on 2025-01-05 17:54_

---

_Label `preview` added by @AlexWaygood on 2025-01-05 17:54_

---

_Merged by @AlexWaygood on 2025-01-05 17:54_

---

_Closed by @AlexWaygood on 2025-01-05 17:54_

---

_Branch deleted on 2025-01-05 17:54_

---
