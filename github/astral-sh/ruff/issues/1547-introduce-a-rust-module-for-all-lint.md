---
number: 1547
title: Introduce a Rust module for all lint implementations
type: issue
state: closed
author: not-my-profile
labels:
  - internal
assignees: []
created_at: 2023-01-02T07:41:30Z
updated_at: 2023-01-14T16:48:03Z
url: https://github.com/astral-sh/ruff/issues/1547
synced_at: 2026-01-07T13:12:14-06:00
---

# Introduce a Rust module for all lint implementations

---

_Issue opened by @not-my-profile on 2023-01-02 07:41_

The code is currently split up across the following Rust modules:

* ast
* autofix
* cache
* checkers
* checks
* checks_gen
* cli
* commands
* cst
* directives
* docstrings
* eradicate
* flake8_2020
* flake8_annotations
* flake8_bandit
* flake8_blind_except
* flake8_boolean_trap
* flake8_bugbear
* flake8_builtins
* flake8_comprehensions
* flake8_datetimez
* flake8_debugger
* flake8_errmsg
* flake8_implicit_str_concat
* flake8_import_conventions
* flake8_print
* flake8_quotes
* flake8_return
* flake8_simplify
* flake8_tidy_imports
* flake8_unused_arguments
* fs
* isort
* iterators
* lex
* lib_native
* linter
* logging
* mccabe
* message
* noqa
* packages
* pandas_vet
* pep8_naming
* printer
* pycodestyle
* pydocstyle
* pyflakes
* pygrep_hooks
* pylint
* python
* pyupgrade
* resolver
* ruff
* rustpython_helpers
* settings
* source_code_generator
* source_code_locator
* source_code_style
* updates
* vendor
* visibility

62 top-level modules is a lot and makes the codebase hard to grasp and navigate, so I recommend moving all modules that implement specific rules/checks/lints ([whatever we call  them](https://github.com/charliermarsh/ruff/issues/1546)) under a common  module, so for example:

* lints
  * eradicate
  * flake8_2020
  * flake8_annotations
  * flake8_bandit
  * flake8_blind_except
  * flake8_boolean_trap
  * flake8_bugbear
  * flake8_builtins
  * flake8_comprehensions
  * flake8_datetimez
  * flake8_debugger
  * flake8_errmsg
  * flake8_implicit_str_concat
  * flake8_import_conventions
  * flake8_print
  * flake8_quotes
  * flake8_return
  * flake8_simplify
  * flake8_tidy_imports
  * flake8_unused_arguments
  * isort
  * mccabe
  * pandas_vet
  * pep8_naming
  * pycodestyle
  * pydocstyle
  * pyflakes
  * pygrep_hooks
  * pylint
  * pyupgrade
  * ruff

So e.g. `ruff:flake8_builtins` would become `ruff:rules:flake8_builtins`, `ruff:ruff` would become `ruff:rules:ruff`, etc.

---

_Comment by @charliermarsh on 2023-01-02 17:07_

This seems reasonable to me.

---

_Label `internal` added by @charliermarsh on 2023-01-02 17:07_

---

_Comment by @squiddy on 2023-01-02 17:54_

Slightly related: Any thoughts on splitting up the code into multiple crates? From my limited understanding this is one way to go for faster compiles. Multiple crates can be compiled in parallel. I did some profiling the other day and a release build takes quite some time because everything is one crate currently.

---

_Comment by @charliermarsh on 2023-01-02 18:34_

Agreed. Are you interested in exploring that? I'd be happy to review a proposal.

---

_Comment by @colin99d on 2023-01-02 22:05_

I have another question here. I am not aware of a specific example of this happening, but if two lints do the exact same thing, are we ever going to track that and only run it once (or at least have a flag to do this)?

---

_Comment by @colin99d on 2023-01-02 22:05_

For example, I believe pylint and flake8 both have a line length checker.

---

_Comment by @charliermarsh on 2023-01-02 22:18_

This problem exists today. The way I view it is that we may want a one-to-many mapping between `CheckKind` and `CheckCode` -- so we'd only have one implementation of the check, but it could map to multiple valid codes. I'm not sure it's really worth the complexity though.


---

_Comment by @squiddy on 2023-01-03 06:01_

> Agreed. Are you interested in exploring that? I'd be happy to review a proposal.

Certainly, but with vacations over this will happen on the weekend the earliest. :)

---

_Comment by @charliermarsh on 2023-01-03 12:23_

Take your time of course, and thank you as always :)

