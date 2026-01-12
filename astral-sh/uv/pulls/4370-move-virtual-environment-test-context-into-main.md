```yaml
number: 4370
title: Move virtual environment test context into main context
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/test-context
created_at: 2024-06-17T21:12:21Z
updated_at: 2024-06-18T14:52:26Z
url: https://github.com/astral-sh/uv/pull/4370
synced_at: 2026-01-12T16:06:11Z
```

# Move virtual environment test context into main context

---

_@zanieb_

It was becoming problematic that the virtual environment test context diverged from the other one i.e. we had to implement filtering twice. This combines the contexts and tweaks the `TestContext` API and filtering mechanisms for Python versions. Combined with my previous changes to the test context at #4364 and https://github.com/astral-sh/uv/pull/4368 this finally unblocks the snapshots for test cases in #4360 and https://github.com/astral-sh/uv/pull/4362.

---

_Label `testing` added by @zanieb on 2024-06-17 21:12_

---

_@zanieb reviewed on 2024-06-18 02:57_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:171 on 2024-06-18 02:57_

Windows test builds failed with the mysterious `LINK : fatal error LNK1318: Unexpected PDB error; LIMIT` without this

---

_Review requested from @konstin by @zanieb on 2024-06-18 03:29_

---

_Review requested from @ibraheemdev by @zanieb on 2024-06-18 03:29_

---

_@charliermarsh approved on 2024-06-18 07:28_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 09:23_

nit: 

```suggestion
pub fn venv_bin_path(venv: &impl AsRef<Path>) -> PathBuf {
```

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:135 on 2024-06-18 09:25_

This test needs to fail

---

_Review comment by @konstin on `crates/uv/tests/venv.rs`:65 on 2024-06-18 09:26_

Can we keep them as `.venv`? These tests are meant to check that we create the venv in the right place with the right name

---

_Review comment by @konstin on `crates/uv/tests/venv.rs`:540 on 2024-06-18 09:28_

Could you filter it out with an extra `/?` after the path?

---

_@konstin reviewed on 2024-06-18 09:29_

Nice work! This improves the test filters a lot


r+ me with the two test regressions fixed

---

_@zanieb reviewed on 2024-06-18 12:59_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 12:59_

What's the difference here?

---

_@zanieb reviewed on 2024-06-18 12:59_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:65 on 2024-06-18 12:59_

I'll check.

---

_@zanieb reviewed on 2024-06-18 13:00_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:540 on 2024-06-18 13:00_

It's not a part of the match, it's a part of the replacement text.

---

_@zanieb reviewed on 2024-06-18 13:02_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:135 on 2024-06-18 13:02_

I don't think this is correct. Why would it fail? You don't need a virtual environment to run `pip compile` and we can find an interpreter on the path.

---

_@zanieb reviewed on 2024-06-18 13:04_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:540 on 2024-06-18 13:04_

Unless we want to use match groups in the replacement, it's a pain to drop. I'm not sure it's worth it.

---

_@zanieb reviewed on 2024-06-18 13:12_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:65 on 2024-06-18 13:12_

So the problem here is we actually have different displays on each platform when you pass the full path like we do in these tests. I'll see what I can do.

---

_Merged by @zanieb on 2024-06-18 14:11_

---

_Closed by @zanieb on 2024-06-18 14:11_

---

_Branch deleted on 2024-06-18 14:11_

---

_@konstin reviewed on 2024-06-18 14:33_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:33_

I find it easier to read if there isn't an indirection through a type parameter

---

_@konstin reviewed on 2024-06-18 14:35_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:135 on 2024-06-18 14:35_

I see, that's a different test than i thought it was

---

_@zanieb reviewed on 2024-06-18 14:39_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:39_

Maybe we should chat as a team about what we prefer, I think this style is pretty common in our code.

---

_@ibraheemdev reviewed on 2024-06-18 14:45_

---

_Review comment by @ibraheemdev on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:45_

Personally prefer `impl` for simple bounds and a where clause for anything else, I find the inline generic bounds to be confusing. But it's not a huge deal, either is fine.

---

_@BurntSushi reviewed on 2024-06-18 14:46_

---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:46_

If this were public API library code, I'd have a stronger opinion on which to prefer.

But I don't think I have a strong preference otherwise. I tend to write `impl Trait` these days when I can. I'm also generally happy for us to mix and match. There are enough cases where you can't use `impl Trait` (so you need the explicit type parameter) and enough cases where  `impl Trait` is convenient enough (where you can omit the explicit type parameter) that we kind of have to live with a mixture anyway.

Also, I think the `&P` (or `&impl AsRef<Path>`) can be changed to `P` (or `impl AsRef<Path>`) here.

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:47_

Thanks guys!

---

_@zanieb reviewed on 2024-06-18 14:47_

---

_@BurntSushi reviewed on 2024-06-18 14:47_

---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:47_

(There is the separate problem that we use generics like this _a lot_, and I have a suspicion that its aggregate effect has a non-trivial impact on compile times. But I haven't tested that hypothesis yet. But this is orthogonal to the style question here.)

---

_@zanieb reviewed on 2024-06-18 14:52_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:587 on 2024-06-18 14:52_

#4383 

---
