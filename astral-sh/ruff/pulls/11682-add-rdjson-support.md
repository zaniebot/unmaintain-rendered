```yaml
number: 11682
title: Add RDJson support
type: pull_request
state: merged
author: tobb10001
labels:
  - configuration
assignees: []
merged: true
base: main
head: rdjson
created_at: 2024-06-01T16:46:53Z
updated_at: 2024-06-02T18:09:40Z
url: https://github.com/astral-sh/ruff/pull/11682
synced_at: 2026-01-10T21:56:00Z
```

# Add RDJson support

---

_Pull request opened by @tobb10001 on 2024-06-01 16:46_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement support for RDJson output for `ruff check`, as requested in #8655.

## Test Plan

Tested using a snapshot test. Same approach as for e.g. the JSON output formatter.

## Additional info

I tried to keep the implementation close to the JSON implementation.

I had to deviate a bit to make the `suggestions` key work: If there are no suggestions, then setting `suggestions` to `null` is invalid according to the JSONSchema. Therefore, I opted for a slightly more complex implementation, that skips the `suggestions` key entirely if there are no fixes available for the given diagnostic. Maybe it would have been easier to set `"suggestions": []`, but I ended up doing it this way.

I didn't consider notebooks, as I _think_ that RDJson doesn't work with notebooks. This should be confirmed, and if so, there should be some form of warning or error emitted when trying to output diagnostics for a notebook.

I also didn't consider `ruff format`, as this comment: https://github.com/astral-sh/ruff/issues/8655#issuecomment-1811446160 suggests that that wouldn't be compatible.

I'm new to Rust, any feedback is appreciated. :slightly_smiling_face: I implemented this in order to have a productive rainy saturday afternoon, I'm not knowledgeable about RDJson beyond the sources linked in the issue.


---

_Comment by @github-actions[bot] on 2024-06-01 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-06-02 17:51_

Nice, this looks good! Thank you!

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/rdjson.rs`:90 on 2024-06-02 17:58_

I decided to just duplicate the repeated fields rather than have to unwrap and mutate. (Sort of an arbitrary decision, this is just my preferred style.)

---

_@charliermarsh reviewed on 2024-06-02 17:58_

---

_@charliermarsh reviewed on 2024-06-02 17:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/message/rdjson.rs`:108 on 2024-06-02 17:58_

I changed this to use an iterator.

---

_@charliermarsh reviewed on 2024-06-02 17:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/types.rs`:516 on 2024-06-02 17:58_

I changed this name so that the command-line accepts `rdjson` rather than `rd-json`. (There are other ways to achieve this, but Rust says initializations should be treated as one word anyway.)

---

_Label `configuration` added by @charliermarsh on 2024-06-02 17:58_

---

_Renamed from "Add RDJson support." to "Add RDJson support" by @charliermarsh on 2024-06-02 17:59_

---

_Merged by @charliermarsh on 2024-06-02 17:59_

---

_Closed by @charliermarsh on 2024-06-02 17:59_

---
