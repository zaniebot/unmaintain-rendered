```yaml
number: 16889
title: Support requirements without an extension
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: charlie/source-cache
head: charlie/extentionless
created_at: 2025-11-28T22:50:18Z
updated_at: 2025-12-02T14:04:20Z
url: https://github.com/astral-sh/uv/pull/16889
synced_at: 2026-01-12T16:12:30Z
```

# Support requirements without an extension

---

_@charliermarsh_

## Summary

This PR un-reverts #16861 by resolving extensionless sources with a different strategy: we return an `Extensionless` variant, then infer the type when we read the file and parse the contents immediately after, thereby avoiding multiple reads.



---

_Review requested from @zanieb by @charliermarsh on 2025-11-28 22:50_

---

_Review requested from @konstin by @charliermarsh on 2025-11-28 22:50_

---

_Label `enhancement` added by @charliermarsh on 2025-11-28 22:50_

---

_Marked ready for review by @charliermarsh on 2025-11-28 22:50_

---

_Comment by @codspeed-hq[bot] on 2025-11-29 01:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fextentionless?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16889 will **degrade performances by 28.44%**

<sub>Comparing <code>charlie/extentionless</code> (7554291) with <code>charlie/source-cache</code> (44e8062)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>charlie/source-cache</code> (d5dad4e) during the generation of this report, so d63e56f was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fextentionless?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/charlie%2Fextentionless?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 785.4 ms | 1,097.4 ms | -28.44% |


---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:366 on 2025-12-01 08:42_

We should special case the error message, asking the user to specify a format manually, as we defaulted to `requirements.txt` and that failed.

```
$ uv pip install -r .editorconfig 
error: Couldn't parse requirement in `.editorconfig` at position 102
  Caused by: no such comparison operator "=", must be one of ~= == != <= >= < > ===
root = true
     ^^^^^^
```

```
$ uv pip install -r uv.lock
error: Couldn't parse requirement in `uv.lock` at position 0
  Caused by: no such comparison operator "=", must be one of ~= == != <= >= < > ===
version = 1
        ^^^
```

---

_@konstin approved on 2025-12-01 09:22_

---

_@konstin approved on 2025-12-01 09:24_

---

_@zanieb reviewed on 2025-12-02 09:20_

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:366 on 2025-12-02 09:20_

There's not a way for them to specify the format though, right?

---

_@zanieb approved on 2025-12-02 09:21_

---

_Merged by @zanieb on 2025-12-02 09:22_

---

_Closed by @zanieb on 2025-12-02 09:22_

---

_Branch deleted on 2025-12-02 09:22_

---

_@konstin reviewed on 2025-12-02 12:33_

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:366 on 2025-12-02 12:33_

We use the filename only, but currently there's no indication that this fails because we're trying to parse anything that doesn't look like a `pylock.toml` as `requirements.txt`

---

_@zanieb reviewed on 2025-12-02 14:02_

---

_Review comment by @zanieb on `crates/uv-requirements/src/specification.rs`:366 on 2025-12-02 14:02_

I think the hint would just be "We tried to parse this as a `requirements.txt` because it doesn't have an supported extension"? 

For `uv.lock`, see https://github.com/astral-sh/uv/issues/16192

---

_@konstin reviewed on 2025-12-02 14:04_

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:366 on 2025-12-02 14:04_

That would be a good hint

---
