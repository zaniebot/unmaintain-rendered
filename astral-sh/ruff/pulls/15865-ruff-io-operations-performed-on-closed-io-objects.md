```yaml
number: 15865
title: "[`ruff`] IO operations performed on closed IO objects (`RUF050`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
base: main
head: RUF061
created_at: 2025-02-01T01:18:18Z
updated_at: 2025-04-28T07:13:00Z
url: https://github.com/astral-sh/ruff/pull/15865
synced_at: 2026-01-12T15:55:52Z
```

# [`ruff`] IO operations performed on closed IO objects (`RUF050`)

---

_@InSyncWithFoo_

## Summary

Resolves #10517.

`RUF050` checks for references of IO objects whose binding is created by a `with` statement and reports if those references might trigger `ValueError: I/O operation on closed file` at runtime.

It recognizes the following patterns:

* Method references: `f.read()`/`_ = f.write`
* Contain checks: `_ in f`
* `for` loops: `for _ in f: ...`

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-01 01:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/d81c3f9b4cbc4d71ff3bf00a079b574c21aa8b22/astropy/io/ascii/ui.py#L1103'>astropy/io/ascii/ui.py:1103:9:</a> RUF050 IO operation performed on closed IO object
+ <a href='https://github.com/astropy/astropy/blob/d81c3f9b4cbc4d71ff3bf00a079b574c21aa8b22/astropy/io/ascii/ui.py#L1104'>astropy/io/ascii/ui.py:1104:9:</a> RUF050 IO operation performed on closed IO object
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF050 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2025-02-01 01:55_

There are a lot of false positives of this kind:

```py
with open() as f:
	f.read()  # A reference to the next `f`

with open() as f:  # Same name
	...

with open() as f:  # Same name
	f.write()  # A reference to either of the previous `f`s
```

This seems to be a bug in the semantic model rather than a problem with this rule.

---

_Label `rule` added by @MichaReiser on 2025-02-03 08:46_

---

_Label `preview` added by @MichaReiser on 2025-02-03 08:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1011 on 2025-02-03 08:51_

Can we use any of the exsiting "holes" in our numbering (assuming they aren't remapped) like 042, 044, 045, 050, 053, 054, 059 instead of introducing new "holes"?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:432 on 2025-02-03 08:51_

We can move the test to the regular `rules` test because the rule contains no `settings.preview_mode` checks.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:84 on 2025-02-03 08:53_

This seems more of an example than proper documentation
```suggestion
// `f.write(...)`
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:85 on 2025-02-03 08:54_

Nit:

```suggestion
fn file_method_name_range(expression_id: NodeId, semantic: &SemanticModel) -> Option<TextRange> {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:121 on 2025-02-03 08:54_

```suggestion
// `_ in f`
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:122 on 2025-02-03 08:55_

Is it necessary to check how `file` is used or what's the motivation to not flag all usages of `f` after the with statement?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:55 on 2025-02-03 08:56_

I think we should validate that `f` is a file and not just any other context variable (assert that it was created using `open`)

---

_@MichaReiser requested changes on 2025-02-03 08:59_

Thanks for working on this.

As mentioned in the original PR, we have to restrict the rule to objects that are files. This should also help to remove the following false positive

https://github.com/apache/airflow/blob/8c9e0b2a8ec0e6f2842883898370741e71c0e802/airflow/models/dagbag.py#L317C12-L317C29

---

_@InSyncWithFoo reviewed on 2025-02-03 14:48_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:122 on 2025-02-03 14:48_

`f.name` accesses are still valid, for example.

---

_Renamed from "[`ruff`] IO operations performed on closed IO objects (`RUF061`)" to "[`ruff`] IO operations performed on closed IO objects (`RUF050`)" by @InSyncWithFoo on 2025-02-03 14:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:155 on 2025-02-03 15:42_

```suggestion

            Some(TextRange::new(start, comparator.end()))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:178 on 2025-02-03 15:42_

```suggestion
    Some(TextRange::new(for_loop.target.start(), iter.end()))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/operation_on_closed_io.rs`:90 on 2025-02-03 15:43_

I think I now have a better understanding of the method names. How about `io_func_range`, `in_file_range`, and `io_in_for_loop_range`?



---

_@MichaReiser reviewed on 2025-02-03 15:44_

I'm concerned about the examples you mentioned as well as if `f` gets deleted. 

It seems that the semantic model copies over all references when shadowing an existing binding. https://github.com/astral-sh/ruff/blob/fe516e24f54bfc33b7b480e8438b9393630f05b1/crates/ruff_linter/src/checkers/ast/mod.rs#L2048-L2062

I think we have to make the rule shadowed-binding aware and skip over references that come from a shadowed binding. 

---

_Comment by @InSyncWithFoo on 2025-02-03 15:59_

> I think we have to make the rule shadowed-binding aware and skip over references that come from a shadowed binding.

I'm not sure how to do this, especially if all references are simply copied over.

---

_Comment by @MichaReiser on 2025-02-03 18:22_

Yeah, me neither. We'd have to look at other rules to see how to handle it. I don't think I've the time right now to do that. Maybe @charliermarsh knows?

---

_Comment by @MichaReiser on 2025-04-28 07:13_

I'll close this because we may want to wait for red knot to get better type inference to correctly identify these cases. Thanks @InSyncWithFoo for exploring this rule.

---

_Closed by @MichaReiser on 2025-04-28 07:13_

---
