```yaml
number: 10845
title: "Warning log: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation."
type: issue
state: closed
author: ancalita
labels:
  - question
  - formatter
assignees: []
created_at: 2024-04-09T10:55:12Z
updated_at: 2024-04-09T12:42:34Z
url: https://github.com/astral-sh/ruff/issues/10845
synced_at: 2026-01-10T11:09:53Z
```

# Warning log: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.

---

_Issue opened by @ancalita on 2024-04-09 10:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I'm replacing black formatter with ruff formatter and when reformatting the codebase with `ruff format` command, I keep seeing this warning:
```
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
```

Could you please confirm if the formatter is stable and not likely to be removed in the future? If so, please consider this warning log since it's very confusing and makes users reluctant to use.


---

_Label `formatter` added by @AlexWaygood on 2024-04-09 11:07_

---

_Comment by @AlexWaygood on 2024-04-09 11:07_

Hi! Would you mind pasting the output you get when you run `ruff --version` in your terminal?

---

_Comment by @ancalita on 2024-04-09 11:16_

@AlexWaygood sure, this is the result:
```
ruff 0.0.291
```

---

_Comment by @153957 on 2024-04-09 11:35_

Please try a more recent version of ruff (latest currently is 0.3.5)
I don't think the formatter will be removed, but (just like for black) some choices in how code is formatted may change.

---

_Comment by @AlexWaygood on 2024-04-09 11:45_

Yeah, as @153957 says, 0.0.291 is quite an old version of Ruff :-) If you use `ruff>=0.2`, the message should go away.

With regards to your broader question on the stability of the formatter: we describe the stability of the formatter in our versioning policy, which you can find here: https://docs.astral.sh/ruff/versioning/. In a nutshell: we promise to do our utmost to ensure that Ruff's "stable" formatting style will never change in a patch release -- this means that as long as your code contains valid syntax, bumping Ruff's version from e.g. 0.3.5 to 0.3.6 shouldn't result in the formatter reformatting your code. (If you bump Ruff from 0.3.6 to 0.4.0, however, it _might_ reformat your code.)

---

_Label `question` added by @AlexWaygood on 2024-04-09 11:45_

---

_Comment by @ancalita on 2024-04-09 12:34_

Nice, thank you for the quick responses 🙌🏻 

---

_Comment by @AlexWaygood on 2024-04-09 12:42_

No worries! I'll close this for now, but let me know if you have any further questions :-)

---

_Closed by @AlexWaygood on 2024-04-09 12:42_

---
