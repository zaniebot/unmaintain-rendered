---
number: 9891
title: Add easier way to gather all available rules from ruff
type: issue
state: closed
author: qarmin
labels:
  - question
  - cli
assignees: []
created_at: 2024-02-08T08:07:04Z
updated_at: 2025-10-20T07:09:53Z
url: https://github.com/astral-sh/ruff/issues/9891
synced_at: 2026-01-07T13:12:15-06:00
---

# Add easier way to gather all available rules from ruff

---

_Issue opened by @qarmin on 2024-02-08 08:07_

Since 0.2.0 version, my fuzzer CI stopped working.

In CI, first I start to search for broken files, minimize sample size and at the end minimize number of rules, that are needed to produce problem.

Minimization of rules is done via this steps:
- getting all rules from ruff - this is done via `ruff rule --all` command and manually parsing all rules in ()
- splitting available rules by half and checking if ruff still produce error
- repeating until found minimal amount of rules

0.2.0 version removed rule PLR1706 which is still available in `ruff rule --all` with section `## Removal` and ruff started to fail each time with message
```
ruff failed
  Cause: Rule `PLR1706` was removed and cannot be selected.
```

At least for now I can modify a little rule parser, but probably this will be more problematic in future.

So maybe ruff could produce simple list of rules(enabled/removed etc.) like `AN001,AN002` or export them to json file via `rull rule --export a.json` or ruff rule --available-simple?

---

_Comment by @charliermarsh on 2024-02-10 23:59_

Does `ruff rule --all --output-format json` work for this?

---

_Label `question` added by @charliermarsh on 2024-02-10 23:59_

---

_Label `cli` added by @charliermarsh on 2024-02-10 23:59_

---

_Comment by @qarmin on 2024-02-11 06:12_

Eh.., I searched for a similar option but did not find it.

It would be good to add to this json the fields `"removed'': bool` and `"deprecated'': bool` to avoid having to parse explanation field.




---

_Comment by @ofek on 2024-02-18 18:12_

Yes I was just about to open a feature request for this; currently the JSON output does not contain any information regarding whether a rule still exists. Either there should be a key or there should be a flag to exclude removed rules.

---

_Referenced in [astral-sh/ruff#20168](../../astral-sh/ruff/pulls/20168.md) on 2025-08-30 16:01_

---

_Closed by @MichaReiser on 2025-10-20 07:09_

---
