```yaml
number: 7420
title: "Warn when trying to `uv sync` a package without build configuration"
type: pull_request
state: merged
author: lucab
labels:
  - cli
assignees: []
merged: true
base: main
head: ups/warn-script-without-build-system
created_at: 2024-09-16T10:02:24Z
updated_at: 2024-09-16T15:57:32Z
url: https://github.com/astral-sh/uv/pull/7420
synced_at: 2026-01-12T16:07:49Z
```

# Warn when trying to `uv sync` a package without build configuration

---

_@lucab_

This enhances `uv sync` logic in order to detect and warn if it is trying to operate on a packaged project with entrypoints.

Ref: https://github.com/astral-sh/uv/issues/6998#issuecomment-2329291764
Closes: https://github.com/astral-sh/uv/issues/7034

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:81 on 2024-09-16 10:05_

I'd call this a package with scripts, otherwise it's confusable with PEP 723 scripts with metadata

---

_Review comment by @konstin on `crates/uv/tests/run.rs`:1956 on 2024-09-16 10:08_

nit: This should be the function docstring

---

_Review comment by @lucab on `crates/uv-workspace/src/pyproject.rs`:114 on 2024-09-16 10:10_

Note: `IgnoredAny` doesn't implement `Eq`, but the trait here seemed unused and fine to drop. Alternatively, we can either 1) store TOML tables in here, or 2) fully deserialize both scripts fields.

---

_Review comment by @lucab on `crates/uv/src/commands/project/run.rs`:454 on 2024-09-16 10:13_

Note: sync is performed on CLI add/remove/sync/run, but it seems to annoying to warn on each command. Thus this new warning is only displayed at `uv run` time, which should be the most effective place. But can be expanded if we prefer.

---

_@lucab reviewed on 2024-09-16 10:13_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:104 on 2024-09-16 11:23_

nit: Maybe just `uv_package`, for an `if_` function i'd have expected it returns a `bool` and not an `Option`

---

_@konstin approved on 2024-09-16 11:24_

---

_@zanieb reviewed on 2024-09-16 12:20_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:85 on 2024-09-16 12:20_

I'd be surprised if we needed this exact combination again. I'd just add a function like `has_scripts` and `&&` that with `is_package`.

---

_@zanieb reviewed on 2024-09-16 12:21_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:88 on 2024-09-16 12:21_

Why does this use `uv_package` instead of `is_package`?


---

_@zanieb reviewed on 2024-09-16 12:22_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:83 on 2024-09-16 12:22_

Should we use `has_build_system` here now?

---

_@zanieb reviewed on 2024-09-16 12:26_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:1970 on 2024-09-16 12:26_

How does this interact with `package = true` and `package = false`? If the user sets `package = true` we will install the scripts. Similarly, if they set `package = false` we won't install the scripts even if there's a build system declared.

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:1985 on 2024-09-16 12:28_

We should be more specific than "correctly". I'd make sure to say something about how we found scripts but they won't be installed. If we're only implementing this for `uv run`, we _could_ be even more specific because we know if they're trying to invoke the target and could raise the warning in a more specific case?

Note they also don't need to add a build-system the call to action could be to add `package = true`.

---

_@zanieb reviewed on 2024-09-16 12:28_

---

_@zanieb reviewed on 2024-09-16 12:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:454 on 2024-09-16 12:29_

I think it makes sense to omit at `add` / `remove`. We might want it at `sync`, otherwise if you sync then active the environment (instead of using `uv run`), you'll never see it.

Maybe with some special-casing per https://github.com/astral-sh/uv/pull/7420/files#r1761054908 we can make the warnings quieter in `uv run`.

---

_Comment by @zanieb on 2024-09-16 12:34_

@lucab thanks for the self-review comments!

---

_@konstin reviewed on 2024-09-16 12:43_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:88 on 2024-09-16 12:43_

that's my fault, i didn't like `is_` returning an `Option<bool>`

---

_@lucab reviewed on 2024-09-16 13:36_

---

_Review comment by @lucab on `crates/uv-workspace/src/pyproject.rs`:88 on 2024-09-16 13:36_

Actually my fault, I misunderstood the condition to detect. No need to split the cases in the way I initially did.

---

_@lucab reviewed on 2024-09-16 14:04_

---

_Review comment by @lucab on `crates/uv/tests/run.rs`:1985 on 2024-09-16 14:04_

This is now a
```
Skipping scripts installation because this project is not a package. To install them, consider setting `tool.uv.package = true` or configuring a custom `build-system.`
```


---

_@zanieb reviewed on 2024-09-16 14:09_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:2365 on 2024-09-16 14:09_

We may want a follow-up issue to enumerate which scripts we aren't installing here, e.g. "Skipping installation of script `entry` because..." (depends on parsing the scripts definition)

---

_@zanieb reviewed on 2024-09-16 14:16_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:81 on 2024-09-16 14:16_

I'd say:

> Skipping installation of entry points (`project.scripts`) because this project is not packaged; to install entry points, set `tool.uv.package = true` or define a `build-system`

This is relatively nuanced, so just a few notes:

- We want to say "entry points" because that's the [official term](https://packaging.python.org/en/latest/specifications/entry-points/) and just "scripts" could be confused with some `foo.py` script in the project.
- We don't use a trailing period in our CLI messages
- Usually you're just requesting use of a common build-system, "custom" doesn't feel like quite the right fit
- "packaged" vs "is a package" to match the language in the relevant [docs](https://docs.astral.sh/uv/concepts/projects/#configuring-project-packaging)

---

_@zanieb reviewed on 2024-09-16 14:16_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:2335 on 2024-09-16 14:16_

Do you think it's worth adding a test case for `package = false` with a build system defined?

---

_Comment by @lucab on 2024-09-16 14:22_

From an out-of-band chat with @zanieb:
 - the condition detection can be simplified as a `has_scripts() && !is_package()` heuristic.
 - `uv sync` is a better place for the warning for now, as `uv run` could be too annoying in the general (non-script) case.
 - this could be improved by parsing the scripts tables and key-match; that allows key-matching in `uv run` and adding more details in `uv sync`. Tracked at https://github.com/astral-sh/uv/issues/7428.
 - The initial `uv run` test can stay in the codebase with a note for the future.

---

_@lucab reviewed on 2024-09-16 14:43_

---

_Review comment by @lucab on `crates/uv/tests/sync.rs`:2335 on 2024-09-16 14:43_

Added, together with a test and a TODO for `uv run`.

---

_@zanieb approved on 2024-09-16 14:44_

---

_Marked ready for review by @lucab on 2024-09-16 14:52_

---

_Renamed from "Warn when trying to `uv run` a package without build configuration" to "Warn when trying to `uv sync` a package without build configuration" by @lucab on 2024-09-16 14:56_

---

_Merged by @lucab on 2024-09-16 15:50_

---

_Closed by @lucab on 2024-09-16 15:50_

---

_Branch deleted on 2024-09-16 15:50_

---

_Label `cli` added by @zanieb on 2024-09-16 15:57_

---
