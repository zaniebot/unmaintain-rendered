```yaml
number: 8212
title: "Improve errors / diagnostics when package is missing `__init__.py`"
type: issue
state: closed
author: drmason13
labels:
  - question
assignees: []
created_at: 2024-10-15T12:19:25Z
updated_at: 2024-12-26T16:33:50Z
url: https://github.com/astral-sh/uv/issues/8212
synced_at: 2026-01-10T04:36:20Z
```

# Improve errors / diagnostics when package is missing `__init__.py`

---

_Issue opened by @drmason13 on 2024-10-15 12:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv 0.4.18`
Running `uv build --package my-project` in a project (within a workspace, though I suspect a more minimal repro is possible) where the workspace member package is laid out in a typical style:

```
pyproject.toml
src/
    my_project/
        < distinct lack of __init__.py >
```

I've seen this error a few times:

> The most likely cause of this is that there is no directory that matches the name of your project (my_project)

but it can be a confusing and frustrating error when there *is* a directory with that name.

The actual problem is that python doesn't recognise the directory as a module/package due to the lack of `__init__.py`.

Forgetting/losing your `__init__.py` is a silly error to make but I think it's common enough that it is worth mentioning in uv's error that your directory also needs an `__init__.py`. You've done the hard part in naming the directory that is problematic.

I might suggest the following:

> The most likely cause of this is that there is no directory that matches the name of your project (my_project) or that directory does not contain an \_\_init\_\_.py file.

<details><summary>Before</summary>

```
ValueError: Unable to determine which files to ship inside the wheel using the following heuristics: https://hatch.pypa.io/latest/plugins/builder/wheel/#default-file-selection

The most likely cause of this is that there is no directory that matches the name of your project (my_project).

At least one file selection option must be defined in the `tool.hatch.build.targets.wheel` table, see: https://hatch.pypa.io/latest/config/build/

As an example, if you intend to ship a directory named `foo` that resides within a `src` directory located at the root of your project, you can define the following:

[tool.hatch.build.targets.wheel]
packages = ["src/foo"]
```

</details>

<details><summary>After</summary>

```
ValueError: Unable to determine which files to ship inside the wheel using the following heuristics: https://hatch.pypa.io/latest/plugins/builder/wheel/#default-file-selection

The most likely cause of this is that there is no directory that matches the name of your project (my_project) or that directory does not contain an __init__.py file.

At least one file selection option must be defined in the `tool.hatch.build.targets.wheel` table, see: https://hatch.pypa.io/latest/config/build/

As an example, if you intend to ship a directory named `foo` that resides within a `src` directory located at the root of your project, you can define the following:

[tool.hatch.build.targets.wheel]
packages = ["src/foo"]
```

</details>

Bonus points if uv actually checks and works out what is missing: \_\_init\_\_.py or the directory

---

_Comment by @charliermarsh on 2024-10-16 13:35_

Unfortunately those errors come from the build backend, which we don't control. (In your case, it's coming from `hatchling`.) I think this is something we can improve once we've shipped our own build backend (see #3957), but I'm hesitant to add our own error handling heuristics around this stuff since different build backends may treat these cases differently, they may rely on build backend-specific configuration, etc.

---

_Label `question` added by @charliermarsh on 2024-10-16 13:35_

---

_Comment by @drmason13 on 2024-10-16 16:13_

Ah I see, thanks for the clarification. I can go ask the same question at the hatchling repo.

Absolutely fine to close this, I agree it's not your issue. Thank you for your excellent work with uv, it's been a joy to use!

---

_Closed by @charliermarsh on 2024-12-26 16:33_

---
