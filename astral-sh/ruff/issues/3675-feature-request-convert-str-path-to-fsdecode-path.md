```yaml
number: 3675
title: "Feature request: convert str(Path) to fsdecode(Path)"
type: issue
state: open
author: m-aciek
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-03-22T22:55:48Z
updated_at: 2026-01-13T23:46:06Z
url: https://github.com/astral-sh/ruff/issues/3675
synced_at: 2026-01-14T00:34:04Z
```

# Feature request: convert str(Path) to fsdecode(Path)

---

_@m-aciek_

I [learned today](https://discuss.python.org/t/make-pathlib-extensible/3428/79) that [os.fsdecode(Path)](https://docs.python.org/3/library/os.html#os.fsdecode) is preferred over str(Path) for [pathlib.Path](https://docs.python.org/3/library/pathlib.html) objects. Ruff could have a check for str(Path) usages.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `rule` added by @charliermarsh on 2023-03-23 17:06_

---

_Comment by @nstarman on 2023-03-26 17:28_

Isn't `.as_posix()` preferred over both?


---

_Comment by @edgarrmondragon on 2023-03-26 17:59_

I think this is hard to accomplish in a useful way without type checking. The only cases this would catch would be when the `Path` object is directly instantiated inside the `str()` call.

---

_Comment by @m-aciek on 2023-03-27 11:30_

Related:
* [PEP 519](https://peps.python.org/pep-0519/)
* https://stackoverflow.com/questions/65313454/how-to-switch-from-path-to-os-path-and-vice-versa

> Isn't `.as_posix()` preferred over both?

No, if you strive to support multiple platforms, including Windows.

Yes, I wonder if the difficulty of case recognition isn't a blocker here. For libraries not supporting Path objects where they should, `fspath` function should be used, but it can return also `bytes`.

---

_Comment by @konstin on 2023-03-28 12:04_

According to https://discuss.python.org/t/make-pathlib-extensible/3428/83 this is only a problem if you don't use type checking or accept a `PathLike` instead of `Path`, so as i understood it would not make sense to enforce this on a `Path`

---

_Comment by @m-aciek on 2023-04-02 23:07_

Hm, `fspath()` is runtime utility to ensure that program really operates on a path, this way it does more than type checking – type checking doesn't prevent always from having a not expected type in runtime. But I agree that for many (majority of?) cases, when path object doesn't come as arbitrary object from outside (API?), the `fspath()` usage wouldn't be needed. Probably recommending using `fspath()` for all cases would be counter-productive.

@brettcannon do you maybe have an opinion on it?

---

_Renamed from "Feature request: convert str(Path) to fsdecode(Path)" to "Feature request: convert str(Path) to fspath(Path)" by @m-aciek on 2023-04-02 23:10_

---

_Comment by @brettcannon on 2023-04-03 21:03_

Since I was asked for my opinion, here's my opinionated opinion 😉:

1. Never use `str(Path)`; can give you `None`
2. Only use `PurePath.as_posix()` if you specifically need the path in a POSIX format
3. Use `os.fsdecode(path)` if you need the string encoding of the `Path` object (don't need to use `os.fspath()` since `pathlib` doesn't deal in `bytes`), but you should avoid this as much as possible and only do it last-minute when you must
4. To be fully generic/duck-typing, us `os.fspath()` anywhere you accept a path and don't care if it's a `str` or `bytes`, else use `os.fsdecode()` or `os.fsencode()`, respectively
5. To make this work you will need type hints

---

_Renamed from "Feature request: convert str(Path) to fspath(Path)" to "Feature request: convert str(Path) to fsdecode(Path)" by @m-aciek on 2023-04-04 00:55_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:29_

---

_Comment by @ericrbg-harmonic on 2026-01-13 11:32_

My understanding is that now we should be using `os.fspath` instead of `fsdecode`, fwiw

---

_Comment by @brettcannon on 2026-01-13 21:22_

> My understanding is that now we should be using `os.fspath` instead of `fsdecode`, fwiw

Who's "we" in that sentence?

---

_Comment by @ericrbg-harmonic on 2026-01-13 23:46_

Apologies, "we" was meant to be interpreted as "one", as in "one should", although a closer reading of [PEP 519](https://peps.python.org/pep-0519/) suggests that both have value (and possibly `fsdecode` is more specific for the string case?)

---
