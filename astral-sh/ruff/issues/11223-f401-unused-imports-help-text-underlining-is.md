```yaml
number: 11223
title: F401 (unused imports) - Help text underlining is confusing
type: issue
state: open
author: plredmond
labels:
  - rule
assignees: []
created_at: 2024-04-30T23:48:27Z
updated_at: 2024-05-09T17:47:38Z
url: https://github.com/astral-sh/ruff/issues/11223
synced_at: 2026-01-10T11:09:53Z
```

# F401 (unused imports) - Help text underlining is confusing

---

_Issue opened by @plredmond on 2024-04-30 23:48_

In https://github.com/astral-sh/ruff/pull/11168 we [noticed](https://github.com/astral-sh/ruff/pull/11168#discussion_r1583791309) that the help text is only underlining the as-name part of an unused import, e.g.
```
47 | from . import renamed as bees # F401: no fix
   |                          ^^^^ F401
   |
   = help: Remove unused import: `.renamed`
```
This might be confusing for users and should be fixed.

---

_Label `rule` added by @dhruvmanila on 2024-05-01 09:43_

---

_Comment by @plredmond on 2024-05-09 17:43_

@zanieb I added this issue at your instruction, but I'm not sure that it's required any longer.

I think the justification for only underlining the /binding/ that is unused, and not the whole /statement/ is because of the case where there are multiple bindings in a single statement, some of which are used and some of which aren't.

<details><summary>Here's a fixture test that demonstrates this:</summary>

https://github.com/astral-sh/ruff/blob/1798425d76a140c1deb9a20b86715489855ff7a2/crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_conditional/__init__.py#L1-L16

and its resolution:

https://github.com/astral-sh/ruff/blob/1798425d76a140c1deb9a20b86715489855ff7a2/crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_29__all_conditional____init__.py.snap#L1-L24

</details>

---
