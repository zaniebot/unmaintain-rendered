```yaml
number: 18609
title: "Move corpus tests to `ty_python_semantic`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/corpus-test-ty-semantic
created_at: 2025-06-10T11:51:15Z
updated_at: 2025-06-11T06:55:32Z
url: https://github.com/astral-sh/ruff/pull/18609
synced_at: 2026-01-10T18:45:04Z
```

# Move corpus tests to `ty_python_semantic`

---

_Pull request opened by @MichaReiser on 2025-06-10 11:51_

This is slightly more annoying than I thought. The main reason the tests were in `ty_project` is that we have access to the `ProjectDatabase`. We don't have any such `Db` struct available in `ty_python_semantic` other than `TestDb` which is gated behind `cfg(test)` and integration tests can't enable individual features. 

We have a few options here:

* Make the corpus test a normal unit test: This feels off, especially considering how long they take to run
* Require a feature for the corpus tests to run: This has the downside that they'll only run when using `--all-features`. 
* Add a new `CorpusDb` similar to what we do in `ty_test`
* Add a `SemanticDb` struct in `ty_python_semantic` and expose it always

I opted for another `CorpusDb`. This is a bit annoying when adding new methods to the `Db` trait but we do this only very rarely. 



---

_Label `testing` added by @MichaReiser on 2025-06-10 11:51_

---

_Label `ty` added by @MichaReiser on 2025-06-10 11:51_

---

_Comment by @github-actions[bot] on 2025-06-10 11:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-06-10 11:57_

---

_Review requested from @carljm by @MichaReiser on 2025-06-10 11:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-10 11:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-10 11:57_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-10 11:57_

---

_Comment by @github-actions[bot] on 2025-06-10 12:05_

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

_Comment by @AlexWaygood on 2025-06-10 12:08_

Could you also move [the corpus itself](https://github.com/astral-sh/ruff/tree/main/crates/ty_project/resources/test/corpus) from `ty_project/resources/` to `ty_python_semantic/resources/`? I think it makes sense for the corpus to be in the same crate as the corpus test

---

_@carljm approved on 2025-06-10 20:17_

Looks good to me, thank you! I agree with @AlexWaygood that preferably the actual corpus would move as well.

---

_Merged by @MichaReiser on 2025-06-11 06:55_

---

_Closed by @MichaReiser on 2025-06-11 06:55_

---

_Branch deleted on 2025-06-11 06:55_

---
