---
number: 13255
title: F821 false positive when using ExceptionGroup
type: issue
state: closed
author: iurii-skorniakov
labels: []
assignees: []
created_at: 2024-09-05T16:57:11Z
updated_at: 2024-09-06T10:55:01Z
url: https://github.com/astral-sh/ruff/issues/13255
synced_at: 2026-01-07T13:12:15-06:00
---

# F821 false positive when using ExceptionGroup

---

_Issue opened by @iurii-skorniakov on 2024-09-05 16:57_

version:  ruff 0.6.4, Python 3.11.6 
command: `ruff check --isolated example.py`
  
when using built-in `ExceptionGroup`
```python
class MyGroup(ExceptionGroup):
    def derive(self, excs):
        return MyGroup(self.message, excs)
```
ruff raises F821
```
example.py:1:15: F821 Undefined name `ExceptionGroup`
  |
1 | class MyGroup(ExceptionGroup):
  |               ^^^^^^^^^^^^^^ F821
2 |     def derive(self, excs):
3 |         return MyGroup(self.message, excs)
  |
```


---

_Comment by @g3rv4 on 2024-09-05 18:58_

seems like a regression from https://github.com/astral-sh/ruff/pull/3167

---

_Comment by @charliermarsh on 2024-09-05 19:02_

Do you have a minimum Python version set, in your `pyproject.toml` or via `--target-version`? Otherwise, Ruff defaults to Python 3.8, and `ExceptionGroup` doesn't exist in that version.

---

_Label `needs-info` added by @dhruvmanila on 2024-09-06 05:29_

---

_Comment by @bonastreyair on 2024-09-06 06:38_

maybe it is another issue but for me it fails finding `anext` when using python3.11
it was working fine with previous version but now it fails because of that

I think the problem comes from here: https://github.com/astral-sh/ruff/pull/13172/files/de081b67c3072829c21cf8176a96baef64793ba7#diff-a5297ef4a78229f14881c57e472a6a79dc17e6a5a9244a676dfa6276d10f0dab

`anext` was before and now it is not present in that same list: https://github.com/astral-sh/ruff/pull/13172/files/de081b67c3072829c21cf8176a96baef64793ba7#r1746582057

related PR: #13172

---

_Comment by @MichaReiser on 2024-09-06 06:47_

@bonastreyair could you share your configuration? `anext` is still in the builtins list but only when targeting Py310 or newer. 

* [Py38](https://play.ruff.rs/c16cf647-56f5-49b2-93a8-dc9ff83c4e5d) -> builtin doesn't exist
* [Py311](https://play.ruff.rs/1252d490-e72e-48f4-a29d-8560b5fc1f34) -> no error

You may need to make a whitespace change for the errors to show up

---

_Comment by @bartfeenstra on 2024-09-06 07:24_

I started having similar problems, also with anext() and aiter().

Project.toml: https://github.com/bartfeenstra/betty/blob/0.4.x/pyproject.toml
Ruff failures: https://github.com/bartfeenstra/betty/actions/runs/10734022531/job/29768422099

Let me know if you need more details :)


---

_Comment by @iurii-skorniakov on 2024-09-06 07:26_

Found my problem. 
I had incorrect configuration in subdirectory, which didn't extend root config. 
With correct configuration it works well
Thank you for your help!

---

_Closed by @iurii-skorniakov on 2024-09-06 07:26_

---

_Label `needs-info` removed by @dhruvmanila on 2024-09-06 08:30_

---

_Referenced in [bartfeenstra/betty#1934](../../bartfeenstra/betty/pulls/1934.md) on 2024-09-06 08:38_

---

_Comment by @bartfeenstra on 2024-09-06 08:50_

Can we reopen this? I still have this problem, with a single root configuration file:
```
[lint]
extend-select = [
    'A',
    'B',
    'C4',
    'D',
    'F',
    'INT',
    'PT',
    'PTH',
    'SIM',
    'SLOT',
    'T20',
    'TCH',
]
ignore = [
    'D105',
    'D107',
    'D200',
    'D203',
    'D212',
    'D401',
    'F501',
    'PTH123',
    'SIM103',
]

[lint.per-file-ignores]
"betty/tests/*" = [
    'D100',
    'D101',
    'D102',
    'D103',
    'D104',
    'D106',
]

```

---

_Comment by @MichaReiser on 2024-09-06 09:14_

The fix is to specify the `requires-python` setting to a version that supports `ExceptionGroup`. 

---

_Comment by @bartfeenstra on 2024-09-06 09:29_

`requires-python = '~= 3.11'` in https://github.com/bartfeenstra/betty/blob/0.4.x/pyproject.toml, yet Ruff fails on ExceptionGroup, aiter, anext... I tried a simpler constraint of `requires-python = '>= 3.11'` but that did not seem to change Ruff's output.

---

_Comment by @MichaReiser on 2024-09-06 09:41_

@bartfeenstra I don't think ruff picks up the `requires-python` constraint if you also have a `.ruff.toml` because the `.ruff.toml` takes precedence. You have to explicitly set the [`target-version`](https://docs.astral.sh/ruff/settings/#target-version) in the ruff.toml. The alternative is to move the ruff configuration into the `pyroject.toml`.

---

_Comment by @bartfeenstra on 2024-09-06 10:55_

That makes sense! I'll give that a try shortly. Thank you!

---

_Referenced in [bartfeenstra/betty#1935](../../bartfeenstra/betty/issues/1935.md) on 2024-09-06 10:55_

---
