```yaml
number: 3320
title: "Add `cargo dev generate-all --check` and catch outdated docs in `cargo test`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: check-docs-in-cargo-test
created_at: 2023-03-03T12:30:19Z
updated_at: 2023-03-07T20:35:45Z
url: https://github.com/astral-sh/ruff/pull/3320
synced_at: 2026-01-12T04:39:44Z
```

# Add `cargo dev generate-all --check` and catch outdated docs in `cargo test`

---

_Pull request opened by @konstin on 2023-03-03 12:30_

`cargo test` now catches outdated generated docs. You can also run the checks manually with `cargo dev generate-all --check`.

Follow up for #3309 which i was too slow for.

---

_Review requested from @charliermarsh by @konstin on 2023-03-03 12:30_

---

_Review comment by @konstin on `crates/ruff_dev/Cargo.toml`:14 on 2023-03-03 12:31_

i'm surprised i didn't find a better `git diff` like library but that's good enough for this case


---

_@konstin reviewed on 2023-03-03 12:31_

---

_@charliermarsh reviewed on 2023-03-03 15:47_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_json_schema.rs`:57 on 2023-03-03 15:47_

Maybe a nit, but how do you think about `.unwrap` vs. `fn test_generate_json_schema() -> Result<>` with `?` and `Ok(())` etc.?

---

_@charliermarsh approved on 2023-03-03 15:47_

LGTM!

---

_@konstin reviewed on 2023-03-03 17:37_

---

_Review comment by @konstin on `crates/ruff_dev/src/generate_json_schema.rs`:57 on 2023-03-03 17:37_

good catch

---

_@konstin reviewed on 2023-03-03 17:38_

---

_Review comment by @konstin on `crates/ruff/src/rule_selector.rs`:187 on 2023-03-03 17:38_

@charliermarsh please have another look i had to add this hack for logical lines

---

_@charliermarsh reviewed on 2023-03-03 18:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_selector.rs`:187 on 2023-03-03 18:10_

Ahh I see, so we want to omit these because as-is, they're _included_ when running the tests with `--all-features`, but _excluded_ when running `cargo dev generate-all` (and, intentionally, excluded from the docs, schema, etc.).

---

_@charliermarsh approved on 2023-03-03 18:10_

---

_Merged by @konstin on 2023-03-06 10:28_

---

_Closed by @konstin on 2023-03-06 10:28_

---

_Branch deleted on 2023-03-06 10:28_

---

_Comment by @sciyoshi on 2023-03-07 19:57_

It seems like this PR removed the `cargo-build` job in CI. Was that intentional?

---

_Comment by @sciyoshi on 2023-03-07 19:59_

Actually I suppose the build is already part of `cargo test`, so that makes sense.

---

_Comment by @charliermarsh on 2023-03-07 20:14_

Yeah that was the reasoning -- that `cargo test` already does a build.

---

_Comment by @konstin on 2023-03-07 20:35_

sorry i somehow missed to put that in the commit message

---
