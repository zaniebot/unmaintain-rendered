```yaml
number: 12106
title: "[`pytest`] Reverse `PT001` and `PT0023` defaults"
type: pull_request
state: merged
author: tjkuson
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: fix/invert-flake8-pytest-style
created_at: 2024-06-29T20:40:04Z
updated_at: 2024-07-01T03:40:14Z
url: https://github.com/astral-sh/ruff/pull/12106
synced_at: 2026-01-10T21:56:00Z
```

# [`pytest`] Reverse `PT001` and `PT0023` defaults

---

_Pull request opened by @tjkuson on 2024-06-29 20:40_

## Summary

This patch inverts the defaults for [pytest-fixture-incorrect-parentheses-style (PT001)](https://docs.astral.sh/ruff/rules/pytest-fixture-incorrect-parentheses-style/) and [pytest-incorrect-mark-parentheses-style (PT003)](https://docs.astral.sh/ruff/rules/pytest-incorrect-mark-parentheses-style/) to prefer dropping superfluous parentheses.

Presently, Ruff defaults to adding superfluous parentheses on pytest mark and fixture decorators for documented purpose of consistency; for example,

```diff
 import pytest


-@pytest.mark.foo
+@pytest.mark.foo()
 def test_bar(): ...
```

This behaviour is counter to the official pytest recommendation and diverges from the flake8-pytest-style plugin as of version 2.0.0 (see https://github.com/m-burst/flake8-pytest-style/issues/272). Seeing as either default satisfies the documented benefit of consistency across a codebase, it makes sense to change the behaviour to be consistent with pytest and the flake8 plugin as well.

This change is breaking, so is gated behind preview (at least under my understanding of Ruff versioning). The implementation of this gating feature is a bit hacky, but seemed to be the least disruptive solution without performing invasive surgery on the `#[option()]` macro.

Related to #8796.

### Caveat

Whilst updating the documentation, I sought to reference the pytest recommendation to drop superfluous parentheses, but couldn't find any official instruction beyond it being a revealed preference within the pytest documentation code examples (as well as the linked issues from a core pytest developer). Thus, the wording of the preference is deliberately timid; it's to cohere with pytest rather than follow an explicit guidance.

## Test Plan

`cargo nextest run`

I also ran

```sh
cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT001.py --no-cache --diff --select PT001
```

and compared against it with `--preview` to verify that the default does change under preview (I also repeated this with `echo '[tool.ruff]\npreview = true' > pyproject.toml` to verify that it works with a configuration file).


---

_Marked ready for review by @tjkuson on 2024-06-29 20:54_

---

_Label `fixes` added by @MichaReiser on 2024-06-30 16:11_

---

_Label `fixes` removed by @MichaReiser on 2024-06-30 16:12_

---

_Label `configuration` added by @MichaReiser on 2024-06-30 16:12_

---

_@charliermarsh approved on 2024-07-01 02:02_

This makes sense to me -- thanks.

---

_Label `preview` added by @charliermarsh on 2024-07-01 02:02_

---

_Renamed from "Reverse PT001 and PT0023 defaults" to "[`pytest`] Reverse `PT001` and `PT0023` defaults" by @charliermarsh on 2024-07-01 02:02_

---

_Merged by @charliermarsh on 2024-07-01 02:06_

---

_Closed by @charliermarsh on 2024-07-01 02:06_

---

_Branch deleted on 2024-07-01 03:40_

---
