```yaml
number: 8311
title: "Clarify or amend output of `--no-package`?"
type: issue
state: open
author: astrojuanlu
labels: []
assignees: []
created_at: 2024-10-17T22:41:25Z
updated_at: 2025-09-25T02:30:49Z
url: https://github.com/astral-sh/uv/issues/8311
synced_at: 2026-01-10T03:23:53Z
```

# Clarify or amend output of `--no-package`?

---

_Issue opened by @astrojuanlu on 2024-10-17 22:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The documentation of the `--no-package` flag of `uv init` says

https://github.com/astral-sh/uv/blob/3fd69b448e27841f7d003d79aab4e31c7995c242/crates/uv-cli/src/lib.rs#L2483

however, `uv init --no-package` does put a `[project]` table in `pyproject.toml`, and in the absence of a `[build-system]` table, PEP 517 mandates a fallback build backend (`setuptools.build_meta:__legacy__`).

In summary, it's not entirely clear how not adding a `[build-system]` table is consistent with the promise of `uv init --no-package` of "not set up the project to be built as a Python package"?

(Comes from https://discuss.python.org/t/pep-735-dependency-groups-in-pyproject-toml/39233/351)

---

_Comment by @charliermarsh on 2024-10-17 22:54_

Yeah, that's all correct. The project is still formally "a package" but we don't treat it as such when you run uv-specific commands like `uv sync` (i.e., we don't build or install it). You can mark it as a package with `package = true` under `[tool.uv]`.

---

_Comment by @charliermarsh on 2024-10-17 23:01_

I think the very presence of a `pyproject.toml` effectively means that a project is "a package" -- you don't even need to have a `[project]` table, since omitting it means that "The lack of a [project] table implicitly means the [build backend](https://packaging.python.org/en/latest/glossary/#term-Build-Backend) will dynamically provide all keys". So the rest is just convention for how tools behave in the presence of such files.

---

_Comment by @zanieb on 2024-10-17 23:01_

Yeah this is a complicated decision we made. We wanted to be able to use the `[project]` table for dependencies in workspaces, even if the project itself was not intended to be packaged. As far as I understand, this isn't quite a specification violation since PEP 517 says:

> If the pyproject.toml file is absent, or the build-backend key is missing, the source tree is not using this specification, and tools should revert to the legacy behaviour of running setup.py (either directly, or by implicitly invoking the `setuptools.build_meta:__legacy__` backend).

Note this isn't a "MUST", and furthermore, we're not really acting as a build frontend in this context — we're acting as a project management tool. In contrast, when installing source trees from elsewhere, we are acting as a build frontend and follow the specification.

The `--no-package` option is intended as a direction to uv. Are you running into problems with this with other tools?



---

_Comment by @zanieb on 2024-10-17 23:02_

It's possible we'll be able to do something else now that PEP 735 is accepted, but I'm not sure.

---

_Comment by @charliermarsh on 2024-10-17 23:09_

Thanks @zanieb for giving a better answer than me.

---

_Comment by @astrojuanlu on 2024-10-18 00:07_

Thanks for the prompt reply as always 🙏🏼 

> Are you running into problems with this with other tools?

No, or at least not yet. More than anything I'm trying to get a precise understanding of the terminology and the different moving parts so that I can teach this effectively.

---

_Comment by @zanieb on 2024-10-18 00:13_

We still have some room to improve our documentation around this. It was a relatively late change. I'll try to remember to respond here when I change things!

---

_Assigned to @zanieb by @zanieb on 2024-10-18 00:13_

---

_Comment by @larsks on 2025-09-25 02:04_

I came here because I don't see any difference between a project initialized with `uv init myproject` and `uv init --no-package myproject`. The generated `pyproject.toml` files are identical. Does this option actually do anything?

```
$ uv init example1/myproject
Initialized project `myproject` at `/home/lars/redhat/moc/uv-demo/example1/myproject`
$ uv init --no-package example2/myproject
Initialized project `myproject` at `/home/lars/redhat/moc/uv-demo/example2/myproject`
$ diff -u example*/myproject/pyproject.toml
```

(Using uv 0.8.13)

---

_Comment by @zanieb on 2025-09-25 02:30_

No, it's the default behavior in the current version. It'll have an effect once we change the defaults in the future to setup a package by default.

---
