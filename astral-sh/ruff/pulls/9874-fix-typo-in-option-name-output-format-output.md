```yaml
number: 9874
title: "Fix typo in option name: `output_format` -> `output-format`"
type: pull_request
state: merged
author: hugovk
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-typo
created_at: 2024-02-07T15:31:55Z
updated_at: 2024-02-07T16:26:24Z
url: https://github.com/astral-sh/ruff/pull/9874
synced_at: 2026-01-12T15:55:30Z
```

# Fix typo in option name: `output_format` -> `output-format`

---

_@hugovk_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Go to https://docs.astral.sh/ruff/settings/#show-source

Read deprecation notice:

> This option has been deprecated. `show_source` is deprecated and is now part of `output_format` in the form of `full` or `concise` options. Please update your configuration.

Copy and paste show_source into search box.

No results.

Run Ruff and see the similar warning:

> warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.



## Test Plan

<!-- How was it tested? -->

Copy and paste show-source into search box.

Get results, including https://docs.astral.sh/ruff/settings/#output-format.

No results.

## Also

It would be great if you can link from `output-format` at https://docs.astral.sh/ruff/settings/#show-source to https://docs.astral.sh/ruff/settings/#output-format

It would also be nice if there was advice -- tell me how the old `show-source` values should be mapped to the new `output-format` options.


---

_Renamed from "Fix typo in option name: output_format -> -format" to "Fix typo in option name: `output_format` -> `output-format`" by @hugovk on 2024-02-07 15:32_

---

_Comment by @github-actions[bot] on 2024-02-07 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2024-02-07 16:11_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/options.rs`:122 on 2024-02-07 16:11_

```suggestion
        note = "`show-source` is deprecated and is now part of `output-format` in the form of `full` or `concise` options. Please update your configuration."
```

---

_Label `documentation` added by @zanieb on 2024-02-07 16:12_

---

_Comment by @zanieb on 2024-02-07 16:13_

Thank you! I totally agree "Please update your configuration" should be more specific. I don't think we have a great way to provide links yet since our documentation is not versioned we cannot ensure the links work permanently.

You can see https://astral.sh/blog/ruff-v0.2.0#output-format for more context on the deprecation.

---

_Merged by @zanieb on 2024-02-07 16:17_

---

_Closed by @zanieb on 2024-02-07 16:17_

---

_Branch deleted on 2024-02-07 16:18_

---

_Comment by @hugovk on 2024-02-07 16:18_

Thanks for the link, could be helpful to link to that from the docs too.

---
