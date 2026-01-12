```yaml
number: 10583
title: "Reduce `WheelFilename` to 48 bytes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/shrink-tags-more
created_at: 2025-01-14T03:25:12Z
updated_at: 2025-01-14T14:49:19Z
url: https://github.com/astral-sh/uv/pull/10583
synced_at: 2026-01-12T16:09:22Z
```

# Reduce `WheelFilename` to 48 bytes

---

_@charliermarsh_

## Summary

Based on some advice from @konstin.

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-14 03:25_

---

_Review requested from @konstin by @charliermarsh on 2025-01-14 03:25_

---

_Label `performance` added by @charliermarsh on 2025-01-14 03:25_

---

_@charliermarsh reviewed on 2025-01-14 03:25_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:33 on 2025-01-14 03:25_

Not `pub`! Fully encapsulated.

---

_@charliermarsh reviewed on 2025-01-14 03:25_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:321 on 2025-01-14 03:25_

Not sure if it's still worth doing this. Debatable. (Only used in the large variant now, which is boxed anyway.)

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:223 on 2025-01-14 03:26_

This is a bit wasteful -- we should instead use an iterator, call `.next()`, and then check if the _next_ `.next()` is `None` for all of them. But that's a lot more code, unclear if it's really worth it.

---

_@charliermarsh reviewed on 2025-01-14 03:26_

---

_Comment by @codspeed-hq[bot] on 2025-01-14 03:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fshrink-tags-more)

### Merging #10583 will **improve performances by 46.73%**

<sub>Comparing <code>charlie/shrink-tags-more</code> (bfabd69) with <code>main</code> (faa4481)</sub>



### Summary

`⚡ 2` improvements  
`✅ 12` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/shrink-tags-more` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing[flyte-short-compatible] `` | 7.7 µs | 5.2 µs | +46.73% |
| ⚡ | `` wheelname_parsing[flyte-short-incompatible] `` | 7.7 µs | 5.3 µs | +46.01% |


---

_Review comment by @konstin on `crates/uv-distribution-filename/src/wheel.rs`:227 on 2025-01-14 07:40_

Fwiw if there comes a case in the future where we want to pass around the compatibility tag separately from the wheel filename, we have to grow the `WheelFilename` a bit again by moving the build tag back in (but for now, we don't have that use case)

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/wheel.rs`:321 on 2025-01-14 07:42_

I'm pro-smallvec, for small types it has always been faster in the cases i tried, and the codepath is allocation-heavy.

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/wheel.rs`:330 on 2025-01-14 07:45_

```suggestion
/// A wheel filename after name and version: python tag, abi tag and platform tag, and build tag.
///
/// Most wheels have a single python tag, abi tag and platform tag, and no build tag, which are represented in the small
/// variant with a smaller memory footprint and less allocations, while the large variant can represent all legal wheel
/// tags
#[derive(
```

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/wheel.rs`:412 on 2025-01-14 07:46_

The build tag is missing

---

_Review comment by @konstin on `crates/uv-distribution-types/src/lib.rs`:1347 on 2025-01-14 07:47_

First time the build dist is smaller than the source dist!

---

_@konstin approved on 2025-01-14 07:48_

---

_Review comment by @BurntSushi on `crates/uv-distribution-filename/src/wheel.rs`:33 on 2025-01-14 13:03_

:exclamation: 

---

_Review comment by @BurntSushi on `crates/uv-distribution-filename/src/wheel.rs`:115 on 2025-01-14 13:04_

I love this function. One of my favorites in all of `std`.

---

_Review comment by @BurntSushi on `crates/uv-distribution-filename/src/wheel.rs`:223 on 2025-01-14 13:06_

Interesting. I would naively assume that it would matter, since it will be running for what looks like every wheel filename parse? Or is there some logic here that results in a short circuit in most cases?

---

_@BurntSushi approved on 2025-01-14 13:08_

LGTM. I like this style of optimization!

---

_@charliermarsh reviewed on 2025-01-14 13:46_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:223 on 2025-01-14 13:46_

Well, it doesn't matter for _small_ tags, right? Since we have to check this regardless? But it does matter for _large_ tags, which is already "expensive".

---

_@charliermarsh reviewed on 2025-01-14 13:46_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:115 on 2025-01-14 13:46_

So so good.

---

_@BurntSushi reviewed on 2025-01-14 13:52_

---

_Review comment by @BurntSushi on `crates/uv-distribution-filename/src/wheel.rs`:223 on 2025-01-14 13:52_

Yeaaaaah, okay, I see what you're saying now. I agree that this is fine.

I think the only possible improvement here would be to bake this conditional logic into the parsing of tags itself. Something like, "parse the first tag and tell me if it looks like there are more tags to parse" _without_ first splitting on `.`. But I'm not even sure that would be a win, and it makes the parsing more complicated.

---

_@charliermarsh reviewed on 2025-01-14 14:36_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:412 on 2025-01-14 14:36_

We already omit it actually. I'll look at it separately.

---

_Merged by @charliermarsh on 2025-01-14 14:49_

---

_Closed by @charliermarsh on 2025-01-14 14:49_

---

_Branch deleted on 2025-01-14 14:49_

---
