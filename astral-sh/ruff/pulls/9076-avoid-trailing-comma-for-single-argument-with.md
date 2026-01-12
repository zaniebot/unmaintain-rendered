```yaml
number: 9076
title: Avoid trailing comma for single-argument with positional separator
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/comma
created_at: 2023-12-09T19:57:53Z
updated_at: 2023-12-09T23:03:32Z
url: https://github.com/astral-sh/ruff/pull/9076
synced_at: 2026-01-10T23:40:55Z
```

# Avoid trailing comma for single-argument with positional separator

---

_Pull request opened by @charliermarsh on 2023-12-09 19:57_

## Summary

In https://github.com/astral-sh/ruff/pull/8921, we changed our parameter formatting behavior to add a trailing comma whenever a single-argument function breaks. This introduced a deviation in the case that a function contains a single argument, but _also_ includes a positional-only or keyword-only separator.

Closes https://github.com/astral-sh/ruff/issues/9074.


---

_Label `bug` added by @charliermarsh on 2023-12-09 19:57_

---

_Label `formatter` added by @charliermarsh on 2023-12-09 19:57_

---

_Comment by @github-actions[bot] on 2023-12-09 20:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+2 -3 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -3 lines across 1 file)</summary>
<p>

<a href='https://github.com/zulip/zulip/blob/f86becfc94c198f2b9038ed917d5b983071ccaca/zerver/lib/message.py#L1249'>zerver/lib/message.py~L1249</a>
```diff
 
 
 def aggregate_pms(
-    *,
-    input_dict: Dict[int, RawUnreadDirectMessageDict],
+    *, input_dict: Dict[int, RawUnreadDirectMessageDict]
 ) -> List[UnreadDirectMessageInfo]:
     lookup_dict: Dict[int, UnreadDirectMessageInfo] = {}
     for message_id, attribute_dict in input_dict.items():
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+2 -3 lines in 1 file in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/zulip/zulip/blob/f86becfc94c198f2b9038ed917d5b983071ccaca/zerver/lib/message.py#L1249'>zerver/lib/message.py~L1249</a>
```diff
 
 
 def aggregate_pms(
-    *,
-    input_dict: Dict[int, RawUnreadDirectMessageDict],
+    *, input_dict: Dict[int, RawUnreadDirectMessageDict]
 ) -> List[UnreadDirectMessageInfo]:
     lookup_dict: Dict[int, UnreadDirectMessageInfo] = {}
     for message_id, attribute_dict in input_dict.items():
```

</p>
</details>




---

_Comment by @charliermarsh on 2023-12-09 20:18_

The one ecosystem change matches Black's formatting (i.e., it's more consistent than on `main`).

---

_Review requested from @MichaReiser by @charliermarsh on 2023-12-09 20:18_

---

_Comment by @BlindChickens on 2023-12-09 20:34_

Yes lets go.

---

_@MichaReiser approved on 2023-12-09 22:35_

---

_Merged by @charliermarsh on 2023-12-09 23:03_

---

_Closed by @charliermarsh on 2023-12-09 23:03_

---

_Branch deleted on 2023-12-09 23:03_

---
