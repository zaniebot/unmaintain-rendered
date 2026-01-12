```yaml
number: 1647
title: "Implement `autoflake`"
type: issue
state: closed
author: charliermarsh
labels:
  - plugin
assignees: []
created_at: 2023-01-05T00:09:48Z
updated_at: 2024-06-01T07:15:34Z
url: https://github.com/astral-sh/ruff/issues/1647
synced_at: 2026-01-12T15:54:41Z
```

# Implement `autoflake`

---

_@charliermarsh_

- [x] Remove unused imports (`F401`)
- [x] Remove duplicate keys (`F601`)
- [x] Remove unused variables (`F841`)
- [ ] Expand star imports (`F403`)
- [x] Useless `pass` statements

---

_Label `plugin` added by @charliermarsh on 2023-01-05 00:12_

---

_Comment by @charliermarsh on 2023-01-05 01:07_

I'm hesitant to add autofix for `F601` and `F403`, because the right behavior isn't clear, and automatically fixing them is very likely to cause problems.

I'd be open to autofixing `F601` in the case that the values are duplicated too.

---

_Comment by @charliermarsh on 2023-01-07 21:17_

Happy with this for now. I'm not planning to do star imports with the current approach, and we already do some subset of `pass` statements.

---

_Closed by @charliermarsh on 2023-01-07 21:17_

---

_Comment by @cclauss on 2023-03-17 06:17_

What about trying the https://github.com/asmeurer/removestar approach for creating an `F403 fixer`?

---

_Comment by @sanmai-NL on 2023-06-22 13:55_

@charliermarsh 

The last item
> Useless pass statements

is already implemented as https://beta.ruff.rs/docs/rules/unnecessary-pass/

---

_Comment by @RobertDeRose on 2024-06-01 07:08_

Why is F841 considered `unsafe`?

## Example

```bash
ruff check --fix --config pyproject.toml
gpio/models/pin.py:
  159:9 F841 Local variable `unused` is assigned to but never used

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

This was a test to see if the behavior of Ruff is the same as autoflake, and it is not. autoflake removes it, Ruff requires the addition of `--unsafe-fixes`

Ruff version: ruff 0.4.4 (3e8878a1c 2024-05-09)

---
