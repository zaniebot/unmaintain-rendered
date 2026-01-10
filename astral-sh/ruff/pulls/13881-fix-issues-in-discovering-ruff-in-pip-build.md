```yaml
number: 13881
title: fix issues in discovering ruff in pip build environments
type: pull_request
state: merged
author: jsurany
labels:
  - bug
assignees: []
merged: true
base: main
head: pip_build_envs
created_at: 2024-10-22T19:49:16Z
updated_at: 2024-10-30T03:23:25Z
url: https://github.com/astral-sh/ruff/pull/13881
synced_at: 2026-01-10T20:59:37Z
```

# fix issues in discovering ruff in pip build environments

---

_Pull request opened by @jsurany on 2024-10-22 19:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Changes in this PR https://github.com/astral-sh/ruff/pull/13591 did not allow correct discovery in pip build environments.

```python
# both of these variables are tuple[str, str] (length is 2)
first, second = os.path.split(paths[0]), os.path.split(paths[1])

# so these length checks are guaranteed to fail even for build environment folders
if (
    len(first) >= 3
    and len(second) >= 3 
    ...
)
```

~~Here we instead use `pathlib`, and we check all `pip-build-env-` paths for the folder that is expected to contain the `ruff` executable.~~

Here we update the logic to more properly split out the path components that we use for `pip-build-env-` inspection.

## Test Plan

I've checked this manually against a workflow that was failing, I'm not sure what to do for real tests. The same issues apply as with the previous PR.


---

_Comment by @github-actions[bot] on 2024-10-22 20:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @gaborbernat on `python/ruff/__main__.py`:42 on 2024-10-22 20:19_

We should only check the last two path, not all.

---

_Review comment by @gaborbernat on `python/ruff/__main__.py`:48 on 2024-10-22 20:20_

You removed also asserting the `normal` entry.

---

_Review comment by @gaborbernat on `python/ruff/__main__.py`:4 on 2024-10-22 20:20_

`pathlib` is expensive to import during the startup, so we actively wanted to avoid using it; use os.path instead

---

_@gaborbernat reviewed on 2024-10-22 20:20_

Needs some work IMHO.


---

_Review requested from @gaborbernat by @jsurany on 2024-10-22 21:13_

---

_@gaborbernat reviewed on 2024-10-22 21:15_

---

_Review comment by @gaborbernat on `python/ruff/__main__.py`:6 on 2024-10-22 21:15_

we only do checks against the last 3 elements, so perhaps we should stop after 3? (could return a tuple of 3, where the elements can be empty if missing)

---

_Review requested from @gaborbernat by @jsurany on 2024-10-22 21:22_

---

_@jsurany reviewed on 2024-10-22 21:47_

---

_Review comment by @jsurany on `python/ruff/__main__.py`:6 on 2024-10-22 21:47_

I'm ok with stopping at 3, not a fan of returning a tuple.

---

_@gaborbernat reviewed on 2024-10-22 21:57_

👍 


---

_Review request for @gaborbernat removed by @zanieb on 2024-10-22 22:21_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-22 22:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-28 14:22_

---

_@charliermarsh reviewed on 2024-10-28 14:58_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:60 on 2024-10-28 14:58_

Can't this lead to an index-out-of-bounds error?

---

_@charliermarsh reviewed on 2024-10-28 14:58_

What was the actual problem here?

---

_Comment by @gaborbernat on 2024-10-28 15:07_

> What was the actual problem here?

https://github.com/astral-sh/ruff/pull/13591 does not work 😆 this fixes that. @jsurany is a colleague of mine and when validating that release discovered this issue.

---

_@gaborbernat reviewed on 2024-10-28 15:09_

---

_Review comment by @gaborbernat on `python/ruff/__main__.py`:60 on 2024-10-28 15:09_

Yeah there is an if statement missing here, ` if len(paths) >= 2:` that got removed by mistake.

---

_@jsurany reviewed on 2024-10-28 17:01_

---

_Review comment by @jsurany on `python/ruff/__main__.py`:60 on 2024-10-28 17:01_

good catch, updated.

---

_Review requested from @charliermarsh by @jsurany on 2024-10-28 17:16_

---

_Review requested from @gaborbernat by @jsurany on 2024-10-28 17:16_

---

_@gaborbernat reviewed on 2024-10-28 17:16_

---

_Review comment by @gaborbernat on `python/ruff/__main__.py`:6 on 2024-10-28 17:16_

I personally would put it after the callsite not before (just as in a book you have foot references at bottom and not at top - this is not C where the declaration needs to be before the use), but I let @charliermarsh to make the executive decision.

---

_@charliermarsh reviewed on 2024-10-29 02:24_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:64 on 2024-10-29 02:24_

Isn't this wrong? The path is like `<prefix>/pip-build-env-<rand>/overlay/bin/ruff`. So then `maybe_normal` would be `['ruff', 'bin', 'overlay']`, right?

---

_@jsurany reviewed on 2024-10-29 14:14_

---

_Review comment by @jsurany on `python/ruff/__main__.py`:64 on 2024-10-29 14:14_

The paths from `$PATH` don't include the actual executable. For instance for `maybe_overlay` the path we parse would look like `<prefix>/pip-build-env-<rand>/overlay/bin`, so `maybe_overlay` is `["bin", "overlay", "pip-build-env-<rand>"]`

---

_Review requested from @charliermarsh by @jsurany on 2024-10-29 14:31_

---

_@charliermarsh reviewed on 2024-10-29 15:45_

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:64 on 2024-10-29 15:45_

Ah right -- thanks!

---

_Review comment by @charliermarsh on `python/ruff/__main__.py`:49 on 2024-10-29 15:46_

I changed this to just use `len(parts)` instead of a separate index, which I believe is equivalent.

---

_@charliermarsh reviewed on 2024-10-29 15:46_

---

_Merged by @charliermarsh on 2024-10-29 15:50_

---

_Closed by @charliermarsh on 2024-10-29 15:50_

---

_Branch deleted on 2024-10-29 15:51_

---

_Label `bug` added by @dhruvmanila on 2024-10-30 03:23_

---
