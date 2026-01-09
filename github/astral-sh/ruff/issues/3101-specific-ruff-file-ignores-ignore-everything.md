---
number: 3101
title: Specific ruff file ignores ignore everything
type: issue
state: closed
author: henryiii
labels:
  - question
assignees: []
created_at: 2023-02-21T22:11:36Z
updated_at: 2023-02-22T01:56:59Z
url: https://github.com/astral-sh/ruff/issues/3101
synced_at: 2026-01-07T13:12:14-06:00
---

# Specific ruff file ignores ignore everything

---

_Issue opened by @henryiii on 2023-02-21 22:11_

The following file:

```python
def main(x):
    a = 1
```

Shows two errors.

```console
$ ruff check --isolated --extend-select ARG tmp.py
tmp.py:2:10: ARG001 Unused function argument: `x`
tmp.py:3:5: F841 [*] Local variable `a` is assigned to but never used
Found 2 errors.
[*] 1 potentially fixable with the --fix option.
```

If I add this to the top of the file:

```python
# ruff: noqa: F821
```

Then both errors disappear, instead of just one. In general, it seems that this is just a blanket noqa. (Also, this should not be too picky - `ruff: noqa F821` is an easy mistake to make! I think I'd require a `#` to put a comment on the same line, if it was my choice).

See https://github.com/pybind/pybind11/pull/4483#discussion_r1113570639.



---

_Comment by @charliermarsh on 2023-02-21 22:12_

Do you know what version this is on?

---

_Comment by @charliermarsh on 2023-02-21 22:13_

In #2978, we added support for directives like `# ruff: noqa: F821` which ignore a specific error.

(Note that Flake8 treats `# flake8: noqa: F821` as a blanket ignore -- as in, with Flake8, that directive will ignore _all_ errors like you're seeing above.)

---

_Label `question` added by @charliermarsh on 2023-02-21 22:13_

---

_Comment by @henryiii on 2023-02-21 22:50_

Locally, my version is a bit older due to being from homebrew - 0.0.245. In pre-commit in the original issue, it should have ~~been the latest version, but I can check~~ - ahh, no, it's 0.0.247. I knew that was recent, but not quite how recent. It would be really helpful if `# ruff: ignore<stuff>` was an error unless `<stuff>` was recognized and supported or started with a `#`.

I'll check and report back later tonight!

---

_Comment by @charliermarsh on 2023-02-22 00:25_

So as of that PR, we do throw warnings -- e.g., if you do `# ruff: noqa F401`:

```
warning: Unexpected suffix on `noqa` directive: "# ruff: noqa F401"
```

---

_Comment by @charliermarsh on 2023-02-22 00:25_

(I think that feature shipped in 0.0.248.)

---

_Comment by @henryiii on 2023-02-22 01:56_

It oddly prints the warning three times:

```
warning: Unexpected suffix on `noqa` directive: "# ruff: noqa SIM201 SIM300 SIM202"
warning: Unexpected suffix on `noqa` directive: "# ruff: noqa SIM201 SIM300 SIM202"
warning: Unexpected suffix on `noqa` directive: "# ruff: noqa SIM201 SIM300 SIM202"

Fixed 19 errors:
- tests/test_enum.py:
    10 × SIM201 (negate-equal-op)
     7 × SIM300 (yoda-conditions)
     2 × SIM202 (negate-not-equal-op)
```

(And I'd be quite fine to have some sort of way to make all warnings errors).

But it does work correctly, and warns as expected, in 0.0.249 (and therefore I'd assume 0.0.251)! Sorry for the noise, I should have tried an update first.

---

_Closed by @henryiii on 2023-02-22 01:56_

---
