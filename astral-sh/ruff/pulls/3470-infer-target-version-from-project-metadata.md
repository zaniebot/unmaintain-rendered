```yaml
number: 3470
title: Infer target-version from project metadata
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: target-version-from-project-metadata
created_at: 2023-03-12T19:28:13Z
updated_at: 2024-07-24T16:50:57Z
url: https://github.com/astral-sh/ruff/pull/3470
synced_at: 2026-01-10T21:47:01Z
```

# Infer target-version from project metadata

---

_Pull request opened by @JonathanPlasse on 2023-03-12 19:28_

- Related to #2039

---

_Comment by @github-actions[bot] on 2023-03-12 19:44_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Comment by @charliermarsh on 2023-03-12 21:38_

To clarify, this implements inference in `flake8-to-ruff`, but IIUC #2039 should remain open until Ruff is able to pick up the version from the `pyproject.toml` file at runtime -- is that right?

---

_Comment by @JonathanPlasse on 2023-03-12 21:43_

> To clarify, this implements inference in `flake8-to-ruff`, but IIUC #2039 should remain open until Ruff is able to pick up the version from the `pyproject.toml` file at runtime -- is that right?

I did not implement it as I did not find how Ruff picks up `line-length` from Black or `isort` configuration at runtime.

---

_Comment by @charliermarsh on 2023-03-12 21:57_

We don't! Those are only picked up in `flake8-to-ruff` :)

---

_Comment by @JonathanPlasse on 2023-03-12 22:40_

I propose to merge this PR as is.
I could make a following PR to pick up Black, isort, and `requires-python`.

---

_@andersk reviewed on 2023-03-12 23:00_

---

_Review comment by @andersk on `crates/ruff/src/settings/types.rs`:91 on 2023-03-12 23:00_

Is this going to do the right thing if `pyproject.toml` specifies a patch release like `requires-python = ">= 3.8.16"`?

---

_Comment by @charliermarsh on 2023-03-12 23:10_

@konstin - Do you mind taking a look at this one given your experience with `requires-python`? :)

---

_Comment by @charliermarsh on 2023-03-12 23:10_

(I think it would be reasonable to merge this as a `flake8-to-ruff` improvement, then look towards doing it at runtime in Ruff. I don't want Ruff to be looking for Black or isort settings at runtime though, I'd prefer to leave those in `flake8-to-ruff`. `requires-python` is different to me since it's part of a tool-agnostic standard.)

---

_@JonathanPlasse reviewed on 2023-03-12 23:33_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/types.rs`:91 on 2023-03-12 23:33_

No, it will return `py39`.

---

_@JonathanPlasse reviewed on 2023-03-13 00:19_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/pyproject.rs`:135 on 2023-03-13 00:19_

It will not load `requires-python` if `ruff.toml`is used.

---

_@JonathanPlasse reviewed on 2023-03-13 00:21_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/types.rs`:91 on 2023-03-13 00:21_

This is fixed, I hard coded the patch number to 100 to avoid special casing.

---

_Review requested from @konstin by @MichaReiser on 2023-03-13 07:56_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:287 on 2023-03-13 08:02_

Is it possible to make the inner `Vec` private (are there any reads-writes outside of this module)?

If not, can we use accessors to make it private, e.g. implement `Deref` with `Target =[VersionSpecifier]` or `IntoIter` on `&RequiresVersion`? 

Doing so has the benefit of not exposing the used type to store the `VersionSpecifier`. E.g. we could consider using a `SmallVec` in the future. 

---

_@MichaReiser approved on 2023-03-13 08:02_

---

_Review comment by @konstin on `crates/ruff/src/settings/types.rs`:289 on 2023-03-13 09:25_

This is great, i've stolen this to integrate it into pep440-rs itself: https://github.com/konstin/pep440-rs/commit/0d15f41b5b5c76c58278575b815d5cfe0bb54787

---

_Review comment by @konstin on `crates/ruff/src/settings/types.rs`:91 on 2023-03-13 09:26_

i tried to come up with something better but i agree a really high patch number is the really the best we can do here

---

_Review comment by @konstin on `crates/ruff/src/settings/pyproject.rs`:135 on 2023-03-13 09:29_

could you add a `debug!` for that? my idea is that when someone goes "why is ruff not picking this up?" they can run `RUST_LOG=ruff=debug` and get the answer

---

_Review comment by @konstin on `crates/ruff/src/settings/types.rs`:287 on 2023-03-13 09:32_

good point, i've also taken this in https://github.com/konstin/pep440-rs/commit/08f3e6b2d91fa1d257cbb2675e0fc22645b2701c

---

_@konstin approved on 2023-03-13 09:33_

neat!

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/types.rs`:71 on 2023-03-13 13:13_

@konstin, do you have a more elegant way to do this?

---

_@JonathanPlasse reviewed on 2023-03-13 13:13_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/types.rs`:71 on 2023-03-13 13:23_

Never mind, I forgot to check your previous comment.

---

_@JonathanPlasse reviewed on 2023-03-13 13:23_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/types.rs`:287 on 2023-03-13 13:34_

I replaced `RequiresVersion` with `VersionSpecifiers`.

---

_@JonathanPlasse reviewed on 2023-03-13 13:34_

---

_@JonathanPlasse reviewed on 2023-03-13 13:36_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/types.rs`:289 on 2023-03-13 13:36_

Thanks, I pulled the new version.

---

_Comment by @JonathanPlasse on 2023-03-13 14:02_

- Only missing konstin/pep440-rs#6, though this is optional.

---

_Comment by @konstin on 2023-03-13 17:15_

Thank you!

---

_Merged by @konstin on 2023-03-13 17:16_

---

_Closed by @konstin on 2023-03-13 17:16_

---

_Comment by @JonathanPlasse on 2023-03-13 17:57_

You are welcome! 🤗

---

_Branch deleted on 2023-03-13 17:57_

---

_Comment by @neutrinoceros on 2023-03-13 21:21_

Thanks for this patch !
@charliermarsh, shouldn't this be in the "settings" section of the release notes rather than in "bug fixes" ?

---

_Comment by @charliermarsh on 2023-03-13 21:22_

Yes, thank you, I just updated them!

---
