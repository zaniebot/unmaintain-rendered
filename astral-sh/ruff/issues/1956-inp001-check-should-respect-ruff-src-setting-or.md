```yaml
number: 1956
title: "INP001 check should respect `ruff.src` setting or have it configured separately"
type: issue
state: closed
author: notypecheck
labels: []
assignees: []
created_at: 2023-01-18T14:08:56Z
updated_at: 2023-02-04T00:20:28Z
url: https://github.com/astral-sh/ruff/issues/1956
synced_at: 2026-01-10T11:09:44Z
```

# INP001 check should respect `ruff.src` setting or have it configured separately

---

_Issue opened by @notypecheck on 2023-01-18 14:08_

A lot of projects nest code under `src` directory by adding `src` to PYTHONPATH:
```
src/
  some_package/
    __init__.py
    ...
  main.py

pyproject.toml
```
```bash
ruff .
```
```bash
src\main.py:1:1: INP001 File `\src\main.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error(s)
```
Ruff version: 0.0.225
Ruff configuration:
```toml
[tool.ruff]
select = ["INP"]
src = ["src"]
```

A solution to this might be using `ruff.src` or different settings to exclude files contained directly in that directory  
But this might be an issue for packages that would want to use `ruff.src` for example for `isort` checks but would still want their package directory to be checked by `INP001`

---

_Comment by @notypecheck on 2023-01-18 14:11_

Adding `__init__.py` actually breaks some tools like mypy:
```py
# src/some_package/test.py
def a() -> int:
    return 42
```
```
# main.py
from some_package.test import a

reveal_type(a())
```

Without `__init__.py`:
```
$ mypy .
src\main.py:3: note: Revealed type is "builtins.int"
Success: no issues found in 3 source files
```

With `__init__.py`:
```
$ mypy .
src\main.py:1: error: Skipping analyzing "some_package.test": module is installed, but missing library stubs or py.typed marker  [import]
src\main.py:1: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
src\main.py:3: note: Revealed type is "Any"
Found 1 error in 1 file (checked 4 source files)
```

---

_Comment by @charliermarsh on 2023-01-18 16:42_

Hmm. I think omitting directories specified by `src` makes sense? Trying to think of where that would cause problems.

---

_Comment by @notypecheck on 2023-01-18 16:51_

Libraries might use the following structure:
```
root/ # Which is the project root
  library/
    __init__.py
    ...
  pyproject.toml
```
In this case developers would want to treat import from `library` which would be in `ruff.src` as first party, but it also should not be omitted from `INP` check

---

_Comment by @edgarrmondragon on 2023-01-18 17:26_

Note that `flake8-no-pep420` has the same behavior:

```console
$ tree check_inp001
check_inp001
└── src
    ├── main.py
    └── some_package
        └── __init__.py

