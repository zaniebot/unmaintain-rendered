```yaml
number: 17316
title: "Preserve absolute paths in `uv add` for local dependencies"
type: pull_request
state: open
author: nooscraft
labels: []
assignees: []
base: main
head: bugfix/17307
created_at: 2026-01-04T18:46:08Z
updated_at: 2026-01-07T03:56:24Z
url: https://github.com/astral-sh/uv/pull/17316
synced_at: 2026-01-10T05:49:14Z
```

# Preserve absolute paths in `uv add` for local dependencies

---

_Pull request opened by @nooscraft on 2026-01-04 18:46_

When adding a local package with an absolute path like `uv add /path/to/package`, keep it absolute instead of converting to relative. Relative paths stay relative.

Fixes #17307

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

---

_Review comment by @EliteTK on `crates/uv-resolver/src/lock/mod.rs`:3652 on 2026-01-06 23:19_

Given that we now have 4 places where we are doing this, I think it's may be a candidate for just having a function. Something which also incorporates this logic:

```rust
if was_absolute_path(&url) {
    std::path::absolute(&install_path)
} else {
    relative_to(&install_path, root)
        .or_else(|_| std::path::absolute(&install_path))
}
```

So the call sites can just become `absolute_or_relative_to(instal_path, root, url).map_err(...)?`.

Not sure about the name or location, will get back to you on that.

---

_@EliteTK reviewed on 2026-01-06 23:28_

I've had a look around / and manually tested some corner cases and it seems like this approach should work for the forward path. But...

There's places where we make up a `PathBuiltDist` on the spot e.g.:

https://github.com/astral-sh/uv/blob/cd55d1ce120f3c4dfe37a9df563e97e25914e1ba/crates/uv-resolver/src/lock/mod.rs#L2805-L2809

And it seems like this change might end up with us getting absolute paths in places where previously we would want relative ones.

I haven't found a case like that yet, but it may be worth changing the tests where the snapshots have changed to using relative paths instead, just to see if it shakes out a case like this.

Overall, I'm not experienced enough in this area of the code to be sure if this is the best approach for this. I'll let others comment though.

---

_@nooscraft reviewed on 2026-01-07 03:56_

---

_Review comment by @nooscraft on `crates/uv-resolver/src/lock/mod.rs`:3652 on 2026-01-07 03:56_

Thanks for the review comments and I have addressed the suggestion. 

---
