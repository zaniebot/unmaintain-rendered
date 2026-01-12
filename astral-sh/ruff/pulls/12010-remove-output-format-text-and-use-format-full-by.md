```yaml
number: 12010
title: "Remove output format `text` and use format `full` by default"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - configuration
  - cli
assignees: []
merged: true
base: ruff-0.5
head: output-format-full-by-default
created_at: 2024-06-24T09:04:41Z
updated_at: 2024-08-12T14:01:00Z
url: https://github.com/astral-sh/ruff/pull/12010
synced_at: 2026-01-12T15:55:40Z
```

# Remove output format `text` and use format `full` by default

---

_@MichaReiser_

## Summary

This PR removes support for the output format `text` in favor of `concise` and makes `full`  the new default (it was already the default for preview mode). The `OutputFormat::Text` is still defined in code so that we can error when it's being used.

Resolves #7349

## Test Plan

I tested that using `--output-format` on the CLI errors:

```shell
❯ cargo run --bin ruff -- check --output-format text
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff check --output-format text`
ruff failed
  Cause: `--output-format=text` is deprecated. Use `--output-format=full` or `--output-format=concise` instead.

❯ cargo run --bin ruff -- check --config="output-format='text'"
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff check '--config=output-format='\''text'\'''`
ruff failed
  Cause: The Setting `output_format=text` has been deprecated. Update your `--config` CLI arguments to use `output-format="concise"` or  `output-format="full"` instead.
```

I verified that using `output-format="text"` in the settings errors

```shell
❯ ../ruff/target/debug/ruff check test.py
ruff failed
  Cause: The Setting `output_format=text` has been deprecated. Update `pyproject.toml` to use `output-format="concise"` or  `output-format="full"` instead.
```

* I verified that using `concise` in the settings or on the CLI works as expected
* I verified that ruff defaults to `full` when no `output-format` is specified on the CLI or in the settings.

---

_Renamed from "Remove output format `text` and use format `full` by defualt" to "Remove output format `text` and use format `full` by default" by @MichaReiser on 2024-06-24 09:04_

---

_Review requested from @zanieb by @MichaReiser on 2024-06-24 09:05_

---

_Review requested from @snowsignal by @MichaReiser on 2024-06-24 09:05_

---

_Label `breaking` added by @MichaReiser on 2024-06-24 09:05_

---

_Label `configuration` added by @MichaReiser on 2024-06-24 09:05_

---

_Label `cli` added by @MichaReiser on 2024-06-24 09:05_

---

_@MichaReiser reviewed on 2024-06-24 09:07_

---

_Review comment by @MichaReiser on `crates/ruff/tests/integration_test.rs`:1223 on 2024-06-24 09:07_

This looks like a bug but it isn't. Or, it is a bug in the test rule that always emits an empty range instead of emitting an actual range.

---

_Added to milestone `v0.5.0` by @MichaReiser on 2024-06-24 09:10_

---

_Comment by @github-actions[bot] on 2024-06-24 09:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-06-24 13:43_

I'm a little worried about making this change when the full format is half-complete (#7352)

---

_Comment by @MichaReiser on 2024-06-24 15:54_

@zanieb what's the part that you're worried about?

I think showing the code frame for diagnostics is already an improvement by itself worth shipping and the format name `full` is generic enough to allow us to also show fixes in a future version.

---

_Comment by @zanieb on 2024-06-24 15:56_

It just doesn't feel as useful over the concise output without the fixes being shown (seen feedback from one user about this). Oh well though.

---

_Comment by @MichaReiser on 2024-06-24 16:03_

I personally find the source text in the rust compiler extremely useful. I don't think I ever look at the line number to get context on where I need to fix something. 

---

_@snowsignal approved on 2024-06-25 06:47_

LGTM - it might be nice to have a small test plan :)

---

_Marked ready for review by @MichaReiser on 2024-06-25 08:15_

---

_Comment by @MichaReiser on 2024-06-25 08:34_

I updated the PR to keep the `Text` option for now  but I made the usage of `Text` a hard error. 

---

_Review comment by @snowsignal on `crates/ruff/src/args.rs`:929 on 2024-06-25 08:38_

We may want to use a word other than "deprecated" here, since that usually implies "available but discouraged", while this is just a hard error. What about: `--output-format=text is no longer supported`?

---

_@snowsignal reviewed on 2024-06-25 08:40_

---

_Merged by @MichaReiser on 2024-06-26 07:40_

---

_Closed by @MichaReiser on 2024-06-26 07:40_

---

_Branch deleted on 2024-06-26 07:40_

---

_Removed from milestone `v0.5.0` by @MichaReiser on 2024-08-12 14:00_

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 14:00_

---

_Removed from milestone `v0.7` by @MichaReiser on 2024-08-12 14:01_

---
