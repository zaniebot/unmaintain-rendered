```yaml
number: 5104
title: Allow conflicting locals when forking
type: pull_request
state: merged
author: ibraheemdev
labels:
  - lock
assignees: []
merged: true
base: main
head: ibraheem/fork-locals
created_at: 2024-07-16T14:18:49Z
updated_at: 2024-07-16T16:57:32Z
url: https://github.com/astral-sh/uv/pull/5104
synced_at: 2026-01-10T13:42:52Z
```

# Allow conflicting locals when forking

---

_Pull request opened by @ibraheemdev on 2024-07-16 14:18_

## Summary

Currently, the `Locals` type relies on there being a single local version for a given package. With marker expressions this may not be true, a similar problem to https://github.com/astral-sh/uv/pull/4435. This changes the `Locals` type to `ForkLocals`, which tracks locals for a given fork. Local versions are now tracked on `PubGrubRequirement` before forking.

Resolves https://github.com/astral-sh/uv/issues/4580.

---

_Label `lock` added by @ibraheemdev on 2024-07-16 14:18_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-07-16 14:18_

---

_Review requested from @konstin by @ibraheemdev on 2024-07-16 14:18_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:27 on 2024-07-16 16:14_

Is it true that if `local` is `Some`, then `Version::is_local` will always be true?

If so, then perhaps this could just be a `Version`, and case analysis uses `Version::is_local` or `Version::local`.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/locals.rs`:16 on 2024-07-16 16:15_

Should this have a `assert!(local.is_local())`?

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6750 on 2024-07-16 16:18_

Sweet

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6823 on 2024-07-16 16:19_

I think this is wrong right, because of #5086? If you could leave a comment on the test noting that, that would be great.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:27 on 2024-07-16 16:20_

After reading through the PR, I retract my suggestion. I see how this is very convenient because the acquisition of a local-only `Version` is pretty naturally expressed via `Option<Version>`. And I don't think you ever look at a `local` and try to access its local segments, so there isn't really any redundant case analysis.

---

_@BurntSushi approved on 2024-07-16 16:21_

w00t!

---

_@ibraheemdev reviewed on 2024-07-16 16:47_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/pubgrub/dependencies.rs`:27 on 2024-07-16 16:47_

> Is it true that if local is Some, then Version::is_local will always be true?

This is actually not true, because we also extract local versions from direct URL/path requirements.

---

_Merged by @ibraheemdev on 2024-07-16 16:57_

---

_Closed by @ibraheemdev on 2024-07-16 16:57_

---

_Branch deleted on 2024-07-16 16:57_

---
