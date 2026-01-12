```yaml
number: 1657
title: Autofix SIM102 (NestedIfStatements)
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: autofix-sim102
created_at: 2023-01-05T09:08:34Z
updated_at: 2023-01-18T14:08:41Z
url: https://github.com/astral-sh/ruff/pull/1657
synced_at: 2026-01-12T05:25:13Z
```

# Autofix SIM102 (NestedIfStatements)

---

_Pull request opened by @andersk on 2023-01-05 09:08_

I tried to write an autofixer for SIM102 (`NestedIfStatements`) using LibCST to preserve comments. But I ran into two issues that potentially point to general problems with the with the `SourceCodeLocator` API and/or the `Fix::replacement` API. So I’m opening this draft to discuss what the plan should be.

---

Firstly, if the input `if` statement is indented, the output `if` body will be misindented:

```diff
 while 1:
     while 2:
-        if 3:
-            if 4:
-                5
+        if 3 and 4:
+    5
```

In this case, `SourceCodeLocator::slice_source_code_range` fails to strip the indentation from the second and following lines:

```python
if 3:
            if 4:
                5
```

and `Fix::replacement` fails to add that indentation.

---

Secondly, if the input `if` statement is actually an `elif`, the fix fails:

```python
while 1:
    while 2:
        if 3:
            4
        elif 5:
            if 6:
                7
```

In this case, `SourceCodeLocator::slice_source_code_range` returns this invalid Python that LibCST can’t parse:

```python
elif 5:
            if 6:
                7
```

---

_Comment by @charliermarsh on 2023-01-06 15:21_

The latter issue came up in a slightly different form here: https://github.com/charliermarsh/ruff/pull/1694.

---

_Comment by @charliermarsh on 2023-01-17 13:10_

@andersk - Ok if I poke around at this branch? Gonna see if I can come up with some helpers for the issues you're describing.

---

_Comment by @charliermarsh on 2023-01-17 21:51_

@andersk - Ok, I think this works? We do skip a few cases: (1) if there are any comments _between_ the two `if` statements, those get destroyed in the fix, so we avoid applying the patch in that case (we _do_ preserve comments in the nested `if` body, so that's fine); and (2) if the resulting statement is too long, we avoid applying it.

---

_Marked ready for review by @charliermarsh on 2023-01-17 21:51_

---

_Comment by @charliermarsh on 2023-01-17 21:51_

Let me know if you have feedback, and how you feel about merging this change.

---

_Review comment by @andersk on `ruff_cli/src/diagnostics.rs`:45 on 2023-01-17 22:27_

Spurious print.

---

_Review comment by @andersk on `src/rules/flake8_simplify/rules/fix_if.rs`:64 on 2023-01-17 22:40_

```suggestion
    // Parse the CST.
```

---

_Review comment by @andersk on `src/rules/flake8_simplify/rules/fix_if.rs`:58 on 2023-01-17 22:47_

This has various failure modes.

```python
while True:
    if True:
        if True:
            """this
is valid"""

            """the indentation on
            this line is significant"""

            "this is" \
"allowed too"

            ("so is"
"this for some reason")
```

---

_@andersk reviewed on 2023-01-17 22:47_

---

_Review comment by @charliermarsh on `src/rules/flake8_simplify/rules/fix_if.rs`:58 on 2023-01-17 23:14_

Right, ok, good call. I believe I can fix this by leveraging LibCST's `indentation` field.

---

_@charliermarsh reviewed on 2023-01-17 23:14_

---

_@charliermarsh reviewed on 2023-01-18 04:28_

---

_Review comment by @charliermarsh on `src/rules/flake8_simplify/rules/fix_if.rs`:58 on 2023-01-18 04:28_

Alright, I think the new version works, but it requires a bunch of trickery.

If the statement is indented, we embed it in a dummy function, like:

```py
def f():
  if ...
```

Then, at the end, we stripe the function. I _think_ this is the most robust way to preserve indentation. It lets us avoid having to do any indentation or dedentation ourselves (but keeps the source code valid for LibCST).

Separately, we _also_ have to handle `elif` blocks via special-case.


---

_Comment by @andersk on 2023-01-18 07:40_

Yeah, that gets the job done I guess.

I cleaned this up a bit more and split out the helper function changes into a series of logical commits suitable for merging with **Rebase and merge**.

---

_Renamed from "Broken autofixer for SIM102 (NestedIfStatements)" to "Autofix SIM102 (NestedIfStatements)" by @andersk on 2023-01-18 08:11_

---

_Merged by @charliermarsh on 2023-01-18 12:37_

---

_Closed by @charliermarsh on 2023-01-18 12:37_

---

_Branch deleted on 2023-01-18 14:08_

---
