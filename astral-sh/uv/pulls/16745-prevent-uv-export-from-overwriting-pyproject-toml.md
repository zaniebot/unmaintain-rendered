```yaml
number: 16745
title: "Prevent `uv export` from overwriting `pyproject.toml`"
type: pull_request
state: merged
author: terror
labels:
  - enhancement
assignees: []
merged: true
base: main
head: prevent-export-overwrite
created_at: 2025-11-15T08:25:00Z
updated_at: 2025-11-21T02:37:36Z
url: https://github.com/astral-sh/uv/pull/16745
synced_at: 2026-01-10T05:58:11Z
```

# Prevent `uv export` from overwriting `pyproject.toml`

---

_Pull request opened by @terror on 2025-11-15 08:25_

Currently, it's possible for `uv export` to overwrite someones `pyproject.toml`. This diff simply rejects project files passed in with `-o`, so we avoid doing that.

---

_Review comment by @konstin on `crates/uv/src/commands/project/export.rs`:302 on 2025-11-19 10:57_

What about this (pseudocode):

```suggestion
            "`pyproject.toml` is not a supported output format. Supported formats: {}", ExportFormat::value_variants()
        .iter()
        .filter_map(|variant| variant.to_possible_value())
        .map(|value| value.get_name())
        .join(", ");
```

---

_@konstin reviewed on 2025-11-19 10:58_

---

_Review requested from @charliermarsh by @konstin on 2025-11-19 10:58_

---

_@terror reviewed on 2025-11-20 17:22_

---

_Review comment by @terror on `crates/uv/src/commands/project/export.rs`:302 on 2025-11-20 17:22_

Yeah I like this, just pushed up a change.

---

_@konstin approved on 2025-11-20 18:58_

---

_Label `enhancement` added by @konstin on 2025-11-20 18:58_

---

_Renamed from "Prevent `uv export` from overwriting pyproject.toml" to "Prevent `uv export` from overwriting `pyproject.toml`" by @konstin on 2025-11-20 18:58_

---

_Comment by @terror on 2025-11-20 22:13_

@konstin Just pushed up a change to placate clippy.

---

_@charliermarsh approved on 2025-11-21 02:12_

---

_Merged by @charliermarsh on 2025-11-21 02:33_

---

_Closed by @charliermarsh on 2025-11-21 02:33_

---

_Comment by @codspeed-hq[bot] on 2025-11-21 02:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/terror%3Aprevent-export-overwrite?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16745 will **degrade performances by 29.69%**

<sub>Comparing <code>terror:prevent-export-overwrite</code> (8396e22) with <code>main</code> (d3a9455)</sub>



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/terror%3Aprevent-export-overwrite?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/terror%3Aprevent-export-overwrite?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 795.8 ms | 1,131.9 ms | -29.69% |


---
