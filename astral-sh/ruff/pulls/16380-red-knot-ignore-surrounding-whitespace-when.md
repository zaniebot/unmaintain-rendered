```yaml
number: 16380
title: "[red-knot] Ignore surrounding whitespace when looking for `<!-- snapshot-diagnostics -->` directives in mdtests"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-html-whitespace
created_at: 2025-02-25T18:50:19Z
updated_at: 2025-02-27T13:30:45Z
url: https://github.com/astral-sh/ruff/pull/16380
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Ignore surrounding whitespace when looking for `<!-- snapshot-diagnostics -->` directives in mdtests

---

_@AlexWaygood_

## Summary

Something unfortunate that happened while I was working on #16321 was that I initially added an HMTL comment like this:

```
<!--snapshot-diagnostics -->
```

...and I failed to notice for quite a while that there where no snapshots being tested for that specific test. (The directive was missing a whitespace between the `<!--` opener and the word "snapshot", meaning that mdtest wasn't recognising it as a directive.)

This PR changes the parsing of these directives so that whitespace surrounding the phrase "snapshot-diagnostics" is ignored inside HTML comments.

## Test Plan

I added a unit test to the `red_knot_test` crate. I also manually verified that if you apply this diff, running `cargo test -p red_knot_python_semantic` creates a new snapshot for you:

```diff
diff --git a/crates/red_knot_python_semantic/resources/mdtest/attributes.md b/crates/red_knot_python_semantic/resources/mdtest/attributes.md
index 6920bf18c..cd6171572 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/attributes.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/attributes.md
@@ -125,6 +125,8 @@ C.only_declared = "overwritten on class"
 
 #### Mixed declarations/bindings in class body and `__init__`
 
+<!--snapshot-diagnostics    -->
```


---

_Label `testing` added by @AlexWaygood on 2025-02-25 18:50_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-25 18:50_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-25 18:50_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-25 18:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-25 18:50_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-02-25 18:52_

---

_Comment by @github-actions[bot] on 2025-02-25 18:59_

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

_Review comment by @BurntSushi on `crates/red_knot_test/src/parser.rs`:430 on 2025-02-25 19:08_

Hmmm, should these have been removed? I think so, because now when this branch is taken, we either successfully parse an HTML comment (in which case, the previous state should probably be retained) or we report an error. It might be worth an added unit test for this? Although I wonder if it's actually impossible to observe. I'm thinking about something where you have

``````
`some/file/path.py`:

<!-- snapshot-diagnostics -->

```py
print("frob")
```
``````

---

_@BurntSushi approved on 2025-02-25 19:08_

Nice!

---

_@AlexWaygood reviewed on 2025-02-27 13:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:430 on 2025-02-27 13:21_

Hmm yeah, good question. I _think_ you're right, I think removing these is probably best?

I'm not entirely sure how to easily add a useful unit test for this (and doesn't seem worth spending a lot of time on), but happy to add one as a followup if you can think of something 😄

---

_Merged by @AlexWaygood on 2025-02-27 13:25_

---

_Closed by @AlexWaygood on 2025-02-27 13:25_

---

_Branch deleted on 2025-02-27 13:25_

---
