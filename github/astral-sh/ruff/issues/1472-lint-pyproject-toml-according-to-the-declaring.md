---
number: 1472
title: "Lint `pyproject.toml` according to the 'Declaring project metadata' spec"
type: issue
state: closed
author: not-my-profile
labels:
  - rule
assignees: []
created_at: 2022-12-30T10:49:51Z
updated_at: 2023-06-02T03:01:38Z
url: https://github.com/astral-sh/ruff/issues/1472
synced_at: 2026-01-07T13:12:14-06:00
---

# Lint `pyproject.toml` according to the 'Declaring project metadata' spec

---

_Issue opened by @not-my-profile on 2022-12-30 10:49_

[Declaring project metadata](https://packaging.python.org/en/latest/specifications/declaring-project-metadata/) (based on [PEP 621](https://peps.python.org/pep-0621/)) defines how metadata in `pyproject.toml` should be defined. Unless I am missing something there doesn't appear to be an existing lint plugin we could model this after, which would make this a RUF check.

This is basically the same idea as rust-lang/rust-clippy/issues/6431 but for Python/pyproject.toml/ruff instead of Rust/Cargo.toml/clippy.

Sidenote: Maybe we want to use [pyproject-toml-rs](https://github.com/PyO3/pyproject-toml-rs) for the parsing step?

---

_Label `enhancement` added by @charliermarsh on 2022-12-30 12:20_

---

_Comment by @charliermarsh on 2022-12-30 12:21_

(I know there's [`validate-pyproject`](https://github.com/abravalheri/validate-pyproject) but it may not be relevant at all, I haven't looked closely.)

---

_Comment by @not-my-profile on 2022-12-30 12:40_

Oh, yes I did miss that ... it does appear to be relevant. Although our checks could probably be more in-depth.

---

_Comment by @charliermarsh on 2022-12-30 12:42_

(I really haven't looked at that project at all, it was just the first thing that came to mind, maybe unhelpful...)

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:12_

---

_Label `rule` added by @charliermarsh on 2022-12-31 18:12_

---

_Referenced in [astral-sh/ruff#1774](../../astral-sh/ruff/issues/1774.md) on 2023-01-10 21:11_

---

_Referenced in [astral-sh/ruff#2103](../../astral-sh/ruff/pulls/2103.md) on 2023-01-24 14:16_

---

_Referenced in [astral-sh/ruff#579](../../astral-sh/ruff/issues/579.md) on 2023-01-26 04:14_

---

_Comment by @JonathanPlasse on 2023-03-04 22:38_

There is also [taplo](https://github.com/tamasfe/taplo) that is more general which is

> A TOML parser, analyzer and formatter library

---

_Comment by @JonathanPlasse on 2023-05-28 09:52_

- Does #4496 close this issue?

---

_Closed by @charliermarsh on 2023-06-02 03:01_

---
