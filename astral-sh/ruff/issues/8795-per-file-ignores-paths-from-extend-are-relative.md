```yaml
number: 8795
title: "`per-file-ignores` paths from `extend` are relative to the extended configuration file"
type: issue
state: open
author: bwildenborg
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2023-11-20T19:29:11Z
updated_at: 2025-08-22T22:12:34Z
url: https://github.com/astral-sh/ruff/issues/8795
synced_at: 2026-01-10T11:09:51Z
```

# `per-file-ignores` paths from `extend` are relative to the extended configuration file

---

_Issue opened by @bwildenborg on 2023-11-20 19:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I am running Ruff 0.1.4 and trying to extend from a set of common ruff settings in a ruff.toml file.
The complete ruff.toml looks like:
```
preview = true
explicit-preview-rules = true

# Enables
select = [
    "E",      # pycodestyle
    "W",      # pycodestyle
    "F",      # Pyflakes
    "I",      # isort
    "N",      # pep8-naming
    "D",      # pydocstyle
    "UP",     # pyupgade
    "ANN",    # flake8-annotations
    "BLE",    # flake8-blind-except
    "FBT",    # flake8-boolean-trap
    "B",      # flake8-bugbear
    "A",      # flake8-builtins
    "C4",     # flake8-comprehensions
    "FA",     # flake8-future-annotations
    "ISC",    # flake8-implicit-str-concat
    "G",      # flake8-logging-format
    "PIE",    # flake8-pie
    "T20",    # flake8-print
    "PYI",    # flake8-pyi
    "RSE",    # flake8-raise
    "RET",    # flake8-return
    "SLF",    # flake8-self
    "SIM",    # flake8-simplify
    "TID",    # flake8-tidy-imports
    "ARG",    # flake8-unused-arguments
    "FIX",    # flake8-fixme
    "ERA",    # eradicate
    "PL",     # Pylint
    "TRY",    # tryceratops
    "PERF",   # perflint
    "RUF",    # Ruff specific
    "CPY",    # flake8-copyright
    "CPY001", # must be explicitly enabled, still in development - checks for copyright header
]

# Ignores
ignore = [
    "ANN101",  # Don't require type annotations for self.
    "ANN102",  # Don't require type annotations for cls.
    "ANN401",  # Any is allowed.
    "D418",    # overload-with-docstring; Overloads should have docstrings when they alter the API/functionality.
    "FIX002",  # Allow TODO tags but no others.
    "TRY003",  # Makes no sense for builtin exceptions.
    "RSE102",  # This is a stylistic choice, but the check doesn't verify the item being raised is an object preventing a function from returning an error to raise.
    "PERF203", # No try...except in for loop - No clear action to take when this is flagged.
    "RUF013",  # This is a very basic check prone to false positives, let pyright handle it
    "E203",    # Whitespace before ':' - Don't lint for style beyond what black cares about.
    "PLR0904", # Too many public methods
    "PLR0911", # Too many return statements
    "PLR0912", # Too many branches
    "PLR0913", # Too many args
    "PLR0915", # Too many statements
    "PLR6301", # no-self-use - Switching to static has functionality considerations and may not be appropriate.
    "PLR2004", # Magic value used in comparison
    "FBT003", # Boolean positional value in function call - this flags an issue twice and forces per use suppression.
    "SIM108", # Use ternary; this can be good, but the check is overzealous and suggests changes that involve line breaks.
    "SIM117", # Use single with; like SIM108 this can be good, but it isn't appropriate in all contexts and could easily result in line breaks.
    "A003", # Class attribute shadowing builtin; doesn't matter for classes.
    "PIE790", # Unnecessary "..." literal; conflicts with pyright for protocols. Removing ... pyright expects a return statement.
    # The settings below are suggested to be disabled when formatting with Ruff.
    "W191", # tab indentation; conflicts with formatting.
    "E111", # indentation with with invalid multiple; conflicts with formatting.
    "E114", # indentation with invalid comment; conflicts with formatting.
    "E117", # over indented; conflicts with formatting.
    "D206", # indent with spaces; conflicts with formatting.
    "D300", # triple single quotes; conflicts with formatting.
    "Q000", # bad quotes inline string; conflicts with formatting.
    "Q001", # bad quotes multiline string; conflicts with formatting.
    "Q002", # bad quotes docstring; conflicts with formatting.
    "Q003", # avoidable escpaped quote; conflicts with formatting.
    "COM812", # missing trailing comma; conflicts with formatting.
    "COM819", # prohibited trailing comma; conflicts with formatting.
    "ISC001", # single line implicit string concatenation; conflicts with formatting.
    "ISC002", # multi line implicit string concatenation; conflicts with formatting.
]

# Allow autofix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]

unfixable = [
    "PIE794", # duplicate class field name - the fix is to delete one entry, which is unlikely to be the correct fix.
]

# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
]

line-length = 132

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[per-file-ignores]
"tests/**/*.py" = [
    "B018",    # B018 - Found useless expression - necessary for testing exceptions are raised.
    "D100",    # D100 - Module dostrings not required in test files.
    "D104",    # D104 - Package dostrings not required in test files.
    "ARG",     # ARG - Unused args are common in tests with mock patches and mock functions.
]

[pydocstyle]
# Use Google-style docstrings.
convention = "google"

[isort]
# Settings for google standard.
force-single-line = true
force-sort-within-sections = true
single-line-exclusions = [
    "typing",
    "collections.abc",
    "typing_extensions",
]
order-by-type = false

[flake8-annotations]
# Allow return annotations to be omitted for __init__ methods.
mypy-init-return = true

[flake8-tidy-imports]
# Disallow all relative imports.
ban-relative-imports = "all"

[flake8-unused-arguments]
ignore-variadic-names = true
```

