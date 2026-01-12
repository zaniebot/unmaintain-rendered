```yaml
number: 14360
title: "[red-knot] Expand test corpus"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/expand-red-knot-corpus
created_at: 2024-11-15T12:29:52Z
updated_at: 2024-11-15T16:09:17Z
url: https://github.com/astral-sh/ruff/pull/14360
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Expand test corpus

---

_@sharkdp_

## Summary

- Add 383 files from `crates/ruff_python_parser/resources` to the test corpus
- Add 1296 files from `crates/ruff_linter/resources` to the test corpus
- Use in-memory file system for tests
- Improve test isolation by cleaning the test environment between checks
- Add a mechanism for "known failures". Mark ~80 files as known failures.
- The corpus test is now *significantly* slower (~~30 seconds~~ 6 seconds, thanks @MichaReiser).

> [!NOTE]  
> While `red_knot` as a command line tool can run over all of these files without panicking, we still have a lot of test failures caused by explicitly "pulling" all types.

## Test Plan

Run `cargo test -p red_knot_workspace` while making sure that
- Introducing code that is known to lead to a panic fails the test
- Removing code that is known to lead to a panic from `KNOWN_FAILURES`-files also fails the test

---

_Label `red-knot` added by @sharkdp on 2024-11-15 12:29_

---

_Review requested from @carljm by @sharkdp on 2024-11-15 12:29_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-15 12:29_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-15 12:29_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:22 on 2024-11-15 12:40_

Nit: Use a `SystemPathBuf` here to "retain" the information that this is a valid UTF8 path
```suggestion
    Ok(SystemPathBuf::from(String::from_utf8(
        std::process::Command::new("cargo")
            .args(["locate-project", "--workspace", "--message-format", "plain"])
            .output()?
            .stdout,
    )?)
```

---

_@MichaReiser approved on 2024-11-15 12:43_

Nice, with the allow list!

---

_@MichaReiser reviewed on 2024-11-15 13:17_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/tests/check.rs`:95 on 2024-11-15 13:17_

We now risk to have a stale file around when `file` gets deleted. 
```suggestion
            memory_fs.write_file(path, &code).unwrap();
            File::sync_path(&mut db, path);

            // this test is only asserting that we can pull every expression type without a panic
            // (and some non-expressions that clearly define a single type)
            let file = system_path_to_file(&db, path).unwrap();

            let result = std::panic::catch_unwind(|| pull_types(&db, file));

            let expected_to_fail = if path.extension().map(|e| e == "pyi").unwrap_or(false) {
                pyi_expected_to_fail
            } else {
                py_expected_to_fail
            };
            if let Err(err) = result {
                if !expected_to_fail {
                    std::panic::resume_unwind(err);
                }
            } else {
                assert!(!expected_to_fail, "Expected to panic, but did not. Consider removing this path from KNOWN_FAILURES");
            }
            
            memory_fs.remove_file(path);
            file.sync(db);
```

---

_Comment by @github-actions[bot] on 2024-11-15 13:50_

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

_Comment by @codspeed-hq[bot] on 2024-11-15 14:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david/expand-red-knot-corpus)

### Merging #14360 will **not alter performance**

<sub>Comparing <code>david/expand-red-knot-corpus</code> (477caac) with <code>main</code> (62d6502)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Merged by @sharkdp on 2024-11-15 16:09_

---

_Closed by @sharkdp on 2024-11-15 16:09_

---

_Branch deleted on 2024-11-15 16:09_

---
