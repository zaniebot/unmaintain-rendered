```yaml
number: 7194
title: "Add `extend-ignore-names` for `flake8-self`"
type: pull_request
state: merged
author: jaap3
labels:
  - configuration
assignees: []
merged: true
base: main
head: add-flake8-self-extend-ignore-names
created_at: 2023-09-06T14:37:10Z
updated_at: 2023-09-09T18:16:01Z
url: https://github.com/astral-sh/ruff/pull/7194
synced_at: 2026-01-12T15:55:23Z
```

# Add `extend-ignore-names` for `flake8-self`

---

_@jaap3_

## Summary

Add a configuration option to extend the list of names that can be accessed without triggering SLF001.

Fixes issue #7018

## Test Plan

Manually tested by creating a python file (`test.py`):

```python
def foo(obj):
    obj._meta
```

and a `ruff.toml` file:

```toml
select = ["SLF"]

[flake8-self]
extend-ignore-names = ["_meta"]
```

Then running `cargo run -p ruff_cli -- check test.py --no-cache` (once with the `extend-ignore-names` line comment out) to see if the configuration option works.

---

_Comment by @jaap3 on 2023-09-06 14:37_

This is my first time reading/writing rust 🤞 

---

_Comment by @jaap3 on 2023-09-06 14:51_

Fixed the order in `ruff.schema.json`, I guess there must be some way to automatically generate that file instead of doing it manually. (EDIT: Somehow missed that bit in `CONTRIBUTING.md`).

---

_Renamed from "Add extend-ignore-names for flake8-self" to "Add `extend-ignore-names` for `flake8-self`" by @jaap3 on 2023-09-06 15:00_

---

_Comment by @github-actions[bot] on 2023-09-06 15:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `configuration` added by @charliermarsh on 2023-09-06 16:10_

---

_@charliermarsh approved on 2023-09-06 16:13_

This looks great, thanks! Are you interested in adding a test? You could look at the `extend_immutable_calls_arg_annotation` test as an example of how to test this kind of configuration option.

---

_Comment by @jaap3 on 2023-09-06 16:36_

I'll look into adding tests tomorrow. There also might be a mistake in my current patch. Looks like setting both ignore-names and extend-ignore-names will not work as expected 🤔. Guess I'll find out with a test 😉

---

_Comment by @jaap3 on 2023-09-07 12:54_

I've added tests for the changes I did. Had some trouble figuring out where to place them as the change itself only affects the options and not the rules module. Hope what I did is OK, if not let me know!

---

_Review requested from @charliermarsh by @jaap3 on 2023-09-07 13:02_

---

_Comment by @charliermarsh on 2023-09-07 13:34_

Thanks, that looks reasonable! I also added a test using a Python snippet.

---

_Merged by @charliermarsh on 2023-09-07 13:41_

---

_Closed by @charliermarsh on 2023-09-07 13:41_

---

_Branch deleted on 2023-09-09 18:16_

---
