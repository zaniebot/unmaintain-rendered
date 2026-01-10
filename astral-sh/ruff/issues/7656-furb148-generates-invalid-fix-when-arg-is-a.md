```yaml
number: 7656
title: FURB148 generates invalid fix when arg is a generator
type: issue
state: closed
author: Skylion007
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-09-25T13:43:19Z
updated_at: 2023-10-03T14:39:16Z
url: https://github.com/astral-sh/ruff/issues/7656
synced_at: 2026-01-10T11:09:49Z
```

# FURB148 generates invalid fix when arg is a generator

---

_Issue opened by @Skylion007 on 2023-09-25 13:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```python
a = (b for b in range(1, 100))
for i, _ in enumerate(a):
    print(i)
```
gets autofixed to:
```python
a = (b for b in range(1, 100))
for i in range(len(a)):
    print(i)
```
but a is a generator, and `len`, surprisingly cannot take a generator (len needs a SizedSequence). Running the code in this autofix will generate the following error:
```
Traceback (most recent call last):
  File "test_file.py", line 2, in <module>
    for i in range(len(a)):
TypeError: object of type 'generator' has no len()
```
So while this would work if object was a SizedSequence like a tuple, list, dict, or set, it does not work in this case and will generate invalid.

As an aside, using enumerate to generate an index here when it's a generator is rather nifty, not sure there is an easier way to actually get a range length on generator, using enumerate might actually be most efficient in this case.

The command run is `ruff test_file.py --select FURB148 --preview --fix`
on `ruff 0.0.291`

---

_Label `bug` added by @charliermarsh on 2023-09-25 13:58_

---

_Comment by @charliermarsh on 2023-09-25 13:58_

Thanks!

---

_Comment by @dhruvmanila on 2023-09-26 02:04_

This will probably require type inference as the variable might be defined outside the `enumerate` call site as is in this example.

---

_Label `type-inference` added by @dhruvmanila on 2023-09-26 02:04_

---

_Comment by @Skylion007 on 2023-09-30 16:00_

@dhruvmanila True, but in the meantime we shouldn't suggest or force the change because if the arg is a generator, enumerate might strictly be better than range (I can't think of a cleaner way to do it).

---

_Comment by @tjkuson on 2023-10-02 22:51_

Suppressing the fix would still have the rule trigger, which is less destructive but still frustrating for the user who has to figure out it's a false negative and suppress the rule. Limiting the rule to known safe sequences (for example, using `ruff_python_semantic::analyze::typing::is_list`) would eliminate this, but result in a whole load of false negatives. We could do that to eventually promote the rule out of the preview category, and then create an issue for generator discrimination when Ruff has better type inference?

---

_Comment by @charliermarsh on 2023-10-02 22:53_

Yeah, I think my suggestion would be to only trigger the enumerate case if `is_list` or `is_set` or `is_dict` or `is_tuple` (i.e., it's a known container) is truthy.

---

_Comment by @tjkuson on 2023-10-02 23:00_

Feel free to assign to me, I can work on a merge request tomorrow.

---

_Assigned to @tjkuson by @charliermarsh on 2023-10-02 23:04_

---

_Closed by @charliermarsh on 2023-10-03 14:39_

---
