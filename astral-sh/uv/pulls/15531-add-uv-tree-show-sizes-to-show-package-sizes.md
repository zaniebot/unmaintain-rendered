```yaml
number: 15531
title: "Add `uv tree --show-sizes` to show package sizes"
type: pull_request
state: merged
author: harsh-ps-2003
labels: []
assignees: []
merged: true
base: main
head: size
created_at: 2025-08-26T13:42:45Z
updated_at: 2025-12-03T12:05:23Z
url: https://github.com/astral-sh/uv/pull/15531
synced_at: 2026-01-12T16:11:48Z
```

# Add `uv tree --show-sizes` to show package sizes

---

_@harsh-ps-2003_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds the enhancement proposed in #15470. Each package in the dependency tree now shows its compressed wheel file size, reading the wheel sizes directly from the lockfile (uv.lock). Doesn't break existing tree formatting or options. If no wheel size is available, nothing is added.

Now, developers can identify large packages in their dependency tree. 

The tree still shows extras exactly as before, and then appends a size for the package.

## Test Plan

Manually tested :
```
harsh@fcr-node:~/uv/test-uv-tree-sizes$ ../target/debug/uv tree
Using CPython 3.13.7
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.13`.
Resolved 4 packages in 6ms
pure-python v0.1.0
├── click v8.2.1
└── six v1.17.0
harsh@fcr-node:~/uv/test-uv-tree-sizes$ ../target/debug/uv tree --show-sizes
Using CPython 3.13.7
warning: No `requires-python` value found in the workspace. Defaulting to `>=3.13`.
Resolved 4 packages in 6ms
pure-python v0.1.0
├── click v8.2.1 (99.8KiB)
└── six v1.17.0 (10.8KiB)
```

---

_Comment by @zanieb on 2025-08-26 14:01_

I think this should probably be gated by `--show-sizes`

---

_@zanieb reviewed on 2025-08-26 18:10_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-26 18:10_

Do we need to do all this? Doesn't wrapping or sign loss seem important?

---

_@zanieb reviewed on 2025-08-26 18:11_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/tree.rs`:290 on 2025-08-26 18:11_

Why use a `.with...` pattern here instead of passing the value to `new` like all the other settings?

---

_Comment by @zanieb on 2025-08-26 18:13_

We'll need a new test case in `it/tree`. What does this do with extras?

---

_@harsh-ps-2003 reviewed on 2025-08-26 18:35_

---

_Review comment by @harsh-ps-2003 on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-26 18:35_

I saw a very similar implementation [here](https://github.com/astral-sh/uv/blob/9eb5fc240ca28d8222f1be6d62379fd795e2f09e/crates/uv/src/commands/mod.rs#L192). 

Just being a bit practical here, file sizes rarely exceed the precision limits of f32 and the slight precision loss should be acceptable for display purposes. They don't seem practically important to me, so I prioritised usability over mathematical precision. 

---

_@zanieb reviewed on 2025-08-26 18:47_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-26 18:47_

Ah okay if that's pre-existing that seems fine. Can we share the implementation somewhere?

---

_@harsh-ps-2003 reviewed on 2025-08-27 08:43_

---

_Review comment by @harsh-ps-2003 on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-27 08:43_

The CLI (crates/uv) cannot be depended on by uv-resolver, so if we strictly want to share, we will need to put it in an existing low-level crate. I don't think exporting a UI-ish formatter from uv-resolver as a public API is great for boundaries. 

Again, being a bit practical here, this ~10 LOC duplication keeps dependencies simple. If we expect more byte-formatting call sites or want strict DRY, then maybe some sort of micro-crate is worth it. Otherwise, keeping both copies is not that bad. 

---

_Comment by @harsh-ps-2003 on 2025-08-27 08:48_

>  What does this do with extras?

It's orthogonal to extras. The size is taken from the first wheel in the lockfile that has a size field. Extras do not change which size is shown; there’s no per-extra size. 

---

_Comment by @harsh-ps-2003 on 2025-08-27 16:11_

I think the failing test is a flake!

---

_Renamed from "add size in uv tree" to "Add `uv tree --show-sizes` to show package sizes" by @zanieb on 2025-08-27 16:17_

---

_@zanieb reviewed on 2025-08-27 16:40_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-27 16:40_

It's less about LOC duplication and more about it being a pretty obscure implementation that opts out of a bunch of our typical safety rules.

I think we could move it into `uv-fs`, `uv-normalize`, or some other shared crate.

---

_@harsh-ps-2003 reviewed on 2025-08-27 17:46_

---

_Review comment by @harsh-ps-2003 on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-27 17:46_

The `uv-fs` is filesystem utilities; bytes-to-human is unrelated, and `uv-normalise` normalises names; not generic formatting. What do you think of `uv-types`? It’s a low-level, tiny, pure helper, which is safe for both crates to depend on, from which both call sites can import it cleanly.

---

_@zanieb reviewed on 2025-08-27 17:55_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/tree.rs`:697 on 2025-08-27 17:55_

🤷‍♀️ I don't think `uv-types` is any better really, it's just a display function not a type that we need to share across a bunch of interfaces. We could also use `uv-console` or `uv-logging`.

---

_@zanieb approved on 2025-08-27 19:21_

---

_Comment by @zanieb on 2025-08-28 18:07_

I can't push the fix

```diff
diff --git a/Cargo.lock b/Cargo.lock
index a65e547f1..11368662b 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -6241,6 +6241,7 @@ dependencies = [
  "uv-cache-key",
  "uv-client",
  "uv-configuration",
+ "uv-console",
  "uv-distribution",
  "uv-distribution-filename",
  "uv-distribution-types",
```

---

_Comment by @harsh-ps-2003 on 2025-08-29 09:32_

> I can't push the fix

I pushed for you, but Docker images are failing now! Weird

---

_Comment by @zanieb on 2025-08-29 13:31_

The Docker image auth is a GitHub issue.

---

_Merged by @zanieb on 2025-08-29 13:31_

---

_Closed by @zanieb on 2025-08-29 13:31_

---

_Branch deleted on 2025-08-29 15:31_

---

_Comment by @pablofueros on 2025-12-03 12:05_

Hello everyone. Thanks for this PR, i also proposed this idea a few months ago on #11883 it's great to see it implemented.

Just wanted to also point it out here, if repeated packages existis, sizes could not be shown more than one time (in my Issue I suggest an idea for that). With this, the total sum computation would trivial, and that was my actual concern, just to make an idea of the total project size.

---
