---
number: 14451
title: Used --statistics option does hide the rule violations
type: issue
state: open
author: ssbarnea
labels:
  - cli
  - needs-design
assignees: []
created_at: 2024-11-19T13:26:34Z
updated_at: 2024-11-20T08:12:10Z
url: https://github.com/astral-sh/ruff/issues/14451
synced_at: 2026-01-07T13:12:16-06:00
---

# Used --statistics option does hide the rule violations

---

_Issue opened by @ssbarnea on 2024-11-19 13:26_

The fact that `ruff check --statistics` does disabled the output of effective line violations makes it impossible to us this as an addon option for improved output, especially when using ruff using as a pre-commit hook.

If that was its initial intention, it was implemented wrongly because it should have being just `--output-format=statistics` if it replaces the output.

This could be sorted by ensuring that `--statistics` does not affect the output format and that is output goes to stderr so it would not break tools that might process the standard output formats, which could be machine parseable.

Story: As an user, I want to see some stats about found violations, after they are listed.

---

_Label `cli` added by @MichaReiser on 2024-11-20 08:08_

---

_Comment by @MichaReiser on 2024-11-20 08:12_

I understand your use case. But just writing to stderr is not necessarily a solution because `--statistics` also impacts other output formats. 

```
❯ uvx ruff check . --statistics --output-format json > output.json

test on  main [$✘!?] via 🐍 v3.12.7 
❯ bat output.json
───────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: output.json
───────┼───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ [
   2   │   {
   3   │     "code": "I001",
   4   │     "name": "unsorted-imports",
   5   │     "count": 6,
   6   │     "fixable": true
   7   │   }
   8   │ ]
```

I think the ask here is to make `--statistics` additive. Ruff should include the statistics in the output in addition to the individual diagnostics. But I'm not 100% sure about that. This requires fleshing out the UX first

---

_Label `needs-design` added by @MichaReiser on 2024-11-20 08:12_

---
