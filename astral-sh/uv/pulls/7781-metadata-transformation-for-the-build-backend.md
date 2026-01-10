```yaml
number: 7781
title: Metadata transformation for the build backend
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend
created_at: 2024-09-29T15:46:45Z
updated_at: 2024-10-07T12:55:59Z
url: https://github.com/astral-sh/uv/pull/7781
synced_at: 2026-01-10T12:53:55Z
```

# Metadata transformation for the build backend

---

_Pull request opened by @konstin on 2024-09-29 15:46_

Implement the validation and transformation from `pyproject.toml` to `METADATA` and `entry_points.txt`. There is some incomplete plumbing code in `crates/uv-build-backend/src/lib.rs` to actually write `METADATA` and `entry_points.txt` into a stub wheel or dist-info directory, the logic lives in `crates/uv-build-backend/src/metadata.rs`.

The code is a translation of the relevant specs:
* <https://packaging.python.org/en/latest/guides/writing-pyproject-toml/>
* <https://packaging.python.org/en/latest/specifications/pyproject-toml/>
* <https://packaging.python.org/en/latest/specifications/core-metadata/>

The code is a translation of these with some contextual knowledge. While there are tests, we will only have real coverage once we do the entire wheel writing process and can use them with other tools.

The feature is still hidden. Reference docs for `pyproject.toml` are in the next branch, to avoid publishing them.

---

_Label `preview` added by @konstin on 2024-09-29 15:46_

---

_@konstin reviewed on 2024-09-30 21:05_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:35 on 2024-09-30 21:05_

This trait will gain a method to add files

---

_@konstin reviewed on 2024-09-30 21:06_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:66 on 2024-09-30 21:06_

I've been considering making the build backend sync to switch to the sync zip crate

---

_Marked ready for review by @konstin on 2024-09-30 21:35_

---

_Review requested from @BurntSushi by @konstin on 2024-09-30 21:35_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:66 on 2024-10-04 15:40_

cc @ibraheemdev 

---

_@BurntSushi reviewed on 2024-10-04 15:40_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:13 on 2024-10-04 17:50_

I usually like to write imports in three distinct groups, separated by a line break: std imports, non-crate imports, crate imports. (I mention this because this is greenfield code and maybe we can start with this convention. :P)

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/metadata.rs`:501 on 2024-10-04 17:59_

Maybe a dumb question (and this is for my own edification probably because I trust you're doing the right thing here), but do we have code somewhere else for parsing `pyproject.toml`? Why can't we use that here?

(Putting an answer to that question in the comments might be useful.)

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/pep639_glob.rs`:78 on 2024-10-04 18:04_

How does one write a glob that matches `]` and only `]`?

(I ask this because I notice there is no handling of escaping here. You can escape most things via brackets, e.g., `[*]`, but I don't think `]` can be matched literally here?)

---

_@BurntSushi approved on 2024-10-04 18:06_

This all looks like a reasonable start to me.

For async, I would bias toward not doing it. @ibraheemdev might have some thoughts here. My understanding is that a build backend is mostly CPU bound anyway, so using async for it might not make a ton of sense?

---

_Review comment by @ibraheemdev on `crates/uv-build-backend/src/lib.rs`:66 on 2024-10-04 19:16_

My intuition would be for the build backend to be synchronous. Unless you see some usefulness out of asynchronous control flow, async will likely just be additional complexity (and worse performance).

The other factor is, how will uv be interfacing with the build backend? If uv needs to call out to the build backend from an asynchronous context, we incur a `spawn_blocking` call if the build backend is synchronous. On the other hand, if the build backend is always spawned as a separate process, then async doesn't provide any benefit.

---

_@ibraheemdev reviewed on 2024-10-04 19:16_

---

_@konstin reviewed on 2024-10-07 08:09_

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:501 on 2024-10-07 08:09_

That's a question for @charliermarsh 

---

_@konstin reviewed on 2024-10-07 08:11_

---

_Review comment by @konstin on `crates/uv-build-backend/src/pep639_glob.rs`:78 on 2024-10-07 08:11_

I don't think you can - If you consider this a concern, best add it to the [discourse thread](https://discuss.python.org/t/pep-639-round-3-improving-license-clarity-with-better-package-metadata/53020/121)

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:66 on 2024-10-07 08:38_

Thanks for the input, follow-up at: https://github.com/astral-sh/uv/pull/7961

---

_@konstin reviewed on 2024-10-07 08:38_

---

_Merged by @konstin on 2024-10-07 08:38_

---

_Closed by @konstin on 2024-10-07 08:38_

---

_Branch deleted on 2024-10-07 08:38_

---

_@BurntSushi reviewed on 2024-10-07 12:55_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/pep639_glob.rs`:78 on 2024-10-07 12:55_

Aye, done: https://discuss.python.org/t/pep-639-round-3-improving-license-clarity-with-better-package-metadata/53020/122

---
