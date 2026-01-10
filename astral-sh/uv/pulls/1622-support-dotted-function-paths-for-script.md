```yaml
number: 1622
title: Support dotted function paths for script entrypoints
type: pull_request
state: merged
author: markmmm
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-18T02:28:54Z
updated_at: 2024-02-19T12:46:47Z
url: https://github.com/astral-sh/uv/pull/1622
synced_at: 2026-01-10T15:33:24Z
```

# Support dotted function paths for script entrypoints

---

_Pull request opened by @markmmm on 2024-02-18 02:28_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Some projects define an script entrypoint with a path separated by `.`. 

For example in Invoke...
```python
    entry_points={
        "console_scripts": [
            "invoke = invoke.main:program.run",
            "inv = invoke.main:program.run",
        ]
    },
```
Before this change `uv` tried to import 'program.run' but
this is not valid, and only up to the first dot should be
imported (as `pip` does).

Resolves: #1573

## Test Plan

- Added a unit test in script.rs to validate that the import_name is set correctly when the function path is dotted.
- Ran the unit tests (`cargo test`)

https://github.com/astral-sh/uv/assets/1203881/8774b281-0811-4520-b4d7-244d0f955cb7




---

_Review comment by @markmmm on `crates/install-wheel-rs/src/script.rs`:19 on 2024-02-18 03:04_

Interestingly I forgot to add this at first (I assume it should be added) - but no test identified it.
Is there anyway to easily unit test this?

---

_@markmmm reviewed on 2024-02-18 03:05_

---

_Assigned to @MichaReiser by @zanieb on 2024-02-18 07:56_

---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/script.rs`:71 on 2024-02-18 15:56_

It seems that `import_name` can always be derived by the `function` name and can easily be computed (rather than be stored on `Script`. This is also what `distlib` does. 

https://github.com/pypa/distlib/blob/1c08845b05d022692252ed45cb07e9cb9647caac/distlib/scripts.py#L239-L243

That's why I would remove the `import_name` field and instead add an `import_name` function on `Script` that computes the import name on the fly. This safes us one allocation (good for performance) and avoids storing redudant data. 

---

_@MichaReiser reviewed on 2024-02-18 15:57_

Wow thanks. I wanted to tackle this on Monday but you beat me to it! Thank you. 

This looks good to me and matches `distlib`'s behavior. I think it would be nice to calculate the data on the fly instead of storing it on `Script`.

---

_@markmmm reviewed on 2024-02-18 23:16_

---

_Review comment by @markmmm on `crates/install-wheel-rs/src/script.rs`:71 on 2024-02-18 23:16_

I did question this when implementing and went this way out of consistency (fields rather than functions)
I am not clear on how changes to the interface impact the PyO3 version of the struct.
If we move to functions - would they still be available to the Python bindings?

If the code switched to functions - `uv` could potentially move to a different process for working with the data (i.e. storing 1 string - and passing offsets). I am not saying we need that - but if `uv` might want to move to that way of returning these values - then easier to do so if implementation is hidden behind functions.

---

_@MichaReiser reviewed on 2024-02-19 09:54_

---

_Review comment by @MichaReiser on `crates/install-wheel-rs/src/script.rs`:71 on 2024-02-19 09:54_

That's fair. I don't think we use the pyo3 interface today. That's why I think it's safe to ignore any concerns regarding it today. 

I don't think we need to store any text ranges. We can return a `&str` and us the same logic that you currently have in the `new` function. I applied the changes to your branch

---

_Label `bug` added by @MichaReiser on 2024-02-19 10:05_

---

_Merged by @MichaReiser on 2024-02-19 10:09_

---

_Closed by @MichaReiser on 2024-02-19 10:09_

---

_Comment by @markmmm on 2024-02-19 12:46_

Thanks - looks great!

---
