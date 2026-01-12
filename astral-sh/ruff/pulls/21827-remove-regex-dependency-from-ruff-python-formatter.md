```yaml
number: 21827
title: "Remove `regex` dependency from `ruff_python_formatter`"
type: pull_request
state: open
author: magic-akari
labels: []
assignees: []
base: main
head: formatter/remove-regex-dependency
created_at: 2025-12-06T21:49:25Z
updated_at: 2025-12-08T13:00:35Z
url: https://github.com/astral-sh/ruff/pull/21827
synced_at: 2026-01-12T15:57:34Z
```

# Remove `regex` dependency from `ruff_python_formatter`

---

_@magic-akari_

## Summary

This PR removes the `regex` crate dependency from `ruff_python_formatter` by replacing two regex patterns with hand-written state machine parsers.

## Background

While working on compiling `ruff_python_formatter` to WebAssembly, I noticed that the `regex` crate was being pulled in solely for two pattern matches in docstring code example detection:

1. **reStructuredText directive**: `^\s*\.\. \s*(?i:code-block|sourcecode)::\s*(?i:python|py|python3|py3)$`
2. **Markdown fenced code block**: `` (?<ticks>`{3,})(?:\s*(?i:python|py|python3|py3)[^`]*)? `` (and similar for tildes)

These patterns match structured, predictable syntax with a small, fixed set of keywords (`code-block`, `sourcecode`) and language identifiers (`py`, `py3`, `python`, `python3`). This is a task well-suited for hand-written parsers and doesn't require the full power of a regex engine.

## Changes

- Removed `regex` from `[dependencies]` in `Cargo.toml`
- Replaced `LazyLock<Regex>` patterns with hand-written parsing functions:
  - `is_rst_directive_start()` - parses reStructuredText code-block directives
  - `parse_markdown_fence_start()` - parses Markdown fenced code blocks
  - `strip_python_lang_prefix()` - a compact state machine that matches Python language identifiers
- Added unit tests for the new state machine

## Benefits

### 1. Reduced Binary Size

The `regex` crate and its dependencies (`regex-automata`, `regex-syntax`) add significant weight to the compiled binary. This is especially impactful for WebAssembly builds where binary size directly affects load times.

### 2. No Lazy Initialization Cost

With `LazyLock<Regex>`, the regex is compiled on first use. The hand-written parsers require no initialization.

### 3. Improved Maintainability

The parsing logic is now explicit and self-documenting. The state machine structure is clearly visible in the code and documentation:

```text
Start -> 'p' -> 'y' -> (accept "py")
                    -> '3' -> (accept "py3")
                    -> 't' -> 'h' -> 'o' -> 'n' -> (accept "python")
                                                -> '3' -> (accept "python3")
```

### 4. Fewer Dependencies

Reducing the dependency footprint simplifies auditing, reduces potential supply chain attack surface, and speeds up builds.

## Test Plan

- [x] All existing tests pass
- [x] Fixture tests for docstring code examples pass unchanged
- [x] Added unit tests for `strip_python_lang_prefix()` covering:
  - Valid matches: `py`, `py3`, `python`, `python3`
  - Case insensitivity: `PY`, `Python`, `PYTHON3`
  - Trailing content: `python3 extra`
  - Invalid prefixes: `p`, `pyt`, `pyth`, `pytho`
  - No word boundary: `pyx`, `pythonx`, `python3x`
  - Completely different inputs: `rust`, `javascript`, empty string


---

_Comment by @astral-sh-bot[bot] on 2025-12-06 21:58_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @magic-akari on 2025-12-06 22:09_

---

_Review requested from @MichaReiser by @magic-akari on 2025-12-06 22:09_

---

_Review requested from @Copilot by @magic-akari on 2025-12-06 22:09_

---

_@copilot-pull-request-reviewer[bot] reviewed on 2025-12-06 22:15_

## Pull request overview

This PR successfully removes the `regex` crate dependency from `ruff_python_formatter` by replacing two regex patterns with hand-written state machine parsers for docstring code example detection. The implementation provides explicit, self-documenting parsing logic while reducing binary size and eliminating lazy initialization costs.

**Key Changes:**
- Removed `regex` dependency from Cargo.toml
- Replaced reStructuredText directive regex with `is_rst_directive_start()` function
- Replaced Markdown fence regex with `parse_markdown_fence_start()` function and `strip_python_lang_prefix()` state machine
- Added comprehensive unit tests for the Python language identifier state machine

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 2 comments.

| File | Description |
| ---- | ----------- |
| crates/ruff_python_formatter/Cargo.toml | Removed `regex` crate from dependencies |
| crates/ruff_python_formatter/src/string/docstring.rs | Replaced regex patterns with hand-written parsers (`is_rst_directive_start`, `parse_markdown_fence_start`, `strip_python_lang_prefix`, `strip_prefix_ignore_ascii_case`) and added unit tests for the state machine |





---

💡 <a href="/astral-sh/ruff/new/main/.github/instructions?filename=*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

_Review comment by @Copilot on `crates/ruff_python_formatter/src/string/docstring.rs`:1102 on 2025-12-06 22:15_

The original regex required the line to end immediately after the Python language identifier with no trailing whitespace: `(?i:python|py|python3|py3)$`. However, this implementation allows trailing whitespace after the language identifier by calling `.trim()`.

This makes the parser more permissive than the original regex. For example, `.. code-block:: python   ` would now match but wouldn't have matched with the original regex.

To match the original behavior, change to:
```rust
// Line must end immediately after the language identifier (no trailing whitespace)
rest.is_empty()
```
```suggestion
            // Line must end immediately after the language identifier (no trailing whitespace)
            rest.is_empty()
```

---

