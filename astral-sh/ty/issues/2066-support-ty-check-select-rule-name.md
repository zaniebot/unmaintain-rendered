```yaml
number: 2066
title: "support `ty check --select ${RULE_NAME}`"
type: issue
state: closed
author: spaceone
labels:
  - cli
assignees: []
created_at: 2025-12-18T12:28:46Z
updated_at: 2025-12-18T12:59:34Z
url: https://github.com/astral-sh/ty/issues/2066
synced_at: 2026-01-10T01:54:00Z
```

# support `ty check --select ${RULE_NAME}`

---

_Issue opened by @spaceone on 2025-12-18 12:28_

It would be very nice, if one can go through all of the same issues.

e.g.:
 `ty check --select 'invalid-argument-type,unresolved-import'`

---

_Label `cli` added by @AlexWaygood on 2025-12-18 12:30_

---

_Label `configuration` added by @AlexWaygood on 2025-12-18 12:30_

---

_Assigned to @MichaReiser by @AlexWaygood on 2025-12-18 12:30_

---

_Comment by @MichaReiser on 2025-12-18 12:32_

Can you say more? Is it about supporting `--select` over `--warn`, `--error`, `--ignore` or that you can pass the arguments comma separated?

---

_Label `needs-info` added by @MichaReiser on 2025-12-18 12:32_

---

_Comment by @spaceone on 2025-12-18 12:44_

I want to only check for the specified rules (or at least one rule).

Think of it exactly like `ruff --select E123,F456`.
I have like 10 violations and don't want to add them all to `--ignore` to go in a structured way through all of the same violations when starting to work on a code base which didn't have any type-check integration.

---

_Comment by @MichaReiser on 2025-12-18 12:47_

I see. I don't think we'll add `--select`. Instead, the solution is to use an `ALL` selector that lets you easily deselect all other rules. See https://github.com/astral-sh/ty/issues/174

---

_Comment by @AlexWaygood on 2025-12-18 12:48_

Yes, I believe the design that we're currently considering is something like `--ignore=ALL --error=invalid-argument-type --error=unresolved-import`.

Being able to use comma-separated values _might_ be nice, though, so you could do `--ignore=ALL --error="invalid-argument-type,unresolved-import"`

---

_Label `needs-info` removed by @MichaReiser on 2025-12-18 12:49_

---

_Label `configuration` removed by @MichaReiser on 2025-12-18 12:49_

---

_Comment by @spaceone on 2025-12-18 12:52_

That's soo much typing. I use it heavily.
From UX perspective it's not good.
The same as that the default of output-format changed to `full` while I now configure the old `concise` to be able to have an overview but when I want to fix stuff I like the explanation. Then I have to type `--output-format full` instead of `-v` or `--full`.

---

_Comment by @MichaReiser on 2025-12-18 12:59_

For now, this isn't planned. We may reconsider after https://github.com/astral-sh/ty/issues/174

---

_Closed by @MichaReiser on 2025-12-18 12:59_

---