In here I am trying to disable a couple of checks for test files.
The pyrpoject.toml looks like:
```
[tool.ruff]
extend = "./extern/ruff.toml"

# Allow imports relative to the "src" and "tests" directories.
src = ["src", "tests"]

extend-select = [
    "PT", # flake8-pytest
]

[tool.ruff.extend-per-file-ignores]
"tests/**/*.py" = [
    "PT019",   # pytest-fixture-without-paramvalue - Flags fixtures that don't return values
]
```
This is successfully suppressing the PT019 check, but the other per-file-ignore checks are still getting flagged (such as D100).
If I remove the extend-per-file-ignores from the pyproject.toml, the PT019 start getting flagged in addition to the per-file-ignores from ruff.toml
As far as I can tell, none of the per-file-ignores from ruff.toml are getting passed through.
I've also tried switching ruff.toml's per-file-ignores to be extend-per-file-ignores (and removing those from my pyproject.toml), but those weren't honored either.

---

_Comment by @zanieb on 2023-11-20 20:39_

Hm... I tested this with a simple structure:

**pyproject.toml**
```toml
[tool.ruff]
per-file-ignores = {"foo.py" = ["RUF001"]}
```
**nested/pyproject.toml**
```toml
[tool.ruff]
extend = "../pyproject.toml"
extend-per-file-ignores = {"foo.py" = ["RUF002"]}
```
**nested/foo.py**
```python
print("Ηello, world!")  # "Η" is the Greek eta (`U+0397`).


def foo():
    """A lovely docstring (with a `U+FF09` parenthesis）."""
```
**from nested/**
```shell
ruff check . --select RUF001,RUF002
```

and could not reproduce your issue, both RUF001 and RUF002 are suppressed. If I copy `foo.py` to `bar.py` the errors are raised there as expected.

Could you try to create a minimal reproducible example?

---

_Comment by @zanieb on 2023-11-20 20:44_

Actually, if I move `foo.py` into a subdirectory and use `subdir/foo.py` as my per-file-ignore this fails as you've described.

I believe the issue here is that the paths are relevant to the pyproject file they are defined in. So.. your imported pyproject file is defining per-file-ignores relative to its directory which do not match the files you are checking elsewhere. I'm not sure this should be changed.

The bigger question here is then: Should extending a config file (via `extend = ...`) make the settings pulled from it relative to the new config file?

---

_Comment by @bwildenborg on 2023-11-20 20:52_

Sure enough, setting the mask in my `ruff.toml` to `../tests/**/*.py` results in the ignores being honored.

---

_Comment by @charliermarsh on 2023-11-21 01:35_

> The bigger question here is then: Should extending a config file (via extend = ...) make the settings pulled from it relative to the new config file?

Both choices can be problematic in different contexts. Using the _extended_ file's directory as the base makes it useless if the extended file is, e.g., a global file that's shared between many projects (like in `~/ruff.toml`) -- but in that case, relative paths seem unusual? Using the directory of the file with the `extend` in it causes issues when you're extending a configuration in a parent directory from the project, as above.

The former seems a lot more common to me, so I’d say our current behavior is preferable to making any changes here.


---

_Label `configuration` added by @charliermarsh on 2023-11-21 01:35_

---

_Label `needs-decision` added by @charliermarsh on 2023-11-21 01:35_

---

_Comment by @charliermarsh on 2023-11-21 01:57_

(The other thing to note is that we match against both the absolute file path and its basename. So the “foo.py” exclusion works in Zanie’s example regardless of whether it’s defined in the parent or child configuration file — since it’ll match any file with the basename “foo.py”. The relative pathing stuff only applies to patterns that match multiple segments.)

---

_Comment by @bwildenborg on 2023-11-21 14:59_

For what it's worth, the only way I could fully ignore a setting in both test files and in their accompanying `__init__.py` was to split my checks into two parts:
```
[per-file-ignores]
"**/test_*.py" = [
    "B018",    # B018 - Found useless expression - necessary for testing exceptions are raised.
    "D100",    # D100 - Module docstrings not required in test files.
    "ARG",     # ARG - Unused args are common in tests with mock patches and mock functions.
]
"../**/test_*/__init__.py" = [
    "D104",    # D104 - Package docstrings not required in test packages.
]
```
The only pattern for the `__init__.py ` part that worked for me was to make it relative.
I had tried variations on `**/test_*/__init__.py` that weren't relative, but I couldn't get any to work.

---

_Renamed from "per-file-ignores not coming in from extend" to "`per-file-ignores` paths from `extend` are relative to the extended configuration file" by @zanieb on 2023-11-27 15:53_

---

_Comment by @fpuga on 2023-12-25 11:56_

I'm using a similar structure

```
my_project
| pyproject.toml
| back/
  | tests/
