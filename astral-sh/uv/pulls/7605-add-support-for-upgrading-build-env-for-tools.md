```yaml
number: 7605
title: Add support for upgrading build env for tools
type: pull_request
state: merged
author: tfsingh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: tfsingh/tool-upgrade-python
created_at: 2024-09-20T22:08:33Z
updated_at: 2024-09-26T02:33:16Z
url: https://github.com/astral-sh/uv/pull/7605
synced_at: 2026-01-12T16:07:54Z
```

# Add support for upgrading build env for tools

---

_@tfsingh_

This PR adds support for upgrading the build environment of tools with the addition of a ```--python``` argument to ```uv upgrade```, as specified in #7471.

Some things to note:
- I added support for individual packages — I didn't think there was a good reason for ```--python``` to only apply to all packages
- Upgrading with ```--python``` also upgrades the package itself — I think this is fair as if a user wants to _strictly_ switch the version of Python being used to build a tool's environment they can use ```uv install```.  This behavior can of course be modified if others don't agree!

Closes https://github.com/astral-sh/uv/issues/6297.

Closes https://github.com/astral-sh/uv/issues/7471.


---

_Marked ready for review by @tfsingh on 2024-09-20 23:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-23 23:48_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/upgrade.rs`:214 on 2024-09-24 00:45_

We may want to do this in two steps. Check out `tool/install.rs`, where it says:

```rust
// If we're creating a new environment, ensure that we can resolve the requirements prior
// to removing any existing tools.
```

We perform the resolution before removing and syncing the environment, to avoid removing an environment and then _failing_ to update.

---

_@charliermarsh reviewed on 2024-09-24 00:45_

---

_@charliermarsh reviewed on 2024-09-24 00:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/upgrade.rs`:211 on 2024-09-24 00:46_

Lets make this stricter. Check out the logic we added to `tool/install.rs` recently (grep for `let same_interpreter = if cfg!(windows) { ... }`).

---

_@charliermarsh reviewed on 2024-09-24 00:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/upgrade.rs`:158 on 2024-09-24 00:47_

In general, prefer `Option<&T>` over `&Option<T>`. The latter can always be converted to the former, but the opposite isn't true. I.e., if you have `&Option<PythonRequest>`, you can cast to `Option<&PythonRequest>` via `as_ref()`. But if you have `Option<&PythonRequest>`, you can't get an `&Option<PythonRequest>`.

---

_@charliermarsh reviewed on 2024-09-24 00:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/upgrade.rs`:143 on 2024-09-24 00:47_

I would use `if let [name] = names.as_slice()` here so that you can avoid the `unwrap` below.

---

_@charliermarsh reviewed on 2024-09-24 00:47_

Generally looks good! Nice job figuring this out. A few comments to address before merging.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-09-24 00:47_

---

_Label `enhancement` added by @charliermarsh on 2024-09-24 00:53_

---

_Label `cli` added by @charliermarsh on 2024-09-24 00:53_

---

_@tfsingh reviewed on 2024-09-24 23:09_

---

_Review comment by @tfsingh on `crates/uv/src/commands/tool/upgrade.rs`:143 on 2024-09-24 23:09_

Done

---

_@tfsingh reviewed on 2024-09-24 23:12_

---

_Review comment by @tfsingh on `crates/uv/src/commands/tool/upgrade.rs`:211 on 2024-09-24 23:12_

Awesome, done — I also factored this check into a method on ```Interpreter``` in line with one of Zanie's todos (went ahead and removed it, hope that's okay).

---

_@tfsingh reviewed on 2024-09-24 23:26_

---

_Review comment by @tfsingh on `crates/uv/src/commands/tool/upgrade.rs`:214 on 2024-09-24 23:26_

Okay, this makes sense — I made some updates, primarily dividing up the two flows (environment/tool) into separate codepaths (sans the entrypoint management, which applies to both). 

It didn't make sense to me to rely on ```update_environment``` for the environment upgrade as I was doing before — we'd either be repeating work (as we also resolve the environment in ```update_environment```) or adding extra parameters/functionality (which would make the API more confusing).

Let me know if anything seems off/obtuse and I'll fix it!

---

_Comment by @tfsingh on 2024-09-24 23:26_

Thanks for the review! Should be ready for another pass

---

_@charliermarsh approved on 2024-09-25 17:28_

Thanks, nice work! I made a few changes to the overall structure, but I'll leave a few comments to point them out.

---

_@charliermarsh reviewed on 2024-09-25 17:31_

---

_Review comment by @charliermarsh on `crates/uv-python/src/environment.rs`:305 on 2024-09-25 17:31_

I think this makes more sense on `PythonEnvironment` since that already depends on `Interpreter` as a field, rather than the other way around.

---

_@charliermarsh reviewed on 2024-09-25 17:31_

---

_Review comment by @charliermarsh on `crates/uv/Cargo.toml`:75 on 2024-09-25 17:31_

This is genuinely unused in the crate now, so better to remove than ignore `cargo-shear`.

---

_@charliermarsh reviewed on 2024-09-25 17:32_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/upgrade.rs`:316 on 2024-09-25 17:32_

I generally tried to structure this such that we avoid all `mut`, and have two branches that return the values we care about.

---

_@tfsingh reviewed on 2024-09-25 17:36_

---

_Review comment by @tfsingh on `crates/uv/Cargo.toml`:75 on 2024-09-25 17:36_

Completely missed that this was factored into a separate crate, thanks for catching!

---

_Merged by @charliermarsh on 2024-09-25 17:40_

---

_Closed by @charliermarsh on 2024-09-25 17:40_

---

_Branch deleted on 2024-09-26 02:33_

---

_Branch restored on 2024-09-26 02:33_

---
