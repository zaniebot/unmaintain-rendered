```yaml
number: 10645
title: "`D100` should be ignored on test modules (at least with Google convention)"
type: issue
state: open
author: sbrudenell
labels:
  - rule
  - docstring
  - linter
assignees: []
created_at: 2024-03-28T06:40:03Z
updated_at: 2024-03-28T13:52:18Z
url: https://github.com/astral-sh/ruff/issues/10645
synced_at: 2026-01-10T11:09:52Z
```

# `D100` should be ignored on test modules (at least with Google convention)

---

_Issue opened by @sbrudenell on 2024-03-28 06:40_

I have:
```
$ ruff --version
ruff 0.3.4
```
```toml
# pyproject.toml
[tool.ruff.lint.pydocstyle]
convention = "google"
```
(I wasn't certain how to provide this config with `--config <something> --isolated`)
```
$ ruff check  --select D100 tests/
tests/fooutil/fooutil_test.py:1:1: D100 Missing docstring in public module
```
But [the Google style guide](https://google.github.io/styleguide/pyguide.html#3821-test-modules) says for this case:
>Module-level docstrings for test files are not required. They should be included only when there is additional information that can be provided.

So I think `D100` shouldn't be generated for test modules by default. I can configure this with `per-file-ignores`, but perhaps this should be implied by `convention = "google"`.

I'm not sure what pydocstyle's intended behavior was in this case. [The pydocstyle hosted docs site](https://pydocstyle.org/en/stable/) isn't loading for me right now. But pydocstyle is fully deprecated (in favor of ruff) and archived, so perhaps there's no value in replicating their behavior.

---

_Label `rule` added by @AlexWaygood on 2024-03-28 06:41_

---

_Label `docstring` added by @AlexWaygood on 2024-03-28 06:41_

---

_Label `linter` added by @AlexWaygood on 2024-03-28 06:41_

---

_Comment by @sbrudenell on 2024-03-28 08:01_

Perhaps `D104` should be disabled for tests as well. The Google style guide does say that packages should have top-level docstrings, and does *not* call out that this is *not* required for *test* packages, but IMHO I can't imagine the authors intended for boilerplate to be required for packages but not for modules.

---

_Comment by @MichaReiser on 2024-03-28 13:47_

I like the idea

I think one part that ruff lacks today to implement this is an understanding of what a test-module is, because that depends on your project structure. That's why `per-file-ignores` is the recommended solution for now.

We have the same problem the other way round where some pytest rules are flagged outside of tests:
* https://github.com/astral-sh/ruff/issues/7286
* https://github.com/astral-sh/ruff/issues/9803

and more

---
