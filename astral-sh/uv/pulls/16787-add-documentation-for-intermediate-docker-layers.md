```yaml
number: 16787
title: Add documentation for intermediate Docker layers in a workspace
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/docker-workspace
created_at: 2025-11-20T14:44:03Z
updated_at: 2025-11-24T15:27:25Z
url: https://github.com/astral-sh/uv/pull/16787
synced_at: 2026-01-12T16:12:26Z
```

# Add documentation for intermediate Docker layers in a workspace

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2025-11-20 14:44_

---

_Renamed from "Add documentation for intermediate layers with a workspace" to "Add documentation for intermediate Docker layers in a workspace" by @zanieb on 2025-11-20 15:41_

---

_Review requested from @charliermarsh by @zanieb on 2025-11-20 17:28_

---

_@charliermarsh reviewed on 2025-11-20 18:55_

---

_Review comment by @charliermarsh on `docs/guides/integration/docker.md`:453 on 2025-11-20 18:55_

I think I would recommend `--frozen` here, isn't it much simpler? That's what we tend to do in our own Dockerfiles.

---

_@zanieb reviewed on 2025-11-20 19:47_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:453 on 2025-11-20 19:47_

In what sense? `--frozen` _here_ doesn't change anything because the whole workspace has been added already, it'd be up above where using `--frozen` instead of `--locked` makes things simpler.

I don't think we should recommend `--frozen` in general though, because I think `--locked` asserting that your `uv.lock` is not stale is meaningful and helpful.

I think it's fine to recommend `--frozen` above and rely on this subsequent `--locked` to catch a stale lock, but I worry that will also be confusing to people (i.e., I recommended exactly this in the issue and the user opted to do the additional mounts instead)

---

_@charliermarsh reviewed on 2025-11-21 00:33_

---

_Review comment by @charliermarsh on `docs/guides/integration/docker.md`:453 on 2025-11-21 00:33_

Sorry, I meant to comment on the line above. I personally find it unnecessary to enumerate all of the `pyproject.toml` files in the `uv sync --no-install-workspace` when you're doing a locked install here anyway? (But -- again personally -- I wouldn't use `--locked` in a Dockerfile, I think that validation should be done in CI.)

In pyx at least, we instead use `--frozen` for this install phase, but enumerate all the workspace members when we do the `ADD` (to minimize invalidation).

---

_@zanieb reviewed on 2025-11-21 16:52_

---

_Review comment by @zanieb on `docs/guides/integration/docker.md`:453 on 2025-11-21 16:52_

Yeah... I'll change it.

---

_Comment by @zhcn000000 on 2025-11-23 12:40_

If I need to install all the packages for a specific workspace, how should I handle it? If I directly use uv sync --package, will the packages in the project root directory be ignored

---

_Comment by @zanieb on 2025-11-24 15:09_

Yeah, use `--package`.

---

_Merged by @zanieb on 2025-11-24 15:22_

---

_Closed by @zanieb on 2025-11-24 15:22_

---

_Branch deleted on 2025-11-24 15:22_

---

_Comment by @codspeed-hq[bot] on 2025-11-24 15:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Fdocker-workspace?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16787 will **degrade performances by 19.45%**

<sub>Comparing <code>zb/docker-workspace</code> (29fc564) with <code>main</code> (d6eb285)</sub>



### Summary

`❌ 1` regression  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb%2Fdocker-workspace?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` resolve_warm_airflow ``](https://codspeed.io/astral-sh/uv/branches/zb%2Fdocker-workspace?uri=crates%2Fuv-bench%2Fbenches%2Fuv.rs%3A%3Auv%3A%3Aresolve_warm_airflow%3A%3Aresolve_warm_airflow&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 808.3 ms | 1,003.6 ms | -19.45% |


---
