```yaml
number: 18079
title: "Use static diagnostic names and cache `Rule`s"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: brent/inline-diagnostic-kind
head: brent/static-diagnostic-name
created_at: 2025-05-13T21:05:10Z
updated_at: 2025-05-14T20:26:30Z
url: https://github.com/astral-sh/ruff/pull/18079
synced_at: 2026-01-10T18:51:01Z
```

# Use static diagnostic names and cache `Rule`s

---

_Pull request opened by @ntBre on 2025-05-13 21:05_

This PR is stacked on https://github.com/astral-sh/ruff/pull/18074 and uses the
separation between `CacheMessage` and the other diagnostic types added there to
store a `Rule` directly on `CacheMessage` and convert it back to a `&'static
str` for use by `Diagnostic` and `DiagnosticMessage`.

In the first commit, I added a generated `Rule::pascal_name` method for converting
back to the Pascal-case name currently used by `Diagnostic` and `DiagnosticMessage`,
but in the second commit I used the `heck` crate to generalize our existing `kebab_case`
macro to convert the `ViolationMetadata::rule_name` output to kebab-case and use
kebab-case everywhere. `heck` was already in our dependency graph because it's what
`strum` uses internally for case conversions.

I think that's probably preferable long-term, but it might be a breaking change in
the LSP because the lint name is used as the title for a quick fix, if a `suggestion` is
unavailable. Otherwise `Diagnostic::name` is only used for `debug!` logging.

Test Plan
--

Existing tests

---

_Label `diagnostics` added by @ntBre on 2025-05-13 21:05_

---

_Comment by @github-actions[bot] on 2025-05-13 21:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>




---

_Marked ready for review by @ntBre on 2025-05-13 21:26_

---

_Review requested from @MichaReiser by @ntBre on 2025-05-13 21:26_

---

_Review comment by @MichaReiser on `crates/ruff_macros/Cargo.toml`:20 on 2025-05-14 06:10_

It's not clear to me why we need to introduce the `heck` dependency here, considering that we already had an implementation that does the kebab case conversion

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/map_codes.rs`:425 on 2025-05-14 06:11_

More for your other PR, but this still mentions `DiagnosticKind`

---

_@MichaReiser approved on 2025-05-14 06:18_

Nice speed up. 

This is great. I would probably remove the heck dependency again, given that we already had an implementation of kebab_case (also reduces the size of your PR).

My only concern is that name is also used in the GitLab reporter to compute a fingerprint

https://github.com/astral-sh/ruff/blob/981bd70d393fe29324c07c3ffce5ecddd40e6bd6/crates/ruff_linter/src/message/gitlab.rs#L126-L134

This means, all diagnostics will be changed for gitlab users. 

I don't think I'm too concerned by this because the implementation already uses `std::hash::DefaultHasher` which isn't guaranteed to be stable across rustc versions (or even platforms! we should probably fix this). 

---

_Label `internal` added by @MichaReiser on 2025-05-14 06:19_

---

_@ntBre reviewed on 2025-05-14 12:41_

---

_Review comment by @ntBre on `crates/ruff_macros/Cargo.toml`:20 on 2025-05-14 12:41_

It looked like our old implementation of `kebab_case` could only handle `SCREAMING_SNAKE_CASE` names, but for this purpose I also needed it to  handle `PascalCase` names. That seemed like a harder case to roll our own version of, but admittedly I didn't try.

Usually I wouldn't reach for a dependency so lightly,  but it's already pulled in for strum, clap, salsa, and is-macro and doesn't have any additional dependencies of its own.

---

_@ntBre reviewed on 2025-05-14 12:46_

---

_Review comment by @ntBre on `crates/ruff_macros/src/map_codes.rs`:425 on 2025-05-14 12:46_

Good catch, thank you! I'll resolve this in the other one.

---

_Comment by @ntBre on 2025-05-14 12:56_

Oh interesting about the gitlab diagnostic. As soon as I saw it was being hashed, I assumed it was okay. From the gitlab [docs](https://docs.gitlab.com/ci/testing/code_quality/#merge-request-widget), it sounds like they shouldn't be too user-facing and only used to deduplicate entries in the merge request widget, so hopefully it's okay.

---

_Comment by @ntBre on 2025-05-14 13:18_

Hm, it does look like we marked a previous change to the hash as breaking: https://github.com/astral-sh/ruff/issues/7159. One other benefit of keeping `heck` is that we could potentially go the other way back to pascal-case and (temporarily) use that for the gitlab emitter, if we want to avoid a breaking change.

---

_Merged by @ntBre on 2025-05-14 20:26_

---

_Closed by @ntBre on 2025-05-14 20:26_

---

_Branch deleted on 2025-05-14 20:26_

---
