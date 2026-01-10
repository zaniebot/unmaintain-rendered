```yaml
number: 3890
title: Disable concurrent progress bars in Jupyter Notebooks
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
assignees: []
merged: true
base: main
head: jupyter-progress
created_at: 2024-05-28T20:49:59Z
updated_at: 2024-05-28T21:05:12Z
url: https://github.com/astral-sh/uv/pull/3890
synced_at: 2026-01-10T14:32:20Z
```

# Disable concurrent progress bars in Jupyter Notebooks

---

_Pull request opened by @ibraheemdev on 2024-05-28 20:49_

## Summary

Resolves https://github.com/astral-sh/uv/issues/3887 by disabling the new progress output when the `JPY_SESSION_NAME` environment variable is detected.

## Test Plan

Fixed output in a Jupyter notebook:

![image](https://github.com/astral-sh/uv/assets/34988408/dd79f259-0740-4c5e-a1ba-bc74929cf9f4)


---

_Review requested from @charliermarsh by @ibraheemdev on 2024-05-28 20:51_

---

_@ibraheemdev reviewed on 2024-05-28 20:52_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/reporters.rs`:271 on 2024-05-28 20:52_

This bug existed before the new progress bars, where if the previous progress bar line was longer than the next one, it wouldn't be fully cleared. Seems like a bug in indicatif.

---

_Comment by @charliermarsh on 2024-05-28 20:53_

Can you also test this in Jupyter Lab? Same process but run `jupyter lab` in the command line instead of `jupyter notebook`. It's kind of the next-gen UI.

---

_Label `bug` added by @charliermarsh on 2024-05-28 20:53_

---

_@charliermarsh approved on 2024-05-28 20:56_

---

_Comment by @codspeed-hq[bot] on 2024-05-28 21:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:jupyter-progress)

### Merging #3890 will **improve performances by 6.72%**

<sub>Comparing <code>ibraheemdev:jupyter-progress</code> (99a1c0c) with <code>main</code> (47db418)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheemdev:jupyter-progress` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 926.7 ns | 868.3 ns | +6.72% |


---

_Comment by @ibraheemdev on 2024-05-28 21:03_

@charliermarsh seems to work there too:
![image](https://github.com/astral-sh/uv/assets/34988408/e0244a6a-33a8-48ea-8e78-2024d033279f)


---

_Merged by @ibraheemdev on 2024-05-28 21:05_

---

_Closed by @ibraheemdev on 2024-05-28 21:05_

---
