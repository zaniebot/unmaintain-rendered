```yaml
number: 10345
title: "Remove `[u64; 4]` from small version to move `Arc` to full version"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/opt-small-version-arc
created_at: 2025-01-07T09:30:14Z
updated_at: 2025-01-07T14:25:33Z
url: https://github.com/astral-sh/uv/pull/10345
synced_at: 2026-01-10T11:44:44Z
```

# Remove `[u64; 4]` from small version to move `Arc` to full version

---

_Pull request opened by @konstin on 2025-01-07 09:30_

Ref #10344

Cloning and dropping the version arc took a significant fraction of the time in the resolver, which is a large overhead especially for the small variant that has only 9 bytes payload.

When moving the `Arc` to only apply to the full variant, the small variant is too large because it stores a `[u64; 4]` to have a release accessor a `&[u64]` that's shared with the `Vec<u64>` of the full variant. We proxy this by first extracting the compressed version digits of the small variant to a proxy type that stores up to 4 u64 on the stack and can be deref'ed to the existing `&[u64]`, minimizing churn.

---

_Review requested from @BurntSushi by @konstin on 2025-01-07 09:30_

---

_Label `performance` added by @konstin on 2025-01-07 09:31_

---

_Comment by @codspeed-hq[bot] on 2025-01-07 09:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti%2Fopt-small-version-arc)

### Merging #10345 will **improve performances by 29.06%**

<sub>Comparing <code>konsti/opt-small-version-arc</code> (c09adcf) with <code>main</code> (14a9008)</sub>



### Summary

`⚡ 3` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/opt-small-version-arc` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` resolve_warm_airflow `` | 1,039.2 ms | 841.5 ms | +23.48% |
| ⚡ | `` resolve_warm_jupyter `` | 95 ms | 73.6 ms | +29.06% |
| ⚡ | `` resolve_warm_jupyter_universal `` | 232.7 ms | 209.6 ms | +11.02% |


---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version.rs`:1424 on 2025-01-07 13:17_

Since this is part of the public API, I'd suggest keeping the first sentence to one line, and then adding more (if necessary) in a paragraph below it. Otherwise, this will show up as an overlong summary in the type listing.

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version.rs`:1439 on 2025-01-07 13:25_

I was wondering if there was a more compact representation than this, so I tried:

```rust
enum ReleaseInner<'a> {
    Small { numbers: [u64; 4], len: u8 },
    Full(&'a [u64]),
}
```

But indeed, it's the same size (which makes sense).

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version.rs`:941 on 2025-01-07 13:27_

Oh man, if we could get rid of `len` here, this type would decrease in size by half I believe. IDK if that would make an actual difference perf-wise... And I think the only way to do it would be to take bits away from `repr`, which in turn means moving more cases to the `Full` representation. Which might be a net negative overall.

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version.rs`:379 on 2025-01-07 13:28_

Octal!?!?! -\_- >\_< o_0 Haha

---

_@BurntSushi approved on 2025-01-07 13:29_

This is awesome! I love it.

---

_@konstin reviewed on 2025-01-07 13:48_

---

_Review comment by @konstin on `crates/uv-pep440/src/version.rs`:941 on 2025-01-07 13:48_

We could get rid of len by counting the zeroes ourselves. The problem is that `Version` doesn't become 64 byte since the `Arc` has only a single niche (`NonNull`) in rustc's view, so both variants don't fit into a 64-bit word.

We could try a tagged pointer library, or we could lean into it and make the small representation `[u8; 11]` or `[u8; 15]` to use the space we're already allocating while leaving a bit (byte) for rustc for the enum discriminant.

This problem MRE:

```rust
use std::sync::Arc;

enum A {
    Small(()),
    Large(Arc<Vec<u8>>),
}

enum B {
    Small(bool),
    Large(Arc<Vec<u8>>),
}

enum C {
    Small([u8; 7]),
    Large(Arc<Vec<u8>>),
}

fn main() {
    println!("{}", size_of::<A>()); // -> 8
    println!("{}", size_of::<B>()); // -> 16, but can it be 8 please?
    println!("{}", size_of::<C>()); // -> 16, but can it be 8 please?
}
```

---

_Review comment by @konstin on `crates/uv-pep440/src/version.rs`:941 on 2025-01-07 14:08_

Another option would be stealing two bits somewhere else (fourth release number, or capping the first release number to 14 bits).

---

_@konstin reviewed on 2025-01-07 14:08_

---

_@BurntSushi reviewed on 2025-01-07 14:10_

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version.rs`:941 on 2025-01-07 14:10_

> We could get rid of len by counting the zeroes ourselves. 

Hmmm, but then I think you might lose the current property that trailing zeros are preserved. See: #10362.

Otherwise yeah, I see. I agree that going further probably means a tagged pointer of some kind.

A less thought out idea is a global interner like we use in `uv-pep508` for markers. But whether that hits the right trade-offs is not at all clear to me. It could let you get a `Version` down to 32-bits pretty easily I think, but then pretty much every interaction with it would require consulting the global interner table. And that will probably destroy any benefits you might otherwise get.

---

_Merged by @konstin on 2025-01-07 14:25_

---

_Closed by @konstin on 2025-01-07 14:25_

---

_Branch deleted on 2025-01-07 14:25_

---
