```yaml
number: 5499
title: "Support `FORCE_COLOR` env var"
type: issue
state: closed
author: MicaelJarniac
labels:
  - cli
assignees: []
created_at: 2023-07-04T04:07:32Z
updated_at: 2024-04-08T21:29:30Z
url: https://github.com/astral-sh/ruff/issues/5499
synced_at: 2026-01-10T11:09:47Z
```

# Support `FORCE_COLOR` env var

---

_Issue opened by @MicaelJarniac on 2023-07-04 04:07_

## Feature Request
Add support for the `FORCE_COLOR` environment variable. When set, regardless of value, all output should have colors enabled, as if the terminal supported them.

As far as I can tell, Ruff does support a similar environment variable, `CLICOLOR_FORCE`.
It seems to me, though, that `FORCE_COLOR` is a lot more common.
In my opinion, it'd be worth supporting both.

Some projects that use this:
- [Pytest](https://docs.pytest.org/en/7.0.x/reference/reference.html#envvar-FORCE_COLOR)
- [Nox](https://nox.thea.codes/en/stable/usage.html#controlling-color-output)
- [Tox](https://tox.wiki/en/4.0.6/changelog.html#features-4-0-0a2)
- [Rich](https://rich.readthedocs.io/en/latest/console.html?highlight=FORCE_COLOR#environment-variables)
- [Mypy](https://github.com/python/mypy/blob/781dc8f82adacce730293479517fd0fb5944c255/mypy/util.py#L523)
- [build](https://pypa-build.readthedocs.io/en/stable/changelog.html?highlight=FORCE_COLOR#id13)

Related:
- https://github.com/Textualize/rich/issues/2425
- https://github.com/wntrblm/nox/issues/502
- https://github.com/pypa/pip/issues/10909
- https://github.com/python-poetry/cleo/issues/341
- https://github.com/pre-commit/pre-commit/issues/2918


---

_Comment by @MichaReiser on 2023-07-04 06:24_

We use `colored` that only supports [`CLICOLOR_FORCE`](https://github.com/colored-rs/colored#features) out of the box.

We could either manually set the override when `FORCE_COLOR` is set or, more generally, explore if we want to migrate away from `colored` (I always found the API a bit clumsy, [owo-colors](https://crates.io/crates/owo-colors), [termcolor](https://crates.io/crates/termcolor), possibly more). 

---

_Comment by @charliermarsh on 2023-07-04 12:31_

Happy to move to a different crate, no objection from me.

---

_Label `cli` added by @charliermarsh on 2023-07-04 21:55_

---

_Comment by @ofek on 2023-10-08 19:45_

This would be a nice feature to have. I have had good experiences with using `owo-colors`.

---

_Closed by @carljm on 2024-04-08 21:29_

---

_Closed by @carljm on 2024-04-08 21:29_

---
