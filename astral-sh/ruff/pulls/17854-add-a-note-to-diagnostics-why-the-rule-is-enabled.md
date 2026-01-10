```yaml
number: 17854
title: Add a note to diagnostics why the rule is enabled
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: micha/diagnostic-lint-source
created_at: 2025-05-05T12:01:36Z
updated_at: 2025-05-06T18:29:05Z
url: https://github.com/astral-sh/ruff/pull/17854
synced_at: 2026-01-10T18:57:03Z
```

# Add a note to diagnostics why the rule is enabled

---

_Pull request opened by @MichaReiser on 2025-05-05 12:01_

## Summary

This PR adds a note to every lint diagnostic that explains why the rule is enabled.

It would be nice if we had a `note` severity.

## Test Plan

See updated snapshots.


---

_Review requested from @carljm by @MichaReiser on 2025-05-05 12:01_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-05 12:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-05 12:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-05 12:01_

---

_Label `ty` added by @MichaReiser on 2025-05-05 12:01_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-05 12:01_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-05-05 12:01_

---

_Comment by @github-actions[bot] on 2025-05-05 12:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @dcreager removed by @MichaReiser on 2025-05-05 12:38_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-05 12:38_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-05 12:38_

---

_@AlexWaygood reviewed on 2025-05-05 13:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/context.rs`:323 on 2025-05-05 13:03_

some wording nits:

```suggestion
                LintSource::Default => format!("`{}` is enabled by default", diag.id()),
                LintSource::Cli => format!("`{}` was selected on the command line", diag.id()),
                LintSource::File => format!("`{}` was selected in a configuration file", diag.id()),
```

---

_@carljm approved on 2025-05-06 00:14_

Looks good. Not sure why primer check has debug output in it?

---

_Comment by @MichaReiser on 2025-05-06 07:22_

I'll give @BurntSushi some time to have a look at this too and I'll then update the message.

---

_@BurntSushi approved on 2025-05-06 11:47_

Makes sense to me!

(I somewhat agree about the `Note` severity.)

---

_Merged by @MichaReiser on 2025-05-06 18:29_

---

_Closed by @MichaReiser on 2025-05-06 18:29_

---

_Branch deleted on 2025-05-06 18:29_

---
