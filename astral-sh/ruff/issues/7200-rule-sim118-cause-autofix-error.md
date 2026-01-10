---
number: 7200
title: Rule SIM118 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-06T15:32:06Z
updated_at: 2023-09-07T15:16:25Z
url: https://github.com/astral-sh/ruff/issues/7200
synced_at: 2026-01-10T01:22:46Z
---

# Rule SIM118 cause autofix error

---

_Issue opened by @qarmin on 2023-09-06 15:32_


Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select SIM118 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
class TestMTH5(unittest.TestCase):
        channel_group = self.mth5_obj.get_channel(
        )
        for key in self.experiment.surveys[0].to_dict(single=True).keys():
                self.assertEqual(
                )
        for key in (
        ):
                    self.assertEqual(
                    )
        for key in (
            self.experiment.surveys[0]
            .stations[0]
            .keys()
        ):
                continue
```

error
```
/home/rafal/test/tmp_folder/35_IDX_0_RAND_169295356978468205385328.py:4:13: SIM118 Use `key in self.experiment.surveys[0].to_dict(single=True)` instead of `key in self.experiment.surveys[0].to_dict(single=True).keys()`
/home/rafal/test/tmp_folder/35_IDX_0_RAND_169295356978468205385328.py:11:13: SIM118 Use `key in self.experiment.surveys[0]
            .stations[0]` instead of `key in self.experiment.surveys[0]
            .stations[0].keys()`
Found 2 errors.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/35_IDX_0_RAND_169295356978468205385328.py`, the rule codes SIM118, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12539686/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2023-09-06 15:50_

---

_Label `fuzzer` added by @MichaReiser on 2023-09-06 15:50_

---

_Comment by @charliermarsh on 2023-09-06 16:06_

@qarmin -- Thanks for reporting these, it's much appreciated. Do you mind holding off on filing additional fuzzer issues for autofixes while we figure out the right way to track these? E.g., we may want to create a single issue to make it easier to manage the issue tracker.

---

_Comment by @qarmin on 2023-09-06 18:17_

No problem, especially since I have currently run out of bugs to report.

If I notice panic errors, I'll still report them because they occur quite rarely and won't litter the issue tracker too much.

---

_Referenced in [astral-sh/ruff#7223](../../astral-sh/ruff/pulls/7223.md) on 2023-09-07 14:53_

---

_Closed by @charliermarsh on 2023-09-07 15:16_

---
