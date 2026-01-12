```yaml
number: 6388
title: "sort dependencies in `pyproject.toml`"
type: pull_request
state: merged
author: ChannyClaus
labels: []
assignees: []
merged: true
base: main
head: sort-dependency
created_at: 2024-08-21T23:11:47Z
updated_at: 2024-08-29T14:55:04Z
url: https://github.com/astral-sh/uv/pull/6388
synced_at: 2026-01-12T16:07:21Z
```

# sort dependencies in `pyproject.toml`

---

_@ChannyClaus_

## Summary
resolves https://github.com/astral-sh/uv/issues/6203

## Test Plan
added a test fixing the bug described in the issue.

---

_Renamed from "Sort dependency" to "sort dependencies after `uv add`" by @ChannyClaus on 2024-08-21 23:12_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-21 23:14_

seemed like this was going to be the cleanest way to get this done. while it can be wasteful (we may end up sorting every time we add a package if `uv add` invocation adds multiple packages?), i imagine most invocations of `uv add` will:
1. add only a few dependencies manually
3. sorting is not expensive with the number of dependencies being sorted here
(probably?)

---

_@ChannyClaus reviewed on 2024-08-21 23:15_

---

_Converted to draft by @ChannyClaus on 2024-08-21 23:17_

---

_Comment by @ChannyClaus on 2024-08-21 23:17_

fixing CI

---

_@ChannyClaus reviewed on 2024-08-22 01:49_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-22 01:49_

actually realized this probably isn't the correct fix since we probably should preserve the sorted order on removal as well.

will fix (maybe this weekend)

---

_Renamed from "sort dependencies after `uv add`" to "sort dependencies in `pyproject.toml`" by @ChannyClaus on 2024-08-22 02:40_

---

_@konstin reviewed on 2024-08-22 09:35_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-22 09:35_

We should only do this when the dependencies are already sorted, if the user has a custom sorting we shouldn't change that and just add at the end. So the idea would be that we first check if the list is sorted, and if it is, scan for the insertion point. 

---

_@ChannyClaus reviewed on 2024-08-22 14:47_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-22 14:47_

ahh good call.

---

_@ChannyClaus reviewed on 2024-08-25 22:44_

---

_Review comment by @ChannyClaus on `crates/uv/tests/edit.rs`:4513 on 2024-08-25 22:44_

wanted to test the case where there's an empty line between dependencies (and it being preserved after `uv add`) but `uv add` seems to remove it currently - should probably be done as a separate PR?

---

_Marked ready for review by @ChannyClaus on 2024-08-26 03:28_

---

_@ChannyClaus reviewed on 2024-08-26 03:30_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:535 on 2024-08-26 03:30_

not 100% sure if it is correct to omit the comparison for `marker` here

---

_Review requested from @konstin by @ChannyClaus on 2024-08-26 03:30_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:519 on 2024-08-26 09:23_

This should only pass when all values are strings. If they aren't the table is invalid; We could error, but for simplicity here it makes more sense to treat invalid files as unsorted, add our requirement to the end of the list and let it fail later when we try to use the pyproject.toml

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-26 09:24_

For the sorted case: Since we don't parse change already requirements, we shouldn't deserialize and then `to_string()` them. Instead, we should convert the new requirement to a string and use this as comparison, finding the index with `partition_point` .

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:535 on 2024-08-26 09:26_

If the deps aren't sorted, we should just insert at the end

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:550 on 2024-08-26 09:27_

We don't need to assign this to a temporary variable (i think there's a clippy lint against this?)

---

_@konstin reviewed on 2024-08-26 09:27_

---

_@konstin reviewed on 2024-08-26 12:49_

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:4037 on 2024-08-26 12:49_

We should avoid syncing here, otherwise the test will be slow.

---

_@ChannyClaus reviewed on 2024-08-26 14:27_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:550 on 2024-08-26 14:27_

> If the deps aren't sorted, we should just insert at the end

i believe the `index` points to the last position if the deps aren't sorted (one of the tests also covers this case)

---

_@ChannyClaus reviewed on 2024-08-26 15:37_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-26 15:37_

let me look into this, it's slightly tricky since `.to_string()` on each of the dependencies yields strings of the form `\n    \"CacheControl[filecache]>=0.14,<0.15\"`; there may be a way to strip those relatively easily.

---

_@konstin reviewed on 2024-08-26 15:58_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-26 15:58_

There is a method in the `toml` crate that allows you to get the contained string value, instead of the string representation of the parsed field with decoration that `.to_string()` seems to use.

---

_Assigned to @konstin by @zanieb on 2024-08-26 18:09_

---

_@ChannyClaus reviewed on 2024-08-27 02:21_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-27 02:21_

trimming the empty space + `\n` turned out to be pretty straight-forward but the formatting part is turning out to be trickier...
```
        Value(
            String(
                Formatted {
                    value: "sentry-sdk",
                    repr: "\"sentry-sdk\"",
                    decor: Decor {
                        prefix: "\n    ",
                        suffix: empty,
                    },
                },
            ),
        ),
        Value(
            String(
                Formatted {
                    value: "pydantic",
                    repr: "default",
                    decor: Decor {
                        prefix: " ",
                        suffix: empty,
                    },
                },
            ),
        ),
```
where `reformat_array_multiline` seems to be operating under the assumption that only the last value of `deps` has an empty prefix.
```
            deps.get_mut(deps.len() - 1)
                .unwrap()
                .decor_mut()
                .set_prefix("\n    ");
```
it's possible to go with the partition solution still if we set the prefix manually for the inserted value and the last value, but it may be cleaner to just keep the current implementation?

---

_@konstin reviewed on 2024-08-27 08:16_

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-27 08:16_

In this case we should update `reformat_array_multiline`. I'm confident that we should sort just by `d.as_str()` (without parsing and serializing), since this is the value that the user sees in the file.

---

_@ChannyClaus reviewed on 2024-08-27 14:37_

---

_Review comment by @ChannyClaus on `crates/uv-workspace/src/pyproject_mut.rs`:524 on 2024-08-27 14:37_

sounds good, i'll take a look again sometime.

---

_Comment by @ChannyClaus on 2024-08-27 22:20_

alright i think it's ready for another look @konstin 

---

_@ChannyClaus reviewed on 2024-08-27 22:53_

---

_Review comment by @ChannyClaus on `crates/uv/tests/edit.rs`:4513 on 2024-08-27 22:53_

https://github.com/astral-sh/uv/issues/6727? not sure if it's worth it, though, since it's strictly just a formatting nit.

---

_Review comment by @konstin on `crates/uv/tests/edit.rs`:4513 on 2024-08-29 14:19_

Yeah #6727 sounds like a good candidate for a separate PR 

---

_@konstin reviewed on 2024-08-29 14:19_

---

_@konstin approved on 2024-08-29 14:26_

---

_Merged by @konstin on 2024-08-29 14:55_

---

_Closed by @konstin on 2024-08-29 14:55_

---
