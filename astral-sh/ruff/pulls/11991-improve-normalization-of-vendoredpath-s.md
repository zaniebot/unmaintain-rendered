```yaml
number: 11991
title: "Improve normalization of `VendoredPath`s"
type: pull_request
state: closed
author: AlexWaygood
labels: []
assignees: []
base: main
head: vendored-parts-parent-part
created_at: 2024-06-23T18:21:34Z
updated_at: 2024-06-23T20:39:30Z
url: https://github.com/astral-sh/ruff/pull/11991
synced_at: 2026-01-10T21:56:00Z
```

# Improve normalization of `VendoredPath`s

---

_Pull request opened by @AlexWaygood on 2024-06-23 18:21_

## Summary

This is a competing PR to #11989. Instead of making `VendoredPath` normalization eager, it makes the lazy normalization that we currently do more principled:
- the normalization function checks for `..` path components that attempt to "escape" from the zip archive altogether (e.g. `VendoredPath("..")` should be rejected)
- the normalization function returns a `Result` instead of panicking if the `VendoredPath` is invalid.

## Test Plan

`cargo test -p ruff_db`


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-23 18:21_

---

_Comment by @codspeed-hq[bot] on 2024-06-23 18:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/vendored-parts-parent-part)

### Merging #11991 will **improve performances by 4.89%**

<sub>Comparing <code>vendored-parts-parent-part</code> (5a18ac8) with <code>main</code> (068b75c)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `vendored-parts-parent-part` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +4.89% |


---

_Comment by @github-actions[bot] on 2024-06-23 18:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:131 on 2024-06-23 18:57_

Dealing with different errors can often times be more difficult than just having one error, even if it contains more variant than are actually possible. 

For example, someone calling `metdata` and `read` now needs to handle both `MetadataLookupError` and `ZipFileReadError` but there's no way to convert one error to the other. It forces the caller to define its own error type that is a union over the two.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:304 on 2024-06-23 19:01_

I quickly checked what the normal FS operations return when you navigate outside the `cwd`. They just return a `NotFound` error, which is probably also enough for us. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:309 on 2024-06-23 19:05_

Am I correct that this is only to handle prefixes? I guess the problem we run into here is that `camino::Utf8Path` path parsing is platform-dependent. I'm not sure how to solve this best but I'm also not sure if this special case is worth returning an `Error` (we control the path creation, it's not that they're provided from externally) 

---

_@MichaReiser reviewed on 2024-06-23 19:10_

I'm not sure if the many extra error types are worth the complexity, especially considering that vendored paths isn't something that comes from externally. 

I generally like narrow error types, but they can be challenging to work with. `metadata` and `read` now return two different, disjoint errors. This can be frustrating because a function that calls both `read` and `metadata` now must define its own error type (or use `anyhow`) to propagate both errors, but that's as good as just using `io::Error`.

The other question is. Is it useful to know that reading a file failed because the path was invalid or because there's an other IO error? Would a caller handle these two cases differently? I would say probably not? I would probably call `unwrap` for invalid paths because I know they are well formed (we control the construction) and there's nothing I can do about IO errors other than propagating them.

I'm leaning towards returning `NotFound` if a path has too many `../` components. Prefixes are a bit more awkward. I'm leaning toward just panicking, because this is a dev error. 

 The alternative is to handle prefixes similar to `root` where you take the starting prefix (win only) and concatenate it with the next component to get "UNIX" like semantics. 

---

_Comment by @MichaReiser on 2024-06-23 19:26_

TIL: This is what `std::path::absolute` does

```rust
if path.as_os_str().is_empty() {
        Err(io::const_io_error!(io::ErrorKind::InvalidInput, "cannot make an empty path absolute",))
    }
```

Uh, this is funny. Too many `../` hasn't any meaning. It just means the path starts at `/`.

```rust
assert_eq!(
            std::path::Path::new("a"),
            std::path::Path::new(
                "../../../../../../../../../../../../../../../../../../../../../../home"
            )
            .canonicalize()
            .unwrap()
        );
```

Fails, and the path resolves to `"/System/Volumes/Data/home"`. I can add as many `../` as I want and the path keeps resolving to `/home`. So I think you're implementation was actually correct. 

Taking all this into consideration. I would return `io::ErrorKind::InvalidInput` when seeing a `Prefix` component

---

_Comment by @AlexWaygood on 2024-06-23 20:31_

> Uh, this is funny. Too many `../` hasn't any meaning. It just means the path starts at `/`.

Huh. Right, yeah, that makes sense. We're not trying to "escape from the zip archive"; we've just reached the root of the zip archive, and we can't go any further.

---

_@AlexWaygood reviewed on 2024-06-23 20:35_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:309 on 2024-06-23 20:35_

Or a `camino::Utf8Component::RootDir`... which I don't think is possible in the middle of a file, but that can't be validated using the type system

---

_Comment by @AlexWaygood on 2024-06-23 20:39_

I think the conclusion here is that none of the changes here are needed:
- Panicking is the correct thing to do if we encounter prefixes in the normalization routine. We control the inputs, so that just indicates a dev error on our part if we come across them, and the correct response to that is to panic rather than return an error
- `..` parts should be treated the same way as in OS paths, and for OS paths Rust allows you to apply an arbitrary number and doesn't error out (they're just treated as no-ops if you hit the root dir)

Thanks for talking this through with me, I appreciate it!

---

_Closed by @AlexWaygood on 2024-06-23 20:39_

---

_Branch deleted on 2024-06-23 20:39_

---
