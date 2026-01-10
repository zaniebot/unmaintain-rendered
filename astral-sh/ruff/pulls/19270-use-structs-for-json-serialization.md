```yaml
number: 19270
title: Use structs for JSON serialization
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/json-types
created_at: 2025-07-10T22:47:58Z
updated_at: 2025-07-11T13:37:46Z
url: https://github.com/astral-sh/ruff/pull/19270
synced_at: 2026-01-10T18:33:12Z
```

# Use structs for JSON serialization

---

_Pull request opened by @ntBre on 2025-07-10 22:47_

## Summary

See https://github.com/astral-sh/ruff/pull/19133#discussion_r2198413586 for recent discussion. This PR moves to using structs for the types in our JSON output format instead of the `json!` macro.

I didn't rename any of the `message` references because that should be handled when rebasing #19133 onto this.

My plan for handling the `preview` behavior with the new diagnostics is to use a wrapper enum. Something like:

```rust
#[derive(Serialize)]
#[serde(untagged)]
pub(crate) enum JsonDiagnostic<'a> {
    Old(OldJsonDiagnostic<'a>),
}

#[derive(Serialize)]
pub(crate) struct OldJsonDiagnostic<'a> {
    // ...
}
```

Initially I thought I could use a `&dyn Serialize` for the affected fields, but I see that `Serialize` isn't dyn-compatible in testing this now.

## Test Plan

Existing tests. One quirk of the new types is that their fields are in alphabetical order. I guess `json!` sorts the fields alphabetically? The tests were failing before I sorted the struct fields.

## Other formats

It looks like the `rdjson`, `sarif`, and `gitlab` formats also use `json!`, so if we decide to merge this, I can do something similar for those before moving them to the new diagnostic format.


---

_Comment by @github-actions[bot] on 2025-07-10 22:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-07-10 22:59_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-10 22:59_

---

_Label `internal` added by @ntBre on 2025-07-10 23:00_

---

_Label `diagnostics` added by @ntBre on 2025-07-10 23:00_

---

_@MichaReiser reviewed on 2025-07-11 07:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/json.rs`:111 on 2025-07-11 07:18_

I'm leaning towards reverting the last commit. The old implementation carefully avoided allocating a new vector for the edits. I also don't think that changing this is necessary for making this more typed. We can just replace the 

```rust
           let value = json!({
                "content": edit.content().unwrap_or_default(),
                "location": location_to_json(location),
                "end_location": location_to_json(end_location)
            });
```

and initialize and then serialize said struct instead.

---

_Review requested from @carljm by @ntBre on 2025-07-11 12:54_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-11 12:54_

---

_Review requested from @sharkdp by @ntBre on 2025-07-11 12:54_

---

_Review requested from @dcreager by @ntBre on 2025-07-11 12:54_

---

_Review request for @dcreager removed by @ntBre on 2025-07-11 12:54_

---

_Review request for @carljm removed by @ntBre on 2025-07-11 12:54_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-11 12:54_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-11 12:54_

---

_@ntBre reviewed on 2025-07-11 12:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/json.rs`:111 on 2025-07-11 12:55_

Makes sense, reverted!

---

_@MichaReiser approved on 2025-07-11 12:58_

---

_Merged by @ntBre on 2025-07-11 13:37_

---

_Closed by @ntBre on 2025-07-11 13:37_

---

_Branch deleted on 2025-07-11 13:37_

---
