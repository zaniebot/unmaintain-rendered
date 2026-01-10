```yaml
number: 9013
title: "Build backend: Switch to custom glob-walkdir implementation"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/uv-build-backend-globset
created_at: 2024-11-11T10:55:00Z
updated_at: 2024-11-14T13:43:16Z
url: https://github.com/astral-sh/uv/pull/9013
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Switch to custom glob-walkdir implementation

---

_Pull request opened by @konstin on 2024-11-11 10:55_

When doing a directory traversal for source dist inclusion, we want to offer the user include and exclude options, and we want to avoid traversing irrelevant directories. The latter is important for performance, especially on network file systems, but also with large data directories, or (not-included) directories with other permissions. To support this, we introduce `GlobDirFilter`, which uses a DFA from regex_automata to determine whether any children of a directory can be included and skips the directory if not.

The globs are based on PEP 639. The syntax is more restricted than glob or globset, but it's standardized. I chose it over glob or globset because we're already using this syntax for `project.license-files` a required by PEP 639, so it makes sense to use the same globs for all includes (see e.g. https://github.com/google-deepmind/alphafold3/blob/4f52a3bb624fd7245fb1ad0bf62d752ffa46f0ce/pyproject.toml#L36-L48 for example with same semantics for include and exclude)

### Semantics

Glob semantics are complex due to mixing directories and files, expectations around simplicity and our need to exclude most of the tree in the project from traversal. The current draft uses a syntax that optimizes for simple default use cases for the start.

#### includes

Glob expressions which files and directories to include in the source distribution.

Includes are anchored, which means that `pyproject.toml` includes only
`<project root>/pyproject.toml`. Use for example `assets/**/sample.csv` to include for all
`sample.csv` files in `<project root>/assets` or any child directory. To recursively include
all files under a directory, use a `/**` suffix, e.g. `src/**`. For performance and
reproducibility, avoid unanchored matches such as `**/sample.csv`.

The glob syntax is the reduced portable glob from
[PEP 639](https://peps.python.org/pep-0639/#add-license-FILES-key).

#### excludes

Glob expressions which files and directories to exclude from the previous source
distribution includes.

Excludes are not, which means that `__pycache__` excludes all directories named
`__pycache__` and it's children anywhere. To anchor a directory, use a `/` prefix, e.g.,
`/dist` will exclude only `<project root>/dist`.

The glob syntax is the reduced portable glob from
[PEP 639](https://peps.python.org/pep-0639/#add-license-FILES-key).

---

_@konstin reviewed on 2024-11-11 10:57_

---

_Review comment by @konstin on `Cargo.lock`:1 on 2024-11-11 10:57_

We aren't actually adding any new dep.

---

_@konstin reviewed on 2024-11-11 10:59_

---

_Review comment by @konstin on `crates/uv-build-backend/src/pep639_glob.rs`:1 on 2024-11-11 10:59_

moved to the new crate and edited

---

_@konstin reviewed on 2024-11-11 10:59_

---

_Review comment by @konstin on `crates/uv-build-backend/src/pep639_glob/tests.rs`:1 on 2024-11-11 10:59_

moved to the new crate and edited

---

_@konstin reviewed on 2024-11-11 11:03_

---

_Review comment by @konstin on `crates/uv-globfilter/src/main.rs`:1 on 2024-11-11 11:03_

currently useful, we can drop it eventually

---

_Marked ready for review by @konstin on 2024-11-13 12:17_

---

_Review requested from @BurntSushi by @konstin on 2024-11-13 12:17_

---

_@konstin reviewed on 2024-11-13 12:18_

---

_Review comment by @konstin on `crates/uv-globfilter/src/glob_dir_filter.rs`:41 on 2024-11-13 12:18_

This is a different strategy than globset, it avoids normalizing the path itself.

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/glob_dir_filter.rs`:8 on 2024-11-13 19:39_

This is probably fine. Or at least, as a whim, I don't see this being a huge issue, especially given the restricted glob syntax.

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/glob_dir_filter.rs`:41 on 2024-11-13 19:40_

Hmmm, what if the path has both a `/` and a `\` in it?

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/glob_dir_filter.rs`:102 on 2024-11-13 19:48_

I believe this is one of the few places where [`OsStr::as_encoded_bytes`](https://doc.rust-lang.org/std/ffi/struct.OsStr.html#method.as_encoded_bytes) is appropriate! This is great because it will avoid an alloc and a UTF-8 validation scan.

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/glob_dir_filter.rs`:111 on 2024-11-13 19:50_

I think this makes sense to me. Nice edge cases.

---

_@BurntSushi approved on 2024-11-13 19:50_

Pretty sweet! Have a few comments but this overall LGTM!

---

_@konstin reviewed on 2024-11-14 12:51_

---

_Review comment by @konstin on `crates/uv-globfilter/src/glob_dir_filter.rs`:41 on 2024-11-14 12:51_

Good question, can we reasonably support this? Given that we want to (need to) be cross platform, should we error if we ever encounter any path with a `/` (windows) or a `\` (linux) inside a component?

---

_@konstin reviewed on 2024-11-14 12:53_

---

_Review comment by @konstin on `crates/uv-globfilter/src/glob_dir_filter.rs`:102 on 2024-11-14 12:53_

> 1.74.0

I've waited for this method for years :tada:  

---

_Label `preview` added by @konstin on 2024-11-14 13:03_

---

_Merged by @konstin on 2024-11-14 13:14_

---

_Closed by @konstin on 2024-11-14 13:14_

---

_Branch deleted on 2024-11-14 13:14_

---

_@BurntSushi reviewed on 2024-11-14 13:41_

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/glob_dir_filter.rs`:102 on 2024-11-14 13:41_

Been about a decade for me hah.

(Although I've always been skeptical of stabilizing it too, because it makes it much easier to use WTF-8 as an interchange format, which is not supposed to happen.)

---

_@BurntSushi reviewed on 2024-11-14 13:43_

---

_Review comment by @BurntSushi on `crates/uv-globfilter/src/glob_dir_filter.rs`:41 on 2024-11-14 13:43_

Oh hmmm, not within a component. What I mean is that I've seen Windows paths where `\` and `/` are inter-mingled. For example, `C:\Users\andrew\home/bin/whatever`. Or at least, I think I have... So in that circumstance, `\` and `/` would both be a separator.

---
