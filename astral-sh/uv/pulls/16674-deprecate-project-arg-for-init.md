```yaml
number: 16674
title: "Deprecate `--project` arg for init"
type: pull_request
state: merged
author: mikaylathompson
labels: []
assignees: []
merged: true
base: main
head: mikayla/init-with-project-arg
created_at: 2025-11-10T19:30:07Z
updated_at: 2025-11-10T23:33:10Z
url: https://github.com/astral-sh/uv/pull/16674
synced_at: 2026-01-12T16:12:23Z
```

# Deprecate `--project` arg for init

---

_@mikaylathompson_

Addresses https://github.com/astral-sh/uv/issues/15790

## Summary

After discussion, the functionality of `--project` vs `--directory` was quite unclear in this case, so deprecating `--project` for `init` is probably the clearest behavior option. This is a breaking change, so it requires being under preview before being rolled out fully.

Included in the PR now:
- new feature flag (`init --project` is deprecated if `--preview` or `--preview-features deprecate-project-for-init` are provided)
- tests (for `--directory` behavior, as well as the current warning and future error)
- documentation updated in docs/concepts/projects/init.md


---

_Renamed from "Mikayla/init with project arg" to "Deprecate `--project` arg for init" by @mikaylathompson on 2025-11-10 19:30_

---

_Review comment by @zanieb on `crates/uv-preview/src/lib.rs`:24 on 2025-11-10 19:33_

I'd probably just call this `init-project-flag` for brevity

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1820 on 2025-11-10 19:36_

The difference in language between these is confusing, e.g., we're switching between "argument" and "flag" here. I'd say

> The `--project` option cannot be used in `uv init`. Use `--directory` instead.

and

> Use of the `--project` option in `uv init` is deprecated and support will be removed in a future release . Consider using `--directory` instead.



---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1810 on 2025-11-10 19:37_

Ah convenient we have this lying around!

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:4145 on 2025-11-10 19:38_

Do we need to snapshot the pyproject.toml? Or do we just need to assert it exists at this path?

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1820 on 2025-11-10 19:40_

It is nice to have something about it being ignored, but "some cases" seems ambiguous. Can't we tell here if it will be ignored? Then we could say

> The `--project` option has no affect in `uv init` and will be removed in a future release. Consider...

---

_@zanieb reviewed on 2025-11-10 19:40_

---

_Comment by @zanieb on 2025-11-10 19:41_

For documentation, I think https://docs.astral.sh/uv/concepts/projects/init/#target-directory is the appropriate place to document using `--directory`. I wouldn't mention `--project` at all.

I'd update the CONTRIBUTING guide in a separate pull request.

---

_@mikaylathompson reviewed on 2025-11-10 19:53_

---

_Review comment by @mikaylathompson on `crates/uv/src/lib.rs`:1820 on 2025-11-10 19:53_

Ah, yep, that wording is clearer.

It's not quite accurate though to say that it has no effect. The case where it's not ignored is if a path is provided (so `uv init --project foo` _is_ different than `uv init` but `uv init --project foo bar` _is not_ different than `uv init bar`). But I'll be more direct about that in the wording.

---

_@zanieb reviewed on 2025-11-10 19:56_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1820 on 2025-11-10 19:56_

Can't we detect that case here though and only say it has no effect if there's also a positional path?

---

_@zanieb reviewed on 2025-11-10 19:56_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1820 on 2025-11-10 19:56_

That also suggests a different hint for that case, e.g., when they do `uv init --project foo` we should suggest `uv init foo` not `uv init --directory foo`, I think.

---

_@mikaylathompson reviewed on 2025-11-10 20:16_

---

_Review comment by @mikaylathompson on `crates/uv/src/lib.rs`:1820 on 2025-11-10 20:16_

Oh, yeah, I can definitely detect that case. Interesting about simplifying to `uv init foo`. I think that makes sense.

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:4090 on 2025-11-10 21:27_

I'd drop the newline — I think it's too hard to distinguish it from other output. We don't have the infrastructure to do nice multiline warnings yet.

---

_@zanieb reviewed on 2025-11-10 21:27_

---

_@zanieb reviewed on 2025-11-10 21:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:4065 on 2025-11-10 21:29_

I don't think we need coverage for both of these cases, that's just testing whether our preview abstraction works correctly. I'd probably only test with the specific preview flag for this change.

---

_@zanieb reviewed on 2025-11-10 21:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:4062 on 2025-11-10 21:29_

Isn't a path already being provided positionally in this case?

---

_@zanieb reviewed on 2025-11-10 21:32_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1826 on 2025-11-10 21:32_

The Rust idiom for this would be like `let suggestion = args.path.is_some().then_some("...").unwrap_or("...")`

---

_@mikaylathompson reviewed on 2025-11-10 21:51_

---

_Review comment by @mikaylathompson on `crates/uv/tests/it/init.rs`:4062 on 2025-11-10 21:51_

Yes -- but I didn't distinguish between the two cases for the error message, just the warning.

My thinking was that once we're in the "enforced" case -- either feature preview or post-breaking change, the nuance of the different behavior with vs without a positional path is gone. At that point we don't need to protect/explain the current behavior, which is what the conditional does for the warnings.

I'm fine with changing that behavior if that seems unintuitive -- but it also keeps the long-term logic in the file simpler.

---

_@mikaylathompson reviewed on 2025-11-10 21:52_

---

_Review comment by @mikaylathompson on `crates/uv/tests/it/init.rs`:4062 on 2025-11-10 21:52_

(I am switching this from `PATH` to `positional path`)

---

_@mikaylathompson reviewed on 2025-11-10 21:58_

---

_Review comment by @mikaylathompson on `crates/uv/src/lib.rs`:1826 on 2025-11-10 21:58_

Ah, thanks. I got too excited about matching.
clippy ended up changing this logic to just a simple if/else, so that's coming momentarily.

---

_@zanieb reviewed on 2025-11-10 22:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:4062 on 2025-11-10 22:06_

I'm just confused that we'd tell someone to use the positional path argument when they've already done so. I expect someone would complain about it eventually, but I don't feel strongly. I'm not so worried about the maintenance cost, we tend to invest into specialized error messages.

---

_Comment by @zanieb on 2025-11-10 23:15_

Feel free to mark any addressed comments as "resolved"

---

_@zanieb approved on 2025-11-10 23:16_

---

_Merged by @mikaylathompson on 2025-11-10 23:33_

---

_Closed by @mikaylathompson on 2025-11-10 23:33_

---

_Branch deleted on 2025-11-10 23:33_

---
