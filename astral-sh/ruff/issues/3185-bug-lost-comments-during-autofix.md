```yaml
number: 3185
title: "bug: lost comments during autofix"
type: issue
state: closed
author: upstartjohnvandivier
labels:
  - fixes
assignees: []
created_at: 2023-02-23T18:58:42Z
updated_at: 2023-02-24T03:48:43Z
url: https://github.com/astral-sh/ruff/issues/3185
synced_at: 2026-01-10T11:09:46Z
```

# bug: lost comments during autofix

---

_Issue opened by @upstartjohnvandivier on 2023-02-23 18:58_

repro snippet autofixed from like:
```
        self.assertRegexpMatches(
            log,
            r"INFO:some.module:"
            # [35] foobar
            r"Titlecasedword [35] foo: "
            # comment i care about
            # other comment i care about
            r"\[\(\d{4}, \d{4,5}\), \d{5}\)\]",
            )
```
to like:
```
    assert re.search(
        "INFO:some.module: ...",
```

the logical code is correctly and nicely refactored but the intermediate comments are lost

can we either collect those just above the assert statement or omit blocks from refactor with interspersed comments?

---

_Label `autofix` added by @charliermarsh on 2023-02-23 19:01_

---

_Comment by @charliermarsh on 2023-02-23 19:02_

Yeah we can omit those from autofixing. We use that behavior for a bunch of rules, but it's not automatic so has to be enabled one-by-one as appropriate.

---

_Comment by @charliermarsh on 2023-02-23 19:02_

(Some autofixes also preserve comments, but it varies.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-24 03:39_

---

_Closed by @charliermarsh on 2023-02-24 03:48_

---
