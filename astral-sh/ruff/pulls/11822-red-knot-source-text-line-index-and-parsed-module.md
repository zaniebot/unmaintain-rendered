```yaml
number: 11822
title: "red-knot: `source_text`, `line_index`, and `parsed_module` queries"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-source
created_at: 2024-06-10T11:19:13Z
updated_at: 2024-06-13T07:52:54Z
url: https://github.com/astral-sh/ruff/pull/11822
synced_at: 2026-01-12T15:55:39Z
```

# red-knot: `source_text`, `line_index`, and `parsed_module` queries

---

_@MichaReiser_

## Summary

This PR adds the most basic file-based queries that are needed for semantic analysis, linting and formatting:

* `source_text`: Reads the content of a file. Changing a file's revision markes the query result as stale. I excluded jupyter notebook support for now. 
* `line_index`: Computes the line index for a file's source text. 
* `parsed_module`: Parses the file's text to a python module, infering the file type from the file extension. 

## Test Plan

I added a few unit tests that demonstrate the basic functionality. 


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-10 11:19_

---

_Label `red-knot` added by @MichaReiser on 2024-06-10 11:19_

---

_Renamed from "red-knot: `source_text`, `line_index`, and `parsed_module` queries" to "red-knot[salsa part 3]: `source_text`, `line_index`, and `parsed_module` queries" by @MichaReiser on 2024-06-11 13:56_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-06-11 13:57_

---

_Review requested from @carljm by @MichaReiser on 2024-06-11 13:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-11 13:57_

---

_Comment by @github-actions[bot] on 2024-06-11 14:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
Failed to clone mlflow/mlflow: error: RPC failed; curl 55 Failed sending HTTP2 data
error: 27308 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @carljm on `crates/ruff_db/src/parsed.rs`:16 on 2024-06-12 13:40_

```suggestion
/// AST even if the file contains syntax errors. The parse errors
/// are then accessible through [`Parsed::errors`].
```

---

_Review comment by @carljm on `crates/ruff_db/src/parsed.rs`:31 on 2024-06-12 13:43_

Does it make sense to assume a file is Python if it has no extension? Is that an existing Ruff behavior? That seems likely to lead to a lot of syntax/parsing errors. From a user perspective, I would expect to have to specially request via config if I want Ruff to treat an unknown-extension file as Python source.

---

_Review comment by @carljm on `crates/ruff_db/src/parsed.rs`:18 on 2024-06-12 13:45_

I found this sentence confusing on first read. Clearer would be:
```suggestion
/// Salsa caches the parse tree between invocations if the `VfsFile` hasn't changed, but the query
/// doesn't make use of Salsa's additional optimization
```

---

_Review comment by @carljm on `crates/ruff_db/src/parsed.rs`:68 on 2024-06-12 13:48_

Tests also for stub file, vendored stub file, and unknown-extension file?

---

_Review comment by @carljm on `crates/ruff_python_ast/src/lib.rs`:87 on 2024-06-12 13:50_

As mentioned above, this does not seem to me like desirable behavior. Even if we do want this behavior at the user level, it still seems like surprising and undesirable behavior at the level of a library method like this to return a default that is a total guess and hides the actual nature of the underlying data. Better to return `None` or `PySourceType::Unknown` and let the caller decide what default they want.

---

_@carljm approved on 2024-06-12 13:53_

---

_Comment by @carljm on 2024-06-12 13:54_

Oh, and let's make sure to fix the clippy errors before merging this.

---

_@MichaReiser reviewed on 2024-06-12 14:46_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:31 on 2024-06-12 14:46_

We'll need to filter out non-python files (according to the user's configuration) when walking the workspace so that we never end up calling parse on these files.

---

_@MichaReiser reviewed on 2024-06-12 14:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/lib.rs`:87 on 2024-06-12 14:47_

I don't plan on changing this, as this matches Ruff's current behavior.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system.rs`:55 on 2024-06-12 15:47_

```suggestion
    /// * [`None`], if there is no file name (e.g. if the path ends in `/..`);
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/parsed.rs`:14 on 2024-06-12 15:48_

```suggestion
/// The query uses Ruff's error-resilient parser. That means that the parser always succeeds to produce a
```

---

_@AlexWaygood approved on 2024-06-12 16:37_

---

_@MichaReiser reviewed on 2024-06-13 07:13_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:68 on 2024-06-13 07:13_

I added a test for vendored paths. I prefer to not add a test for unknown extensions and stub files because I then end up re-testing `PySourceType::from_extension`. I rather want to have integration tests for different file types (We write a lot of integration tests in ruff and only few unit tests. What we have in these PRs is already way above the average number of unit tests)

---

_@MichaReiser reviewed on 2024-06-13 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system.rs`:55 on 2024-06-13 07:14_

This is the documentation from `Path::extension` :D I rather keep it as is.

---

_Renamed from "red-knot[salsa part 3]: `source_text`, `line_index`, and `parsed_module` queries" to "red-knot: `source_text`, `line_index`, and `parsed_module` queries" by @MichaReiser on 2024-06-13 07:19_

---

_Merged by @MichaReiser on 2024-06-13 07:37_

---

_Closed by @MichaReiser on 2024-06-13 07:37_

---

_Branch deleted on 2024-06-13 07:37_

---
