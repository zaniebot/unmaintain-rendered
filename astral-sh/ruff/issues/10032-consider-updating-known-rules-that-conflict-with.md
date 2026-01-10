```yaml
number: 10032
title: Consider updating known rules that conflict with formatter
type: issue
state: closed
author: ofek
labels: []
assignees: []
created_at: 2024-02-18T20:33:11Z
updated_at: 2024-02-19T07:32:32Z
url: https://github.com/astral-sh/ruff/issues/10032
synced_at: 2026-01-10T11:09:52Z
```

# Consider updating known rules that conflict with formatter

---

_Issue opened by @ofek on 2024-02-18 20:33_

This document https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules

I just upgraded Hatch static analysis to use the latest 0.2.2 from 0.1.9 and I think rules E301-E306 either conflict or are at least superfluous so I'm de-selecting those.

---

_Comment by @MichaReiser on 2024-02-19 07:32_

Thanks for the feedback. These are preview rules and you're feedback on the formatter incompatibility is extremely helpful. 

We should make the rules compatible with the formatter inside of pyi files.  I'll merge this with https://github.com/astral-sh/ruff/issues/10039

---

_Closed by @MichaReiser on 2024-02-19 07:32_

---
