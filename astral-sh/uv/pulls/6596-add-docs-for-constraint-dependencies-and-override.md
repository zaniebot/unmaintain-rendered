```yaml
number: 6596
title: "Add docs for `constraint-dependencies` and `override-dependencies`"
type: pull_request
state: merged
author: Di-Is
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-docs-constraint-dependency
created_at: 2024-08-25T08:19:01Z
updated_at: 2024-08-26T23:40:06Z
url: https://github.com/astral-sh/uv/pull/6596
synced_at: 2026-01-12T16:07:26Z
```

# Add docs for `constraint-dependencies` and `override-dependencies`

---

_@Di-Is_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Add missing portions of documents reported in #6518 and #5248(Comment).

## Summary

<img width="600" alt="override" src="https://github.com/user-attachments/assets/062f0036-8672-4c68-b21c-aebdeb79b58b">

<img width="600" alt="constraint" src="https://github.com/user-attachments/assets/f5ef1aa2-0662-4352-a1a0-3af1127fb7fb">


---

_Marked ready for review by @Di-Is on 2024-08-25 08:43_

---

_Comment by @Di-Is on 2024-08-25 11:12_

I noticed later that the current project interface (e.g. `uv sync`) does not read `override-dependencies` and `constraint-dependencies` from uv.toml.

Should project interface commands respect `constraint-dependencies` and `override-dependencies` in this file?

If so, this needs to be addressed in a separate issue and PR.
~~If not, the uv.toml example needs to be removed from the added documentation.~~
(No need to remove `uv pip install/compile` as it uses uv.toml.)

---

_Comment by @charliermarsh on 2024-08-25 11:44_

> I noticed later that the current project interface (e.g. uv sync) does not read override-dependencies and constraint-dependencies from uv.toml.

I believe it does, but only from the workspace root. Are you seeing otherwise?

---

_Comment by @Di-Is on 2024-08-25 12:05_

> I believe it does, but only from the workspace root. Are you seeing otherwise?

The user-level configuration (~/.config/uv/uv.toml) might be worth respecting.

---

_Comment by @Di-Is on 2024-08-25 13:17_

For reference, it appears that cargo allows dependencies to be overridden from user-level settings.

https://doc.rust-lang.org/cargo/reference/config.html#patch

---

_Comment by @Di-Is on 2024-08-25 14:57_

> I believe it does, but only from the workspace root. Are you seeing otherwise?

Sorry, I misread your answer.

`uv pip install` and `uv pip compile` respect `override-dependencies` and `constraint-dependencies` in uv.toml.
I share a simple but reproducible command.

```bash
$ echo 'override-dependencies=["werkzeug==2.3.0"]' > ~/.config/uv/uv.toml
$ echo flask | uv pip compile -
~~~~
werkzeug==2.3.0
    # via
    #   --override (workspace)
    #   flask
```

---

_Comment by @charliermarsh on 2024-08-25 14:58_

I'm not sure... I consider this "data" rather than "configuration".

---

_Comment by @zanieb on 2024-08-25 15:08_

I think it's important for this to be scoped to the project to avoid different resolutions for a project per developer.

---

_Comment by @Di-Is on 2024-08-26 11:35_

> I think it's important for this to be scoped to the project to avoid different resolutions for a project per developer.

Ok, that is correct.
To avoid user confusion, I emphasized that only the setting values in pyproject.toml are valid for project interfaces(e.g. `uv lock`).

---

_Label `documentation` added by @zanieb on 2024-08-26 17:20_

---

_Assigned to @zanieb by @zanieb on 2024-08-26 17:20_

---

_@charliermarsh approved on 2024-08-26 23:32_

---

_Merged by @charliermarsh on 2024-08-26 23:40_

---

_Closed by @charliermarsh on 2024-08-26 23:40_

---
