```yaml
number: 20661
title: "[`ty`] Fix playground crashes when accessing vendored files with leading slashes"
type: pull_request
state: merged
author: danparizher
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: fix-astral-sh/ty/issues/1290
created_at: 2025-10-01T05:20:55Z
updated_at: 2025-10-07T17:16:37Z
url: https://github.com/astral-sh/ruff/pull/20661
synced_at: 2026-01-12T15:57:07Z
```

# [`ty`] Fix playground crashes when accessing vendored files with leading slashes

---

_@danparizher_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1290


---

_Review requested from @carljm by @danparizher on 2025-10-01 05:20_

---

_Review requested from @MichaReiser by @danparizher on 2025-10-01 05:20_

---

_Review requested from @sharkdp by @danparizher on 2025-10-01 05:20_

---

_Review requested from @dcreager by @danparizher on 2025-10-01 05:20_

---

_Comment by @github-actions[bot] on 2025-10-01 05:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MeGaGiGaGon on 2025-10-01 05:36_

Thanks for putting up with my silly. A couple things:
1. I think you linked the wrong issue for closing, it should be https://github.com/astral-sh/ty/issues/1290
2. Looking at the [docs for `Utf8Component`](https://docs.rs/camino/latest/camino/enum.Utf8Component.html), it looks like the only variant that can hit the `unsupported` match after this is `Prefix`. I tested a bunch of paths like `vendored:\\?\` and `vendored:C:\`, but could not trigger a crash with hitting it, so I guess it's fine? Though it may make since to not panic in there either just in-case it is reachable.

Edit: Looking at the docs more, for `Prefix` it says:
> Does not occur on Unix.

So if the playground is being hosted on a Unix machine (which I image it is), that branch would be unreachable, but it could still crash on windows.

---

_Comment by @MichaReiser on 2025-10-01 06:00_

What's the code path where we call into `VendoredPath::from` that then crashes? I wonder if we should fix this at the API boundary instead.

---

_Label `playground` added by @MichaReiser on 2025-10-01 06:00_

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-01 07:49_

---

_Comment by @danparizher on 2025-10-01 19:02_

> What's the code path where we call into `VendoredPath::from` that then crashes? I wonder if we should fix this at the API boundary instead.

The crash originated from the playground’s vendored URI flow. The path is:
1) Frontend creates `vendored://...` URIs and extracts a path string (can start with “/”) for vendored files.
2) That string crosses the WASM boundary via `getVendoredFile(path)` and is passed directly to `VendoredPath::new(path)`.

https://github.com/astral-sh/ruff/blob/caf48f4bfc54bb587342bf794a4f5167d855f529/crates/ty_wasm/src/lib.rs#L605-L620

3) Inside vendored FS, we normalize via `NormalizedVendoredPath::from(&VendoredPath)`. Previously, a leading “/” became a `RootDir` component and hit the “unsupported” arm, which panicked. We now ignore `RootDir` and Windows `Prefix` components so paths are treated as relative to the archive root instead of panicking.

https://github.com/astral-sh/ruff/blob/1f1c59709da6af09bbd9c6b9030314945ca0603a/crates/ruff_db/src/vendored.rs#L393-L453




---

_Comment by @MichaReiser on 2025-10-02 06:12_

> Frontend creates vendored://... URIs and extracts a path string (can start with “/”) for vendored files.

Yes, I think we should prevent the UI from creating vendored file paths similar to https://github.com/astral-sh/ruff/pull/20666

---

_Comment by @danparizher on 2025-10-02 19:51_

Would we like to remove the logic from `crates/ruff_db/src/vendored.rs` or keep it as a fallback?

---

_@MichaReiser reviewed on 2025-10-03 06:19_

---

_Review comment by @MichaReiser on `playground/ty/src/Editor/Editor.tsx`:210 on 2025-10-03 06:19_

I don't think this is the right place. we need to prevent users from creating a file named `vendored://` , similar to what we've done in https://github.com/astral-sh/ruff/pull/20666

---

_Review requested from @MichaReiser by @danparizher on 2025-10-03 20:45_

---

_@MichaReiser approved on 2025-10-04 11:38_

Thanks. I don't think it's strictly necessary here as the implementation is very close to how we handle leading `/` but I very appreciate if playground changes have a short video as test plan. It helps me to understand what you've tested :) 

---

_Merged by @MichaReiser on 2025-10-04 11:40_

---

_Closed by @MichaReiser on 2025-10-04 11:40_

---

_Branch deleted on 2025-10-04 13:56_

---

_Label `ty` added by @amyreese on 2025-10-07 17:16_

---
