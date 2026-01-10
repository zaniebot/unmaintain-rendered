```yaml
number: 19370
title: "Move JUnit rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/junit
created_at: 2025-07-15T18:25:51Z
updated_at: 2025-07-17T22:24:16Z
url: https://github.com/astral-sh/ruff/pull/19370
synced_at: 2026-01-10T17:58:13Z
```

# Move JUnit rendering to `ruff_db`

---

_Pull request opened by @ntBre on 2025-07-15 18:25_

Summary
--

This PR moves the JUnit output format to the new rendering infrastructure. As I
mention in a TODO in the code, there's some code that will be shared with the
`grouped` output format. Hopefully I'll have that PR up too by the time this one
is reviewed.

Test Plan
--

Existing tests moved to `ruff_db`


---

_Label `internal` added by @ntBre on 2025-07-15 18:25_

---

_Label `diagnostics` added by @ntBre on 2025-07-15 18:25_

---

_Comment by @github-actions[bot] on 2025-07-15 18:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-15 19:00_

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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @ntBre on 2025-07-15 21:18_

---

_Review requested from @carljm by @ntBre on 2025-07-15 21:18_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-15 21:18_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-15 21:18_

---

_Review requested from @sharkdp by @ntBre on 2025-07-15 21:18_

---

_Review requested from @dcreager by @ntBre on 2025-07-15 21:18_

---

_Review request for @dcreager removed by @ntBre on 2025-07-15 21:18_

---

_Review request for @carljm removed by @ntBre on 2025-07-15 21:18_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-15 21:18_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-15 21:18_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:18 on 2025-07-16 06:18_

Could you add a link to the format specification?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:55 on 2025-07-16 06:25_

Given that this is a free text field, I'd prefer if we omit `row` and `column` for diagnostics without a location rather than making up a location. For the future, I could see us use the full diagnostic renderer for the description similar to https://www.ibm.com/docs/en/developer-for-zos/16.0.x?topic=formats-junit-xml-format

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:72 on 2025-07-16 06:28_

Same here, I think I'd prefer not to insert a made-up line and column information if they are missing

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:75 on 2025-07-16 06:30_

I know that's pre-existing but I wonder if the test case name is supposed to be unique (but it isn't if a file contains multiple diagnostics with the same code). But this isn't something that we need to solve in this PR

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:62 on 2025-07-16 06:31_

We can move this out of the diagnostics loop because file name never changes for a single group

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:62 on 2025-07-16 06:33_

I think what this does is to strip the extension? `file_path.with_extension("")` would be more obvious

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/junit.rs`:89 on 2025-07-16 06:44_

This has now come up a couple of times (JSON reports too). 

We could write a `io::Write` to `fmt::Write` adapter instead. It's slightly less efficient because it needs to validate that the string is valid UTF8

```rust
struct FmtAdapter<'a> {
    fmt: &'a mut dyn std::fmt::Write,
}

impl std::io::Write for FmtAdapter<'_> {
    fn write(&mut self, buf: &[u8]) -> std::io::Result<usize> {
        self.fmt
            .write_str(str::from_utf8(buf).map_err(|_| {
                std::io::Error::new(
                    std::io::ErrorKind::InvalidData,
                    "Invalid UTF-8 in JUnit report",
                )
            })?)
            .map_err(io_error)?;

        Ok(buf.len())
    }

    fn flush(&mut self) -> std::io::Result<()> {
        Ok(())
    }

    fn write_fmt(&mut self, args: Arguments<'_>) -> std::io::Result<()> {
        self.fmt.write_fmt(args).map_err(io_error)
    }
}

fn io_error(error: std::fmt::Error) -> std::io::Error {
    std::io::Error::new(std::io::ErrorKind::Other, error)
}
```

The other alternative (but this might be worth a separate PR) is to explore if we can pass an `std::io::Write` instead of an `std::fmt::Write`

---

_@MichaReiser approved on 2025-07-16 06:44_

---

_@ntBre reviewed on 2025-07-16 13:25_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/junit.rs`:75 on 2025-07-16 13:25_

That's a good point, it does seem like it's probably supposed to be unique. This [site](https://llg.cubic.org/docs/junit/) says it's the test method name in the test's class specified by `classname`.

---

_@ntBre reviewed on 2025-07-16 13:26_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/junit.rs`:89 on 2025-07-16 13:26_

Thanks, I can add this for now. I poked a bit at passing a `std::io::Write` to all of these formats before, but I think it would justify its own PR like you said.

---

_Merged by @ntBre on 2025-07-17 22:24_

---

_Closed by @ntBre on 2025-07-17 22:24_

---

_Branch deleted on 2025-07-17 22:24_

---
