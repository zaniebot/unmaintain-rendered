```yaml
number: 338
title: Improve error messages for incompatible direct requirements
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: main
head: zanie/direct-incompat
created_at: 2023-11-06T16:51:23Z
updated_at: 2023-11-20T15:13:27Z
url: https://github.com/astral-sh/uv/pull/338
synced_at: 2026-01-10T15:50:28Z
```

# Improve error messages for incompatible direct requirements

---

_Pull request opened by @zanieb on 2023-11-06 16:51_

Addresses https://github.com/astral-sh/puffin/issues/309#issuecomment-1792648969

Instead of running the resolver with a requirement with an empty range of versions, we error early with a message indicating the conflict that resulted in an empty set.

Example at https://github.com/astral-sh/puffin/pull/338/files#diff-d09432742ed63e9e76729f7fdb52772366e983f14e9551bc6f64e4329bdd20e7

---

_@zanieb reviewed on 2023-11-06 16:53_

---

_Review comment by @zanieb on `vendor/pubgrub/src/range.rs`:124 on 2023-11-06 16:53_

Needed a way to check for emptiness...

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:43 on 2023-11-06 16:57_

Clippy is upset by the size of this variant. Suggestions?

---

_@zanieb reviewed on 2023-11-06 16:57_

---

_Comment by @zanieb on 2023-11-06 17:01_

There are some limitations here, like we will display the _merged_ version on one side if there has been more than one previous version specifiers.

---

_@konstin reviewed on 2023-11-06 17:02_

---

_Review comment by @konstin on `crates/puffin-resolver/src/error.rs`:43 on 2023-11-06 17:02_

For e.g. error structs i ignore them

---

_@zanieb reviewed on 2023-11-06 17:06_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:43 on 2023-11-06 17:06_

I think then we also have to ignore in all the methods with `#[allow(clippy::result_large_err)]`? Seems annoying

---

_Renamed from "Improve error messages for incompatible, direct requirements" to "Improve error messages for incompatible direct requirements" by @zanieb on 2023-11-06 18:01_

---

_@zanieb reviewed on 2023-11-06 18:53_

---

_Review comment by @zanieb on `vendor/pubgrub/src/range.rs`:124 on 2023-11-06 18:53_

Note this tweaks the vendored pubgrub

---

_@charliermarsh reviewed on 2023-11-06 19:47_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 19:47_

I think there's a problem with this approach which I also ran into with the "error if a package is requested under two different URLs" thing, which is that it doesn't let us backtrack out of the error state. So if the resolver _ever_ sees a package with a version that has these conflicting requirements internally, it will immediately fail, rather than rejecting that version and attempting to backtrack to a working state.

Is that clear? I don't know how to solve this.


---

_@zanieb reviewed on 2023-11-06 19:54_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 19:54_

That makes sense. Do we do `try_from_requirements` on requirements of dependencies i.e. indirect dependencies?

---

_@charliermarsh reviewed on 2023-11-06 20:02_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 20:02_

Yeah we do.

---

_@charliermarsh reviewed on 2023-11-06 20:02_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 20:02_

(I tend to call those "transitive dependencies".)

---

_@charliermarsh reviewed on 2023-11-06 20:05_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 20:05_

I guess the other issue here is that we won't print out a "tree" of incompatibilities, we'll just tell you which package failed, but not _why_ that package was required. I'm wondering if we can add a new incompatibility kind to support this instead.

---

_@zanieb reviewed on 2023-11-06 20:05_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 20:05_

Ah yes that's a good word for them. Okay... let me look for a better way to handle this then.

---

_@zanieb reviewed on 2023-11-06 20:09_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:195 on 2023-11-06 20:09_

Yeah it seems like it should be an incompatibility, I did not realize this was used for transitive dependencies.

---

_@konstin reviewed on 2023-11-07 08:46_

---

_Review comment by @konstin on `crates/puffin-resolver/src/error.rs`:43 on 2023-11-07 08:46_

ugh that is significantly more annoying :/ in that case you might really want to box 

---

_Closed by @zanieb on 2023-11-20 15:13_

---

_Comment by @zanieb on 2023-11-20 15:13_

Using #424 instead

---
