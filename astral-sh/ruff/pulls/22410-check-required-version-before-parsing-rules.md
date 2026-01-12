```yaml
number: 22410
title: Check required-version before parsing rules
type: pull_request
state: merged
author: jelle-openai
labels:
  - configuration
assignees: []
merged: true
base: main
head: fix-19922
created_at: 2026-01-06T00:15:57Z
updated_at: 2026-01-07T17:51:32Z
url: https://github.com/astral-sh/ruff/pull/22410
synced_at: 2026-01-12T15:57:49Z
```

# Check required-version before parsing rules

---

_@jelle-openai_

## Summary

Fixes #19922.

The approach I used is to check the required-version separately before parsing the TOML. This feels a bit ugly because we have to parse the TOML separately, but other approaches I considered seemed worse:

- Put the version validation inside SerDe deserialization for RequiredVersion. But this feels wrong: a required version that doesn't match the current version should still parse. Also, I think this means the user might see more errors than just the one for the version mismatch, when ideally they should only see the version mismatch error.
- Rewrite Options to be much more lenient in parsing everything else (in particular, don't validate rule names). However, this would be a bigger rewrite, and might still cause issues similar to #19922 if e.g. a new field is added.

## Test Plan

New test cases added.

---

_@MichaReiser reviewed on 2026-01-06 09:43_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:48 on 2026-01-06 09:43_

I think we can avoid the double parsing by doing something like this:

```rust
    let value: toml::Value =
        toml::from_str(&contents).with_context(|| format!("Failed to parse {}", path.display()))?;

    check_required_version(&value, path, &["tool", "ruff"])?;

    value
        .try_into()
        .with_context(|| format!("Failed to parse {}", path.display()))
```

---

_Label `configuration` added by @MichaReiser on 2026-01-06 09:46_

---

_Comment by @MichaReiser on 2026-01-06 09:46_

Nice

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 09:49_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @MichaReiser on 2026-01-06 09:51_

Hmm, the ecosystem changes are a bit surprising. Can you try rebasing on main?

---

_Comment by @jelle-openai on 2026-01-06 16:58_

Hm do those changes mean it found more autofixes? That would be weird.

I'll apply your suggested change and merge in with latest main.

---

_@jelle-openai reviewed on 2026-01-06 18:37_

---

_Review comment by @jelle-openai on `crates/ruff_workspace/src/pyproject.rs`:48 on 2026-01-06 18:37_

That version caused a regression where we no longer showed exact error locations for errors in the TOML. Codex came up with an approach that avoids the double parse while preserving exact error locations.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:138 on 2026-01-07 08:28_

What's the reason that we need to check for `required_version` in addition to `required-version`? Can we add a test demonstrating this behavior (assuming it is needed?)

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:62 on 2026-01-07 08:28_

Nice find!

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:149 on 2026-01-07 08:29_

I suspect that we lose the span in the error message compared to when deserializing the value as part of the serde deserialization. I'm inclined to return `Ok` here and defer to the "full" deserialization in that case.

---

_@MichaReiser approved on 2026-01-07 08:36_

Thanks. This is great and nice find with `DeTable`. 

I only have a question whether we need the `required_version` fallback and if we retain the spans in error messages if the required version fails to parse.

---

_@jelle-openai reviewed on 2026-01-07 16:32_

---

_Review comment by @jelle-openai on `crates/ruff_workspace/src/pyproject.rs`:62 on 2026-01-07 16:32_

Codex found it :D

---

_@jelle-openai reviewed on 2026-01-07 16:34_

---

_Review comment by @jelle-openai on `crates/ruff_workspace/src/pyproject.rs`:138 on 2026-01-07 16:34_

Yeah this was the AI's code and I should have validated it's needed. It looks like Ruff only supports the dash version, so we shouldn't look at the underscored one.

---

_@jelle-openai reviewed on 2026-01-07 16:34_

---

_Review comment by @jelle-openai on `crates/ruff_workspace/src/pyproject.rs`:149 on 2026-01-07 16:34_

I'll add a test for this case too.

---

_@jelle-openai reviewed on 2026-01-07 16:54_

---

_Review comment by @jelle-openai on `crates/ruff_workspace/src/pyproject.rs`:149 on 2026-01-07 16:54_

You were right, changed it as you suggested.

---

_@jelle-openai reviewed on 2026-01-07 16:56_

---

_Review comment by @jelle-openai on `crates/ruff_workspace/src/pyproject.rs`:138 on 2026-01-07 16:56_

Removed the underscore fallback

---

_@MichaReiser approved on 2026-01-07 17:01_

---

_Merged by @MichaReiser on 2026-01-07 17:29_

---

_Closed by @MichaReiser on 2026-01-07 17:29_

---

_Branch deleted on 2026-01-07 17:51_

---

_Comment by @jelle-openai on 2026-01-07 17:51_

Thanks for the review and merge @MichaReiser!

---
