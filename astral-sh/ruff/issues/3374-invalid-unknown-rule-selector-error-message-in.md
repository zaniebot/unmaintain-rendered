---
number: 3374
title: "Invalid \"unknown rule selector\" error message in some cases"
type: issue
state: closed
author: jvtm
labels:
  - bug
  - cli
assignees: []
created_at: 2023-03-06T23:31:19Z
updated_at: 2023-03-07T00:04:54Z
url: https://github.com/astral-sh/ruff/issues/3374
synced_at: 2026-01-10T01:22:42Z
---

# Invalid "unknown rule selector" error message in some cases

---

_Issue opened by @jvtm on 2023-03-06 23:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

There seems to be an invalid "unknown rule selector" error message when unknown `--select`  keyword prefix matches a known keyword.

Current Ruff version is 0.0.254.

Can be replicated without config file, or defining the similar setting in the config. Demonstrating command line only is easier.

This looks OK:
```
$ ruff check --select "X" .
error: Unknown rule selector `X`
```

This looks OK (doesn't remove first character)
```
$ ruff check --select "XX" .
error: Unknown rule selector `XX`
```

This gives broken token (`UG` instead of `BUG`):
```
$ ruff check --select "BUG" .
error: Unknown rule selector `UG`
```

Similarly:
```
$ ruff check --ignore "FAIL"
error: Unknown rule selector `AIL`
```

Also this fails (eating two characters):
```
$ ruff check --select "PLX" .
error: Unknown rule selector `X`
```

`B` and `F` are legit selectors, and so are `PL`, `PLC`, `PLE`, ...

Bonus -- I think this is OK :smile: 
```
$ ruff check --select ""
error: Unknown rule selector ``
```

Same effect when:
* using config file
* using `--ignore` switch / setting, and maybe others

Additionally the message written to stderr is missing a newline, which is a bit annoying.

---

_Comment by @charliermarsh on 2023-03-06 23:33_

Hm yeah, I guess the message here is confusing but the logic is "correct". When we match `BUG`, we match `B` to `flake8-bugbear`, then attempt to find the `UG` rule within the `flake8-bugbear` rule set.

---

_Label `bug` added by @charliermarsh on 2023-03-06 23:33_

---

_Label `cli` added by @charliermarsh on 2023-03-06 23:33_

---

_Comment by @jvtm on 2023-03-06 23:37_

Thanks for the quick response.

I think printing the whole token would be nicer to end-user, even when the whole alphabetic prefix is legit.

For example this feels weird:
```
$ ruff check --ignore ANN555
error: Unknown rule selector `555`
```


---

_Comment by @charliermarsh on 2023-03-06 23:38_

Yeah totally agree.

---

_Referenced in [astral-sh/ruff#3375](../../astral-sh/ruff/pulls/3375.md) on 2023-03-06 23:43_

---

_Closed by @charliermarsh on 2023-03-07 00:04_

---
