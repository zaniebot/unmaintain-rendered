```yaml
number: 13292
title: "[`flake8-pytest-style`] Fix defaults when `lint.flake8-pytest-style` config section is empty (`PT001`, `PT023`)"
type: pull_request
state: merged
author: dizzy57
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.7
head: pytest-parentheses-style-options-default
created_at: 2024-09-09T13:59:37Z
updated_at: 2024-10-08T17:01:23Z
url: https://github.com/astral-sh/ruff/pull/13292
synced_at: 2026-01-12T15:55:43Z
```

# [`flake8-pytest-style`] Fix defaults when `lint.flake8-pytest-style` config section is empty (`PT001`, `PT023`)

---

_@dizzy57_

## Summary

The documentation states that `lint.flake8-pytest-style.*-parentheses` options default to `false`, but if the user provides an empty `[lint.flake8-pytest-style]` config section, those options are suddenly set to `true`.

This is a bug. After #12838 changed the defaults, `Flake8PytestStyleOptions::try_into_settings` wasn't updated and still relies on the preview mode to set the defaults.


## Reproduction
```
% ruff --version
ruff 0.6.4

% cat example.py
import pytest


@pytest.fixture()
def parentheses(): ...


@pytest.fixture
def no_parentheses(): ...

% ruff check --select PT001 --output-format concise example.py
example.py:4:1: PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
Found 1 error.
[*] 1 fixable with the `--fix` option.

% echo '[lint.flake8-pytest-style]' > ruff.toml

% ruff check --select PT001 --output-format concise example.py
example.py:8:1: PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

## Test Plan

Manual testing, see reproduction section.

```
% ./target/debug/ruff check --select PT001 --output-format concise example.py
warning: Detected debug build without --no-cache.
example.py:4:1: PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
Found 1 error.
[*] 1 fixable with the `--fix` option.

% echo '[lint.flake8-pytest-style]' > ruff.toml

% ./target/debug/ruff check --select PT001 --output-format concise example.py
warning: Detected debug build without --no-cache.
example.py:4:1: PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Given the fact that this was the only reference to `ruff_linter::settings::types::PreviewMode` in `crates/ruff_workspace/src/options.rs`, looks like no other settings depend on the preview mode, and it doesn't make much sense to me to write any additional tests that verify there's no interaction between preview mode and other default values.

---

_Renamed from "[`flake8-pytest-style`] Fix defaults for missing `lint.flake8-pytest-style.*-parentheses` configs (`PT001`, `PT023`)" to "[`flake8-pytest-style`] Fix defaults when `lint.flake8-pytest-style` config section is empty (`PT001`, `PT023`)" by @dizzy57 on 2024-09-09 14:02_

---

_Comment by @github-actions[bot] on 2024-09-09 14:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-09-09 20:53_

LGTM! Sorry we didn't catch this, that's on us :-(

Unfortunately, however, this is will be a breaking change for some users, so we'll have to merge this as part of the 0.7 release. That should come at some point in the next few weeks, though; shouldn't be too long to wait :)

---

_Added to milestone `v0.7` by @AlexWaygood on 2024-09-09 20:53_

---

_Label `breaking` added by @MichaReiser on 2024-09-09 21:07_

---

_Label `configuration` added by @MichaReiser on 2024-09-09 21:07_

---

_Merged by @AlexWaygood on 2024-10-08 13:33_

---

_Closed by @AlexWaygood on 2024-10-08 13:33_

---

_Branch deleted on 2024-10-08 17:01_

---