| config
  | pyproject.toml  
```

where 

**pyproject.toml**

```toml
# configurations personalized for this project 

[tool.ruff]
extend="./config/pyproject.toml"
line-length = 200
```


**config/pyproject.toml**

```toml
# common configurations for "all" projects of the team

[tool.ruff]
extend="./config/pyproject.toml"
line-length = 88

[tool.ruff.per-file-ignores]
"/**/tests/*" = ["S101"]
```

Playing with `--show-settings` I found that whitout a leading slash `"**/tests/*"` the paths are relative to the config file where are defined.


```
per_file_ignores: [
              (
                  GlobMatcher {
                      pat: Glob {
                          glob: "/var/development/myproject/myproject/config/**/tests/*",
                          re: "(?-u)^/var/development/myproject/myproject/config(?:/|/.*/)tests/.*$",
                          opts: GlobOptions {
                              case_insensitive: false,
                              literal_separator: false,
                              backslash_escape: true,
                              empty_alternates: false,
                          },
```

 But with a leading slash `/**/tests/*` it works as desired

```
per_file_ignores: [
              (
                  GlobMatcher {
                      pat: Glob {
                          glob: "/**/tests/*",
                          re: "(?-u)^(?:/|/.*/)tests/.*$",
                          opts: GlobOptions {
                              case_insensitive: false,
                              literal_separator: false,
                              backslash_escape: true,
                              empty_alternates: false,
                          },

```

` 


---

_Comment by @ewu63 on 2025-08-22 22:12_

Just ran into this issue so bumping it lightly. I have a global global config file located at `~/.config/ruff/ruff.toml`, and within a repo there is a specific `ruff.toml` that overrides some config
```
extend = "~/.config/ruff/ruff.toml"
<extra-config-here>
```

Based on the docs

> any relative paths in that configuration file (like `exclude` globs or `src` paths) are resolved relative to the _current working directory_.

I had expected my globs in the `per-file-ignores` section of my global config file to be respected relative to CWD when I import it via the `extend` flag, but that was not the case. When I run `ruff check` I get errors which were specifically ignored from the global config. However, if I run `ruff check` with the `--config-file ruff.toml` pointing to the _local_ config file, then the global paths become relative for some reason, even though _both invocations use the same local `ruff.toml` file_, and I no longer get any errors.

I think the expected behaviour from me would be for there to be no difference between the two invocations, and both times the globs in the `per-file-ignores` should be matched relative to the CWD.

---