$ flake8 --select INP check_inp001
check_inp001/src/main.py:1:1: INP001 File is part of an implicit namespace package. Add an __init__.py?
```

I'm also not sure how common it is to have modules directly nested under `src/`, but the single file in the case above could be ignored:

```toml
[tool.ruff.per-file-ignores]
"src/main.py" = ["INP001"]
```

---

_Comment by @notypecheck on 2023-01-18 17:36_

I'd say having `src` directory is quite common, and specifying multiple files in `per-file-ignores` is a bit tedious 
Maybe there is a way to ignore files that are placed directly in a directory? 🤔 

---

_Comment by @edgarrmondragon on 2023-01-18 17:42_

> I'd say having `src` directory is quite common

Oh, I'm aware of that, I use it all the time 😄. It's how common it is to have Python files _directly_ nested, e.g. `src/main.py` that I wonder.


---

_Comment by @notypecheck on 2023-01-18 17:50_

> > I'd say having `src` directory is quite common
> 
> Oh, I'm aware of that, I use it all the time 😄. It's how common it is to have Python files _directly_ nested, e.g. `src/main.py` that I wonder.

I'd say it's common, if there can be a package, there can be a module, they're interchangeable in some sense

---

_Comment by @charliermarsh on 2023-01-18 17:57_

But in that case, should there not be an `__init__.py` in that directory?

---

_Comment by @charliermarsh on 2023-01-18 17:57_

(Genuinely asking. I'm not certain.)

---

_Comment by @notypecheck on 2023-01-18 18:00_

In src? `src` itself is not a package, so, no 🤔   
@charliermarsh Maybe there's a way to select direct children in `per-file-ignores`? This would kind of solve this


---

_Comment by @charliermarsh on 2023-01-18 18:14_

Hmm, yeah, right now, this matches _any_ Python file under `src`, even in nested directories:

```toml
[tool.ruff.per-file-ignores]
"src/*.py" = ["INP"]
```

It's possible to change this internally, such that `*` doesn't match `/`, in which case, these would mean two different things:

```toml
[tool.ruff.per-file-ignores]
"src/*.py" = ["INP"]
"src/**/*.py" = ["INP"]
```

(The globbing library supports this as the `literal_separator` option.)

Let me look at how other tools do this. It would be a breaking change so I'd like to be confident in it.


---

_Comment by @charliermarsh on 2023-01-18 18:17_

(Flake8 has the same behavior (`"src/*.py"` matches recursively).)

---

_Comment by @charliermarsh on 2023-01-18 18:27_

@not-my-profile - Do you have an opinion on enabling `literal_separator` here? It seems strictly more expressive for users, at the cost of being a breaking change in some cases. (I know you mentioned that setting in https://github.com/charliermarsh/ruff/issues/1545.)

---

_Comment by @charliermarsh on 2023-01-18 19:51_

If we enable `literal_separator`, then `*.py` will no longer match "any Python file, anywhere" -- you'll have to do `**/*.py`.

---

_Comment by @not-my-profile on 2023-01-19 03:34_

Regarding the problem described by this issue: I think ideally ruff would be smart enough to understand the packaging config in `pyproject.toml` to correctly determine the purpose of the `src` directory, e.g by understanding:

```toml
[tool.setuptools.packages.find]
where = ["src"]
```

(see [setuptools-specific configuration](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#setuptools-specific-configuration))

However since there are a bunch of such packaging tools (setuptools, poetry, flit, etc.) accurately supporting all of them would be quite the challenge (I think it's unfortunate that such configuration isn't better standardized via a PEP).

So yes `per-file-ignores` does seem like a good way of solving this for now and increasing the expressivity of our glob patterns also seems like a good idea. I want to add that the [`glob.glob`](https://docs.python.org/3/library/glob.html#glob.glob) function from the Python standard library also does not have `*` match `/`, so following that behavior might actually be what Python developers are more accustomed to.

I do think that such breaking config changes are unfortunate, especially when they can break stuff without you noticing ... perhaps we could introduce something like Rust's `package.edition` .... e.g. `tool.ruff.config-version` so changes like these would result in a config version bump with ruff still supporting the older version and supporting an automated config upgrade (which would translate all existing globs to the new syntax). Implementing that would however of course take some effort and such "sneaky" config changes (that just change how values are interpreted) are probably quite rare anyway ... although as ruff continues to gain adoption the collective pain caused by such breaking changes would also grow, so perhaps we should consider it?

---

_Comment by @notypecheck on 2023-01-19 05:49_

I'd say since glob uses `**` for recursive matching it's a good idea to either make it standard or add an option to choose between the two formats.

---

_Comment by @charliermarsh on 2023-01-19 12:44_

Yeah using `**` does seem better to me. But it does feel like a disruptive change if it's not coupled with some kind of opt-in. Let me think about how to handle.

It still seems to me that omitting the `src` directories from this check would solve this problem, since anything is `src` is assumed to be part of the Python path, effectively.
 

---

_Comment by @not-my-profile on 2023-01-19 15:01_

I'd rather not have a `glob_syntax` setting for the sake of simplicity ... I think it makes more sense to have a versioned config format and deprecate the old syntax in favor of the new one ... which could also be done in a backwards-compatible manner (and let us easily drop support for the old syntax at some point in the future).

---

_Comment by @charliermarsh on 2023-01-19 15:08_

Yes agreed.

---

_Closed by @charliermarsh on 2023-02-04 00:20_

---
