```yaml
number: 12290
title: "ruff doesn't find the same E721 violations as new flake8 does"
type: issue
state: closed
author: timj
labels:
  - bug
assignees: []
created_at: 2024-07-11T16:27:07Z
updated_at: 2024-07-15T08:57:10Z
url: https://github.com/astral-sh/ruff/issues/12290
synced_at: 2026-01-10T11:09:54Z
```

# ruff doesn't find the same E721 violations as new flake8 does

---

_Issue opened by @timj on 2024-07-11 16:27_

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

I searched for E721 but couldn't find a previous ticket.

```$ /opt/homebrew/bin/ruff --version
ruff 0.5.1
```

For the file https://github.com/lsst/geom/blob/main/tests/test_coordinates.py flake8 7.1.0 reports this error:

```
tests/test_coordinates.py:299:16: E721 do not compare types, for exact checks use `is` / `is not`, for instance checks use `isinstance()`
```

The relevant line is:

```python
if type(result) != expected:
```

ruff 0.5.1 does not report any E721 violation. There is no ruff configuration in this directory when I clone.

I have other examples of inconsistency. In https://github.com/lsst/fgcm flake8 reports:

```
/fgcm/fgcmConfig.py:30:20: E721 do not compare types, for exact checks use `is` / `is not`, for instance checks use `isinstance()`
./fgcm/fgcmConfig.py:33:20: E721 do not compare types, for exact checks use `is` / `is not`, for instance checks use `isinstance()`
./fgcm/fgcmConfig.py:59:16: E721 do not compare types, for exact checks use `is` / `is not`, for instance checks use `isinstance()`
```

ruff reports:

```
fgcm/fgcmConfig.py:33:20: E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
   |
31 |                     raise TypeError("Default is the wrong datatype.")
32 |             if self._value is not None:
33 |                 if type(self._value) != datatype:
   |                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E721
34 |                     raise TypeError("Value is the wrong datatype.")
   |

fgcm/fgcmConfig.py:59:16: E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
   |
58 |         if self._datatype is not None:
59 |             if type(self._value) != self._datatype:
   |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ E721
60 |                 raise ValueError("Datatype mismatch for %s (got %s, expected %s)" %
61 |                                  (name, str(type(self._value)), str(self._datatype)))
   |

fgcm/sharedNumpyMemManager.py:100:15: E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
    |
 98 |         elif (dtype == np.int16):
 99 |             ctype = ctypes.c_int16
100 |         elif (dtype == bool):
    |               ^^^^^^^^^^^^^ E721
101 |             ctype = ctypes.c_bool
102 |         else:
    |
```

where two of the warnings agree but flake8 is reporting one from line 30 of `fgcmConfig.py` which ruff does not report, and ruff reports one from `sharedNumpyMemManager.py` that flake8 does not report.




---

_Comment by @timj on 2024-07-11 16:46_

I see #9570 special cases numpy, which explains why line 98 above does not trigger the warning.

---

_Comment by @zanieb on 2024-07-11 18:11_

Hm I'm not sure why we're not raising a violation there. It looks like a false negative as far as I can tell?

Note some more context on false positives for this rule at https://github.com/astral-sh/ruff/issues/4560

---

_Comment by @timj on 2024-07-11 19:43_

A different example from https://github.com/RobertLuptonTheGood/eups.

The code:

```python
return isinstance(self, EupsCmd) and type(self) != EupsCmd
```
triggers E721 in flake8 but not in ruff.

```
python/eups/cmd.py:253:46: E721 do not compare types, for exact checks use `is` / `is not`, for instance checks use `isinstance()`
```

ruff does find 3 places that flake8 doesn't though.

---

_Comment by @timj on 2024-07-11 21:12_

I'm also seeing cases where `type(x) != type(y)` is not triggering a warning in ruff but will in flake8.

---

_Comment by @charliermarsh on 2024-07-12 12:22_

> I'm also seeing cases where type(x) != type(y) is not triggering a warning in ruff but will in flake8.

Can you give a concrete example of this?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 12:35_

---

_Label `bug` added by @charliermarsh on 2024-07-12 12:35_

---

_Comment by @charliermarsh on 2024-07-12 12:35_

I believe these are fixed by https://github.com/astral-sh/ruff/pull/12300.

---

_Closed by @charliermarsh on 2024-07-12 13:21_

---

_Comment by @timj on 2024-07-12 15:37_

> Can you give a concrete example of this?

Sorry. That was bad of me.

The code in question was https://github.com/lsst/ip_isr/blob/main/python/lsst/ip/isr/calibType.py#L139

```python
elif type(attrSelf) != type(attrOther):
```
flake8 gave:
```
python/lsst/ip/isr/calibType.py:139:18: E721 do not compare types, for exact checks use `is` / `is not`, for instance checks use `isinstance()`
```
and ruff said nothing.

```
$ ruff --version
ruff 0.5.1
$ ruff check --select E721
All checks passed!
```


---

_Comment by @dhruvmanila on 2024-07-15 08:57_

@timj No worries! It's fixed and released in the latest version (`0.5.2`): https://play.ruff.rs/59525efe-9d7f-4683-83a9-808f15caacd6

---
