```yaml
number: 11989
title: "[red-knot] Eagerly normalize `VendoredPathBuf`s"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/eagerly-normalize-vendored-paths
created_at: 2024-06-23T15:06:35Z
updated_at: 2024-06-28T12:46:08Z
url: https://github.com/astral-sh/ruff/pull/11989
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Eagerly normalize `VendoredPathBuf`s

---

_Pull request opened by @AlexWaygood on 2024-06-23 15:06_

## Summary

Currently `VendoredPath`s are only normalized when you actually try to use them to lookup a path in the vendored zip archive. This has a couple of disadvantages:
- The path is only "dynamically" checked when you actually try to use it. If the path is invalid, we should probably validate it and normalize it at the point where it's constructed, so that it's impossible to create a `VendoredPath(Buf)` to begin with.
- Every time you query whether a path exists (or query some other metadata about the path) in the zip archive, the normalization has to allocate a new `String`. But this allocation is hidden from the user of the `VendoredFileSystem` APIs, and feels somewhat wasteful if you're making several queries with the same path.

This PR changes the design of the `VendoredFileSystem`, `VendoredPath` and `VendoredPathBuf` so that normalization is done eagerly at the point when `VendoredPath` and `VendoredPathBuf` are created, rather than lazily when you try to use them to query paths in the zip archive. The main _disadvantage_ of this is that it becomes a lot more annoying to construct a `VendoredPath` from an `&str`. Previously you could do `VendoredPath::new("foo.pyi")`; now you must do `&VendoredPathBuf::try_from("foo.pyi").unwrap()`.

I'm not wedded to this PR as it does feel like it makes the API somewhat more awkward to use. But I said I'd look into this as a followup for https://github.com/astral-sh/ruff/pull/11863 (see e.g. https://github.com/astral-sh/ruff/pull/11863#discussion_r1639324437). So this is the followup!

## Test Plan

`cargo test -p ruff_db`


---

_Label `red-knot` added by @AlexWaygood on 2024-06-23 15:06_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-23 15:06_

---

_Comment by @github-actions[bot] on 2024-06-23 15:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-06-23 15:53_

I think my preferred solution here would be to improve the normalization logic to avoid allocating if the path's already normalized. But I'm not opposed to do the normalization eagerly.

If we so the normalization eagerly, than I don't think the Path variant still makes sense, we should just use &PathBuf

What's unclear to me is how the normalization works when joining paths because we would then have to normalize both paths before we can join them (which includes an allocation)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored/path.rs`:22 on 2024-06-23 15:55_

I would keep the components naming because it's more familiar 

---

_@MichaReiser reviewed on 2024-06-23 15:56_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored/path.rs`:25 on 2024-06-23 16:38_

This will yield an incorrect result if you have ../ and normalized_parts is empty 

---

_Comment by @MichaReiser on 2024-06-23 16:47_

Reading through this more, I'm leaning towards keeping it as is because it makes the API more cumbersome to use without very convincing benefits. 

> The path is only "dynamically" checked when you actually try to use it. If the path is invalid, we should probably validate it and normalize it at the point where it's constructed, s

It's unclear to me if we actually do want to do this. I don't think there's anything wrong with `VendoredPath::new("root/sub").join("../other")` where `../other` would no longer be a valid `VendoredPath` (because it starts with a `../`)

> Every time you query whether a path exists (or query some other metadata about the path) in the zip archive, the normalization has to allocate a new String.

I agree that this is not ideal but I think we can instead change the normalization to return a `Cow` and only allocate if the path isn't normalized. 

I also expect that this won't be a very hot path because module resolution is cached. So we only resolve every module once. 


---

_@MichaReiser reviewed on 2024-06-23 16:47_

---

_@AlexWaygood reviewed on 2024-06-23 18:40_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored/path.rs`:25 on 2024-06-23 18:40_

Good point. Fixed in https://github.com/astral-sh/ruff/pull/11989/commits/0775bc715c1b905c265e496ea74e765e11bccc89.

---

_Comment by @AlexWaygood on 2024-06-23 18:45_

> Reading through this more, I'm leaning towards keeping it as is because it makes the API more cumbersome to use without very convincing benefits.

Yeah, I am also unsure. But I think part of the issue is that the current implementation on `main` isn't very principled: it panics if it encounters an unnormalized path, when it should probably return an error instead. https://github.com/astral-sh/ruff/pull/11991 is an alternative to this PR that tries to do the normalization in a more principled way, and it also makes the APIs more cumbersome to use.

> It's unclear to me if we actually do want to do this. I don't think there's anything wrong with `VendoredPath::new("root/sub").join("../other")` where `../other` would no longer be a valid `VendoredPath` (because it starts with a `../`)

Hmm, that's an interesting point. We could possibly have something like a `join_str()` method instead, that allows you to join a fragment to this path even if the fragment itself is not a valid path. But I agree that that's not ideal...

---

_Comment by @codspeed-hq[bot] on 2024-06-23 18:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/eagerly-normalize-vendored-paths)

### Merging #11989 will **improve performances by 4.92%**

<sub>Comparing <code>alex/eagerly-normalize-vendored-paths</code> (4bd1fe8) with <code>main</code> (068b75c)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `alex/eagerly-normalize-vendored-paths` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.92% |


---

_Comment by @AlexWaygood on 2024-06-28 12:46_

Leaving this for the time being

---

_Closed by @AlexWaygood on 2024-06-28 12:46_

---

_Branch deleted on 2024-06-28 12:46_

---
