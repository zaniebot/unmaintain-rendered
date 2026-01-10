---
number: 4673
title: "`ARG001`/`ARG002` being thrown for `pytest` fixtures"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2023-05-26T19:50:26Z
updated_at: 2023-11-01T14:17:13Z
url: https://github.com/astral-sh/ruff/issues/4673
synced_at: 2026-01-10T01:22:43Z
---

# `ARG001`/`ARG002` being thrown for `pytest` fixtures

---

_Issue opened by @jamesbraza on 2023-05-26 19:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

### Motivation

Here is an overcomplicated minimal repro of using a fake filesystem from [`pyfakefs==5.2.2`](https://github.com/pytest-dev/pyfakefs), a very useful tool for unit testing with filesystems.  I am using `pytest==7.3.1`.

Aside: if you don't know `pyfakefs`, here's a snippet from its docs:

> Pyfakefs creates a new empty in-memory file system at each test start, which replaces the real filesystem during the test. Think of pyfakefs as making a per-test temporary directory, except for an entire file system.

```python
import csv
import pathlib

import pytest

THIS_DIRECTORY = pathlib.PurePath(__file__).parent

@pytest.fixture(name="this_dir_fs")
def fixture_this_dir_fs(fs):
    fs.add_real_directory(THIS_DIRECTORY, read_only=False)
    return fs

def test_with_numpy_genfromtxt(this_dir_fs) -> None:  # ARG001 gets thrown here
    csv_file = THIS_DIRECTORY / "stub.csv"
    with open(csv_file, mode="w", newline="", encoding="utf-8") as f:
       csv.writer(f).writerows([(1, 2), (3, 4)])
```

The fake filesystem `pytest` fixture is not supposed to be used, it's more of an inclusion in the signature.  It works more invisibly behind the scene, patching lots file I/O stuff.

Thus, we run into false positive `ARG001` with `ruff==0.0.270`:

```console
> ruff --select=ARG001 --isolated a.py
a.py:13:32: ARG001 Unused function argument: `this_dir_fs`
```

Similarly, if the test is moved to a test method, the error code thrown will be `ARG002`.

### Request

`pyfakefs` is one of many cases where a `pytest` fixture gets included in a test signature, but goes unused within the test's body.

Is there anything ruff can do for this?  Can `ruff` detect if an argument is a `pytest` fixture?

I am wondering if `ARG001`/`ARG002` should not be thrown for `pytest` fixtures in signatures.

For what it's worth, `pylint`'s `unused-argument` has the same problem.

---

_Renamed from "`ARG001` being thrown for `pytest` fixtures" to "`ARG001`/`ARG002` being thrown for `pytest` fixtures" by @jamesbraza on 2023-05-26 20:17_

---

_Comment by @calumy on 2023-05-26 23:34_

This issue could be avoided by using pytest’s usefixtures decorator, see: https://docs.pytest.org/en/7.1.x/how-to/fixtures.html#use-fixtures-in-classes-and-modules-with-usefixtures

---

_Comment by @jamesbraza on 2023-05-26 23:44_

Oh I forgot about that!  Using it no longer emits `ARG001`.

```python
import csv
import pathlib

import pytest

THIS_DIRECTORY = pathlib.PurePath(__file__).parent

@pytest.fixture(name="this_dir_fs")
def fixture_this_dir_fs(fs):
    fs.add_real_directory(THIS_DIRECTORY, read_only=False)
    return fs

@pytest.mark.usefixtures("this_dir_fs")
def test_with_numpy_genfromtxt() -> None:  # ARG001 is no longer thrown
    csv_file = THIS_DIRECTORY / "stub.csv"
    with open(csv_file, mode="w", newline="", encoding="utf-8") as f:
       csv.writer(f).writerows([(1, 2), (3, 4)])
```

Feel free to close this out, with the resolution to have `pytest.mark.usefixtures`

---

_Comment by @charliermarsh on 2023-05-27 00:33_

Oh, great :) Thanks @calumy.

---

_Closed by @charliermarsh on 2023-05-27 00:33_

---

_Comment by @sanga on 2023-11-01 14:17_

So as I understand it, the problem with 'usefixtures' is that it can't be used on fixtures (fixtures may depend upon other fixtures in pytest) only on tests. That being the case, how would the ruff project feel about the following solution? If one were able to define a regular expression that if a function matched, then the unused argument check would be disabled. That would allow pytest ruff uses to define for example

'''
arg_ignore_regex = "fix_|test_"
'''

And that would cause unused checking to be disabled for functions or methods that began with 'fix_' or 'test_'. And that would default to the empty string i.e. we would always check. I realise that this is somewhat ugly, but I suppose that pytest is pretty widely used and we are not the only ones suffering from this "unused argument" problem. Also, it would be basically invisible to anyone who didn't care about this problem, so the only cost would be the maintenance load of that code

If the ruff project thinks that that is an okay idea, I could look at sending in a PR for it. Assuming it's doable with a reasonable amount of effort. 

(Apologies for any potential formatting or grammar problems here I'm tapping this out on my phone)

---

_Referenced in [pytest-dev/pytest#12923](../../pytest-dev/pytest/issues/12923.md) on 2024-10-28 11:48_

---
