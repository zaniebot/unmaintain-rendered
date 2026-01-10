```yaml
number: 1468
title: "Is there a way to not check the whole file like ruff's \"# ruff: noqa\""
type: issue
state: closed
author: monchin
labels:
  - cli
  - suppression
assignees: []
created_at: 2025-11-03T08:09:38Z
updated_at: 2025-11-05T04:10:20Z
url: https://github.com/astral-sh/ty/issues/1468
synced_at: 2026-01-10T02:06:25Z
```

# Is there a way to not check the whole file like ruff's "# ruff: noqa"

---

_Issue opened by @monchin on 2025-11-03 08:09_

### Question

Say I'm using ty to check my codes and I put it as one step of my pre-commit. Although I set "include" in `pyproject.toml` to just check some specified files, if I change some file not in "include" and be to commit it, pre-commit would still let ty to check it.

I know that ruff has a feature to ignore the entire file, that is writing a comment "# ruff: noqa" on the very top of the file. So I wonder if ty has similar features or other method to ignore the entire file. Thank you!

### Version

ty 0.0.1a24

---

_Label `question` added by @monchin on 2025-11-03 08:09_

---

_Label `suppression` added by @AlexWaygood on 2025-11-03 12:06_

---

_Comment by @MichaReiser on 2025-11-03 12:47_

I think you can add a `type: ignore` at the top of the file, as seen [here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/suppressions/type_ignore.md#file-level-suppression)

However, I'd consider this more a workaround. The "proper" solution on the ty side is to add a [`--force-exclude`](https://docs.astral.sh/ruff/settings/#force-exclude) option similar to the one in ruff, that force exclusion even for path explicitly passed to ty.

Related to https://github.com/astral-sh/ty/issues/269

---

_Label `question` removed by @MichaReiser on 2025-11-03 12:47_

---

_Label `cli` added by @MichaReiser on 2025-11-03 12:47_

---

_Comment by @MichaReiser on 2025-11-03 12:57_

Another option is that you repeat your `exclude` configuration in pre-commit https://pre-commit.com/#regular-expressions

I understand that this is rather annoying because you want a single source of truth for which files are excluded but it avoids the need to clutter your files with `type: ignore` comments.

I also opened  https://github.com/astral-sh/ty/issues/1469 to add a new CLI flag to improve the pre-commit experience.

---

_Comment by @AlexWaygood on 2025-11-03 13:00_

> Another option is that you repeat your `exclude` configuration in pre-commit https://pre-commit.com/#regular-expressions

A better pre-commit-specific solution would probably be to set `pass_filenames: false` for the hook (https://pre-commit.com/#hooks-pass_filenames)

---

_Comment by @MichaReiser on 2025-11-03 13:23_

> A better pre-commit-specific solution would probably be to set pass_filenames: false for the hook ([pre-commit.com#hooks-pass_filenames](https://pre-commit.com/#hooks-pass_filenames))

Oh nice. That should work well for as long as all files have no errors. I suspect that this would lead to errors being shown for unstaged files.

---

_Comment by @monchin on 2025-11-05 04:10_

Sorry for late reply. Thank you very much!

---

_Closed by @monchin on 2025-11-05 04:10_

---
