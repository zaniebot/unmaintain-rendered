```yaml
number: 4327
title: Add support for adding/removing development dependencies
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
assignees: []
merged: true
base: main
head: ibraheem/uv-add-dev
created_at: 2024-06-14T14:00:08Z
updated_at: 2024-06-14T19:17:31Z
url: https://github.com/astral-sh/uv/pull/4327
synced_at: 2026-01-12T16:06:09Z
```

# Add support for adding/removing development dependencies

---

_@ibraheemdev_

## Summary

Support adding/removing dependencies from `tool.uv.dev-dependencies` with `uv add/remove --dev`.

Part of https://github.com/astral-sh/uv/issues/3959.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-14 14:05_

---

_@zanieb reviewed on 2024-06-14 14:38_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1644 on 2024-06-14 14:38_

If I try to remove a dependency that's not present in the production dependencies but in the development dependencies should we just infer that you meant dev? Inference might be too far, but we should at least provide a hint suggesting `--dev` is what you wanted.

---

_@ibraheemdev reviewed on 2024-06-14 14:59_

---

_Review comment by @ibraheemdev on `crates/uv/src/cli.rs`:1644 on 2024-06-14 14:59_

See https://github.com/astral-sh/uv/pull/4327/files#diff-ba8cc7de2d3484ebe70b612b0ecb1ec65d09e426593158e93382cfc0af426640R47.

---

_@zanieb reviewed on 2024-06-14 15:06_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1644 on 2024-06-14 15:06_

Wow you're on it :)

---

_@zanieb reviewed on 2024-06-14 15:06_

---

_Review comment by @zanieb on `crates/uv/src/cli.rs`:1644 on 2024-06-14 15:06_

I guess I should actually read the pull request before I comment on the UX haha

---

_Review requested from @zanieb by @ibraheemdev on 2024-06-14 17:00_

---

_@zanieb reviewed on 2024-06-14 17:05_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:170 on 2024-06-14 17:05_

Why this change?

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:391 on 2024-06-14 17:06_

Similar to above, why are we toggling the Windows filters?

---

_@zanieb reviewed on 2024-06-14 17:06_

---

_@zanieb reviewed on 2024-06-14 17:07_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:391 on 2024-06-14 17:07_

I'm a little confused, where's the `--dev` flag?

---

_@zanieb reviewed on 2024-06-14 17:08_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:429 on 2024-06-14 17:08_

Why are we testing lock and sync during `add --dev`?

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:391 on 2024-06-14 17:09_

Ah okay I see now that you're using `true`/`false` to toggle dev not the `windows_filters`.

---

_@zanieb reviewed on 2024-06-14 17:09_

---

_@zanieb reviewed on 2024-06-14 17:10_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:324 on 2024-06-14 17:10_

This isn't our usual pattern, I'd specify `.arg("--dev")` on each invocation instead.

---

_@zanieb reviewed on 2024-06-14 17:10_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:391 on 2024-06-14 17:10_

See https://github.com/astral-sh/uv/pull/4327#discussion_r1640130152

---

_@zanieb approved on 2024-06-14 17:14_

Implementation looks good, some questions about the tests.

---

_@ibraheemdev reviewed on 2024-06-14 17:15_

---

_Review comment by @ibraheemdev on `crates/uv/tests/common/mod.rs`:324 on 2024-06-14 17:15_

That's nicer, updated.

---

_Review comment by @ibraheemdev on `crates/uv/tests/edit.rs`:429 on 2024-06-14 17:17_

`add` currently implies a full `uv lock && uv sync` with dev deps and all extras enabled. That might be changed, see https://github.com/astral-sh/uv/pull/4193#discussion_r1633167997, but testing the current behavior for now.

---

_@ibraheemdev reviewed on 2024-06-14 17:17_

---

_@zanieb reviewed on 2024-06-14 17:19_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:429 on 2024-06-14 17:19_

Ah okay that seems reasonable. Thanks! A comment may be nice, e.g. "Adding implies a lock and sync including development dependencies"

---

_Label `preview` added by @ibraheemdev on 2024-06-14 17:25_

---

_Merged by @ibraheemdev on 2024-06-14 19:17_

---

_Closed by @ibraheemdev on 2024-06-14 19:17_

---

_Branch deleted on 2024-06-14 19:17_

---
