```yaml
number: 9124
title: "Fix `can_omit_optional_parentheses` for expressions with a right most fstring"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-can-omit-optional-parentheses-f-strings
created_at: 2023-12-14T04:46:06Z
updated_at: 2023-12-14T04:58:19Z
url: https://github.com/astral-sh/ruff/pull/9124
synced_at: 2026-01-12T15:55:27Z
```

# Fix `can_omit_optional_parentheses` for expressions with a right most fstring

---

_@MichaReiser_

## Summary

This PR fixes an issue in our `can_omit_optional_parentheses` implementation. 

The goal of `can_omit_optional_parentheses` is to return true when the expression starts or ends with a parenthesized expression.

```python
if a + [
	parentheses,
	are,
	not,
	required,
	here
]: pass
```

However, our implementation incorrectly traversed into fstrings and other expressions that end with a token. We should skip these 
(similar to other parenthesized expressions) because they're not at the end of the expression (they're wrapped). 

A silly example, but it should do it for illustrating what it is about

```python
if a + f"parentheses are required because the list isn't at the very end, the closing quote is {[1, 2]}": 
	pass

``` 

## Test Plan

Added test. The similarity for our old compatibility check remains unchanged. The ecosystem only shows one intentional change.


---

_Comment by @MichaReiser on 2023-12-14 04:46_

Current dependencies on/for this PR:
* `main`
  * **PR #9124** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9124?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9124?utm_source=stack-comment).

---

_Label `bug` added by @MichaReiser on 2023-12-14 04:46_

---

_Label `formatter` added by @MichaReiser on 2023-12-14 04:46_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-14 04:46_

---

_Review requested from @konstin by @MichaReiser on 2023-12-14 04:46_

---

_@charliermarsh approved on 2023-12-14 04:53_

---

_Comment by @github-actions[bot] on 2023-12-14 04:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+5 -4 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+5 -4 lines across 1 file)</summary>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/e245b6b6a0f90f71c8aba6e0299f4bcc31638330/tests/test_event.py#L239'>tests/test_event.py~L239</a>
```diff
     assert spec.args[0][1].equals(Var.create_safe("testkey"))
     assert spec.args[1][0].equals(Var.create_safe("options"))
     assert spec.args[1][1].equals(Var.create_safe(options))
-    assert format.format_event(
-        spec
-    ) == f'Event("_remove_cookie", {{key:`testkey`,options:{json.dumps(options)}}})'
+    assert (
+        format.format_event(spec)
+        == f'Event("_remove_cookie", {{key:`testkey`,options:{json.dumps(options)}}})'
+    )
 
 
 def test_clear_local_storage():
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+5 -4 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+5 -4 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/e245b6b6a0f90f71c8aba6e0299f4bcc31638330/tests/test_event.py#L239'>tests/test_event.py~L239</a>
```diff
     assert spec.args[0][1].equals(Var.create_safe("testkey"))
     assert spec.args[1][0].equals(Var.create_safe("options"))
     assert spec.args[1][1].equals(Var.create_safe(options))
-    assert format.format_event(
-        spec
-    ) == f'Event("_remove_cookie", {{key:`testkey`,options:{json.dumps(options)}}})'
+    assert (
+        format.format_event(spec)
+        == f'Event("_remove_cookie", {{key:`testkey`,options:{json.dumps(options)}}})'
+    )
 
 
 def test_clear_local_storage():
```

</p>
</details>




---

_Merged by @MichaReiser on 2023-12-14 04:58_

---

_Closed by @MichaReiser on 2023-12-14 04:58_

---

_Branch deleted on 2023-12-14 04:58_

---