_Review comment by @Copilot on `crates/ruff_python_formatter/src/string/docstring.rs`:1523 on 2025-12-06 22:15_

This implementation is more strict than the original regex regarding word boundaries for language identifiers. The original regex `(?i:python|py|python3|py3)[^`]*` would match language identifiers as prefixes (e.g., ` ```python3x ` would match as "python" followed by "3x"). The new implementation requires an exact match followed by whitespace or end of string.

This is likely an improvement (more conservative matching), but it is a behavioral change from the original regex. Consider documenting this or adding a test case to explicitly cover this behavior change.

---

_Comment by @MichaReiser on 2025-12-07 10:32_

Do you have any numbers by how much does it reduce the wasm size?

---

_Comment by @magic-akari on 2025-12-07 11:41_

With this PR changes:

```diff
diff --git a/Cargo.toml b/Cargo.toml
index 985a2c3..438ecd1 100644
--- a/Cargo.toml
+++ b/Cargo.toml
@@ -17,9 +17,9 @@ resolver = "2"
 
 	[workspace.dependencies]
 	ruff_fmt_config       = { path = "crates/ruff_fmt_config" }
-	ruff_formatter        = { git = "https://github.com/astral-sh/ruff.git", tag = "0.14.8" }
-	ruff_python_ast       = { git = "https://github.com/astral-sh/ruff.git", tag = "0.14.8" }
-	ruff_python_formatter = { git = "https://github.com/astral-sh/ruff.git", tag = "0.14.8" }
+	ruff_formatter        = { path = "../ruff/crates/ruff_formatter" }
+	ruff_python_ast       = { path = "../ruff/crates/ruff_python_ast" }
+	ruff_python_formatter = { path = "../ruff/crates/ruff_python_formatter" }
 
 
 	anyhow             = "1.0"
```

Binary size reduced from 1.8 MB to 989 KB.

```bash
❯ ls -lh ./ruff_fmt_bg.wasm
-rw-r--r--@ 1 akari  staff   1.8M Dec  7 19:39 ./ruff_fmt_bg.wasm
```

```bash
❯ ls -lh ./ruff_fmt_bg.wasm                                                    
-rw-r--r--@ 1 akari  staff   989K Dec  7 19:34 ./ruff_fmt_bg.wasm
```

---

_Comment by @MichaReiser on 2025-12-07 17:15_

Wow, that's a pretty significant reduction. But I assume it requires removing the ahoacorsik dependency fromthe ast?

---

_Comment by @magic-akari on 2025-12-07 17:37_

No, **aho-corasick** removal is not needed - it's already eliminated automatically.

## Why
1. Dependency path: `aho-corasick` ← `globset` ← `ruff_cache`
2. ruff_cache is only used for `CacheKey` trait and derive macros in the formatter
3. The Formatter doesn't use `impl CacheKey for Regex/Pattern/Glob` - these implementations are only needed by linter/CLI
4. LTO + DCE at link time automatically removes all unreferenced code from `aho-corasick`, `regex-automata`, `globset`, etc.

---

Note: Only verified for the `format_module_source` use case. Ref: https://github.com/wasm-fmt/ruff_fmt/blob/9ecf34b5efacc0f2e696d626a8a221460a83bfae/crates/ruff_fmt/src/lib.rs#L5

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1506 on 2025-12-08 09:36_

Let's move this after `info_string = rest.trim()` 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1951 on 2025-12-08 09:37_

I think this will panic for non ASCII strings because you might end up slicing between char boundaries

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/docstring.rs`:1980 on 2025-12-08 09:37_

Same here: I think this panics for non ASCII strings if the first character is multi-byte character (e.g. an emoji)

---

_@MichaReiser reviewed on 2025-12-08 09:38_

Curious to hear @BurntSushi take. The size win is impressive but the rewrite also shows that doing all this manually makes it very easy to introduce new bugs

---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/string/docstring.rs`:1980 on 2025-12-08 11:50_

Thanks for the review! You're right to be cautious here.

For `strip_python_lang_prefix`, the `bytes.get(..2)?` approach is actually safe because:
1. We use `bytes.get()` which returns `Option` instead of panicking
2. The `eq_ignore_ascii_case(b"py")` check only succeeds when the first two bytes are ASCII `'p'` and `'y'`
3. Once we've confirmed an ASCII match at the byte level, the subsequent string slice is guaranteed to be at a valid UTF-8 boundary (since each ASCII character is exactly one byte)

That said, I've updated the code to use `s.get(n..)` instead of `&s[n..]` for added defensiveness and clarity, and added a SAFETY comment explaining this.


---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/string/docstring.rs`:1951 on 2025-12-08 11:53_

Good catch! The original `s[..prefix.len()]` could indeed panic on non-ASCII input (e.g., if `s` starts with a multi-byte character like `"你abc"` and we try to slice at index 2).

I've fixed this by switching to `s.as_bytes().get(..prefix_len)?` which safely returns `None` instead of panicking when the string is too short or would slice at an invalid boundary.

---

_Review comment by @magic-akari on `crates/ruff_python_formatter/src/string/docstring.rs`:1506 on 2025-12-08 11:53_

Done! Moved it after `info_string = rest.trim()` — makes more sense to check the trimmed info string.

---

_@magic-akari reviewed on 2025-12-08 11:56_

---

_Comment by @BurntSushi on 2025-12-08 13:00_

The `regex` crate has a number of options for decreasing binary size. I'd rather see those options explored before switching to something hand-written here. I actually think the regexes simplify things quite a bit.

In particular, I don't think these regexes need to be "fast"? So the first thing I'd try here is switching to [`regex-lite`](https://docs.rs/regex-lite). It should considerably reduce binary size.

---
