---
number: 484
title: "`--fix` flag removes imports in `__init__` files if there is a lack of `__all__`"
type: issue
state: closed
author: Czaki
labels: []
assignees: []
created_at: 2022-10-27T10:09:58Z
updated_at: 2024-06-26T03:21:13Z
url: https://github.com/astral-sh/ruff/issues/484
synced_at: 2026-01-07T13:12:14-06:00
---

# `--fix` flag removes imports in `__init__` files if there is a lack of `__all__`

---

_Issue opened by @Czaki on 2022-10-27 10:09_

If `__init__` file does not contain `__all__` variable and `--fix` option is specified then `ruff` removes imports instead of create `__all__` variable with include all variables imported from submodules. 
Or report it as an error without modifications to files. And highlight lack of `__all__` variable.  

---

_Renamed from "--fix flag removes imports in __init__ files if there is a lack of __all__" to "`--fix` flag removes imports in `__init__` files if there is a lack of `__all__`" by @Czaki on 2022-10-27 10:10_

---

_Comment by @charliermarsh on 2022-10-27 13:16_

Will take a look at this today!

---

_Comment by @charliermarsh on 2022-10-27 15:00_

Some relevant discussion from Flake8: https://github.com/PyCQA/pyflakes/issues/162, https://github.com/PyCQA/pyflakes/issues/471

---

_Comment by @charliermarsh on 2022-10-27 15:03_

A couple options:

1. Suppress these errors entirely in `__init__.py` (always ignore unused imports for those files).
2. Suppress these errors when `__init__.py` only contains imports (potentially confusing).
3. Keep these errors under F401, but don't autofix them.
4. Keep these errors under F401, but don't autofix them _and_ add a custom message regarding `__all__`.
5. Move these errors to a new error code, and don't autofix them.

My preferences are either (1) or (4).


---

_Comment by @Czaki on 2022-10-27 15:13_

I like 4. 

---

