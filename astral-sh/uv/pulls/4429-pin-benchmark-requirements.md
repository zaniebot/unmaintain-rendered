```yaml
number: 4429
title: Pin benchmark requirements
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: ibraheem/pin-bench
created_at: 2024-06-20T17:54:33Z
updated_at: 2024-06-20T18:13:40Z
url: https://github.com/astral-sh/uv/pull/4429
synced_at: 2026-01-10T13:48:28Z
```

# Pin benchmark requirements

---

_Pull request opened by @ibraheemdev on 2024-06-20 17:54_

## Summary

This should make benchmarks more consistent.

---

_Review comment by @zanieb on `crates/bench/benches/uv.rs`:164 on 2024-06-20 17:56_

Not sure if it'd be preferred here, but this is what we do in the tests https://github.com/astral-sh/uv/blob/e783a79955a3a4eb6a4c546f51f89e88b64047bb/crates/uv/tests/common/mod.rs#L26

---

_@zanieb reviewed on 2024-06-20 17:56_

---

_@zanieb reviewed on 2024-06-20 17:57_

---

_Review comment by @zanieb on `crates/bench/benches/uv.rs`:164 on 2024-06-20 17:57_

This is parsed by the CLI but you might want to use a a constant like this? 

---

_Review comment by @konstin on `crates/bench/benches/uv.rs`:164 on 2024-06-20 17:57_

nit: `ExcludeNewer::from_str` is probably easier

---

_Review comment by @konstin on `crates/bench/benches/uv.rs`:53 on 2024-06-20 17:59_

I'd leave that constraint as it is, the difficulty for that resolution is partially unifying those two constraints; with the exclude newer it will still be stable

---

_@konstin approved on 2024-06-20 17:59_

---

_@ibraheemdev reviewed on 2024-06-20 18:00_

---

_Review comment by @ibraheemdev on `crates/bench/benches/uv.rs`:164 on 2024-06-20 18:00_

I wanted to avoid the string parsing because it would end up part of the benchmark.. probably negligible but I think it's fine.

---

_Comment by @codspeed-hq[bot] on 2024-06-20 18:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheem/pin-bench)

### Merging #4429 will **improve performances by 11.55%**

<sub>Comparing <code>ibraheem/pin-bench</code> (35c9f29) with <code>main</code> (e783a79)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheem/pin-bench` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_airflow` | 1.4 s | 1.3 s | +11.55% |


---

_Merged by @ibraheemdev on 2024-06-20 18:13_

---

_Closed by @ibraheemdev on 2024-06-20 18:13_

---

_Branch deleted on 2024-06-20 18:13_

---
