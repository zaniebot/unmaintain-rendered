```yaml
number: 15480
title: "[`unconventional-import-alias`] Fix infinite loop between ICN001 and I002 (`ICN001`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/icn001-conflict
created_at: 2025-01-14T20:54:12Z
updated_at: 2025-01-16T15:45:27Z
url: https://github.com/astral-sh/ruff/pull/15480
synced_at: 2026-01-10T20:34:00Z
```

# [`unconventional-import-alias`] Fix infinite loop between ICN001 and I002 (`ICN001`)

---

_Pull request opened by @ntBre on 2025-01-14 20:54_

## Summary

This fixes the infinite loop reported in #14389 by raising an error to the user about conflicting ICN001 (`unconventional-import-alias`) and I002 (`missing-required-import`) configuration options.

## Test Plan

Added a CLI integration test reproducing the old behavior and then confirming the fix.

Closes #14389 

---

_Comment by @github-actions[bot] on 2025-01-14 21:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-01-14 21:06_

Can you tell me about what you tried to do in the configuration and why it didn't work?

---

_Comment by @ntBre on 2025-01-14 21:19_

Well, I didn't really try anything exactly. I looked at #14477 that @AlexWaygood sent me as a similar fix, and then I spent some time looking through how configurations are constructed. But #14477 looked like validation on a single configuration field that could be handled during deserialization, while this issue looks to me like it requires validation between two configuration fields (`LinterSettings.isort.required_imports` and `LinterSettings.flake8_import_conventions.aliases`), so I didn't think I could handle it in the same way.

Then I found `LintConfiguration::as_rule_table`, which looked like a possible place to do this kind of validation. I only noticed warnings being emitted earlier, but now I see that it returns a `Result`. Could I do the validation there and return an `Err` on this conflict? Or if you have another suggestion I'd be happy to hear it. I should have asked before the PR, but this approach was mentioned in the issue, and it seemed reasonable to me too.

---

_Label `bug` added by @dhruvmanila on 2025-01-15 05:14_

---

_Comment by @MichaReiser on 2025-01-15 08:19_

There are a few places where we verify configurations but they all have their trade-offs:

* Inside `Configuration::from_options`: The main advantage validating options in `from_options` is that Ruff knows the corresponding configuration file, allowing it to point users directly to which file contains the incorrect configuration. However, `from_options` is called before combining different configuration file, meaning, it's an incomplete view. 
* Inside `Configuration::into_settings`: `into_settings` is called after all configuration files were combined. It's where Ruff has a complete view of all settings, but it no longer knows the source of individual setting values.

In your case, `Configuration::into_settings` is probably the better fit because you need the fully resolved configuration. 

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:1581 on 2025-01-15 19:13_

Can we add an advice on how to resolve the error

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:1580 on 2025-01-15 19:13_

Should we use the actual setting names here?

---

_@MichaReiser approved on 2025-01-15 19:14_

Lgtm

Would you mind updating the PR summary to reflect that this is now implemented in the settings 

---

_Comment by @ntBre on 2025-01-16 13:48_

I updated the PR summary and expanded the error message. Is the new message along the lines of what you had in mind?

---

_Comment by @MichaReiser on 2025-01-16 13:53_

Looks good, thanks

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/configuration.rs`:1583 on 2025-01-16 13:59_

```suggestion
            `lint.flake8-import-conventions.extend-aliases` (ICN001):\
```

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/configuration.rs`:1574 on 2025-01-16 14:01_

Maybe add backticks? Not sure though

```suggestion
            writeln!(err_body, "    - `{name}` -> `{alias}`").unwrap();
```

---

_@AlexWaygood approved on 2025-01-16 14:02_

Nice!

---

_@ntBre reviewed on 2025-01-16 14:34_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/configuration.rs`:1583 on 2025-01-16 14:34_

Good catches again, thank you!

---

_Merged by @ntBre on 2025-01-16 15:45_

---

_Closed by @ntBre on 2025-01-16 15:45_

---

_Branch deleted on 2025-01-16 15:45_

---
