```yaml
number: 9660
title: Better build error messages
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/better-build-errors
created_at: 2024-12-05T12:53:34Z
updated_at: 2024-12-17T15:44:45Z
url: https://github.com/astral-sh/uv/pull/9660
synced_at: 2026-01-12T16:08:55Z
```

# Better build error messages

---

_@konstin_

Build failures are one of the most common user facing failures that aren't "obivous" errors (such as typos) or resolver errors. Currently, they show more technical details than being focussed on this being an error in a subprocess that is either on the side of the package or - more likely - in the build environment, e.g. the user needs to install a dev package or their python version is incompatible.

The new error message clearly delineates the part that's important (this is a build backend problem) from the internals (we called this hook) and is consistent about which part of the dist building stage failed. We have to calibrate the exact wording of the error message some more. Most of the implementation is working around the orphan rule, (this)error rules and trait rules, so it came out more of a refactoring than intended.

Example:

![image](https://github.com/user-attachments/assets/2bc12992-db79-4362-a444-fd0d94594b77)




---

_Label `error messages` added by @konstin on 2024-12-05 12:53_

---

_Review requested from @zanieb by @konstin on 2024-12-05 12:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:4961 on 2024-12-05 14:40_

Why do we identify the build backend here but not in the other case?

> Build backend failed to build wheel through `build_wheel` (exit status: 1)

---

_@zanieb reviewed on 2024-12-05 14:40_

---

_@zanieb reviewed on 2024-12-05 14:42_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/error.rs`:80 on 2024-12-05 14:42_

I'd rephrase a little (applies elsewhere)

```suggestion
    #[error("The build backend returned an error; this usually indicates a problem with the package or your environment.")]
```

---

_@zanieb reviewed on 2024-12-05 14:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:766 on 2024-12-05 14:42_

I'm a little hesitant to have the hint in the error chain. Like "this usually indicates a problem with the package or your environment" feels more suitable _after_ the error chain? Stylistically, it seems nice to have the error chain be factual and save our helpful messages for a separate section. What do you think?

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/dist_error.rs`:7 on 2024-12-05 14:45_

Might need to rope in @BurntSushi to review this implementation detail

---

_@zanieb reviewed on 2024-12-05 14:45_

---

_@zanieb reviewed on 2024-12-05 14:46_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/dist_error.rs`:33 on 2024-12-05 14:46_

`AnyBuildError`?

---

_Review comment by @zanieb on `crates/uv-requirements/src/lib.rs`:59 on 2024-12-05 14:47_

Doesn't need to happen here, but is it feasible for us to consolidate this repeated logic?

---

_@zanieb reviewed on 2024-12-05 14:47_

---

_Comment by @zanieb on 2024-12-05 14:50_

Would it also be feasible to explain _why_ we're building a package? Like are we doing it for install or just to read its metadata? Is that already obvious?

---

_@zanieb reviewed on 2024-12-05 15:00_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/error.rs`:80 on 2024-12-05 15:00_

Sorry, my edit wasn't quite sufficient — Charlie would also point out since this is a single sentence (after my change :D) that it shouldn't end in a period anymore.

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/dist_error.rs`:16 on 2024-12-05 17:08_

Do you mean `IsBuildBackendError`?

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/dist_error.rs`:16 on 2024-12-05 17:13_

Also, I would mention that `AsDynError` is an internal thing in `thiserror`. It took me a bit to figure that out as I was confused about it not being in `thiserror`'s public API docs.

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/dist_error.rs`:10 on 2024-12-05 17:15_

Would it be possible to spell out the concrete cycle here? I think that would aide understanding what specifically this trait is trying to do.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/error.rs`:57 on 2024-12-05 17:17_

Hmmm. I believe previously, with `anyhow::Error`, this would result in printing its entire chain. But I'm not sure this new definition does? It might only emit the top of the chain when printing.

---

_Review comment by @BurntSushi on `crates/uv-distribution-types/src/dist_error.rs`:7 on 2024-12-05 17:19_

One thing that would be useful here is a little more docs like, "If the orphan-rule didn't exist, we would do ... instead."

---

_Review comment by @BurntSushi on `crates/uv-build-frontend/src/error.rs`:74 on 2024-12-05 17:21_

Same as below, but this might be dropping any chain that might have been on `anyhow::Error`? Not 100% sure

---

_@BurntSushi reviewed on 2024-12-05 17:23_

I think this is probably fine. At least, I don't immediately see a simpler solution. But some extra docs (see comments) might help clear up some things.

---

_Converted to draft by @konstin on 2024-12-06 11:08_

---

_@konstin reviewed on 2024-12-06 11:50_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/dist_error.rs`:10 on 2024-12-06 11:50_

I'll leave this here as context for the update later.

![image](https://github.com/user-attachments/assets/8d9c60a6-4747-41df-b922-6c6615ad3233)


---

_@konstin reviewed on 2024-12-06 13:14_

---

_Review comment by @konstin on `crates/uv-build-frontend/src/error.rs`:74 on 2024-12-06 13:14_

`AnyErrorBuild` preserves the chain, it passes `.source()` calls through to its wrapped type.

---

_@konstin reviewed on 2024-12-06 13:14_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/dist_error.rs`:33 on 2024-12-06 13:14_

It's more like "`anyhow::Error`, but with an extra trait for build error querying on top".

---

_Review comment by @BurntSushi on `crates/uv-build-frontend/src/error.rs`:74 on 2024-12-06 13:20_

I mean its `Display` impl. The `Display` impl for a `Box<dyn Error>` doesn't emit the chain. But a `anyhow::Error` will.

---

_@BurntSushi reviewed on 2024-12-06 13:20_

---

_@konstin reviewed on 2024-12-06 13:47_

---

_Review comment by @konstin on `crates/uv-build-frontend/src/error.rs`:74 on 2024-12-06 13:47_

afaik all the error types only show their inner message, but not the chain, over which you have to iterate manually. When testing,

```rust
use thiserror::Error;

#[derive(Error, Debug)]
#[error("Wrapper Err")]
struct WrappedErr;

fn main() {
    let err: anyhow::Error = WrappedErr.into();
    println!("{}", err);
    println!("{}", err.context("2"));
}
```

shows

```
Wrapper Err
2
```

and we don't seem to be losing and context in our snapshot tests.

---

_@konstin reviewed on 2024-12-06 14:07_

---

_Review comment by @konstin on `crates/uv-build-frontend/src/error.rs`:74 on 2024-12-06 14:07_

anyhow does show the chain, but only in alternate mode:

```rust
use thiserror::Error;

#[derive(Error, Debug)]
#[error("Inner Err")]
struct InnerErr;

#[derive(Error, Debug)]
#[error("Wrapper Err")]
struct WrappedErr(#[source] InnerErr);

fn main() {
    let err = WrappedErr(InnerErr);
    println!("{:#}", err);
    let err: anyhow::Error = WrappedErr(InnerErr).into();
    println!("{:#}", err);
    println!("{:#}", err.context("2"));
}
```

shows

```
Wrapper Err
Wrapper Err: Inner Err
2: Wrapper Err: Inner Err
```

---

_Marked ready for review by @konstin on 2024-12-06 14:10_

---

_@BurntSushi reviewed on 2024-12-06 14:12_

---

_Review comment by @BurntSushi on `crates/uv-build-frontend/src/error.rs`:74 on 2024-12-06 14:12_

Ah okay, so `anyhow::Error`'s `Display` impl is the same as `Box<dyn Error>`. Interesting. All righty.

And yeah, I realize the snapshot tests indicate nothing untoward is happening here. I don't have a full grasp of the surrounding code and I wasn't sure how easy it would be to break something in the future. But I think you've demonstrated my concern is unfounded. Thank you. :)

---

_Review requested from @zanieb by @konstin on 2024-12-12 15:57_

---

_Review requested from @BurntSushi by @konstin on 2024-12-12 15:57_

---

_Review comment by @BurntSushi on `crates/uv-types/src/traits.rs`:203 on 2024-12-12 16:33_

Nice thank you!

---

_@BurntSushi approved on 2024-12-12 16:33_

Thank you!

---

_@zanieb approved on 2024-12-17 15:44_

---

_Comment by @zanieb on 2024-12-17 15:44_

Awesome. Sorry for the delay!

---

_Merged by @zanieb on 2024-12-17 15:44_

---

_Closed by @zanieb on 2024-12-17 15:44_

---

_Branch deleted on 2024-12-17 15:44_

---
