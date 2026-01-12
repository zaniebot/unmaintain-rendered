```yaml
number: 10984
title: "`ruff server`: Introduce settings for directly configuring the linter and formatter"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/editor-settings
created_at: 2024-04-16T21:12:28Z
updated_at: 2024-04-20T15:23:31Z
url: https://github.com/astral-sh/ruff/pull/10984
synced_at: 2026-01-12T15:55:34Z
```

# `ruff server`: Introduce settings for directly configuring the linter and formatter

---

_@snowsignal_

## Summary

The following client settings have been introduced to the language server:
* `lint.preview`
* `format.preview`
* `lint.select`
* `lint.extendSelect`
* `lint.ignore`
* `exclude`
* `lineLength`

`exclude` and `lineLength` apply to both the linter and formatter.

This does not actually use the settings yet, but makes them available for future use.

## Test Plan

Snapshot tests have been updated.

---

_Label `server` added by @snowsignal on 2024-04-16 21:12_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-16 21:12_

---

_Marked ready for review by @snowsignal on 2024-04-16 21:37_

---

_Comment by @github-actions[bot] on 2024-04-16 21:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @snowsignal on `crates/ruff_linter/src/line_width.rs`:40 on 2024-04-16 22:22_

I'd like to _not_ do this if possible, but I couldn't find a better way to initialize `LineLength`. Maybe I should use `serde_json::from_value(serde_json::Value::Number(80))` instead?

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:272 on 2024-04-16 22:25_

I should probably explain why I'm not just removing `ResolvedClientSettings::resolve_or` entirely and using `resolve_optional(...).unwrap_or(<default>)` in its place. My reasoning is that I don't want to move the logic that determines default values out of a dedicated function, because it opens up the possibility that someone might use `resolve_optional(...).unwrap_or_default()` or some other incorrect method of resolving a default value. We should always make the default value explicit when resolving a setting.

---

_@snowsignal reviewed on 2024-04-16 22:25_

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-16 22:30_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-16 22:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/line_width.rs`:40 on 2024-04-17 06:53_

Why don't you use `LineLength::try_from(length).unwrap()` directly in your tests? It gives you the same assertions and isn't longer than `LineLength::from_u16_for_testing_only()`. 

I agree that we shouldn't add this. We can have a similar helper function in the tests that need. 

Unrelated: Two changes i would make if we wanted to have the function. 

* Use `Self::MAX` instead of 320`
* Use `#[test]` to exclude the method from production code.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:50 on 2024-04-17 06:57_

We don't need to do this in this PR (or at all). 

But the naming we use in Ruff for all the different configuration related structures is:

* `*Options`: The structs into which serde deserializes the TOML. E.g `FormatOptions`
* `*Configuration`: A deserialization format independent representation of the configuration from a single file. Allows combining different configurations
* `*Settings`: The settings struct used internally by Ruff that only represents the information relevant at runtime. For example, it no longer has `select` and `ignore`. It only tracks the enabled rules. 


I think it would be good to use the same terminology in the LSP. That means, the structs here would be renamed to `Options`. 

---

_@MichaReiser approved on 2024-04-17 07:00_

---

_@MichaReiser reviewed on 2024-04-17 07:01_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:651 on 2024-04-17 07:01_

Yeah, like the function is used exactly once. This doesn't warrent adding the method. I recommend using `LineLength::try_from(80).unwrap()`

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:272 on 2024-04-17 13:00_

Maybe the documentation of `resolve_or` could point to `resolve_optional` and similarly you could add `resolve_or` in `resolve_optional`'s documentation. The wording would be:
```rs
// Use [`ResolvedClientSettings::resolve_optional`] for ...
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:38 on 2024-04-17 13:02_

Why use `Option<bool>`? Can we define a default value instead? It would probably be `false` for both `lint_preview` and `format_preview`.

Same question for other settings where it makes sense. I guess I'm missing some context here.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:50 on 2024-04-17 13:04_

I think we should document this difference in possibly a `CONTRIBUTING.md` of `ruff_workspace` crate.

---

_@dhruvmanila approved on 2024-04-17 13:08_

---

_@snowsignal reviewed on 2024-04-17 16:15_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/line_width.rs`:40 on 2024-04-17 16:15_

Somehow I did not see the `TryFrom` implementation 🤦‍♀️ 

 That's a lot easier, thanks!

---

_@snowsignal reviewed on 2024-04-18 06:06_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:38 on 2024-04-18 06:06_

The reason why these are optional is because we'll eventually merge them with the project configuration. If we set them to a default value here, we wouldn't have a way of knowing whether that was an actually defined value or just the default value resulting from it being un-set on the client side. I'll clarify this in a comment.

---

_Merged by @snowsignal on 2024-04-18 07:53_

---

_Closed by @snowsignal on 2024-04-18 07:53_

---

_Branch deleted on 2024-04-18 07:53_

---

_Comment by @charliermarsh on 2024-04-20 03:14_

Will `--config` come later, or is it already supported? That was the last piece of the proposal, IIRC -- but may be misremembering.

---

_Comment by @snowsignal on 2024-04-20 15:23_

@charliermarsh That will come later! I'll be working on that next week.

---
