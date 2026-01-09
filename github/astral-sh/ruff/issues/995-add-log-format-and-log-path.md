---
number: 995
title: Add --log-format and --log-path
type: issue
state: closed
author: JonathanPlasse
labels:
  - cli
assignees: []
created_at: 2022-12-02T12:55:43Z
updated_at: 2023-06-20T16:51:56Z
url: https://github.com/astral-sh/ruff/issues/995
synced_at: 2026-01-07T13:12:14-06:00
---

# Add --log-format and --log-path

---

_Issue opened by @JonathanPlasse on 2022-12-02 12:55_

I use Ruff hook in `pre-commit`.
In CI, I want to run `pre-commit run --all-files` and I would like that it generate a log in Junit format in addition to the nice output in `stdout`.
If `--log-format` and `--log-path` are unspecified, no log would be generated.
If only `--log-path` is specified, default to the default format.
If only `--log-format` is specified, default to write a log to the root with a default name (what should the default name be?)
I could submit a pull request if it is ok.

---

_Comment by @charliermarsh on 2022-12-02 14:22_

Can we find an example of a tool or two that does this well, and mirror their API?

---

_Label `enhancement` added by @charliermarsh on 2022-12-02 14:22_

---

_Label `configuration` added by @charliermarsh on 2022-12-02 14:22_

---

_Comment by @JonathanPlasse on 2022-12-02 15:26_

mypy has `junit_xml = "reports/mypy.xml"` in `pyproject.toml`.
pytest has `--junitxml=reports/pytest.xml"` in the CLI.

I would propose `--junit-xml` in the CLI and `junit-xml` in `pyproject.toml`.
Only the Junit format interest me.

---

_Comment by @JonathanPlasse on 2022-12-03 19:31_

I found an alternative; I skip Ruff in pre-commit with:
```shell
SKIP=ruff pre-commit run --all-files
```
then run
```shell
ruff --format=junit > reports/ruff.xml
```
But I still think it could be a useful feature.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:11_

---

_Label `configuration` removed by @charliermarsh on 2022-12-31 18:11_

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:11_

---

_Referenced in [astral-sh/ruff#4754](../../astral-sh/ruff/issues/4754.md) on 2023-05-31 19:01_

---

_Comment by @charliermarsh on 2023-06-20 16:51_

We now support outputting to a file.

---

_Closed by @charliermarsh on 2023-06-20 16:51_

---
