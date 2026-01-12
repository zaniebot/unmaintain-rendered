```yaml
number: 2519
title: Support multiple target versions
type: issue
state: closed
author: JonathanPlasse
labels:
  - question
assignees: []
created_at: 2023-02-03T09:11:30Z
updated_at: 2023-03-13T23:09:38Z
url: https://github.com/astral-sh/ruff/issues/2519
synced_at: 2026-01-12T15:54:42Z
```

# Support multiple target versions

---

_@JonathanPlasse_

- To implement #2039 we need to support multiple target versions.
- We should use the minimum target version for pyupgrade.
-  For isort known standard library, we should check that the module is present for all target version
- …
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Renamed from "Support multiple target version" to "Support multiple target versions" by @JonathanPlasse on 2023-02-03 09:12_

---

_Comment by @charliermarsh on 2023-02-03 18:16_

What's the difference in practice between supporting a minimum version, and a range of versions?

---

_Label `question` added by @charliermarsh on 2023-02-03 18:16_

---

_Comment by @JonathanPlasse on 2023-02-03 18:27_

For isort known standard library, you could have some modules that are removed for later versions.
If you use the minimum version, you would have a false negative for the later versions if you use a dropped module.

---

_Comment by @charliermarsh on 2023-02-05 23:56_

I guess I'm not sure _what_ should happen if you specify that your minimum version is `3.7`, and you import `tomllib`.

---

_Comment by @JonathanPlasse on 2023-02-12 14:48_

It should report this as an error as it is not available in the Python 3.7 standard library.

---

_Comment by @stinodego on 2023-02-26 19:56_

I know that `black` is actually trying to get rid of target ranges and wants to switch to a single minimum target version.

I don't think supporting target ranges is worthwhile.

> * To implement [RFE: infer `target-version` from project metadata #2039](https://github.com/charliermarsh/ruff/issues/2039) we need to support multiple target versions.

This is not true - you can infer a minimum target version from the `requires-python` specifier. That's how I initially implemented it for `black`, before I learned more about how their target versions work.

---

_Comment by @charliermarsh on 2023-02-26 23:27_

Yeah I'd like to only support a minimum target version until convinced otherwise (i.e., until that's proven insufficient).


---

_Comment by @charliermarsh on 2023-03-13 23:09_

Closing for now as I'm okay with current behavior (only supporting minimums).

---

_Closed by @charliermarsh on 2023-03-13 23:09_

---