---

_Comment by @kaos on 2023-01-08 13:15_

> Slightly related: Any thoughts on splitting up the code into multiple crates? From my limited understanding this is one way to go for faster compiles. Multiple crates can be compiled in parallel. I did some profiling the other day and a release build takes quite some time because everything is one crate currently.

I'm here hoping to find a crate I could use in a rust project to re-use the AST logic to do some custom node visiting. Perhaps the core AST related parts could be in a published crate? :) 
I'm rather new to Rust but happy to assist if this makes sense to y'all..

Edit: Found #1407 which I think could work for my use case as well.
Edit2: Ah, unless it's very high level API only..
Edit3: Or, I see most parsing comes from the `rustpython_parser` crate, so I might just as well try using that one directly.

---

_Comment by @not-my-profile on 2023-01-08 14:45_

@kaos ruff uses the parser from [RustPython](https://github.com/RustPython/RustPython), which unfortunately [has not been published to crates.io in over 2 years](https://crates.io/search?q=%22rustpython%22) for some reason. The relevant issue for that is https://github.com/RustPython/RustPython/issues/4179.

---

_Comment by @charliermarsh on 2023-01-08 20:41_

@kaos - Yeah my suggestion would be to use the RustPython parser. You have to depend on the Git hash directly for now, unfortunately.

---

_Comment by @kaos on 2023-01-09 00:45_

I think I can work with that. Thanks for the quick responses. 🙏🏽 

---

_Referenced in [astral-sh/ruff#1739](../../astral-sh/ruff/pulls/1739.md) on 2023-01-09 03:20_

---

_Comment by @charliermarsh on 2023-01-11 16:35_

I'd also like to adopt a more typical project structure. E.g., we could follow `ripgrep`, and have a top-level `crates` directory,  with a single library facade (`crates/grep`), and the binary distributed as the top-level `Cargo.toml`.

---

_Comment by @not-my-profile on 2023-01-11 16:46_

I think I'd rather have the crates in the top-level directory, which is also quite common (e.g. [clippy has this structure](https://github.com/rust-lang/rust-clippy/)).

However I don't really think it's that important. What I do dislike is the top-level `ruff` directory since it's non-descriptive ... I think it would make sense to move all the Python-distribution-specific stuff (pyproject.toml etc.)  to `ruff_python/` or something like that.

---

_Comment by @charliermarsh on 2023-01-11 16:56_

Happy to review a PR for that if anyone gets around to it, probably requires a bunch of Maturin reconfiguration.

---

_Comment by @charliermarsh on 2023-01-12 00:54_

A distinction that does seem potentially useful, from `ripgrep`, is that the library crate is published separately from the binary.

---

_Comment by @charliermarsh on 2023-01-12 03:47_

Or maybe @messense knows how to rename the top-level `ruff` dir.

---

_Comment by @messense on 2023-01-12 03:49_

You could move `ruff` to `python/ruff` and set `python-source` in pyproject.toml

```toml
[tool.maturin]
python-source = "python"
```

---

_Comment by @charliermarsh on 2023-01-12 03:49_

Oh nice, thank you very much.

---

_Referenced in [astral-sh/ruff#1805](../../astral-sh/ruff/issues/1805.md) on 2023-01-12 03:55_

---

_Referenced in [astral-sh/ruff#1781](../../astral-sh/ruff/issues/1781.md) on 2023-01-12 14:55_

---

_Referenced in [astral-sh/ruff#1820](../../astral-sh/ruff/issues/1820.md) on 2023-01-12 16:13_

---

_Comment by @not-my-profile on 2023-01-12 16:14_

> Slightly related: Any thoughts on splitting up the code into multiple crates? From my limited understanding this is one way to go for faster compiles. Multiple crates can be compiled in parallel. I did some profiling the other day and a release build takes quite some time because everything is one crate currently.

I opened a dedicated issue for that (#1820) to keep this issue on topic :)

---

_Referenced in [astral-sh/ruff#1865](../../astral-sh/ruff/pulls/1865.md) on 2023-01-14 05:34_

---

_Closed by @charliermarsh on 2023-01-14 16:48_

---

_Referenced in [astral-sh/ruff#2059](../../astral-sh/ruff/issues/2059.md) on 2023-01-21 14:45_

---
