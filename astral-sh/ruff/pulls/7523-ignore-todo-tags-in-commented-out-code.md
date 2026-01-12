```yaml
number: 7523
title: "Ignore TODO tags in `commented-out-code`"
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
assignees: []
merged: true
base: main
head: allow-todo
created_at: 2023-09-19T15:46:07Z
updated_at: 2023-09-29T10:11:32Z
url: https://github.com/astral-sh/ruff/pull/7523
synced_at: 2026-01-12T15:55:24Z
```

# Ignore TODO tags in `commented-out-code`

---

_@tjkuson_

## Summary

Extend the `task-tags` checking logic to ignore TODO tags (with or without parentheses). For example,

```python
# TODO(tjkuson): Rewrite in Rust
```

is no longer flagged as commented-out code.

Closes #7031.

I also updated the documentation to inform users that the rule is prone to false positives like this!

EDIT: Accidentally linked to the wrong issue when first opening this PR, now corrected.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-09-19 16:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-09-19 16:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/eradicate/detection.rs`:9 on 2023-09-19 16:58_

Can we find a way to do this generically, for all `task_tags`? Since this wouldn't extend to `FIXME`, etc. I'm wondering if we can instead split on parentheses on line 53, where we iterate over the task tags?

---

_Label `bug` added by @charliermarsh on 2023-09-20 13:27_

---

_@charliermarsh requested changes on 2023-09-20 13:28_

No rush, just bumping out of my queue :)

---

_@MichaReiser reviewed on 2023-09-20 16:13_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/eradicate/detection.rs`:9 on 2023-09-20 16:13_

We should probably also make sure to respect https://docs.astral.sh/ruff/settings/#task-tags

It may also be possible to implement this without using a regex, but could be a bit of a pain.

---

_@charliermarsh reviewed on 2023-09-20 16:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/eradicate/detection.rs`:9 on 2023-09-20 16:15_

We do respect task tags but it's not part of the regex, it's down on line 53.

---

_Comment by @tjkuson on 2023-09-21 07:07_

Moved the tag checking logic to before the regex matching, as I believe it is less expensive. Also, now checks for lowercase tags (for example, `# todo(tom): Rewrite in Rust`).

---

_Review requested from @charliermarsh by @tjkuson on 2023-09-21 07:08_

---

_@charliermarsh approved on 2023-09-28 23:03_

Thanks!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/eradicate/detection.rs`:46 on 2023-09-28 23:04_

I ended up removing the uppercase check here because it requires an allocation (would be expensive give how often this runs), and is probably rare.

---

_@charliermarsh reviewed on 2023-09-28 23:04_

---

_Merged by @charliermarsh on 2023-09-28 23:13_

---

_Closed by @charliermarsh on 2023-09-28 23:13_

---

_@tjkuson reviewed on 2023-09-29 09:45_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/eradicate/detection.rs`:46 on 2023-09-29 09:45_

Makes sense, thanks!

---

_Branch deleted on 2023-09-29 10:11_

---
