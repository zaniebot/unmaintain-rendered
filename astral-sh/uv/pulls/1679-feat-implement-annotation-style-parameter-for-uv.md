```yaml
number: 1679
title: "feat: Implement `--annotation-style` parameter for `uv pip compile`"
type: pull_request
state: merged
author: drjackild
labels:
  - configuration
assignees: []
merged: true
base: main
head: feature/implement-annotation-style
created_at: 2024-02-19T02:37:22Z
updated_at: 2024-02-21T13:46:37Z
url: https://github.com/astral-sh/uv/pull/1679
synced_at: 2026-01-12T16:04:41Z
```

# feat: Implement `--annotation-style` parameter for `uv pip compile`

---

_@drjackild_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Hello there! The motivation for this feature is described here #1678 

## Test Plan

I've added unit tests and also tested this manually on my work project by comparing it to the original `pip-compile` output - it looks much like the `pip-compile` generated lock file.


---

_Renamed from "Implement `--annotation-style` parameter for `uv pip compile`" to "feat: Implement `--annotation-style` parameter for `uv pip compile`" by @drjackild on 2024-02-19 02:38_

---

_Comment by @zanieb on 2024-02-19 03:40_

Thanks for contributing! Welcome to the project :)

I'm not entirely sure we want to accept more configuration here, I'll need to check with @charliermarsh.

It looks like the Windows snapshots are failing. We automatically filter out some Windows-only dependencies, but it's failing since they're not separated by a newline anymore. You'll need to adjust the regex at https://github.com/astral-sh/uv/blob/ad12d97e714aefdf79267faa8c51a36d4783459c/crates/uv/tests/common/mod.rs#L261-L264

---

_Label `configuration` added by @zanieb on 2024-02-19 03:40_

---

_@frayandy approved on 2024-02-19 04:05_

---

_Comment by @drjackild on 2024-02-19 10:51_

@zanieb I hope you'll accept this one :) Regarding the tests - fixed!

---

_@charliermarsh reviewed on 2024-02-20 04:59_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:428 on 2024-02-20 04:59_

Can you walk me through `line:24` here?

---

_Comment by @charliermarsh on 2024-02-20 05:00_

I'm happy to include this.

---

_@drjackild reviewed on 2024-02-20 09:29_

---

_Review comment by @drjackild on `crates/uv-resolver/src/resolution.rs`:428 on 2024-02-20 09:29_

Sure! I'm trying to keep the same logic, as `pip-tools`, to minimize the diff in the output file. The align `24` is something they use in the `pip-tools` as a reasonable number to align the annotation comments: https://github.com/jazzband/pip-tools/blob/main/piptools/writer.py#L318 and I agree with them. Most of the time annotation will be aligned except for some extra-long dependencies.

```requirements.txt
monotonic==1.6            # via analytics-python
oauthlib==2.1.0           # via pyproject.toml
opentelemetry-api==1.22.0  # via ddtrace
packaging==21.3           # via gunicorn, pyproject.toml
prompt-toolkit==3.0.43    # via click-repl
protobuf==4.25.3          # via ddsketch, ddtrace
```

---

_@charliermarsh reviewed on 2024-02-20 18:04_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:428 on 2024-02-20 18:04_

Ahh okay, thank you, perfect!

I'm wondering if we should just put more of this method within a `match` on the annotation style. I might find the logic easier to follow, but I'd need to see it to know for sure.

---

_@drjackild reviewed on 2024-02-20 22:00_

---

_Review comment by @drjackild on `crates/uv-resolver/src/resolution.rs`:428 on 2024-02-20 22:00_

Do you mean to extract `annotation_line` and `annotation_split` onto the `match` statement? I could do this, they're not very big, should be fine

---

_@drjackild reviewed on 2024-02-20 22:56_

---

_Review comment by @drjackild on `crates/uv-resolver/src/resolution.rs`:428 on 2024-02-20 22:56_

Yeah, you're right, it's easier to read without those function calls :)

---

_Comment by @drjackild on 2024-02-20 23:19_

I have no idea, why it's failing 🤔

UPD: Fixed 😊

---

_Assigned to @charliermarsh by @zanieb on 2024-02-21 00:27_

---

_Comment by @charliermarsh on 2024-02-21 00:29_

Will review and merge tonight, hopefully.

---

_@charliermarsh approved on 2024-02-21 01:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:429 on 2024-02-21 02:00_

@DrJackilD -- I just tweaked this to couple the presence of the comment and separator.

---

_@charliermarsh reviewed on 2024-02-21 02:00_

---

_Merged by @charliermarsh on 2024-02-21 02:08_

---

_Closed by @charliermarsh on 2024-02-21 02:08_

---

_Comment by @charliermarsh on 2024-02-21 02:20_

Thank you! Great contribution.

---

_Comment by @drjackild on 2024-02-21 11:52_

> Thank you! Great contribution.

Thank you for accepting it so quick!

---

_Branch deleted on 2024-02-21 13:46_

---
