```yaml
number: 2621
title: Fixable error is not fixed
type: issue
state: closed
author: WhyNotHugo
labels:
  - fixes
assignees: []
created_at: 2023-02-07T11:31:52Z
updated_at: 2023-02-09T02:28:30Z
url: https://github.com/astral-sh/ruff/issues/2621
synced_at: 2026-01-10T11:09:45Z
```

# Fixable error is not fixed

---

_Issue opened by @WhyNotHugo on 2023-02-07 11:31_



Sample code: https://github.com/WhyNotHugo/python-barcode/blob/f824d00592daabbe62a9c9a68b7a3113c215207d/barcode/writer.py#L264

```console
> ruff .      
barcode/writer.py:264:17: SIM108 [*] Use ternary operator `xpos = bxs + (bxe - bxs) / 2.0 if self.center_text else bxs` instead of if-else-block
Found 1 error.
[*] 1 potentially fixable with the --fix option.

> ruff --fix .
barcode/writer.py:264:17: SIM108 [*] Use ternary operator `xpos = bxs + (bxe - bxs) / 2.0 if self.center_text else bxs` instead of if-else-block
Found 1 error.
```

But nothing is fixed. `pyproject.toml`:

```toml
[build-system]
requires = ["setuptools>=45", "wheel", "setuptools_scm>=6.2"]

[tool.setuptools_scm]
write_to = "barcode/version.py"
version_scheme = "post-release"

[tool.black]
target-version = ['py37']

[tool.ruff]
select = [
    "E",
    "F",
    "W",
    "B0",
    "I",
    "UP",
    "N",
    "C4",
    "PT",
    "SIM",
    "TID",
]
target-version = "py37"

[tool.ruff.isort]
force-single-line = true
```




---

_Comment by @charliermarsh on 2023-02-07 15:42_

The problem is likely that fixing this error would result in a line that exceeds the specified line length.


---

_Label `autofix` added by @charliermarsh on 2023-02-07 15:42_

---

_Comment by @charliermarsh on 2023-02-07 15:42_

Which is intended -- that we skip the fix -- but could be better communicated in the output.

---

_Comment by @ngnpope on 2023-02-07 15:48_

Also related: https://github.com/charliermarsh/ruff/issues/1766, https://github.com/charliermarsh/ruff/issues/1791.

It would be hard to know what to do with the placement of the comment too which may no longer make sense.

I guess it does say "potentially fixable" 😅 

---

_Comment by @WhyNotHugo on 2023-02-07 15:49_

Makes perfect sense to skip the change if there are comments.

But also agree that output should make clear what's up.

---

_Comment by @charliermarsh on 2023-02-07 15:49_

We should probably detect that we _won't_ fix that case, and not add the `[*]`, and not include it in the list of potentially fixable errors.

---

_Comment by @charliermarsh on 2023-02-08 21:12_

(Oh sorry, I didn't actually look at the code -- it is indeed because of the comment, that we avoid fixing it. But we should still make that clear.)

---

_Comment by @spaceone on 2023-02-08 21:15_

shouldn't the comment cause that it's not detected as `SIM108` as the comment is the reason that it should not be written as one-liner?

---

_Comment by @charliermarsh on 2023-02-08 21:18_

We used to have that behavior but I changed it. It led to confusing behavior like https://github.com/charliermarsh/ruff/issues/2442 whereby adding a `noqa` comment would turn off that check and lead to an unused `noqa`.

---

_Closed by @charliermarsh on 2023-02-09 02:28_

---
