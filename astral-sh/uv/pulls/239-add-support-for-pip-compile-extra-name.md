```yaml
number: 239
title: "Add support for `pip-compile --extra <name>`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/extra
created_at: 2023-10-30T19:18:37Z
updated_at: 2023-10-31T16:59:41Z
url: https://github.com/astral-sh/uv/pull/239
synced_at: 2026-01-12T16:03:49Z
```

# Add support for `pip-compile --extra <name>`

---

_@zanieb_

Adds support for `pip-compile --extra <name> ...` which includes optional dependencies in the specified group in the resolution.

Following precedent in `pip-compile`, if a given extra is not found, there is no error. ~We could consider warning in this case.~ We should probably add an error but it expands scope and will be considered separately in #241 

---

_Comment by @konstin on 2023-10-30 19:24_

> Following precedent in pip-compile, if a given extra is not found, there is no error. We could consider warning in this case.

I'd prefer throwing an error, pip/pip-compile are generally way to lenient, causing many errors to go unnoticed. My main argument is that the set of extras is fixed and known, so specifying a non-existent extra is almost certainly an error

---

_Comment by @zanieb on 2023-10-30 19:24_

> My main argument is that the set of extras is fixed and known, so specifying a non-existent extra is almost certainly an error

Happy to do that!


---

_Review comment by @konstin on `crates/puffin-package/src/extra_name.rs`:14 on 2023-10-30 19:25_

I'd keep the clone explicit

---

_Review comment by @konstin on `crates/puffin-package/src/extra_name.rs`:26 on 2023-10-30 19:25_

I'd add some comment about normalization on the type itself too

---

_Review comment by @konstin on `crates/puffin-cli/src/requirements.rs`:90 on 2023-10-30 19:28_

Does https://doc.rust-lang.org/stable/std/option/enum.Option.html#method.cloned work here?

---

_@zanieb reviewed on 2023-10-30 19:48_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_extra.snap`:19 on 2023-10-30 19:48_

Unclear why my `--extra foo` flag is excluded here; it is present in a `cargo run -p puffin-cli -- pip-compile` run.

---

_@zanieb reviewed on 2023-10-30 19:48_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/snapshots/pip_compile__compile_pyproject_toml_extra.snap`:19 on 2023-10-30 19:48_

Perhaps the cache dir filter

---

_Marked ready for review by @zanieb on 2023-10-30 19:52_

---

_@zanieb reviewed on 2023-10-30 19:54_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:87 on 2023-10-30 19:54_

We do not normalize the keys of the `optional_dependencies` so this could have confusing failure cases where the user is not using the normalized name. We'll want to fix that...

---

_@zanieb reviewed on 2023-10-30 19:56_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:87 on 2023-10-30 19:56_

I may separate this into a different issue though. I'd love suggestions on how to normalize all the keys cleanly. May make sense as a part of #241 where we will collect all the keys separately anyway?


---

_@charliermarsh reviewed on 2023-10-30 19:56_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:42 on 2023-10-30 19:56_

I think this should go after `constraints`, since those constitute the input requirements.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/main.rs`:101 on 2023-10-30 19:56_

Nit: add a docstring here? This shows up in the CLI help.

---

_@charliermarsh reviewed on 2023-10-30 19:56_

---

_@charliermarsh reviewed on 2023-10-30 19:58_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/requirements.rs`:80 on 2023-10-30 19:58_

I think this can be removed (without any other code changes).

---

_@charliermarsh reviewed on 2023-10-30 19:58_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/requirements.rs`:90 on 2023-10-30 19:58_

Nit: `.cloned()` would be more idiomatic.

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/requirements.rs`:90 on 2023-10-30 19:58_

I think I agree with the suggestion that we should hard-error here.

---

_@charliermarsh reviewed on 2023-10-30 19:58_

---

_@zanieb reviewed on 2023-10-30 20:01_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:80 on 2023-10-30 20:01_

Thanks! Leftover from a previous impl

---

_@zanieb reviewed on 2023-10-30 20:02_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:90 on 2023-10-30 20:02_

Gosh I wish clippy had suggested that instead haha

---

_@zanieb reviewed on 2023-10-30 20:02_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:90 on 2023-10-30 20:02_

See #241 — this is non-trivial because extras may be missing from one source but present in another.

---

_@charliermarsh reviewed on 2023-10-30 21:44_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/requirements.rs`:87 on 2023-10-30 21:44_

Hmm yeah, we probably need to iterate over all the keys then compare the normalized representations.

---

_@zanieb reviewed on 2023-10-30 22:22_

---

_Review comment by @zanieb on `crates/puffin-cli/src/requirements.rs`:87 on 2023-10-30 22:22_

Opting for follow-up in #245 

---

_@konstin approved on 2023-10-31 11:38_

(somehow it didn't submit my review the first time?)

---

_Review comment by @charliermarsh on `crates/puffin-package/src/extra_name.rs`:14 on 2023-10-31 14:20_

I agree. You should be able to use `.clone()` for this, which makes it explicit when users need to copy memory.

---

_@charliermarsh reviewed on 2023-10-31 14:20_

---

_@zanieb reviewed on 2023-10-31 15:38_

---

_Review comment by @zanieb on `crates/puffin-package/src/extra_name.rs`:14 on 2023-10-31 15:38_

This was copied from `PackageName` — I didn't write it :) I'd prefer to follow with a second pull request that resolves both uses.

---

_@zanieb reviewed on 2023-10-31 15:39_

---

_Review comment by @zanieb on `crates/puffin-package/src/extra_name.rs`:26 on 2023-10-31 15:39_

This also is missing from `PackageName`, I can write about normalization of both in a follow-up adding documentation.

---

_@charliermarsh reviewed on 2023-10-31 15:43_

---

_Review comment by @charliermarsh on `crates/puffin-package/src/extra_name.rs`:14 on 2023-10-31 15:43_

That means I wrote it 😎 

---

_Merged by @zanieb on 2023-10-31 16:59_

---

_Closed by @zanieb on 2023-10-31 16:59_

---

_Branch deleted on 2023-10-31 16:59_

---
