```yaml
number: 2660
title: "Support globbing in `banned-api`"
type: issue
state: open
author: charliermarsh
labels:
  - configuration
assignees: []
created_at: 2023-02-08T15:45:12Z
updated_at: 2025-04-22T09:34:50Z
url: https://github.com/astral-sh/ruff/issues/2660
synced_at: 2026-01-10T11:09:45Z
```

# Support globbing in `banned-api`

---

_Issue opened by @charliermarsh on 2023-02-08 15:45_

E.g., `top` should match `import top`, but not `import top.internal`, whereas `top.*` should match both (I think? We'll do whatever `flake8-tidy-imports` does).

See: #2656.

---

_Label `configuration` added by @charliermarsh on 2023-02-08 15:45_

---

_Comment by @not-my-profile on 2023-02-10 03:47_

No ... I don't think that's a good idea because `import top.internal` also imports `top`.

---

_Comment by @charliermarsh on 2023-02-10 03:57_

Yes good point! I agree that we shouldn't implement the submodule semantics described above. It was a lazy example.

The question here is really: what does `flake8-tidy-imports` do? And do we want to support _that_? (E.g., do we want to support globbing like `example.*.truck`, as in `flake8-tidy-imports`?)

(I don't really want to support it, and I won't be working on it any time soon, but that's the intent of the issue.)


---

_Comment by @seamuswn on 2024-03-27 17:12_

We have a Python monorepo in my org, where we have developed shared abstractions built under `src.core`. Ideally we would like to prevent importing private / internal packages within that core package. Specifically restricting subpackages at any level  within `src.core` named 'internal' (i.e. `src.core.*.internal`), or any subpackages where the package name starts with `_` (i.e. `src.core._*`/`src.core.*._*`).

I think this would be a useful feature for that type of use case, where we want to import restrictions based on our package structure

---

_Comment by @ashrub-holvi on 2024-05-22 07:12_

I have probably similar idea `__init__` file is a sort of public interface of a package, so outside of package nobody should do `from mypackage.xxx import yyy,` they should do `from mypackage import yyy` only. Tried to do
```
[tool.ruff.lint.flake8-tidy-imports.banned-api]
"mypackage.*.*".msg = "Import from mypackage"
```
but was not able to make it work.

---

_Comment by @Avasam on 2024-08-11 20:34_

If globbing (or at least submodules) support is added to https://docs.astral.sh/ruff/rules/banned-api/ (TID251), could it also be added to https://docs.astral.sh/ruff/rules/banned-module-level-imports/ (TID253) ? I'd like to configure it like such:
```py
[lint.flake8-tidy-imports]
banned-module-level-imports = [
	# platform-specific imports
	"android",
	"jnius",
	"platformdirs.android", # still allowing top-level platformdirs
	"platformdirs.linux" ,# still allowing top-level platformdirs
	"platformdirs.macos", # still allowing top-level platformdirs
	"platformdirs.windows", # still allowing top-level platformdirs
	"winreg",
]
```
And at this point, https://docs.astral.sh/ruff/rules/banned-import-from/ (ICN003) as well.

---

_Comment by @rh-sp on 2025-04-22 09:30_

To expand on @ashrub-holvi's take, I would like to limit import to canonical locations in a Django app. So for example, models should always be imported from `my_app.models`, never `my_app.models.foo` or `my_app.models.bar`. A more pressing version of this, a colleague of mine has identified a potential issue with Django signals and how they're imported. Not sure how much of this is our setup vs. Django conventions, but at least in our case, signals are only registered if they're imported in `my_app.signals`. If we allow importing a signal from `my_app.signals.foo`, it will not work if it's not also imported by `my_app.signals`'s `__init__.py`.

So Ideally I would like to have some setting like:

```toml
[lint.something]
banned-something = [
    "*.signals.**",
    "*.models.**"
]
```

Which would lead to the following:

```python
from my_app.models import Product  # allowed
from my_app.models.product import Product  # banned
from my_app.models.product.simple import SimpleProduct  # banned (** matches one or more components)
from something_else.foo.models.product import Product  # allowed (* matches only single component)
```

The last line being allowed isn't required for us, but it would be nice to be able to make this distinction.

---
