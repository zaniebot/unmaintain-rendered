```yaml
number: 18162
title: "[ty] feat: Add JSON output format to diagnostics"
type: pull_request
state: closed
author: agustif
labels:
  - ty
assignees: []
draft: true
base: main
head: feat/output-json
created_at: 2025-05-18T14:38:01Z
updated_at: 2025-07-10T14:12:13Z
url: https://github.com/astral-sh/ruff/pull/18162
synced_at: 2026-01-12T15:56:13Z
```

# [ty] feat: Add JSON output format to diagnostics

---

_@agustif_

## Summary
![ty_json_focused_demo](https://github.com/user-attachments/assets/eca724e7-ad0a-4470-b261-9c64d1e07a2a)

This pull request (HEAD commit `241d861e0cf39e6668ac5d0413f6b421d1ea67d8`) introduces a `--output-format json` option to the `ty` CLI, enabling machine-readable diagnostic output. The core functionality for JSON serialization of diagnostics already existed within `ruff_db` and `ruff_linter`. These changes primarily focus on exposing this capability through `ty`'s command-line interface and internal type systems.

Specifically, this involved:
- Extending `ruff.crates.ty.src.args.OutputFormat` with a `Json` variant and appropriate `clap` attributes.
- Extending `ruff.crates.ruff_db.src.diagnostic.mod.rs.DiagnosticFormat` with a corresponding `Json` variant.
- Updating the `Display` implementation in `ruff.crates.ruff_db.src.diagnostic.render.rs` to serialize diagnostics to JSON when the `Json` format is selected. This includes adding `serde_json` as an optional dependency to `ruff_db` activated by its `serde` feature.
- Modifying `ruff.crates.ty.src.lib.rs` in the `run_check` function's message handling loop to:
    - Correctly map `ty`'s `OutputFormat::Json` to `ruff_db`'s `DiagnosticFormat::Json`.
    - Ensure that when no diagnostics are found and JSON output is selected, an empty JSON array (`[]`) is printed to stdout.
    - Iterate through diagnostics and print them as a JSON array when the `Json` format is active.
- Updating documentation (`docs/reference/configuration.md`), CLI help strings (`long_help` in `clap`), and `ty.schema.json` to include the new `json` output format option.

## Test Plan

The changes were tested through the following steps:
- Successfully built the `ty` package within the `ruff` workspace (`cargo build --package ty`).
- **Manual CLI Testing:**
    - Ran `ty check --output-format json path/to/file_with_type_errors.py | jq .` and verified that the output was a valid JSON array containing the expected diagnostic objects (code, severity, message, path, location).
    - Ran `ty check --output-format json path/to/file_without_errors.py | jq .` and verified that the output was an empty JSON array (`[]`).
    - Ran `ty check --output-format xml path/to/file.py` and confirmed it produced a `clap` error (exit code 2), indicating `xml` is an invalid value and listing `json` as a possible value.
- **Automated Unit Testing:**
    - Added a new Rust unit test (`check_json_output`) to `ruff/crates/ty/tests/cli.rs`. This test:
        - Creates a temporary Python file with a known type error.
        - Invokes the `ty` binary with `--output-format json` on this file.
        - Asserts that the command exits with the expected status code (e.g., 1, due to the type error).
        - Parses the standard output as a `Vec<serde_json::Value>` to confirm it's a valid JSON array.
        - Asserts that the array is not empty and that the first diagnostic object contains the expected fields and values (code, severity, message, etc.).

This comprehensive testing ensures that the new JSON output format is correctly implemented, handles both error and no-error cases, and integrates properly with the existing CLI infrastructure.

---

_Review requested from @carljm by @agustif on 2025-05-18 14:38_

---

_Review requested from @MichaReiser by @agustif on 2025-05-18 14:38_

---

_Review requested from @AlexWaygood by @agustif on 2025-05-18 14:38_

---

_Review requested from @sharkdp by @agustif on 2025-05-18 14:38_

---

_Review requested from @dcreager by @agustif on 2025-05-18 14:38_

---

_Renamed from "[ty]feat: Add JSON output format" to "[ty] feat: Add JSON output format to diagnostics" by @agustif on 2025-05-18 14:44_

---

_Label `ty` added by @AlexWaygood on 2025-05-18 15:10_

---

_Comment by @github-actions[bot] on 2025-05-18 15:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser requested changes on 2025-05-18 15:21_

Thanks for working on this. There are many changes that are unrelated to adding the new output format. Please revert any unrelated changes

---

_Review comment by @MichaReiser on `Cargo.lock`:1 on 2025-05-18 15:56_

Can you revert all changes in the `Cargo.lock` and then run `cargo build`. There are some unrelated dependency upgrades in the lockfile changes

---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:56 on 2025-05-18 15:57_

Hmm. This makes me wonder if we should move the diagnostic rendering out of `ruff_db` to avoid adding tons of dependnecies to it. Interested to hear what @BurntSushi thinks

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:157 on 2025-05-18 15:59_

Can you explain why it's necessary to store the resolver here?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:154 on 2025-05-18 16:00_

I'd prefer if the `JsonDiagnostic` mimics the `Diagnostic` structure almost 1:1. E.g. not all diagnostics have a `path` or even a `message`. 

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:298 on 2025-05-18 16:01_

Can you explain why Json needs special handling compared to the rest of the diagnostic rendering?

---

_@MichaReiser reviewed on 2025-05-18 16:01_

---

_@agustif reviewed on 2025-05-18 16:09_

---

_Review comment by @agustif on `Cargo.lock`:1 on 2025-05-18 16:09_

on it heh, thanks for the feedback/help!

---

_Converted to draft by @MichaReiser on 2025-05-18 16:11_

---

_Closed by @agustif on 2025-05-18 16:56_

---

_Reopened by @agustif on 2025-05-18 16:56_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-18 19:24_

---

_Review comment by @BurntSushi on `crates/ruff_db/Cargo.toml`:56 on 2025-05-20 13:29_

It looks like it's just `serde_json` here? I think that probably doesn't move the needle for me here. Granted, there are perhaps some other dependencies that would be removed as a result of kicking rendering out (like `annotate-snippets` and `anstyle` and maybe others).

I am certainly not opposed to the idea of splitting rendering out of this crate. Or perhaps even diagnostics themselves. But I'm not sure I'd argue in favor of it currently.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:116 on 2025-05-20 13:36_

Nit: I think this should be `let json = serde_json::to_string(&json_diagnostic).map_err(|_| std::fmt::Error)?;` and then `write!(f, "{json}")`.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:157 on 2025-05-20 13:38_

Yeah it looks like it can be removed from this struct.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:154 on 2025-05-20 13:39_

Yeah I agree.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:151 on 2025-05-20 13:40_

Can we use `DiagnosticId` and `Severity` here instead of `String`?

---

_Review comment by @BurntSushi on `crates/ty/src/lib.rs`:314 on 2025-05-20 13:45_

I'd rather see this done by using [`Serializer::serialize_seq`](https://docs.rs/serde/1.0.218/serde/ser/trait.Serializer.html#tymethod.serialize_seq) and [`SerializeSeq::serialize_element`](https://docs.rs/serde/1.0.218/serde/ser/trait.SerializeSeq.html#tymethod.serialize_element) instead of hand-writing the JSON.

And yeah, if we could push this down into a routine for rendering a sequence of diagnostics defined closer to the existing diagnostic rendering, that would be better.

Finally, I'm a little skeptical of using a JSON array here. Using either arrays of primitives or arrays at the top-level has bitten be hard in the past when it comes to compatibility, because it's impossible to augment them in compatible ways. I'd much rather see, e.g., `{"diagnostics": [ ... ]}` instead. That way, if we ever need to add any top-level information, we can do so in a non-breaking way.

---

_Review comment by @BurntSushi on `crates/ty/tests/cli.rs`:1501 on 2025-05-20 13:45_

I don't think I quite understand the diff above.

---

_@BurntSushi requested changes on 2025-05-20 13:45_

---

_Review request for @carljm removed by @carljm on 2025-05-20 16:18_

---

_Comment by @MichaReiser on 2025-07-10 14:12_

Thanks for your contribution. I'll close this as it seems to have gone stale. Anyone interested in picking this up. Feel free to open a new PR which addresses the feedback.

---

_Closed by @MichaReiser on 2025-07-10 14:12_

---
