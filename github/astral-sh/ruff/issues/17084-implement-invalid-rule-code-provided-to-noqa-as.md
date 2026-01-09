---
number: 17084
title: "Implement \"Invalid rule code provided to `# noqa`\" as rule"
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2025-03-31T08:43:46Z
updated_at: 2025-04-04T08:06:00Z
url: https://github.com/astral-sh/ruff/issues/17084
synced_at: 2026-01-07T13:12:16-06:00
---

# Implement "Invalid rule code provided to `# noqa`" as rule

---

_Issue opened by @spaceone on 2025-03-31 08:43_

### Summary

I have a lot of warning about codes which were removed in ruff:

`warning: Invalid rule code provided to '# noqa' at foo.py:123: ASYNC101`

It would be nice if that wasn't a warning but another ruff rule, so that i can easily let the wrong codes be removed with `--fix`. 

---

_Label `rule` added by @MichaReiser on 2025-03-31 08:46_

---

_Comment by @maxmynter on 2025-04-01 18:30_

I would try my luck with this one, if that's fine :) 

---

_Assigned to @maxmynter by @ntBre on 2025-04-01 18:32_

---

_Referenced in [astral-sh/ruff#17138](../../astral-sh/ruff/pulls/17138.md) on 2025-04-02 03:20_

---

_Closed by @MichaReiser on 2025-04-04 08:06_

---
