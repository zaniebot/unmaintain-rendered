---
number: 7094
title: Rules F841 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-03T18:30:59Z
updated_at: 2023-09-03T21:00:45Z
url: https://github.com/astral-sh/ruff/issues/7094
synced_at: 2026-01-07T13:12:15-06:00
---

# Rules F841 cause autofix error

---

_Issue opened by @qarmin on 2023-09-03 18:30_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select F841 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
gitm_varnames = {'Argon Mixing Ratio': ['r_Ar', 'linear', ''],
                 'Electron Energy Flux': ['ElectronEnergyFlux', 'exponential',
                                           ''],
                 'Total Absolute EUV': ['TotalAbsoluteEUV', 'linear',
                                           'K per timestep'],
              'AltIntNOCooling (W/m2)': 'linear'}
def _read(varlist, attrs, newfile=True):
    '''
    '''
    if len(rawRecLen) < 4:
        var.append(unpack(endChar+'%is' % (recLen), f.read(recLen))[0])
    attrs['time'] = datetime(yy, mm, dd, hh, mn, ss, ms).replace(
        tzinfo=timezone.utc).isoformat(sep=' ')
    (oldLen) = unpack(endChar+'l', f.read(4))
```

error
```
/home/rafal/test/tmp_folder/6735003.py:14:6: F841 Local variable `oldLen` is assigned to but never used
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/6735003.py`, the rule codes F841, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```
        
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12506800/python_compressed.zip)


---

_Label `fuzzer` added by @zanieb on 2023-09-03 19:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-03 20:49_

---

_Label `bug` added by @charliermarsh on 2023-09-03 20:49_

---

_Referenced in [astral-sh/ruff#7110](../../astral-sh/ruff/pulls/7110.md) on 2023-09-03 20:51_

---

_Closed by @charliermarsh on 2023-09-03 21:00_

---
