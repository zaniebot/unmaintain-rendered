```yaml
number: 18597
title: "Deprecate `pytest-warns-without-warning` (`PT029`)?"
type: issue
state: open
author: ntBre
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-09T19:57:24Z
updated_at: 2025-06-09T19:59:01Z
url: https://github.com/astral-sh/ruff/issues/18597
synced_at: 2026-01-12T15:54:56Z
```

# Deprecate `pytest-warns-without-warning` (`PT029`)?

---

_@ntBre_

While preparing to stabilize the rule in 0.12, we realized that at a minimum the rationale in the rule docs:

> pytest.warns expects to receive an expected warning as its first argument. If omitted, **the pytest.warns call will fail at runtime.**

is no longer correct in pytest 8.4. Further, it seems this has not been correct since at least pytest 7.0 in early 2022. The "new" default is `Warning`, which is quite broad and could naturally fall under [pytest-warns-too-broad (PT030)](https://docs.astral.sh/ruff/rules/pytest-warns-too-broad/#pytest-warns-too-broad-pt030) instead.

> Yeah, I think at least we should not stabilize this for now. I think the change is [here](https://docs.pytest.org/en/stable/changelog.html#pytest-7-0-0-2022-02-03:~:text=%238645%3A%20pytest.warns(None)%20is%20now%20deprecated%20because%20many%20people%20used%20it%20to%20mean%20%E2%80%9Cthis%20code%20does%20not%20emit%20warnings%E2%80%9D%2C%20but%20it%20actually%20had%20the%20effect%20of%20checking%20that%20the%20code%20emits%20at%20least%20one%20warning%20of%20any%20type%20%2D%20like%20pytest.warns()%20or%20pytest.warns(Warning).) in the [pytest 7.0.0rc1](https://docs.pytest.org/en/stable/changelog.html#pytest-7-0-0rc1-2021-12-06) release notes from 2021-12-06 and in this PR: https://github.com/pytest-dev/pytest/issues/8645.

_Originally posted by @ntBre in https://github.com/astral-sh/ruff/issues/18567#issuecomment-2956831200_
            

---

_Label `rule` added by @ntBre on 2025-06-09 19:57_

---

_Label `needs-decision` added by @ntBre on 2025-06-09 19:57_

---
