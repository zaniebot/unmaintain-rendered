```yaml
number: 3758
title: Implement more flake8-bugbear opinionated rules
type: issue
state: open
author: sjdemartini
labels:
  - rule
assignees: []
created_at: 2023-03-27T16:08:43Z
updated_at: 2024-12-02T17:22:20Z
url: https://github.com/astral-sh/ruff/issues/3758
synced_at: 2026-01-10T11:09:46Z
```

# Implement more flake8-bugbear opinionated rules

---

_Issue opened by @sjdemartini on 2023-03-27 16:08_

Picking up where https://github.com/charliermarsh/ruff/issues/2954 left off, there were a few opinionated (B9xx flake8-bugbear rules) checks left to be implemented in Ruff:

* [x]  B901: Using `return x` in a generator function. _(Somewhat bad reasoning in flake8-bugbear description, talks about Python 2, see comment here https://github.com/charliermarsh/ruff/issues/2954#issuecomment-1441162976 around its utility.)_
- [x] B902: _Implemented as N804 and N805._
* [ ]  B903: Use `collections.namedtuple` (or `typing.NamedTuple`) for data classes that only set attributes in an `__init__` method, and do nothing else. _(Probably should include dataclasses recommendation? NamedTuple injects extra tuple methods and is meant for backward compat, not a data class replacement. That's `dataclasses` nowadays.)_
* [ ]  ~B906: `visit_` function with no further call to a `visit` function.~
- [ ] B907: Consider replacing f"'{foo}'" with f"{foo!r}" which is both easier to read and will escape quotes inside foo if that would appear.
- [x] B908: _Partly implemented as PT012_.

There's an open question on how these should be included, since it would deviate from flake8-bugbear to have these on by default just by turning on the rest of the bugbear rules (see comment https://github.com/charliermarsh/ruff/issues/2954#issuecomment-1483594606).

There's also one outstanding non-opinionated rule:

- [x] B036: Found except BaseException: without re-raising (no raise in the top-level of the except block). This catches all kinds of things (Exception, SystemExit, KeyboardInterrupt...) and may prevent a program from exiting as expected. _Implemented as BLE001._
- [x] B038: _Renamed to B909 in bugbear; implemented as B909 is Ruff._


---

_Label `rule` added by @charliermarsh on 2023-03-27 17:06_

---

_Comment by @Pierre-Sassoulas on 2023-03-29 11:03_

``B906`` is flake8 plugins specific and has a false positive when the visit function is visiting an astroid node in a pylint plugin. It would be nice to not have this FP in ruff :)

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:29_

---

_Comment by @mikaelarguedas on 2024-01-06 17:47_

Since then some new rule apppeared

- [ ] B907:  (previous B028)  Consider replacing `f"'{foo}'"` with `f"{foo!r}"` which is both easier to read and will escape quotes inside foo if that would appear. The check tries to filter out any format specs that are invalid together with !r. If you're using other conversion flags then e.g. `f"'{foo!a}'"` can be replaced with `f"{ascii(foo)!r}"`. Not currently implemented for python<3.8 or str.format() calls.
- [ ] B908: (looks like PT012 to me): Contexts with exceptions assertions like with `self.assertRaises` or with `pytest.raises` should not have multiple top-level statements. Each statement should be in its own context. That way, the test ensures that the exception is raised only in the exact statement where you expect it.

---

_Label `good first issue` added by @charliermarsh on 2024-01-06 19:42_

---

_Label `help wanted` added by @charliermarsh on 2024-01-06 19:42_

---

_Label `accepted` removed by @charliermarsh on 2024-01-06 19:42_

---

_Comment by @charliermarsh on 2024-01-06 19:42_

Good first issues for anyone interested :)

---

_Comment by @mikaelarguedas on 2024-01-14 10:10_

Another 2 rules appeared in flake8-bugbear:

- [x] B037: Found `return <value>`, `yield`, `yield <value>`, or `yield from <value>` in class `__init__()` method. No values should be returned or yielded, only bare returns are ok. (PLE0100)

- [ ] B038: Found a mutation of a mutable loop iterable inside the loop body. Changes to the iterable of a loop such as calls to list.remove() or via del can cause unintended bugs. https://github.com/astral-sh/ruff/issues/9511

---

_Comment by @Skylion007 on 2024-01-14 16:51_

B037 is already implemented as PLE0100, @charliermarsh maybe we should consider migrating PLE0100 to B037 though?

---

_Comment by @charliermarsh on 2024-01-14 17:07_

@Skylion007 - Yes good call -- I prefer indexing under bugbear over Pylint since it's more popular for Ruff users. I'll add a note to the 0.2.0 release list.

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Closed by @zanieb on 2024-02-01 19:35_

---

_Comment by @mikaelarguedas on 2024-02-02 05:11_

@zanieb Was this closed on purpose ?

I'm under the impression that some rules listed here are not part of ruff yet:
B036, B038, B901, B902, B903, B907, B908

---

_Comment by @charliermarsh on 2024-02-02 05:19_

@mikaelarguedas - I think it was an oversight, but I'll leave it to @zanieb to confirm in the AM.

---

_Comment by @mscheifer on 2024-05-07 22:04_

I think B036 is covered/aliased by BLE001.

---

_Comment by @mikaelarguedas on 2024-05-08 14:06_

@charliermarsh @zanieb friendly :bellhop_bell: if it was closed by mistake, is it possible to reopen and update according to current state ?

Current state:
B036 -> covered by BLE001
B038 -> no implemented
B901 -> no implemented
B902 ->no implemented
B903 ->no implemented
B907 ->no implemented
B908 ->no implemented

---

_Reopened by @charliermarsh on 2024-05-08 14:18_

---

_Comment by @charliermarsh on 2024-05-08 14:19_

Will update the list, thanks.

---

_Comment by @charliermarsh on 2024-05-08 14:52_

Updated. We do actually have B038 (which bugbear moved to B909).

---

_Removed from milestone `v0.2.0` by @dhruvmanila on 2024-06-29 05:02_

---

_Comment by @jnrbsn on 2024-08-23 16:56_

This issue says that `B902` is "Implemented as `N804` and `N805`", but that is not accurate because `N804` incorrectly says you should use `cls` for "metaclass class methods" (e.g. `__new__`, `__prepare__`). These methods are the metaclass-equivalent of class methods and get passed the _**metaclass itself**_ as the first argument. flake8-bugbear recommends `metacls`, and pylint recommends `mcs` (which flake8-bugbear will also accept).

---

_Comment by @gagandeepp on 2024-10-07 12:06_

Interesting task,can I help?

---

_Label `good first issue` removed by @charliermarsh on 2024-12-02 17:22_

---

_Label `help wanted` removed by @charliermarsh on 2024-12-02 17:22_

---
