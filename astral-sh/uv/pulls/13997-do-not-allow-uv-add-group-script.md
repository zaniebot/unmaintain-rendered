```yaml
number: 13997
title: "Do not allow `uv add --group ... --script`"
type: pull_request
state: merged
author: blueraft
labels: []
assignees: []
merged: true
base: main
head: do-not-allow-add-groups
created_at: 2025-06-12T13:57:14Z
updated_at: 2025-06-12T14:48:30Z
url: https://github.com/astral-sh/uv/pull/13997
synced_at: 2026-01-10T11:10:42Z
```

# Do not allow `uv add --group ... --script`

---

_Pull request opened by @blueraft on 2025-06-12 13:57_

## Summary

Closes #13988 

## Test Plan

`cargo test`

---

_Review requested from @Gankra by @Gankra on 2025-06-12 14:02_

---

_Assigned to @Gankra by @Gankra on 2025-06-12 14:02_

---

_Unassigned @Gankra by @Gankra on 2025-06-12 14:02_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/add.rs`:117 on 2025-06-12 14:04_

Nice! It's notable that there's `--group` and `--dev` that apply here. My proposed solution was worried about there being a bunch of flag inheritance, but now that I've looked at it all the relevant flags are defined together so we should ideally be able to just adds conflicts_with("script") to those two flags (and then we don't need to quibble about error message formatting 😄).

https://github.com/astral-sh/uv/blob/806cc5cad9ac8abc3c29537a9f27bec66f409389/crates/uv-cli/src/lib.rs#L3516-L3534

---

_@Gankra requested changes on 2025-06-12 14:04_

---

_Comment by @codspeed-hq[bot] on 2025-06-12 14:13_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/blueraft%3Ado-not-allow-add-groups)

### Merging #13997 will **improve performances by 16.67%**

<sub>Comparing <code>blueraft:do-not-allow-add-groups</code> (d66c303) with <code>main</code> (806cc5c)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 112 ns | 96 ns | +16.67% |


---

_@blueraft reviewed on 2025-06-12 14:17_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/add.rs`:117 on 2025-06-12 14:17_

Makes sense :D I've added it to the remove command args too  

---

_Comment by @Gankra on 2025-06-12 14:21_

The test you checked in is actually useful, too late to add it back?

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:3566 on 2025-06-12 14:22_

Ah good call!

---

_@Gankra approved on 2025-06-12 14:22_

---

_Comment by @blueraft on 2025-06-12 14:32_

Added the test back

---

_Merged by @Gankra on 2025-06-12 14:45_

---

_Closed by @Gankra on 2025-06-12 14:45_

---

_Comment by @Gankra on 2025-06-12 14:45_

Thanks!

---
