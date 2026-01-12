```yaml
number: 9444
title: Encode mutually-incompatible pairs of markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - enhancement
assignees: []
merged: true
base: main
head: charlie/dnf
created_at: 2024-11-26T15:54:12Z
updated_at: 2024-12-07T01:51:45Z
url: https://github.com/astral-sh/uv/pull/9444
synced_at: 2026-01-12T16:08:48Z
```

# Encode mutually-incompatible pairs of markers

---

_@charliermarsh_

## Summary

This is an alternative to #9344. If accepted, I need to audit the codebase and call sites to apply it everywhere, but the basic idea is: rather than encoding mutually-incompatible pairs of markers in the representation itself, we have an additional method on `MarkerTree` that expands the false-y definition to take into account assumptions about which markers can be true alongside others. We then check if the the current marker implies that at least one of them is true.

So, for example, we know that `sys_platform == 'win32'` and `platform_system == 'Darwin'` are mutually exclusive. When given a marker expression like `python_version >= '3.7'`, we test if `python_version >= '3.7'` and `sys_platform != 'win32' or platform_system != 'Darwin'` are disjoint, i.e., if the following can't be satisfied:

```
python_version >= '3.7' and (sys_platform != 'win32' or platform_system != 'Darwin')
```

Since, if this can't be satisfied, it implies that the left-hand expression requires `sys_platform == 'win32'` and `platform_system == 'Darwin'` to be true at the same time.

I think the main downsides here are:

1. We can't _simplify_ markers based on these implications. So we'd still write markers like `sys_platform == 'win32' and platform_system != 'Darwin'`, even though we know the latter expression is redundant.
2. It might be expensive? I'm not sure. I don't think we test for falseness _that_ often though.

Closes #7760.
Closes #9275.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-26 15:54_

---

_Review requested from @konstin by @charliermarsh on 2024-11-26 15:54_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-11-26 15:54_

---

_Label `bug` added by @charliermarsh on 2024-11-26 15:54_

---

_Label `enhancement` added by @charliermarsh on 2024-11-26 15:54_

---

_@charliermarsh reviewed on 2024-11-26 15:54_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/tree.rs`:845 on 2024-11-26 15:54_

On the bright side, this is highly flexible so we can encode more known incompatibilities, like this.

---

_@charliermarsh reviewed on 2024-11-26 15:55_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/environment.rs`:497 on 2024-11-26 15:55_

I need to do this in more places.

---

_@charliermarsh reviewed on 2024-11-26 15:59_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/environment.rs`:497 on 2024-11-26 15:59_

It might be a pain... Right now, `env_marker.is_disjoint(&not_marker)` returns `true` if either of the expressions are false, but that doesn't include the `is_conflicting` definition here -- it only looks at the `FALSE` node.

---

_@konstin reviewed on 2024-11-26 16:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:497 on 2024-11-26 16:35_

You could make `is_conflicting` into `simplify_conflicting(...) -> MarkerTree` that returns a FALSE node if it matched one of the impossible cases, that might simplify

---

_@charliermarsh reviewed on 2024-11-26 17:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/environment.rs`:497 on 2024-11-26 17:03_

Do you think the approach makes sense though?

---

_@konstin approved on 2024-11-26 19:58_

---

_@konstin reviewed on 2024-11-26 20:01_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:497 on 2024-11-26 20:01_

we'd have to try it out, but it does sound more elegant in theory

---

_@konstin reviewed on 2024-11-26 20:02_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:861 on 2024-11-26 20:02_

We may want to cache those, @ibraheemdev will know the perf characteristics

---

_@BurntSushi reviewed on 2024-11-27 15:45_

I think the idea here is sound.

My main concern is probably what you might expect: that you have to remember to call this in a bunch of places around the code. I suppose the mitigation to that is that this isn't a correctness issue but a simplification issue.

Is there any way to do this simplification via `MarkerTree`'s smart constructors? Before @ibraheemdev's work, we had this problem of markers possibly being simplified or not, and we'd call the simplification routine in various spots. But now we have a nice guarantee that markers are always simplified, as guaranteed by the smart constructors. Maybe we can do that in our `MarkerTree::and` and `MarkerTree::or` methods?

---

_Comment by @charliermarsh on 2024-11-27 16:00_

Yeah I like that idea.

---

_@konstin reviewed on 2024-11-27 16:28_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:497 on 2024-11-27 16:28_

andrew's idea is even better than that

---

_Comment by @ibraheemdev on 2024-11-27 23:29_

I wonder if you could even define `MarkerTree::TRUE` and `MarkerTree::FALSE` to be the set of known impossible states.

---

