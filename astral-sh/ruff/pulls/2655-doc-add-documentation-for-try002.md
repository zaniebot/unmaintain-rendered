```yaml
number: 2655
title: "doc: add documentation for TRY002"
type: pull_request
state: merged
author: nunokaeru
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2023-02-08T10:05:59Z
updated_at: 2023-02-08T16:05:00Z
url: https://github.com/astral-sh/ruff/pull/2655
synced_at: 2026-01-12T04:52:00Z
```

# doc: add documentation for TRY002

---

_Pull request opened by @nunokaeru on 2023-02-08 10:05_

Add documentation for `Tryceratops TC002` or as it's know in Ruff `TRY002`

This is a copy from the upstream Tryceratops repository.
I think there should also be a section for linking the original repository and documentation.

Follows up on issue:
https://github.com/charliermarsh/ruff/issues/1467

---

_Review comment by @bluetech on `crates/ruff/src/rules/tryceratops/rules/raise_vanilla_class.rs`:14 on 2023-02-08 15:12_

FWIW, I am unable to understand these explanations. I would write something like this instead (disregarding `BaseException`'s existence for clarity):

```suggestion
    /// ### What it does
    /// Checks for code which raises `Exception(...)` directly.
    ///
    /// ## Why is this bad?
    /// In order to handle such an exception, the handling code must use `except Exception`,
    /// which catches *any* raised exception, including failed assertions, division by zero,
    /// and others.
    ///
    /// Prefer to raise your own custom subclass of `Exception`. Then the handling code can
    /// catch only your specific exception type, without over-catching unintended
    /// exceptions.
```

---

_@bluetech reviewed on 2023-02-08 15:12_

---

_@nunokaeru reviewed on 2023-02-08 15:15_

---

_Review comment by @nunokaeru on `crates/ruff/src/rules/tryceratops/rules/raise_vanilla_class.rs`:14 on 2023-02-08 15:15_

I just copied over from `Tryceratops` since I don't know what the repository stance is on these topics.

If I am allowed to write better explanations over using the existing ones I will do so!

Thanks for the help.

---

_Comment by @nunokaeru on 2023-02-08 15:17_

Any idea why the build is failing? Could use some help on it, thought it was easy pickings but cargo is complaining.
Would not expect comments to fail an entire build but I also don't know much about rust so 🤷‍♂️ 

---

_Comment by @charliermarsh on 2023-02-08 15:43_

@nm-remarkable - Thanks! I might top of the doc for consistency but will merge this. You need to run `cargo dev generate-all` since we auto-generate parts of the README etc. based on the inline documentation. But I can also run it prior to merging, not a big deal.

---

_Merged by @charliermarsh on 2023-02-08 16:04_

---

_Closed by @charliermarsh on 2023-02-08 16:04_

---

_Label `documentation` added by @charliermarsh on 2023-02-08 16:05_

---
