```yaml
number: 17866
title: "Update `ty version` to use parent Git repository information"
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/version
created_at: 2025-05-05T18:01:36Z
updated_at: 2025-05-06T12:14:33Z
url: https://github.com/astral-sh/ruff/pull/17866
synced_at: 2026-01-10T18:57:03Z
```

# Update `ty version` to use parent Git repository information

---

_Pull request opened by @zanieb on 2025-05-05 18:01_

Currently, `ty version` pulls its information from the Ruff repository — but we want this to pull from the repository in the directory _above_ when Ruff is a submodule.

I tested this in the `ty` repository after tagging an arbitrary commit:

```
❯ uv run --refresh-package ty --reinstall-package ty ty version
      Built ty @ file:///Users/zb/workspace/ty
Uninstalled 1 package in 2ms
Installed 1 package in 1ms
ty 0.0.0+3 (34253b1d4 2025-05-05)
```

We also use the last Git tag as the source of truth for the version, instead of the crate version. However, we'll need a way to set the version for releases still, as the tag is published _after_ the build. We can either tag early (without pushing the tag to the remote), or add another environment variable. (**Note, this approach is changed in a follow-up. See https://github.com/astral-sh/ruff/pull/17868**)

From this repository, the version will be `unknown`:

```
❯ cargo run -q --bin ty -- version
ty unknown
```

We could add special handling like... `ty unknown (ruff@...)` but I see that as a secondary goal.

Closes https://github.com/astral-sh/ty/issues/5

The reviewer situation in this repository is unhinged, cc @Gankra and @MichaReiser for review.

---

_Label `ty` added by @zanieb on 2025-05-05 18:01_

---

_@zanieb reviewed on 2025-05-05 18:05_

---

_Review comment by @zanieb on `crates/ty/build.rs`:52 on 2025-05-05 18:05_

Apparently a bare `describe` only works for annotated tags? This works for unannotated tags too.

---

_Marked ready for review by @zanieb on 2025-05-05 20:42_

---

_Review requested from @carljm by @zanieb on 2025-05-05 20:42_

---

_Review requested from @MichaReiser by @zanieb on 2025-05-05 20:42_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-05 20:42_

---

_Review requested from @sharkdp by @zanieb on 2025-05-05 20:42_

---

_Review requested from @dcreager by @zanieb on 2025-05-05 20:42_

---

_Comment by @github-actions[bot] on 2025-05-05 20:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@Gankra approved on 2025-05-05 21:48_

---

_Review request for @dcreager removed by @MichaReiser on 2025-05-06 07:06_

---

_Review request for @carljm removed by @MichaReiser on 2025-05-06 07:06_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-05-06 07:06_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-05-06 07:06_

---

_@MichaReiser approved on 2025-05-06 07:08_

---

_Merged by @zanieb on 2025-05-06 12:14_

---

_Closed by @zanieb on 2025-05-06 12:14_

---

_Branch deleted on 2025-05-06 12:14_

---