_Referenced in [astral-sh/ruff#489](../../astral-sh/ruff/pulls/489.md) on 2022-10-27 17:07_

---

_Closed by @charliermarsh on 2022-10-27 17:08_

---

_Comment by @charliermarsh on 2022-10-27 17:10_

(Going out in v0.0.86, which is building now.)

---

_Comment by @fsouza on 2022-10-28 14:44_

@charliermarsh any thoughts on instructing users to configure `per-file-ignores` instead? Or adding a dedicated flag/config option (similar to autoflake's `--ignore-init-module-imports`)?

I feel like that if an import error is reported is should be fixable?

---

_Comment by @charliermarsh on 2022-10-28 14:47_

I’m open to making it a configuration setting!

---

_Referenced in [astral-sh/ruff#1029](../../astral-sh/ruff/issues/1029.md) on 2022-12-04 15:21_

---

_Comment by @smackesey on 2022-12-04 18:48_

+1 for making it an option to autofix these. Our project ([Dagster](https://github.com/dagster-io/dagster)) has two types of `__init__.py` files (1) pure "export" files (i.e. they just import from submodules); (2) those that define a bunch of functions themselves (and so are more like standard modules). The rule as currently implemented prevents removing all of the unused imports from type (2), which can be frustrating.

FWIW, one can get around the conundrum of having intended-to-be-reexported symbols in `__init__.py` files appear to be unused (and flagged with F401) by using redundant aliases. That's what we do for [top-level `dagster`](https://github.com/dagster-io/dagster/blob/master/python_modules/dagster/dagster/__init__.py):

```python
from dagster._core.definitions.decorators.sensor_decorator import (
    asset_sensor as asset_sensor,
    sensor as sensor,
    multi_asset_sensor as multi_asset_sensor,
)
from dagster._core.definitions.decorators.source_asset_decorator import (
    observable_source_asset as observable_source_asset,
)
from dagster._core.definitions.dependency import (
    DependencyDefinition as DependencyDefinition,
    MultiDependencyDefinition as MultiDependencyDefinition,
    NodeInvocation as NodeInvocation,
)
```

We had to add this because pyright/pylance complains the symbols are private otherwise (since they are imported rather than defined in the module).

So since other popular tools require you to do this (or use `__all__`) to mark imported symbols as public, and this is also a developing [Python standard](https://github.com/python/typing/blob/master/docs/source/libraries.rst#how-much-of-my-library-needs-types), it seems reasonable to me to by default not treat `__init__.py` imports as special, and have users opt into the "don't fix" special-casing.


---

_Comment by @charliermarsh on 2022-12-04 18:50_

Yeah, that makes sense. I'll fix this today.

---

_Referenced in [astral-sh/ruff#1042](../../astral-sh/ruff/pulls/1042.md) on 2022-12-04 19:22_

---

_Comment by @charliermarsh on 2022-12-04 19:47_

This is going out in [v0.0.157](https://github.com/charliermarsh/ruff/releases/tag/v0.0.157) (building now), feedback welcome!

---

_Comment by @mara004 on 2023-05-14 12:16_

Can [ignore-init-module-imports](https://beta.ruff.rs/docs/settings/#ignore-init-module-imports) only be set in a configuration file, or is it also possible to pass this on the command line? (I'm new to ruff.)

---

_Comment by @dhruvmanila on 2023-05-14 17:37_

Currently, it's not possible to pass arbitrary config keys as flag on the command-line but that's likely to change in the future (https://github.com/charliermarsh/ruff/pull/4297#issuecomment-1539466250).

---

_Comment by @dream-dasher on 2023-10-14 01:32_

Was this fix/improvement rolled back?

using `ruff 0.0.292` and it has the same behavior as described originally : it complains about unused imports in __init__.py [`F401`].

And the `--fix` option merely deletes them.

![error reporting](https://github.com/astral-sh/ruff/assets/33399972/aa029182-c03f-4de9-bbaf-10ebf07b3ee1)

![auto-fix changes](https://github.com/astral-sh/ruff/assets/33399972/fe1d7d0e-88b0-490b-85bc-a55a6c6d2908)

---

_Comment by @charliermarsh on 2023-10-14 01:58_

Do you have the relevant setting enabled? https://docs.astral.sh/ruff/settings/#ignore-init-module-imports

---

_Referenced in [NREL/floris#775](../../NREL/floris/pulls/775.md) on 2024-01-18 22:05_

---

_Comment by @cthorrez on 2024-01-19 08:14_

I do have the [ignore-init-module-imports](https://docs.astral.sh/ruff/settings/#ignore-init-module-imports) setting enabled in my pyproject.toml but it doesn't really "ignore" them. It doesn't fix them but it still fails the ruff run which is an issue if ruff is being used in a precommit hook.

with `ignore-init-module-imports = false`, it just removes the imports breaking my code

with `ignore-init-module-imports = true` I get this type of error blocking my commit

```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

riix/utils/__init__.py:1:25: F401 `.data_utils.RatingDataset` imported but unused; consider adding to `__all__` or using a redundant alias
riix/utils/__init__.py:1:40: F401 `.data_utils.generate_matchup_data` imported but unused; consider adding to `__all__` or using a redundant alias
Found 2 errors.```

---

_Referenced in [Ostorlab/oxo#560](../../Ostorlab/oxo/pulls/560.md) on 2024-01-23 16:24_

---

_Comment by @joostsijm on 2024-01-31 14:14_

@cthorrez workaround for pre-commit hook picking up the error message is to ignore F401 error per file type like in the following pyproject.toml:
```toml
[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
```

---

_Comment by @vergenzt on 2024-02-09 19:39_

FYI all, I ended up writing an [`ast-grep`](https://ast-grep.github.io/) rule to rewrite such imports in my codebase to use the redundant import alias. (We're just getting started with `ruff`.) Figured I'd share it here:

```yaml
id: _
language: py
files:
- ./src/**/__init__.py

rule:
  all:

  - pattern: $NAME

  - inside:
      kind: import_from_statement
      stopBy: end

  - kind: dotted_name

  # has to NOT be the first dotted_name in the import statement (that's the source module)
  - follows:
      any:
      - has: { kind: dotted_name }
      - kind: dotted_name
      stopBy:
        kind: import_from_statement

transform:
  REDUNDANT_ALIAS:
    replace:
      source: $NAME
      replace: (.*)
      by: $1 as $1

fix: $REDUNDANT_ALIAS

```

---

_Comment by @grahamannett on 2024-03-19 22:33_

> @cthorrez workaround for pre-commit hook picking up the error message is to ignore F401 error per file type like in the following pyproject.toml:
> 
> ```toml
> [tool.ruff.per-file-ignores]
> "__init__.py" = ["F401"]
> ```

this is very helpful and what I ended up using but seems like it is then required for any code base that uses ruff with F401 (or pyflakes rules) and `__init__.py` as a way to import classes/functions/etc from elsewhere.  

Is this not the suggested project structure and there is a more modern/appropriate way to do that?  I don't really think `__all__` makes sense in this case

---

_Comment by @zanieb on 2024-03-20 01:09_

Note we changed this behavior again recently in https://github.com/astral-sh/ruff/pull/10365, you can see more discussion there.

> __init__.py as a way to import classes/functions/etc from elsewhere

What do you mean? I would say that `__init__` is very much _not_ intended for external imports unless you are intentionally exposing them as a part of your public interface in which case `__all__` or a redundant alias `import foo as foo` are the appropriate ways to do so.



---

_Referenced in [flexcompute/Flow360#242](../../flexcompute/Flow360/pulls/242.md) on 2024-04-17 09:05_

---

_Comment by @me-v2 on 2024-06-26 03:21_

fix it ?

---
