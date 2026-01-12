```yaml
number: 3781
title: Incorporate build tag into wheel prioritization
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/build-tag
created_at: 2024-05-23T00:52:17Z
updated_at: 2024-05-23T21:12:54Z
url: https://github.com/astral-sh/uv/pull/3781
synced_at: 2026-01-12T16:05:51Z
```

# Incorporate build tag into wheel prioritization

---

_@charliermarsh_

## Summary

It turns out that in the [spec](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#file-name-convention), if a wheel filename includes a build tag, then we need to use it to break ties. This PR implements that behavior. (Previously, we dropped the build tag entirely.)

Closes #3779.

## Test Plan

Run: `cargo run pip install -i https://pypi.anaconda.org/intel/simple mkl_fft==1.3.8 --python-platform linux --python-version 3.10`. This now resolves without error. Previously, we selected build tag 63 of `mkl_fft==1.3.8`, which led to an incompatibility with NumPy. Now, we select build tag 70.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-23 00:52_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-23 00:52_

---

_Label `bug` added by @charliermarsh on 2024-05-23 00:52_

---

_Label `compatibility` added by @charliermarsh on 2024-05-23 00:52_

---

_Comment by @codspeed-hq[bot] on 2024-05-23 00:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/build-tag)

### Merging #3781 will **degrade performances by 6.1%**

<sub>Comparing <code>charlie/build-tag</code> (1a5ee74) with <code>main</code> (5bebadd)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 10` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/build-tag)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/build-tag` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_parsing_failure[flyte-long-extension]` | 1.8 µs | 1.9 µs | -6.1% |
| ⚡ | `wheelname_tag_compatibility[flyte-long-incompatible]` | 1.5 µs | 1.5 µs | +6% |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 926.7 ns | 868.3 ns | +6.72% |


---

_Review comment by @charliermarsh on `crates/distribution-filename/src/build_tag.rs`:36 on 2024-05-23 01:01_

Is there a more compact representation for this?

---

_@charliermarsh reviewed on 2024-05-23 01:01_

---

_Review comment by @BurntSushi on `crates/uv-cache/src/lib.rs`:617 on 2024-05-23 11:23_

Nice. I was looking for this!

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/wheel.rs`:86 on 2024-05-23 11:24_

Does this not need the build tag somewhere in the formatted string?

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/build_tag.rs`:36 on 2024-05-23 11:41_

Yes. Using `Option<Arc<str>>` here will use one fewer words of memory (24 bytes instead of 32 on 64-bit targets). You could use `Box<str>`, but I would go with `Arc` because I noticed this is being cloned in `version_map`.

Getting more compact than that I think would look more like how we deal with `Version`: optimize some set of common cases into a more compact form and then use a bigger representation for "everything else." But this relies on being able to identify the common cases _and_ the common cases being subject to optimization.

For example, maybe 95% of all build tags have a number less than 256 and no more than 7 bytes of suffix. Those could all be represented by `[u8; 8]`. So something like this:

```rust
pub struct BuildTag(Arc<BuildTagInner>);

enum BuildTagInner {
    Compact([u8; 8]),
    General(u32, Option<Arc<str>>),
}
```

You'd probably want to use a bit somewhere in the `[u8; 8]` to indicate whether the suffix is present or not.

But the problem here is that the size of `BuildTagInner` is still the size of its biggest variant. So, you wouldn't really be saving any heap memory here. And I'm not sure you get much from the compact representation unless it can make comparisons substantially faster in a place where it matters.

However, the heap indirection here does reduce the size of `BuildTag` itself to one word of memory. Maybe that's worth it to make `WheelFilename` overall smaller. So, something like this:

```rust
pub struct BuildTag(Arc<BuildTagInner>);

struct BuildTagInner(u32, Option<Box<str>>);
```

But, this means every build tag necessarily requires an allocation, where as the representation you have now only does an alloc when there is a non-empty suffix.

Unless we have some data pointing at this as a problem, I'd just switch `Option<String>` to `Option<Arc<str>>` and call that good enough for now.

---

_@BurntSushi approved on 2024-05-23 11:42_

Nice!

---

_@charliermarsh reviewed on 2024-05-23 21:07_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/build_tag.rs`:36 on 2024-05-23 21:07_

Sounds good. The vast majority of wheels have no build tag, and of those that do, I've only ever seen them be numerical, so good not to allocate there. (But the spec says they _can_ be non-numerical, hence this dance.)

---

_Merged by @charliermarsh on 2024-05-23 21:12_

---

_Closed by @charliermarsh on 2024-05-23 21:12_

---

_Branch deleted on 2024-05-23 21:12_

---