_@ibraheemdev reviewed on 2024-11-27 23:30_

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/tree.rs`:861 on 2024-11-27 23:30_

This is in a `OnceCell` so it should be good.

---

_Comment by @charliermarsh on 2024-11-28 01:14_

Ok, the version I pushed does this in the smart constructors...

---

_@charliermarsh reviewed on 2024-11-28 01:15_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/tree.rs`:719 on 2024-11-28 01:15_

Is it better to hold the lock and re-use it in `simplify`, or re-`lock()` as I'm doing here?

---

_@ibraheemdev reviewed on 2024-11-28 02:33_

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/tree.rs`:719 on 2024-11-28 02:33_

Probably re-use it, these are pretty much only used by a single thread.

---

_Comment by @charliermarsh on 2024-11-28 03:07_

I think what I have here works, but I'm worried about the performance costs and complexity.

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:734 on 2024-11-28 08:42_

Took me a while to understand that this is `is_disjoint` and not `!is_disjoint` because `MUTUAL_EXCLUSIONS` is the negated exclusions, so it's the valid space.

![image](https://github.com/user-attachments/assets/28d16937-db91-4603-b8a2-dafce9ea9b8c)

Plus when reading the code you need to consider we call this every time while building the marker, so when only B from `A or B` (we previously saw in a lockfile) lies in the excluded area, we still get A because B was converted to FALSE upon construction.

---

_@konstin reviewed on 2024-11-28 08:42_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/tree.rs`:734 on 2024-11-28 14:24_

Should I be changing something in response to this comment? :)

---

_@charliermarsh reviewed on 2024-11-28 14:24_

---

_@konstin reviewed on 2024-11-28 14:48_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:734 on 2024-11-28 14:48_

Could you add an inline comment like:

> `MUTUAL_EXCLUSIONS` is the possible space, if the markers are `is_disjoint` with it, they are solely in the impossible space.

---

_@konstin reviewed on 2024-11-28 14:49_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:734 on 2024-11-28 14:49_

(I'll leave the smart constructor review to andrew/ibraheem)

---

_Comment by @charliermarsh on 2024-11-29 18:58_

Ok, I think _this_ version should have a minimal performance hit. I moved the logic into the algebra itself, and we now only perform this check when merging nodes that could yield a conflict (i.e., two marker trees that contain variables that could conflict). Per @ibraheemdev suggestion, I made those variables "highest-priority", so if they aren't present at the top of the marker tree, we know they aren't present anywhere beneath it.

---

_Review requested from @konstin by @charliermarsh on 2024-11-29 18:58_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/algebra.rs`:818 on 2024-11-29 19:04_

I need the above methods because I need versions that _don't_ recursively call `.exclusions()`. If I just use `and` and `or` here, I create an infinite loop.

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/algebra.rs`:822 on 2024-11-29 19:04_

I assume it's fine to just use an `Option` here rather than a `OnceCell`?

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/algebra.rs`:461 on 2024-11-29 19:06_

Not a huge fan of having this entirely separate method. But it's equivalent to `is_disjoint`, except it doesn't do the mutually-incompatible marker check. If it _did_, we'd create an infinite loop, since `is_disjoint` calls `and` in that case which then calls `disjointness`.

---

_@charliermarsh reviewed on 2024-11-29 19:06_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-29 19:07_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/algebra.rs`:339 on 2024-11-29 19:24_

The idea is to give the "possibly-conflicting" markers highest priority, so they're always at the top of the tree.

As such, when we AND two expressions, we only have to perform the "possibly-conflicting" check if they both have a conflicting variable at the very top.

---

_@charliermarsh reviewed on 2024-11-29 19:24_

---

_Comment by @charliermarsh on 2024-12-01 14:26_

Ok, I added some sorting to the DNF to minimize the amount of churn in the expressions.

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/tree.rs`:499 on 2024-12-03 13:27_

Is this _only_ used for backcompat sorting? If so, it might be useful to add a comment indicating as such. And that the order here matters?

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:461 on 2024-12-03 13:48_

Yeah I feel like a little copying here is probably the way to go. I don't see a simple way of unifying them without making it more obscure.

Might be worth moving this comment into the code, for future wranglers wondering about it.

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:818 on 2024-12-03 13:51_

Makes sense.

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/algebra.rs`:822 on 2024-12-03 13:51_

Yeah. Rust wouldn't let you mess this up.

---

_@BurntSushi approved on 2024-12-03 13:52_

I think this makes sense to me. And I like that this doesn't require any sort of knowledge on the callers part to explicitly simplify the markers. Nice work. :-)

---

_Merged by @charliermarsh on 2024-12-07 01:51_

---

_Closed by @charliermarsh on 2024-12-07 01:51_

---

_Branch deleted on 2024-12-07 01:51_

---
