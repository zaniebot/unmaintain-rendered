```yaml
number: 17021
title: Add free-threaded selector hint for requires-python
type: pull_request
state: open
author: kxzk
labels: []
assignees: []
base: main
head: freethreaded-selector-hint
created_at: 2025-12-08T04:11:59Z
updated_at: 2025-12-15T02:31:17Z
url: https://github.com/astral-sh/uv/pull/17021
synced_at: 2026-01-12T16:12:34Z
```

# Add free-threaded selector hint for requires-python

---

_@kxzk_

## Summary

When users specify a free-threaded Python version (e.g., `>=3.14t`) in `requires-python`, the parse error doesn't explain why the `t` suffix is invalid or what to do instead. This adds a hint to guide users toward the correct approach.

**Before:**
```
error: Failed to parse: pyproject.toml
  Caused by: TOML parse error at line 4, column 19
  |
4 | requires-python = ">=3.14t"
  |                   ^^^^^^^^^
Failed to parse version: after parsing 3.14, found t, which is not part of a valid version
```

**After:**
```
error: Failed to parse: pyproject.toml
  Caused by: TOML parse error at line 4, column 19
  |
4 | requires-python = ">=3.14t"
  |                   ^^^^^^^^^
Failed to parse version: after parsing 3.14, found t, which is not part of a valid version

hint: requires-python cannot include a free-threaded selector; use uv python pin 3.14t instead
```

Closes #16963

## Test Plan

- Created a `pyproject.toml` with `requires-python = ">=3.14t"` and verified the hint appears
- Existing tests pass

---

_Review comment by @kxzk on `crates/uv-workspace/src/pyproject.rs`:247 on 2025-12-08 04:12_

Probably too naive. But where can I find a full list of free-threaded Python version variants so I know the correct logic to handle all cases?

---

_@kxzk reviewed on 2025-12-08 04:12_

---

_Marked ready for review by @kxzk on 2025-12-08 04:57_

---

_Review requested from @Copilot by @kxzk on 2025-12-08 04:57_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-12-08 05:00_

## Pull request overview

This PR adds a helpful hint when users attempt to use free-threaded Python version selectors (e.g., `>=3.14t`) in `requires-python` fields. Since free-threaded selectors are not valid in PEP 440 version specifiers, the hint guides users to use `uv python pin` instead.

- Introduces `RequiresPythonSpecifiers` wrapper type around `VersionSpecifiers` with custom deserialization
- Detects the specific pattern of free-threaded selector errors (`t` suffix after a digit)
- Updates `Project` and `DependencyGroupSettings` to use the new wrapper type

### Reviewed changes

Copilot reviewed 4 out of 4 changed files in this pull request and generated 1 comment.

| File | Description |
| ---- | ----------- |
| `crates/uv-workspace/src/pyproject.rs` | Adds `RequiresPythonSpecifiers` wrapper type with custom deserializer, updates `Project` and `DependencyGroupSettings` to use it, includes comprehensive tests |
| `crates/uv-workspace/src/workspace.rs` | Calls `.into_inner()` to extract `VersionSpecifiers` from `RequiresPythonSpecifiers` when needed, adds explanatory comments |
| `crates/uv-workspace/src/dependency_groups.rs` | Adds comment explaining why `FlatDependencyGroup` uses `VersionSpecifiers` directly |
| `uv.schema.json` | Adds `RequiresPythonSpecifiers` schema definition and references it in `DependencyGroupSettings` |





---

💡 <a href="/astral-sh/uv/new/main/.github/instructions?filename=*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

_Review comment by @Copilot on `uv.schema.json`:1856 on 2025-12-08 05:00_

The `RequiresPythonSpecifiers` definition is missing a description. For consistency with other schema definitions like `RequiredVersion` and `Requirement`, consider adding a description explaining what this type represents.

Suggested addition:
```json
"RequiresPythonSpecifiers": {
  "description": "A PEP 440 version specifier for Python compatibility, e.g., `>=3.8` or `>=3.8,<3.13`.",
  "type": "string"
}
```
```suggestion
    "RequiresPythonSpecifiers": {
      "description": "A PEP 440 version specifier for Python compatibility, e.g., `>=3.8` or `>=3.8,<3.13`.",
```

---

_Comment by @kxzk on 2025-12-08 19:23_

CI failures are transient - `python_install_emulated_windows_x86_on_x64` timed out at harness level despite completing successfully, and DNF hit a mirror outage (`No URLs in mirrorlist`). Unrelated to this PR.

---

_@zanieb reviewed on 2025-12-09 21:03_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:247 on 2025-12-09 21:03_

I think I'd expect to use `uv_python::VersionRequest::from_str` and check `request.variant().is_freethreaded()`?

---

_@zanieb reviewed on 2025-12-09 21:05_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:2000 on 2025-12-09 21:05_

I think I'd probably do an inline snapshot of the error instead of asserting

---

_@zanieb reviewed on 2025-12-09 21:06_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:608 on 2025-12-09 21:06_

I don't think we need to include this comment

---

_@zanieb reviewed on 2025-12-09 21:07_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:771 on 2025-12-09 21:07_

Was this important?

---

_@zanieb reviewed on 2025-12-09 21:07_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:26 on 2025-12-09 21:07_

I think it'd probably be okay to omit this comment, though I don't feel strongly

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:203 on 2025-12-09 21:08_

```suggestion
/// A wrapper specializing [`VersionSpecifiers`] for `requires-python` fields.
```

---

_@zanieb reviewed on 2025-12-09 21:08_

---

_@zanieb reviewed on 2025-12-09 21:08_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:247 on 2025-12-09 21:08_

(or pull out the variant parsing from there so we can re-use it?)

---

_@kxzk reviewed on 2025-12-15 02:22_

---

_Review comment by @kxzk on `crates/uv-workspace/src/pyproject.rs`:247 on 2025-12-15 02:22_

First suggestion makes sense to me if you're okay adding `uv-python` as a dep to `uv-workspace`. For the second `uv-pep440` seems plausible (both crates already depend on it). To keep a single source of truth, `uv-python` would also need to use it, which expands scope. Happy to go either route - what's your preference?

---

_@kxzk reviewed on 2025-12-15 02:22_

---

_Review comment by @kxzk on `crates/uv-workspace/src/pyproject.rs`:2000 on 2025-12-15 02:22_

Got it, makes sense.

---

_@kxzk reviewed on 2025-12-15 02:31_

---

_Review comment by @kxzk on `crates/uv-workspace/src/pyproject.rs`:771 on 2025-12-15 02:31_

It was, but not anymore - `VersionSpecifiers` doesn't implement `JsonSchema`, so the attribute was required. The new `RequiresPythonSpecifiers` wrapper implements it directly, making the override redundant. Obviously, this is subject to change based on the final impl.

---
