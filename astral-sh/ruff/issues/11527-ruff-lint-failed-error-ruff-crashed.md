```yaml
number: 11527
title: "Ruff: Lint failed - error: Ruff crashed."
type: issue
state: closed
author: david-waterworth
labels:
  - needs-info
assignees: []
created_at: 2024-05-24T07:04:26Z
updated_at: 2024-08-17T14:37:00Z
url: https://github.com/astral-sh/ruff/issues/11527
synced_at: 2026-01-10T11:09:53Z
```

# Ruff: Lint failed - error: Ruff crashed.

---

_Issue opened by @david-waterworth on 2024-05-24 07:04_

I got this message as a 'pop-up' from vscode
```
Ruff: Lint failed (
error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs:215:18:
called `Option::unwrap()` on a `None` value
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
)
```

I guess it's from the vscode extension (extension v2024.22.0)

My pyproject.toml contains

```[tool.ruff]
line-length = 120
select = [
    "E",  # pycodestyle errors (settings from FastAPI, thanks, @tiangolo!)
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "C",  # flake8-comprehensions
    "B",  # flake8-bugbear
]
ignore = [
    "E501",  # line too long, handled by black
    "C901",  # too complex
]
per-file-ignores = { "*.ipynb" = ["E402", "F704", "B018"] }

[tool.ruff.isort]
order-by-type = true
relative-imports-order = "closest-to-furthest"
extra-standard-library = ["typing"]
section-order = ["future", "standard-library", "third-party", "first-party", "local-folder"]
known-first-party = []
```

---

_Label `server` added by @snowsignal on 2024-05-24 07:17_

---

_Assigned to @snowsignal by @snowsignal on 2024-05-24 07:17_

---

_Comment by @dhruvmanila on 2024-05-24 07:20_

Can you provide the source code on which it crashed? Also, the Ruff version used by the extension? There's just one `unwrap` in the file which shouldn't really fail.

---

_Label `server` removed by @MichaReiser on 2024-05-27 11:00_

---

_Label `rule` added by @MichaReiser on 2024-05-27 11:00_

---

_Label `rule` removed by @charliermarsh on 2024-05-28 01:27_

---

_Label `needs-info` added by @charliermarsh on 2024-05-28 01:27_

---

_Comment by @david-waterworth on 2024-05-28 22:32_

Sorry I have no idea what code it crashed on and I closed vscode shortly after seeing the error, and the pop-up message doesn't say (I think it was from the vscode notification system - the bell icon in the lower rhs of the bottom window border). If it happens again I'll try and figure it out - I've seen it several times but not been able to work out what it means.

I'm using extension version v2024.22.0 which according to the docs ships with `ruff==0.4.5` (I also have ruff==0.4.6 installed globally via pipx and my project uses the ruff precommit hook (https://github.com/astral-sh/ruff-pre-commit) which is version 0.4.4 - so I'm only assuming it was the version from the extension that was running because the other two shouldn't be running in the background afaik.

---

_Comment by @charliermarsh on 2024-05-28 22:35_

All good @david-waterworth. That trace usually means Ruff had a problem with the specific contents of the file, so if you see it again, do you mind sharing them here (if you can)? Otherwise hard for us to resolve.

---

_Comment by @MichaReiser on 2024-08-17 14:36_

I'll close this for now as there's nothing actionable for us to do. @david-waterworth let us know if you still run into this and I'm happy to re-open the issue.

---

_Closed by @MichaReiser on 2024-08-17 14:36_

---
