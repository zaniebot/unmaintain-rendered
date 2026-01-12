```yaml
number: 20953
title: Refactor format tests to use CliTest helper
type: pull_request
state: merged
author: Renkai
labels:
  - testing
assignees: []
merged: true
base: main
head: renkai-refactor-config.rs
created_at: 2025-10-18T03:44:03Z
updated_at: 2025-10-19T10:31:59Z
url: https://github.com/astral-sh/ruff/pull/20953
synced_at: 2026-01-12T15:57:13Z
```

# Refactor format tests to use CliTest helper

---

_@Renkai_

Part of https://github.com/astral-sh/ruff/issues/19016

---

_Comment by @github-actions[bot] on 2025-10-18 04:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @MichaReiser on 2025-10-18 08:21_

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:737 on 2025-10-18 08:22_

Nit: You could avoid some repetition by adding a `format_command` method to `test` that always sets `format --no-cache` and maybe `--isolated`

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/main.rs`:45 on 2025-10-18 08:28_

Can you tell me why this is now necessary? Is it because of the output format tests? If so, could we use the same approach as in linter tests https://github.com/astral-sh/ruff/blob/15af4c0a34dd6d86d9aa98115ae1e1e9392c7eef/crates/ruff/tests/cli/lint.rs#L3293-L3298

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/snapshots/cli__format__output_format_grouped.snap`:10 on 2025-10-18 08:28_

Is using `--preview` here intentional?

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:46 on 2025-10-18 08:29_

I suggest that we use `CliTest` for all tests, including tests that don't use `with_files` or `write_file`. Using `CliTest` has the benefit that it e.g. clears all environment variables, ensuring better test isolation

---

_@MichaReiser reviewed on 2025-10-18 08:29_

Thank you. i've a few questions

---

_@Renkai reviewed on 2025-10-18 08:32_

---

_Review comment by @Renkai on `crates/ruff/tests/cli/main.rs`:45 on 2025-10-18 08:32_

Yes it's for  output format tests. I'm not successful yet, windows is too weird 😢. Will take a look at your suggested approach soon.

---

_@Renkai reviewed on 2025-10-18 08:33_

---

_Review comment by @Renkai on `crates/ruff/tests/cli/main.rs`:45 on 2025-10-18 08:33_

Why should we add a different filter in a test, but not the common class?

---

_@MichaReiser reviewed on 2025-10-18 08:40_

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/main.rs`:45 on 2025-10-18 08:40_

I decided to use a specific filter for that test because the criteria aren't very specific, risking that we accidentally filter out unrelated content.

---

_@Renkai reviewed on 2025-10-18 11:53_

---

_Review comment by @Renkai on `crates/ruff/tests/cli/snapshots/cli__format__output_format_grouped.snap`:10 on 2025-10-18 11:53_

The diff is from https://github.com/astral-sh/ruff/pull/20443 I think, not sure why the snapshot is not change on his PR but mine

---

_Review requested from @MichaReiser by @Renkai on 2025-10-18 12:34_

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:231 on 2025-10-18 14:30_

We can just pass "ruff.toml" now, because `CliTest::command` ensures that the current working directory is the test root.

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/format.rs`:636 on 2025-10-18 14:31_

What's the reason what we need those new escapes that weren't needed before? Are they also needed when using `CliTest::with_settings`?

---

_@MichaReiser reviewed on 2025-10-18 14:31_

---

_@MichaReiser approved on 2025-10-19 10:10_

Thank you

---

_Merged by @MichaReiser on 2025-10-19 10:25_

---

_Closed by @MichaReiser on 2025-10-19 10:25_

---
