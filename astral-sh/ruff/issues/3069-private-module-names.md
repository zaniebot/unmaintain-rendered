---
number: 3069
title: Private module names
type: issue
state: closed
author: WilliamJamieson
labels: []
assignees: []
created_at: 2023-02-20T21:17:13Z
updated_at: 2023-02-20T21:28:51Z
url: https://github.com/astral-sh/ruff/issues/3069
synced_at: 2026-01-10T01:22:41Z
---

# Private module names

---

_Issue opened by @WilliamJamieson on 2023-02-20 21:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

https://beta.ruff.rs/docs/rules/invalid-module-name/ is now raised on all the modules in my project that I have a leading underscore on. In my project, we have the convention that `_module.py` represents a private part of the API with the specific objects collected into public `module.py` files and the `__init__.py` file. This is to discourage end users from doing a `from my_package import _module` in their code (even though we specifically exclude `_module` from the `__init__.py`.

Typically the `_module` modules contain code that is "internally" public meaning we use it quite extensively inside the library, but it should not be used externally.

Would it be possible to add a configuration option to ruff that turns off the error for having a leading `_` on module names?

---

_Comment by @WilliamJamieson on 2023-02-20 21:21_

Quoting part of [PEP8](https://peps.python.org/pep-0008/#public-and-internal-interfaces).

> Even with __all__ set appropriately, internal interfaces (packages, modules, classes, functions, attributes or other names) should still be prefixed with a single leading underscore.

So my internal interfaces, that is the aforementioned "private" modules, should be prefixed with a `_`.

---

_Comment by @charliermarsh on 2023-02-20 21:24_

Can you try 0.0.249? The rules were significantly relaxed and should allow leading underscores.

---

_Comment by @WilliamJamieson on 2023-02-20 21:24_

Sure, this was picked up by the pre-commit.ci update this morning.


---

_Comment by @WilliamJamieson on 2023-02-20 21:26_

> Sure, this was picked up by the pre-commit.ci update this morning.

Yes 0.0.249 fixed this problem. Sorry I had not realized there was a newer release.

---

_Closed by @WilliamJamieson on 2023-02-20 21:26_

---

_Comment by @charliermarsh on 2023-02-20 21:27_

No worries. Sorry for the churn. I turned around a new release specifically for this issue.

---

_Comment by @WilliamJamieson on 2023-02-20 21:28_

No worries, it forced me to actually read part of PEP8, which was probably a good thing.

---
