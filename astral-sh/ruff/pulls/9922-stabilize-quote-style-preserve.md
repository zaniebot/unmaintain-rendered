```yaml
number: 9922
title: "Stabilize quote-style `preserve`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - formatter
assignees: []
merged: true
base: main
head: stabilize-quote-style-preserve
created_at: 2024-02-10T06:29:35Z
updated_at: 2024-02-12T09:41:22Z
url: https://github.com/astral-sh/ruff/pull/9922
synced_at: 2026-01-10T22:57:09Z
```

# Stabilize quote-style `preserve`

---

_Pull request opened by @MichaReiser on 2024-02-10 06:29_

This PR removes the `--preview` requirement for `quote-style: preserve` and changes the semantics of
the style to apply to all strings (including triple quoted and doc-strings).

The motivation for changing the semantic to apply to all strings is:

* The option is intended as a replacement for black's `--skip-string-normalization` that applies to all strings
* The option is intended for projects that can't enforce a consistent quote style. These are mainly projects that want to use a formatter but can't agree on a single quote style or changing all strings leads to too large diffs. Allowing these users to use a formatter is more important than being opinionated on the quote style
* We received reports that users were surprised that `preserve` didn't apply to all strings.

Fixes https://github.com/astral-sh/ruff/issues/9185


## Test Plan

`cargo test`. Reviewed the snapshot changes. Tested the CLI that using `quote-style: preserve` without the preview flag no longer aborts.


---

_Label `cli` added by @MichaReiser on 2024-02-10 06:31_

---

_Label `formatter` added by @MichaReiser on 2024-02-10 06:31_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-10 06:32_

---

_Comment by @github-actions[bot] on 2024-02-10 06:47_

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

_@charliermarsh reviewed on 2024-02-10 15:16_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2968 on 2024-02-10 15:16_

I'd probably omit this last sentence (it's implied in the previous sentences that this is intended for existing projects). (If it stays, "project" should be "projects".)

---

_@charliermarsh approved on 2024-02-10 15:16_

---

_@charliermarsh reviewed on 2024-02-10 15:20_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2951 on 2024-02-10 15:20_

"regardless of the configured quote style" has the potential to confuse since it's no longer true of `preserve`, maybe... "regardless of whether `single` or `double` is selected."?

---

_@charliermarsh approved on 2024-02-10 15:20_

---

_Comment by @T-256 on 2024-02-12 00:29_

is this PR is going to merge for next patch release?
it is unknown in versioning policy how config options changes should be applied.

---

_Comment by @MichaReiser on 2024-02-12 08:30_

> is this PR is going to merge for next patch release? it is unknown in versioning policy how config options changes should be applied.

Yes, this will ship as part of the next patch release. Changing the semantic is is in line with our versioning policy because `quote-style: preserve` is in preview:

> The preview mode is not intended to gate access to work that is incomplete or features that we are likely to remove. However, we reserve the right to make changes to any behavior gated by the mode including the removal of preview features or rules. [source](https://docs.astral.sh/ruff/versioning/#preview-mode)



---

_Merged by @MichaReiser on 2024-02-12 09:30_

---

_Closed by @MichaReiser on 2024-02-12 09:30_

---

_Branch deleted on 2024-02-12 09:30_

---
