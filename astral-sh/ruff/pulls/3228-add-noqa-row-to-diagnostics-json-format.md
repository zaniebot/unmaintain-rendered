```yaml
number: 3228
title: "Add `noqa_row` to diagnostics JSON format"
type: pull_request
state: merged
author: bluetech
labels: []
assignees: []
merged: true
base: main
head: noqa-line-json
created_at: 2023-02-25T22:22:23Z
updated_at: 2023-02-25T23:13:25Z
url: https://github.com/astral-sh/ruff/pull/3228
synced_at: 2026-01-12T04:39:44Z
```

# Add `noqa_row` to diagnostics JSON format

---

_Pull request opened by @bluetech on 2023-02-25 22:22_

In ruff-lsp (https://github.com/charliermarsh/ruff-lsp/pull/76) we want to add a "Disable \<rule\> for this line" quickfix. However, finding the correct line into which the `noqa` comment should be inserted is non-trivial (multi-line strings for example).

Ruff already has this info, so expose it in the JSON output for use by ruff-lsp.

---

_Comment by @bluetech on 2023-02-25 22:23_

Initially I thought to add it to `Violation` but that's not really possible. So I added it to `Message` which is much more straightforward. But if there's a more preferable way, please let me know.

---

_@charliermarsh reviewed on 2023-02-25 22:28_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/integration_test.rs`:116 on 2023-02-25 22:28_

Now that this is part of the public API, I'm wondering if it should be `noqa_row`, for consistency with `row` in `location`. What do you think?

---

_Review comment by @bluetech on `crates/ruff_cli/tests/integration_test.rs`:116 on 2023-02-25 22:31_

Makes sense. I'll change it in `Message` and the JSON; if you'd like me to change all of the existing places as well, can do that as well.

---

_@bluetech reviewed on 2023-02-25 22:31_

---

_@charliermarsh reviewed on 2023-02-25 22:34_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/integration_test.rs`:116 on 2023-02-25 22:34_

I think it'd be best to change it everywhere, but I'd be fine merging this with just the public API changes.

---

_Renamed from "Add `noqa_line` to diagnostics JSON format" to "Add `noqa_row` to diagnostics JSON format" by @bluetech on 2023-02-25 22:37_

---

_Review comment by @bluetech on `crates/ruff_cli/tests/integration_test.rs`:116 on 2023-02-25 22:37_

OK, made the API change, will send a PR doing the internal renaming later.

---

_@bluetech reviewed on 2023-02-25 22:37_

---

_Merged by @charliermarsh on 2023-02-25 23:13_

---

_Closed by @charliermarsh on 2023-02-25 23:13_

---

_Comment by @charliermarsh on 2023-02-25 23:13_

Thanks!

---
