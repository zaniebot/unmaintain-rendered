```yaml
number: 20300
title: "Add fixes to `output-format=sarif`"
type: pull_request
state: merged
author: njhearp
labels:
  - cli
assignees: []
merged: true
base: main
head: sarif-fixes
created_at: 2025-09-08T03:29:21Z
updated_at: 2025-09-18T07:37:04Z
url: https://github.com/astral-sh/ruff/pull/20300
synced_at: 2026-01-10T17:40:28Z
```

# Add fixes to `output-format=sarif`

---

_Pull request opened by @njhearp on 2025-09-08 03:29_

## Summary

Addresses #19962

This PR extends `SarifEmitter` to include `fixes` in the SARIF ouput, if an auto fix is available. Before SARIF output did not include the `fixes` field. This makes Ruff's SARIF output more compliant with the spec.

Key changes:
- `SarifFix`, `SarifArtifactChange`, and related structs to represent fixes in SARIF.
- `fixes` is outputted only if an auto fix is available.

## Test Plan

Check for SARIF spec compliance and regressions.


---

_Comment by @github-actions[bot] on 2025-09-08 03:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `cli` added by @MichaReiser on 2025-09-16 07:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:222 on 2025-09-16 08:00_

You can use `serde(rename_all="camelCase")` to avoid having to rename every field. See https://serde.rs/container-attrs.html#rename_all

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:175 on 2025-09-16 08:01_

I'd prefer if the fields use the same names as in the Sarif specification. It makes it easier to match the fields to their output.

```suggestion
    #[serde(rename = "artifactLocation")]
    artifactLocation: SarifArtifactLocation,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:177 on 2025-09-16 08:05_

Unrelated to your PR: It would be great to include the link to `https://docs.oasis-open.org/sarif/sarif/v2.1.0/csprd01/sarif-v2.1.0-csprd01.html#_Toc10541319` somewhere in the documentation (on `SarifResult` or on `SarifEmitter`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:167 on 2025-09-16 08:05_


```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:195 on 2025-09-16 08:10_


```suggestion
    text: String,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:189 on 2025-09-16 08:13_

This should be an `Option` and be omitted when it is `None` (e.g. for a removal)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:219 on 2025-09-16 08:16_

Should this be an `Url`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:248 on 2025-09-16 08:18_

A fix is guaranteed (statically) to always have at least one edit
```suggestion
                
                artifact_changes.push(SarifArtifactChange {
                    uri: SarifArtifactLocation { uri: uri.clone() },
                    replacements,
                });
                
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:295 on 2025-09-16 08:19_

Let's extract this into a function to avoid repeating the same code twice

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:344 on 2025-09-16 08:20_

Hmm, interesting. Can't we derive `Serialize` on `SarifResult` instead and skip serializing `fixes` if the container is empty (https://serde.rs/field-attrs.html#skip_serializing_if)?

---

_@MichaReiser reviewed on 2025-09-16 08:21_

Thank you, this is great. I've a few suggestions on how to improve this

---

_@njhearp reviewed on 2025-09-16 18:22_

---

_Review comment by @njhearp on `crates/ruff_linter/src/message/sarif.rs`:219 on 2025-09-16 18:22_

I think the spec says `uri`. https://docs.oasis-open.org/sarif/sarif/v2.1.0/csprd01/sarif-v2.1.0-csprd01.html#_Toc10540865

---

_@njhearp reviewed on 2025-09-16 18:23_

---

_Review comment by @njhearp on `crates/ruff_linter/src/message/sarif.rs`:175 on 2025-09-16 18:23_

Makes sense. I've changed it.

---

_@njhearp reviewed on 2025-09-16 18:24_

---

_Review comment by @njhearp on `crates/ruff_linter/src/message/sarif.rs`:222 on 2025-09-16 18:24_

That's a great suggestion to make it more concise. I've changed this.

---

_@njhearp reviewed on 2025-09-16 18:28_

---

_Review comment by @njhearp on `crates/ruff_linter/src/message/sarif.rs`:189 on 2025-09-16 18:28_

Thanks. I've updated it.

---

_Review comment by @njhearp on `crates/ruff_linter/src/message/sarif.rs`:344 on 2025-09-17 03:17_

I had to do some additional changes and I've encountered a merge conflict from main. I'll work on fixing the merge conflict.

---

_@njhearp reviewed on 2025-09-17 03:17_

---

_@MichaReiser reviewed on 2025-09-17 07:41_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:259 on 2025-09-17 07:41_

I think there are two issues with how we map edits:

1. `inserted_content` should be `None` if `edit.content()` is `None`. 
2. We need to use the range from the edit for the deleted range and not the diagnostic range. You can get the range by calling `edit.range()`. The range is guaranteed to be empty if it is an insertion. It's non empty for replacements or deletions

---

_Review requested from @carljm by @njhearp on 2025-09-17 16:47_

---

_Review requested from @AlexWaygood by @njhearp on 2025-09-17 16:47_

---

_Review requested from @sharkdp by @njhearp on 2025-09-17 16:47_

---

_Review requested from @dcreager by @njhearp on 2025-09-17 16:47_

---

_Review requested from @dhruvmanila by @njhearp on 2025-09-17 16:47_

---

_Comment by @github-actions[bot] on 2025-09-17 16:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Converted to draft by @njhearp on 2025-09-17 16:51_

---

_Comment by @github-actions[bot] on 2025-09-17 16:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @dcreager removed by @MichaReiser on 2025-09-17 16:56_

---

_Review request for @carljm removed by @MichaReiser on 2025-09-17 16:56_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-09-17 16:56_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-09-17 16:56_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2025-09-17 16:56_

---

_Comment by @njhearp on 2025-09-17 16:58_

Sorry for the noisy update, I accidentally pushed extra commits when merging. I'm going to fix this to remove the noise.

---

_@njhearp reviewed on 2025-09-18 04:12_

---

_Review comment by @njhearp on `crates/ruff_linter/src/message/sarif.rs`:257 on 2025-09-18 04:12_

I thought I would add error handling if there is a problem with the source file.

---

_Marked ready for review by @njhearp on 2025-09-18 04:43_

---

_Comment by @MichaReiser on 2025-09-18 07:04_

> Sorry for the noisy update, I accidentally pushed extra commits when merging. I'm going to fix this to remove the noise.

Don't worry. It happened to all of us :)

---

_@MichaReiser reviewed on 2025-09-18 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:257 on 2025-09-18 07:18_

That makes sense. I changed the error handling to omit the fix when the source file is missing. Emitting a fix with made-up locations seems problematic, as it most likely will result in invalid code when applied.

---

_@MichaReiser approved on 2025-09-18 07:18_

---

_Renamed from "Extended `SarifEmitter` to include `fixes` in the SARIF ouput" to "Include fixes in `output-format=sarif`" by @MichaReiser on 2025-09-18 07:24_

---

_Renamed from "Include fixes in `output-format=sarif`" to "Add fixes in `output-format=sarif`" by @MichaReiser on 2025-09-18 07:24_

---

_Renamed from "Add fixes in `output-format=sarif`" to "Add fixes to `output-format=sarif`" by @MichaReiser on 2025-09-18 07:24_

---

_Merged by @MichaReiser on 2025-09-18 07:37_

---

_Closed by @MichaReiser on 2025-09-18 07:37_

---
