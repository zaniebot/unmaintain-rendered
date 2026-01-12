```yaml
number: 12251
title: "`RUF100` should probably delete an entire comment `noqa` is part of"
type: issue
state: closed
author: yury-fedotov
labels:
  - bug
  - good first issue
  - fixes
  - help wanted
assignees: []
created_at: 2024-07-09T05:37:13Z
updated_at: 2024-08-29T05:20:17Z
url: https://github.com/astral-sh/ruff/issues/12251
synced_at: 2026-01-12T15:54:51Z
```

# `RUF100` should probably delete an entire comment `noqa` is part of

---

_@yury-fedotov_

I think it applies to all error codes, but let me give an example where I encountered that.

Consider:

```python
data = do_some_transformations()
return data
```

Which is a violation of [`RET504`](https://docs.astral.sh/ruff/rules/unnecessary-assign/) - unnecessary assignment.

I needed to perform the assignment and `return data`, not `return do_some_transformations()`, so added a `noqa: RET504` with explanation of why I want to bypass the rule:

```python
data = do_some_transformations()
return data  # noqa: RET504 - to be able to debug the returned object.
```

Then I disabled `RET504` in `ruff` config completely, and this `noqa` became unused. Here is where `RUF100` comes into play, as it complains on unused error codes, and provides a fix.

The fix was:

```diff
-    return data  # noqa: RET504 - to be able to debug the returned object
+    return data  # - to be able to debug the returned object
```

While I believe it should have been:

```diff
-    return data  # noqa: RET504 - to be able to debug the returned object
+    return data
```

As it's probably quite safe to assume that entire content of the comment `noqa` is part of is explaining why a rule should by bypassed.

* List of keywords you searched for before creating this issue: `RUF100`
* Most similar open issue: https://github.com/astral-sh/ruff/issues/7851
* A minimal code snippet that reproduces the bug: see above.
* The command you invoked: `ruff check --fix`
* The current Ruff version: `0.5.1`.

---

_Label `bug` added by @MichaReiser on 2024-07-09 06:34_

---

_Label `fixes` added by @MichaReiser on 2024-07-09 06:34_

---

_Label `help wanted` added by @MichaReiser on 2024-07-09 06:34_

---

_Comment by @charliermarsh on 2024-07-12 13:21_

I'm unsure -- I think the current behavior is intentional. But either seems reasonable.

---

_Comment by @MichaReiser on 2024-07-12 18:01_

I think deleting the entire comment is fine, for as long as it doesn't delete anything coming after another `#`:

```patch
-    return data  # noqa: RET504 # fmt:skip
+    return data  # fmt: skip
```

---

_Comment by @charliermarsh on 2024-07-12 18:15_

I agree!

---

_Label `good first issue` added by @charliermarsh on 2024-07-12 18:15_

---

_Comment by @charliermarsh on 2024-07-12 18:17_

I think this is all here if anyone is interested in a good issue: https://github.com/astral-sh/ruff/blob/940df67823dc5237f95d36a94ef3a74dc4bd36fb/crates/ruff_linter/src/fix/edits.rs#L65

---

_Comment by @jeremeybingham on 2024-07-18 05:23_

Is there a standard or a strong feeling here on preserving trailing whitespace following 'Preserved' comments? Meaning: 

can:  `x = 1  # noqa  # type: ignore` 
and: `x = 1  # noqa  # type: ignore    ` 

both resolve to: `x = 1  # type: ignore`  ?

or is there a conceivable edge case where that matters?

At any rate, I believe I've mostly got this but will spend some time tomorrow getting the testing framework and everything set up, should work regardless of the above, just changes the logic a bit to save those characters if we want 'em.

---

_Comment by @dhruvmanila on 2024-07-18 05:52_

It should be fine to remove trailing whitespace but just a note that any other comments shouldn't be deleted (https://github.com/astral-sh/ruff/issues/12251#issuecomment-2226110486).

---

_Comment by @jeremeybingham on 2024-07-18 21:50_

this was... much better than I expected the first pass to go, will have a PR on this once I figure out what's going on with these, but I'll take 4/2057 as a start!

![Screenshot 2024-07-18 174653](https://github.com/user-attachments/assets/86d5af90-a191-435d-bfa0-0a0625e17c7a)


---

_Comment by @dhruvmanila on 2024-07-24 06:02_

@jeremeybingham Can you open the PR? I can help you debug these test failures.

---

_Comment by @jeremeybingham on 2024-07-24 15:20_

will try asap, yes - have "fixed" my way into various fun new failure modes, and am now trying to consolidate the working bits of all that into something that compiles. 

---

_Closed by @dhruvmanila on 2024-08-29 05:20_

---
