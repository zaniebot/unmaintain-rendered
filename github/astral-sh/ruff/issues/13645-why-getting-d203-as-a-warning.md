---
number: 13645
title: Why getting D203 as a warning?
type: issue
state: closed
author: naveenmaan
labels:
  - question
  - configuration
  - docstring
assignees: []
created_at: 2024-10-05T17:12:42Z
updated_at: 2024-11-20T12:01:56Z
url: https://github.com/astral-sh/ruff/issues/13645
synced_at: 2026-01-07T13:12:15-06:00
---

# Why getting D203 as a warning?

---

_Issue opened by @naveenmaan on 2024-10-05 17:12_

I am working on trying to run a linter over my script. which has some lint issues. 
I want my output as JSON. But I am getting warning before my json.

```shell
python -m ruff check screen.py --output-format=json
```

```shell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
```

---

_Label `question` added by @AlexWaygood on 2024-10-05 17:28_

---

_Label `configuration` added by @AlexWaygood on 2024-10-05 17:28_

---

_Label `docstring` added by @AlexWaygood on 2024-10-05 17:28_

---

_Comment by @AlexWaygood on 2024-10-05 18:35_

Hiya! The warning is telling you that you've enabled two rules that would tell you to do contradictory things. That's obviously going to cause some confusing output from Ruff :)

Do you have a `pyproject.toml`, `ruff.toml` or `.ruff.toml` file somewhere where you've listed the rules you wanted enabled? If so, maybe you could share that, and we might be able to see where the problem is.

The documentation pages for these rules can be found here: https://docs.astral.sh/ruff/rules/#pydocstyle-d

---

_Comment by @MichaReiser on 2024-10-07 08:42_

The warning messages are written to `stderr`. To get the json output, redirect it e.g. to a file.

```bash
python -m ruff check screen.py --output-format=json > result.json
```

---

_Closed by @MichaReiser on 2024-11-20 12:01_

---
