```yaml
number: 5846
title: Allow applying rule classes with noqa
type: issue
state: open
author: kieran-ryan
labels:
  - suppression
  - needs-decision
assignees: []
created_at: 2023-07-17T23:00:05Z
updated_at: 2023-07-19T01:22:01Z
url: https://github.com/astral-sh/ruff/issues/5846
synced_at: 2026-01-12T15:54:45Z
```

# Allow applying rule classes with noqa

---

_@kieran-ryan_

Multiple violations from the same rule class can be detected in a single statement e.g. two errors from the `FBT` rule class:

```console
warning: Invalid `# noqa` directive on line 57: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
package/main.py:57:5: FBT001 Boolean positional arg in function definition
package/main.py:57:20: FBT002 Boolean default value in function definition
Found 2 errors.
```

To suppress these violations, we can specify each error code as follows: `# noqa: FBT001, FBT002`.

Suggest to provide the functionality to specify the rule class, similar to how we can select or ignore rules with this pattern i.e. `# noqa: FBT`.

An autofix could be applied to update a noqa to use the rule class code where all rules of the class are specified e.g. autofix `# noqa: NYP001, NYP002, NYP003` to `# noqa: NYP`; and alternatively from the rule class back to specific error codes if its not suppressing violations for each rule in that class e.g. `# noqa: NYP` back to `# noqa: NYP001` - this would conflict with my first example as it would autofix `# noqa: FBT` to `# noqa: FBT001, FBT002` as the line does not violate all three errors of `FBT` - though is another option for how to approach this request.

---

_Label `noqa` added by @charliermarsh on 2023-07-19 01:22_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-19 01:22_

---
