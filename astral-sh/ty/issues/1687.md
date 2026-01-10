```yaml
number: 1687
title: "Can't import utils with Pytest"
type: issue
state: closed
author: AndreuCodina
labels:
  - question
assignees: []
created_at: 2025-11-30T16:31:19Z
updated_at: 2025-12-07T10:39:28Z
url: https://github.com/astral-sh/ty/issues/1687
synced_at: 2026-01-10T01:56:40Z
```

# Can't import utils with Pytest

---

_Issue opened by @AndreuCodina on 2025-11-30 16:31_

### Summary

If you have test utils (for example, a builder class) and you want to use them with Pytest, then you add

```toml
[tool.pytest]
pythonpath = ["tests"]
```

to your pyproject to import code from the tests folder.

But ty currently returns an error.

For example, if you test it with my repository (https://github.com/AndreuCodina/python-template), this is the output when you run `uv run -- ty check`:


```
error[unresolved-import]: Cannot resolve imported module `test_utils.builders.domain.entities.product_builder`
 --> tests/integration/api/workflows/products/discontinue_product/test_discontinue_product_workflow.py:2:6
  |
1 | import pytest
2 | from test_utils.builders.domain.entities.product_builder import ProductBuilder
  |      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
3 |
4 | from python_template.api.dependency_container import DependencyContainer
  |
info: Searched in the following paths during module resolution:
info:   1. /python-template/src (first-party code)
info:   2. /python-template (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /python-template/.venv/lib/python3.14/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
```

### Version

0.0.1a29

---

_Comment by @MichaReiser on 2025-12-01 07:16_

Similar to pytest, you have to tell ty about the extra search paths using `environment.root`:

```toml
[tool.ty.environment]
root = ["src", ".", "tests"]
```

In your case, we need three:

* `src`: For your production code
* `tests`: To support `from test_utils`
* `.`: Because there's one place where you import from `tests.test_utils`. Not sure if you want to change this import?

The downside of adding `tests` to the search paths is that ty now won't catch if you import a test module from `src`. We're discussing other options for how to handle test folders in https://github.com/astral-sh/ty/issues/1539

---

_Label `question` added by @MichaReiser on 2025-12-01 07:16_

---

_Comment by @AndreuCodina on 2025-12-05 10:51_

I tested your configuration and it works, but now the linked issue is closed, and, reading https://docs.astral.sh/ty/reference/configuration/#root, which contains the text `Besides, if a ./python or ./tests directory exists and is not a package (i.e. it does not contain an __init__.py or __init__.pyi file), it will also be included in the first party search path.`,

then if I don't add configuration and I remove the `tests/__init__.py` file, it works.

I'm not sure if everything is OK. I understand my repository should have a file `tests/__init__.py` and not remove it.


---

_Comment by @carljm on 2025-12-05 22:57_

@AndreuCodina If you want to always write absolute imports of your testing code as `tests.test_utils`, then you should have a `tests/__init__.py` file, and you should _not_ place `tests/` directory in your ty `environment.roots` (only `.` and `src`), and you probably shouldn't use the pytest setting `pythonpath = ["tests"]` either. In this scenario `tests/` is a top-level Python package.

But (what I think is probably better for your case?) if you want to write that import as just `test_utils` (not `tests.test_utils`), then you do want pytest's `pythonpath = ["tests"]` and you want `root = ["src", "tests"]` for ty, and you do NOT want `tests/__init__.py` file. In this scenario `tests/` is not a Python package, it is an "import root" which contains top-level Python packages and modules.

It is definitely better to choose one of these or the other, and not try to do both simultaneously.

---

_Comment by @AndreuCodina on 2025-12-07 09:33_

> [@AndreuCodina](https://github.com/AndreuCodina) If you want to always write absolute imports of your testing code as `tests.test_utils`, then you should have a `tests/__init__.py` file, and you should _not_ place `tests/` directory in your ty `environment.roots` (only `.` and `src`), and you probably shouldn't use the pytest setting `pythonpath = ["tests"]` either. In this scenario `tests/` is a top-level Python package.
> 
> But (what I think is probably better for your case?) if you want to write that import as just `test_utils` (not `tests.test_utils`), then you do want pytest's `pythonpath = ["tests"]` and you want `root = ["src", "tests"]` for ty, and you do NOT want `tests/__init__.py` file. In this scenario `tests/` is not a Python package, it is an "import root" which contains top-level Python packages and modules.
> 
> It is definitely better to choose one of these or the other, and not try to do both simultaneously.

Hi @carljm

Your suggestion of having `tests/__init__.py`, removing `pythonpath = ["tests"]` and not adding any `[tool.ty.environment]` configuration is awesome because it's all I want: the simplest configuration, as other ecosystems.

I prefer the first option because it's clear I'm using code from `tests/` instead of application code, but what I really appreciate is that: no configuration or few configuration, and a working solution.

Thanks! :)

---

_Closed by @MichaReiser on 2025-12-07 10:39_

---
