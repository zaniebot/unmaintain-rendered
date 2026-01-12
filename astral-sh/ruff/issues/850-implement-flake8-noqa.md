```yaml
number: 850
title: Implement flake8-noqa
type: issue
state: open
author: JonathanPlasse
labels:
  - plugin
assignees: []
created_at: 2022-11-21T11:28:19Z
updated_at: 2025-08-26T08:49:22Z
url: https://github.com/astral-sh/ruff/issues/850
synced_at: 2026-01-12T15:54:40Z
```

# Implement flake8-noqa

---

_@JonathanPlasse_

# [`flake8-noqa`](https://pypi.org/project/flake8-noqa/)

## Error codes

- [x] `NQA001` "#noqa" must have a single space after the hash, e.g. "# noqa"
- [x] `NQA002` "# noqa X000" must have a colon, e.g. "# noqa: X000"
- [x] `NQA003` "# noqa : X000" must not have a space before the colon, e.g. "# noqa: X000"'
- [ ] `NQA004` "# noqa:  X000" must have at most one space before the codes, e.g. "# noqa: X000"
- [x] `NQA005` "# noqa: X000, X000" has duplicate codes, remove X000
- [x] `NQA101` "# noqa" has no violations
- [x] `NQA102` "# noqa: X000" has no matching violations
- [x] `NQA103` as `RUF100` "# noqa: X000, X001" has unmatched code(s), remove X001
- [x] `NQA104` as `PGH004` "# noqa" must have codes, e.g. "# noqa: X000" (enable via noqa-require-code)

### Flake8 specific rules

Should we ignore them?

- [ ] `NQA011` "#flake8: noqa" must have a single space after the hash, e.g. "# flake8: noqa"
- [ ] `NQA012` "# flake8 noqa" must have a colon or equals, e.g. "# flake8: noqa"
- [ ] `NQA013` "# flake8 : noqa" must not have a space before the colon, e.g. "# flake8: noqa"

## Options

- [x] `noqa-require-code` as `PGH004` : Require code(s) to be included in # noqa comments
- [x] `noqa-no-require-code` : Do not require code(s) in # noqa comments (default setting)
- [ ] `noqa-include-name` : Include plugin name in messages
- [ ] `noqa-no-include-name` : Do not include plugin name in messages (default setting)

---

_Label `rule` added by @charliermarsh on 2022-11-21 14:49_

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:18_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:18_

---

_Comment by @charliermarsh on 2023-03-22 03:11_

We have `NQA101`, `NQA102`, and `NQA103` as `RUF100`, but it'd be great to have better diagnostics around noqa validation.

---

_Comment by @augustelalande on 2024-03-08 21:53_

`NQA001` seems covered by `E262`

---

_Comment by @augustelalande on 2024-03-08 22:37_

I'm gonna work on `NQA002`, `NQA003`, `NQA004`, and `NQA005`.

---

_Comment by @augustelalande on 2024-04-27 04:17_

The functionality of `NQA002` and `NQA003` was added to `PGH004`

---

_Comment by @augustelalande on 2024-04-27 17:12_

The functionality of `NQA005` was added to `RUF100`

---

_Comment by @augustelalande on 2024-04-27 17:13_

FYI @JonathanPlasse `NQA001` and `NQA005` should be checked in the summary, but `NQA004` should be unchecked for now.

---

_Comment by @charliermarsh on 2024-04-27 17:51_

Okay, I think I corrected them.

---
