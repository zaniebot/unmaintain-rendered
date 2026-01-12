```yaml
number: 7853
title: "Add `uv-` prefix to all internal crates"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rename
created_at: 2024-10-01T23:46:37Z
updated_at: 2024-10-02T07:12:14Z
url: https://github.com/astral-sh/uv/pull/7853
synced_at: 2026-01-12T16:08:02Z
```

# Add `uv-` prefix to all internal crates

---

_@charliermarsh_

## Summary

Brings more consistency to the repo and ensures that all crates automatically show up in `--verbose` logging.

---

_Label `internal` added by @charliermarsh on 2024-10-01 23:46_

---

_Marked ready for review by @charliermarsh on 2024-10-01 23:46_

---

_Comment by @codspeed-hq[bot] on 2024-10-01 23:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/rename)

### Merging #7853 will create unknown performance changes

<sub>Comparing <code>charlie/rename</code> (4525df8) with <code>main</code> (7b55e97)</sub>



### Summary


`🆕 14` new benchmarks
`⁉️ 14` dropped benchmarks


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/rename)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/rename` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `build_platform_tags[burntsushi-archlinux]` | 1.3 ms | N/A | N/A |
| ⁉️ | `wheelname_parsing[flyte-long-compatible]` | 9.9 µs | N/A | N/A |
| ⁉️ | `wheelname_parsing[flyte-long-incompatible]` | 12.6 µs | N/A | N/A |
| ⁉️ | `wheelname_parsing[flyte-short-compatible]` | 6.3 µs | N/A | N/A |
| ⁉️ | `wheelname_parsing[flyte-short-incompatible]` | 6.4 µs | N/A | N/A |
| ⁉️ | `wheelname_parsing_failure[flyte-long-extension]` | 1.9 µs | N/A | N/A |
| ⁉️ | `wheelname_parsing_failure[flyte-short-extension]` | 1.9 µs | N/A | N/A |
| ⁉️ | `wheelname_tag_compatibility[flyte-long-compatible]` | 2.1 µs | N/A | N/A |
| ⁉️ | `wheelname_tag_compatibility[flyte-long-incompatible]` | 1.5 µs | N/A | N/A |
| ⁉️ | `wheelname_tag_compatibility[flyte-short-compatible]` | 2 µs | N/A | N/A |
| ⁉️ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 1 µs | N/A | N/A |
| ⁉️ | `resolve_warm_airflow` | 2 s | N/A | N/A |
| ⁉️ | `resolve_warm_jupyter` | 87.4 ms | N/A | N/A |
| ⁉️ | `resolve_warm_jupyter_universal` | 343.3 ms | N/A | N/A |
| 🆕 | `build_platform_tags[burntsushi-archlinux]` | N/A | 1.3 ms | N/A |
| 🆕 | `wheelname_parsing[flyte-long-compatible]` | N/A | 9.8 µs | N/A |
| 🆕 | `wheelname_parsing[flyte-long-incompatible]` | N/A | 13.5 µs | N/A |
| 🆕 | `wheelname_parsing[flyte-short-compatible]` | N/A | 6.3 µs | N/A |
| 🆕 | `wheelname_parsing[flyte-short-incompatible]` | N/A | 6.4 µs | N/A |
| 🆕 | `wheelname_parsing_failure[flyte-long-extension]` | N/A | 1.9 µs | N/A |
| ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/uv/branches/charlie/rename)._


---

_Merged by @charliermarsh on 2024-10-02 00:15_

---

_Closed by @charliermarsh on 2024-10-02 00:15_

---

_Branch deleted on 2024-10-02 00:15_

---

_Comment by @FishAlchemist on 2024-10-02 07:10_

@charliermarsh Is there a plan to update the [README in crates](https://github.com/FishAlchemist/uv/blob/main/crates/README.md)? I was thinking of modifying it manually, but I'm not sure if there's an automated way to generate it, so I'll hold off for now.

However, the README seems to have been out of date for a long time. Even the links that were broken before are still there.

---
