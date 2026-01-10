```yaml
number: 19340
title: "Move Pylint rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/pylint
created_at: 2025-07-14T21:24:51Z
updated_at: 2025-07-15T14:14:51Z
url: https://github.com/astral-sh/ruff/pull/19340
synced_at: 2026-01-10T17:58:13Z
```

# Move Pylint rendering to `ruff_db`

---

_Pull request opened by @ntBre on 2025-07-14 21:24_

Summary
--

This is a very simple output format, the only decision is what to do if the file
is missing from the diagnostic. For now, I opted to `unwrap_or_default` both the
path and the `OneIndexed` row number, giving `:1: main diagnostic message` in
the test without a file.

Another quirk here is that the path is relativized. I just pasted in the
`relativize_path` and `get_cwd` implementations from `ruff_linter::fs` for now,
but maybe there's a better place for them.

I didn't see any details about why this needs to be relativized in the original
[issue](https://github.com/astral-sh/ruff/issues/1953),
[PR](https://github.com/astral-sh/ruff/pull/1995), or in the pylint
[docs](https://flake8.pycqa.org/en/latest/internal/formatters.html#pylint-formatter),
but it did change the results of the CLI integration test when I tried deleting
it. I haven't been able to reproduce that in the CLI, though, so it may only
happen with `Command::current_dir`.

Test Plan
--

Tests ported from `ruff_linter` and a new test for the case with no file


---

_Label `internal` added by @ntBre on 2025-07-14 21:24_

---

_Label `diagnostics` added by @ntBre on 2025-07-14 21:24_

---

_@ntBre reviewed on 2025-07-14 21:25_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/pylint.rs`:53 on 2025-07-14 21:25_

This is carried over from the original code, but we could use the lint name as a backup again.

---

_Comment by @github-actions[bot] on 2025-07-14 21:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-14 21:41_

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

_Marked ready for review by @ntBre on 2025-07-14 21:43_

---

_Review requested from @carljm by @ntBre on 2025-07-14 21:43_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-14 21:43_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-14 21:43_

---

_Review requested from @sharkdp by @ntBre on 2025-07-14 21:43_

---

_Review requested from @dcreager by @ntBre on 2025-07-14 21:43_

---

_Review request for @dcreager removed by @ntBre on 2025-07-14 21:43_

---

_Review request for @carljm removed by @ntBre on 2025-07-14 21:43_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-14 21:43_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-14 21:43_

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:40 on 2025-07-15 08:02_

The `ruff_db` crate shouldn't depend on any OS specific behavior except for code gated by the `os` feature. It otherwise makes compiling on other tagets (like you noticed with WASM) a fair bit more complicated. 

For ty, making the path relative is implemented in `FileResolver`. Now, whether this is correct I'm not sure because it does mean that the paths are relative for all rendered output formats (including JSON where we might want to use absolute paths, although I'm not sure). But it is the right place to abstract away the integration logic between ty and Ruff. 

https://github.com/astral-sh/ruff/blob/988479d7359a82418906d226280466d75ef27ec0/crates/ruff_db/src/diagnostic/render.rs#L714-L716



One solution is to change `UnifiedFile::path` to also return a relative path for Ruff files. A better solution is probably to add a `current_directory` method to `FileResolver` which returns `system.current_directory` for ty and whatever `path-absolutize` uses for ruff. You can then add a `relative_path` method to `UnifiedFile` which makes `::path` relative to `file_resolver.current_directory`


---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/pylint.rs`:37 on 2025-07-15 08:03_

Let's not add TODO for let-chain. Either clippy fixes them automatically for us or we can rewrite the code when we make chagnes in the future

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/pylint.rs`:47 on 2025-07-15 08:08_

Nit: I find mutable references harder to reason about because it's less clear when they get initialized (it requires tracing the entire code to find all places where the variable is written to). 

I would rewrite this to:

```rust
            let (filename, row) = diagnostic
                .primary_span_ref()
                .map(|span| {
                    let file = span.file();

                    let row =span.range().filter(|_| !self.resolver.is_notebook(file)).map(|range| {
                        file.diagnostic_source(self.resolver)
                            .as_source_code()
                            .line_column(range.start())
                            .line,
                    });


                    (file.path(self.resolver), row)
                })
                .unwrap_or_default();
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/pylint.rs`:53 on 2025-07-15 08:16_

I'm leaning towards doing that. It should also remove the need for the extra `format` and `to_string` calls

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/pylint.rs`:58 on 2025-07-15 08:19_

Interesting, it seems that this matches pylint's VS code reporter and not the text reporter:

https://github.com/pylint-dev/pylint/blob/a14b2c4e68d3585c74fb7a3bbd5636e807a3ce77/pylint/reporters/text.py#L204



---

_@MichaReiser reviewed on 2025-07-15 08:34_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/pylint.rs`:58 on 2025-07-15 12:44_

I think it might be `ParseableTextReporter` actually: https://github.com/pylint-dev/pylint/blob/a14b2c4e68d3585c74fb7a3bbd5636e807a3ce77/pylint/reporters/text.py#L189

The VS Code one doesn't have a colon between `path` and `row`. This is the link in the renderer docstring that I copied over from `ruff_linter`: https://flake8.pycqa.org/en/latest/internal/formatters.html#pylint-formatter. I think it's supposed to be the Pylint output format from the flake8 tool.

Working on these definitely makes me curious if/how people are using the various output formats!

---

_@ntBre reviewed on 2025-07-15 12:44_

---

_@ntBre reviewed on 2025-07-15 12:54_

---

_Review comment by @ntBre on `crates/ruff_db/Cargo.toml`:40 on 2025-07-15 12:54_

I was playing with both the ty and ruff CLIs yesterday, and I think they have similar behavior here. As I mentioned in the description, the only place I saw a difference was in the integration tests where the `[TMP]/` prefix appeared in the output even though it's supposed to be the `current_dir`. So I was thinking that this relativize call might be unnecessary in general.

But I'll add a method to the `FileResolver` just in case, that seems like a nice approach.

---

_Comment by @MichaReiser on 2025-07-15 12:59_

> I didn't see any details about why this needs to be relativized in the original
For the why: This is so that ruff only prints `main.py` if it is in the root of your project and not the full path

---

_Comment by @ntBre on 2025-07-15 13:16_

Oh yeah, I understand why we relativize the path in general, I just meant that I couldn't find any cases in the CLI where _this_ relativize call made a difference. However, I found one immediately today, so I guess I wasn't testing the right thing yesterday.

---

_Review requested from @MichaReiser by @ntBre on 2025-07-15 14:00_

---

_@MichaReiser approved on 2025-07-15 14:04_

Thank you

---

_Merged by @ntBre on 2025-07-15 14:14_

---

_Closed by @ntBre on 2025-07-15 14:14_

---

_Branch deleted on 2025-07-15 14:14_

---
