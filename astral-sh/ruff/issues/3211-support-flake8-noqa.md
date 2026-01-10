```yaml
number: 3211
title: "Support `flake8-noqa`"
type: issue
state: closed
author: szymonmaszke
labels: []
assignees: []
created_at: 2023-02-24T18:18:34Z
updated_at: 2023-02-24T18:55:50Z
url: https://github.com/astral-sh/ruff/issues/3211
synced_at: 2026-01-10T11:09:46Z
```

# Support `flake8-noqa`

---

_Issue opened by @szymonmaszke on 2023-02-24 18:18_

Hey, we are currently using [`flake8-noqa`](https://github.com/plinss/flake8-noqa) in our FOSS [`py-template`](https://github.com/inovintell/py-template) project, but we would like to move to `ruff` exclusively from `flake8` + `flakeheaven`. 

This plugin parses `# noqa` comments and validates whether these are correct. I am not sure how are you parsing them, maybe this plugin is not needed at all if your rules are a little less strict than the ones in `flake8` (or some variation of it would be better?)

## Error Codes

| Code   | Message |
|--------|---------|
| NQA001 | "`#noqa`" must have a single space after the hash, e.g. "`# noqa`" |
| NQA002 | "`# noqa X000`" must have a colon, e.g. "`# noqa: X000`" |
| NQA003 | "`# noqa : X000`" must not have a space before the colon, e.g. "# noqa: X000"' |
| NQA004 | "<code># noqa:&nbsp;&nbsp;X000</code>" must have at most one space before the codes, e.g. "`# noqa: X000`" |
| NQA005 | "`# noqa: X000, X000`" has duplicate codes, remove X000 |
| NQA011 | "`#flake8: noqa`" must have a single space after the hash, e.g. "`# flake8: noqa`" |
| NQA012 | "`# flake8 noqa`" must have a colon or equals, e.g. "`# flake8: noqa`" |
| NQA013 | "`# flake8 : noqa`" must not have a space before the colon, e.g. "# flake8: noqa" |
| NQA101 | "`# noqa`" has no violations |
| NQA102 | "`# noqa: X000`" has no matching violations |
| NQA103 | "`# noqa: X000, X001`" has unmatched code(s), remove X001 |
| NQA104 | "`# noqa`" must have codes, e.g. "`# noqa: X000`" (enable via `noqa-require-code`) |

Is support for [`flake8-noqa`](https://github.com/plinss/flake8-noqa) planned/would be considered? Unfortunately we cannot pick up this issue ourselves at the current time.

---

_Comment by @JonathanPlasse on 2023-02-24 18:43_

- Duplicate of #850

There is already an option to warn if there is a blanket now's.


---

_Comment by @charliermarsh on 2023-02-24 18:55_

Some of these (`NQA101`, `NQA102`, and `NQA103`) are covered by [`RUF100`](https://beta.ruff.rs/docs/rules/#ruff-specific-rules-ruf), which detects unmatched `noqa` directives (including those with unknown codes). But yes I'm definitely open to including these.

---

_Comment by @charliermarsh on 2023-02-24 18:55_

Just gonna close as we have #850 already to track this, but consider this a +1 for that issue.

---

_Closed by @charliermarsh on 2023-02-24 18:55_

---
