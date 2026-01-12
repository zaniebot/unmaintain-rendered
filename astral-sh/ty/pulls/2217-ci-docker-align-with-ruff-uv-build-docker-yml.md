```yaml
number: 2217
title: "ci(docker): align with ruff/uv build-docker.yml"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: build-docker-cleanup
created_at: 2025-12-25T02:30:09Z
updated_at: 2025-12-25T22:41:35Z
url: https://github.com/astral-sh/ty/pull/2217
synced_at: 2026-01-12T15:54:28Z
```

# ci(docker): align with ruff/uv build-docker.yml

---

_@samypr100_

## Summary

Aligns build-docker.yml with ruff (from [f9afcc4](https://github.com/astral-sh/ruff/blob/f9afcc400ce56b1cc0dd1f20f3910afb0bb37d44/.github/workflows/build-docker.yml)) and uv (from [b918557](https://github.com/astral-sh/uv/blob/b918557ae70b4a1f666433df07a8dc32971ec59e/.github/workflows/build-docker.yml))

1. Changed to read version from pyproject.toml rather than dist-workspace.toml
2. Ensures alpine 3.23 and trixie are used to start ty with an up-to-date base.
3. Fixes tag_value usages to align with ruff and uv usage.

zizmor complained about this block because [zizmor.yml](https://github.com/astral-sh/ruff/blob/19b10993e1bfcdcb0be8fad8b4259a0f504860c6/.github/zizmor.yml) is missing in this repo. It seems better to avoid a blanket ignore so I've kept it despite the differences. This should be a fix done in ruff side.

```yaml
permissions:
  contents: read
  # TODO(zanieb): Ideally, this would be `read` on dry-run but that will require
  # significant changes to the workflow.
  packages: write # zizmor: ignore[excessive-permissions]
```

## Test Plan

Did a release on my fork
* [CI Run](https://github.com/samypr100/ty/actions/runs/20497471638)
* [CI Dry Run](https://github.com/samypr100/ty/actions/runs/20497358071)
* [Released Packages](https://github.com/samypr100/ty/pkgs/container/ty/versions?filters%5Bversion_type%5D=tagged) 

---

_Marked ready for review by @samypr100 on 2025-12-25 02:30_

---

_@MichaReiser approved on 2025-12-25 17:11_

Thank you

---

_Comment by @MichaReiser on 2025-12-25 17:17_

Looks like the permissions are too broad.

---

_Comment by @samypr100 on 2025-12-25 17:56_

> Looks like the permissions are too broad.

Thanks, it seems it was failing because the zizmor configuration was missing from [ruff zizmor.yml](https://github.com/astral-sh/ruff/blob/19b10993e1bfcdcb0be8fad8b4259a0f504860c6/.github/zizmor.yml)

---

_Comment by @samypr100 on 2025-12-25 18:13_

> > Looks like the permissions are too broad.
> 
> Thanks, it seems it was failing because the zizmor configuration was missing from [ruff zizmor.yml](https://github.com/astral-sh/ruff/blob/19b10993e1bfcdcb0be8fad8b4259a0f504860c6/.github/zizmor.yml)

Should be good to go now. pre-commit should pass. I'll make a PR on ruff side to align on zizmor fixes.

---

_Merged by @MichaReiser on 2025-12-25 20:48_

---

_Closed by @MichaReiser on 2025-12-25 20:48_

---

_Branch deleted on 2025-12-25 22:41_

---
