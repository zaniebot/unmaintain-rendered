```yaml
number: 14477
title: "[`flake8-import-conventions`] Syntax check aliases supplied in configuration for `unconventional-import-alias (ICN001)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - configuration
assignees: []
merged: true
base: main
head: unconventional-alias
created_at: 2024-11-20T05:01:51Z
updated_at: 2024-11-21T15:59:02Z
url: https://github.com/astral-sh/ruff/pull/14477
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-import-conventions`] Syntax check aliases supplied in configuration for `unconventional-import-alias (ICN001)`

---

_Pull request opened by @dylwil3 on 2024-11-20 05:01_

This PR introduces a validation step to the deserialization of the configuration for [unconventional-import-alias (ICN001)](https://docs.astral.sh/ruff/rules/unconventional-import-alias/#unconventional-import-alias-icn001). We verify that the import module and alias supplied have valid syntax in order to avoid a panic if these aliases are added in during a fix.

Closes #14439


---

_Comment by @dylwil3 on 2024-11-20 05:04_

Two notes:

- Sorry for bloating the workspace crate with serde and especially with serde_json (since I only use the latter for a test)... let me know if you'd like me to work around that
- I don't add a deserialization check for "banned aliases" or "banned from" since a fix would never _add_ those to the source code

---

_Comment by @github-actions[bot] on 2024-11-20 05:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `configuration` added by @MichaReiser on 2024-11-20 07:32_

---

_@MichaReiser reviewed on 2024-11-20 07:33_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/Cargo.toml`:46 on 2024-11-20 07:33_

This should be a dev-dependency if it is only used in tests

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:3449 on 2024-11-20 07:35_

I'm leaning towards making this a ruff cli integration test. It then also demonstrates how the error message is displayed to the users. 

See https://github.com/astral-sh/ruff/blob/c847cad389f202edae868765e690cb7042e88264/crates/ruff/tests/lint.rs#L23

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1394 on 2024-11-20 07:36_

I suggest that we use two dedicated errors: One for `alias` and one for `module`. 

---

_Label `configuration` removed by @MichaReiser on 2024-11-20 07:37_

---

_Label `bug` added by @MichaReiser on 2024-11-20 07:37_

---

_@MichaReiser reviewed on 2024-11-20 07:37_

---

_Comment by @AlexWaygood on 2024-11-20 08:48_

Let's try to get this into ruff 0.8 if we can — while it _is_ a bugfix, it might still be breaking for some users who are currently "using the rule incorrectly"

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-20 08:48_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-20 08:49_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-11-20 08:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-20 08:49_

---

_Review request for @carljm removed by @AlexWaygood on 2024-11-20 08:49_

---

_Review request for @sharkdp removed by @AlexWaygood on 2024-11-20 08:49_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-11-20 08:49_

---

_Comment by @AlexWaygood on 2024-11-20 08:52_

Sorry — I optimistically tried to change the target branch, but it messed up the diff, so I swiftly reverted that back 😅

It's probably fine if this goes straight into `main`, though, as long as we merge it before the 0.8 release goes out. I highly doubt we'll be cutting another patch release before 0.8, since everything is roughly on schedule for the minor release

---

_Label `breaking` added by @AlexWaygood on 2024-11-20 09:48_

---

_Comment by @MichaReiser on 2024-11-20 12:08_

I also don't consider this breaking. It doesn't change the intent of the rule. 

---

_Comment by @AlexWaygood on 2024-11-20 12:12_

> I also don't consider this breaking. It doesn't change the intent of the rule.

I agree that it doesn't change the intent of the rule, but clearly _some_ users are not using the rule _as it was intended_: https://github.com/astral-sh/ruff/issues/14439#issuecomment-2485302976. We will break those users; this is a case of [Hyrum's law](https://www.hyrumslaw.com/).

I strongly agree with the change, I'd just prefer to have it go into Ruff 0.8 and to list it in `BREAKING_CHANGES.md`, out of an abundance of caution.

---

_@dylwil3 reviewed on 2024-11-20 19:18_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/options.rs`:3449 on 2024-11-20 19:18_

Done!

---

_@dylwil3 reviewed on 2024-11-20 19:18_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/options.rs`:1394 on 2024-11-20 19:18_

Done!

---

_@dylwil3 reviewed on 2024-11-20 19:19_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/Cargo.toml`:46 on 2024-11-20 19:19_

No longer used (test moved to integration test)

---

_Comment by @dylwil3 on 2024-11-20 19:20_

> I agree that it doesn't change the intent of the rule, but clearly _some_ users are not using the rule _as it was intended_: [#14439 (comment)](https://github.com/astral-sh/ruff/issues/14439#issuecomment-2485302976). We will break those users; this is a case of [Hyrum's law](https://www.hyrumslaw.com/).

[Relevant xkcd](https://xkcd.com/1172/)



---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:1999 on 2024-11-20 20:18_

Hmm. The errors aren't great. Let's at least include the alias in the error message to help users figure out what's going on. The same for the module name.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1333 on 2024-11-20 20:20_

We would probably get better error messages if you create a newtype for `ModuleName` and `Alias` and implement `Deserialize` for them. I would expect that serde can then narrow the error message to the specific line rather than the entire table

---

_@MichaReiser requested changes on 2024-11-20 20:22_

Thanks for adding the integration tests. They show that the error messages aren't great. We should either:

* minimal: Include the alias and module name in the error message
* better: Use newtype wrappers for `Module` and `Alias` (`FxHashMap<ModuleName, Alias>`) and implement `Deserialize` on the newtype wrappers. That should allow serde to highlight the relevant line and column in the error message.

---

_Removed from milestone `v0.8` by @AlexWaygood on 2024-11-21 15:17_

---

_@AlexWaygood reviewed on 2024-11-21 15:34_

---

_Review comment by @AlexWaygood on `crates/ruff_workspace/src/options.rs`:1333 on 2024-11-21 15:34_

This PR does actually already seem to be a small improvement on the status quo when it comes to line numbers. Currently if I make this change to typeshed's pyproject.toml file:

```diff
diff --git a/pyproject.toml b/pyproject.toml
index 46014bcd2..927da6162 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -111,6 +111,9 @@ ignore = [
     "E501", # Line too long
 ]
 
+[tool.ruff.lint.flake8-import-conventions.aliases]
+"pandas" = 42
+
```

Then Ruff v0.7.1 gives me this error message:

```
ruff failed
  Cause: Failed to parse /Users/alexw/dev/typeshed/pyproject.toml
  Cause: TOML parse error at line 27, column 1
   |
27 | [tool.ruff.lint]
   | ^^^^^^^^^^^^^^^^
invalid type: integer `42`, expected a string
```

---

_Comment by @MichaReiser on 2024-11-21 15:40_

I pushed a change that implements `Deserialize` for the key and value. I'm surprised that serde isn't better at handling this.

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-21 15:40_

---

_@MichaReiser approved on 2024-11-21 15:41_

@AlexWaygood should we merge?

---

_@AlexWaygood approved on 2024-11-21 15:49_

Thanks @dylwil3!

---

_Label `bug` removed by @MichaReiser on 2024-11-21 15:50_

---

_Label `configuration` added by @MichaReiser on 2024-11-21 15:50_

---

_Comment by @dylwil3 on 2024-11-21 15:51_

@MichaReiser you beat me to it! Much improved error message, thanks!

---

_Label `breaking` removed by @MichaReiser on 2024-11-21 15:51_

---

_Merged by @MichaReiser on 2024-11-21 15:54_

---

_Closed by @MichaReiser on 2024-11-21 15:54_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-21 15:55_

---
