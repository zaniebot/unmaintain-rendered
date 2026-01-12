```yaml
number: 4728
title: Make the release workflow more resilient
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: release_rework
created_at: 2023-05-30T15:58:05Z
updated_at: 2023-06-20T18:57:17Z
url: https://github.com/astral-sh/ruff/pull/4728
synced_at: 2026-01-12T03:43:29Z
```

# Make the release workflow more resilient

---

_Pull request opened by @konstin on 2023-05-30 15:58_

## Summary

Currently, it is possible to create a tag and then have the release fail, which is a problem since we can't edit the tag (https://github.com/charliermarsh/ruff/issues/4468). This change the release process so that the tag is created inside the release workflow. This leaves as a failure mode that we have published to pypi but then creating the tag or GitHub release doesn't work, but in this case we can restart and the pypi upload is just skipped because we use the skip existing option.

The release workflow is started by a workflow dispatch with the tag instead of creating the tag yourself. You can start the release workflow without a tag to do a dry run which does not publish an artifacts. You can optionally add a git sha to the workflow run and it will verify that the release runs on the mentioned commit.

This also adds docs on how to release and a small style improvement for the maturin integration.

## Test Plan

Testing is hard since we can't do real releases, i've tested a minimized workflow in a separate dummy repository.


---

_Review requested from @MichaReiser by @konstin on 2023-05-30 15:58_

---

_Review requested from @charliermarsh by @konstin on 2023-05-30 15:58_

---

_Comment by @github-actions[bot] on 2023-05-30 16:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.00   1369.2±2.29µs    12.2 MB/sec    1.00   1367.8±3.93µs    12.2 MB/sec
formatter/numpy/globals.py                 1.00    132.8±0.36µs    22.2 MB/sec    1.00    132.4±3.71µs    22.3 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.02ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.03ms     3.0 MB/sec    1.01     13.8±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.8 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.0±1.05µs     8.1 MB/sec    1.00    361.8±1.07µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.8 MB/sec    1.01      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1477.9±4.80µs    11.3 MB/sec    1.01   1487.2±2.56µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.3±0.46µs    18.6 MB/sec    1.00    158.4±0.21µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.36ms     4.8 MB/sec    1.01      8.6±0.46ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1732.4±88.51µs     9.6 MB/sec    1.02  1763.8±100.93µs     9.4 MB/sec
formatter/numpy/globals.py                 1.02   188.3±22.47µs    15.7 MB/sec    1.00   183.9±14.36µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.18ms     7.2 MB/sec    1.00      3.6±0.28ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.57ms     2.5 MB/sec    1.01     16.6±0.56ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.19ms     3.8 MB/sec    1.01      4.5±0.22ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   556.5±33.41µs     5.3 MB/sec    1.00   550.2±38.77µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.42ms     3.4 MB/sec    1.02      7.6±0.38ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.9±0.47ms     4.6 MB/sec    1.00      8.8±0.33ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1881.4±88.33µs     8.9 MB/sec    1.00  1857.9±78.38µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   231.8±13.48µs    12.7 MB/sec    1.00   230.9±13.43µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.1±0.30ms     6.3 MB/sec    1.00      4.0±0.21ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `.github/workflows/release.yaml`:423 on 2023-05-30 20:22_

Can we exit with an error message that tells the user what went wrong?

---

_Review comment by @MichaReiser on `How_to_Release.md`:9 on 2023-05-30 20:24_

Should it be possible to provide an optional commit to the release workflow that it then uses as the version to publish? This could be useful to retry a release after something has been merged. But we could also support this by reverting all changes and then releasing

---

_@MichaReiser approved on 2023-05-30 20:25_

---

_@konstin reviewed on 2023-05-30 20:48_

---

_Review comment by @konstin on `.github/workflows/release.yaml`:423 on 2023-05-30 20:48_

I'm printing the versions so you can compare, but otherwise it doesn't look like bash has an equivalent to `bail!("tag mismatch: {} {}", tag, version)` (https://stackoverflow.com/a/24597890/3549270)

---

_@MichaReiser reviewed on 2023-05-30 21:06_

---

_Review comment by @MichaReiser on `.github/workflows/release.yaml`:423 on 2023-05-30 21:06_

I think an echo would be useful, mentioning that some files still reference the old version (what happens if one of our rust dependencies use the same version number?)

---

_@konstin reviewed on 2023-05-31 10:21_

---

_Review comment by @konstin on `How_to_Release.md`:9 on 2023-05-31 10:21_

done through `git checkout`

---

_Comment by @konstin on 2023-05-31 10:21_

started a dry run: https://github.com/charliermarsh/ruff/actions/runs/5131675732

---

_@konstin reviewed on 2023-05-31 11:17_

---

_Review comment by @konstin on `How_to_Release.md`:9 on 2023-05-31 11:17_

This doesn't work like i would like it to, i reverted the change.

The main problem with this is that we run the workflow on code from a different commit than the release.yml is from, and this could become really strange. I'd instead prefer to merge new changes to main and then run on those.

---

_Comment by @konstin on 2023-05-31 11:18_

![image](https://github.com/charliermarsh/ruff/assets/6826232/2235e91a-5a01-4774-ac3c-8a81dcfb7cef)

through the environment, we also get reports of failures from workflow triggers

---

_@charliermarsh reviewed on 2023-06-02 14:58_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:11 on 2023-06-02 14:58_

I don't think this is used (at least, as far as I can tell).

---

_@charliermarsh reviewed on 2023-06-02 14:58_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:11 on 2023-06-02 14:58_

Aren't we able to select a branch anyway in the UI?

---

_@charliermarsh reviewed on 2023-06-02 14:58_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:7 on 2023-06-02 14:58_

Nit: "If omitted, will initiate a dry-run, skipping any upload steps."

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:15 on 2023-06-02 14:59_

I'd be fine to omit this and just do it manually, personally.

---

_@charliermarsh reviewed on 2023-06-02 14:59_

---

_@charliermarsh reviewed on 2023-06-02 14:59_

---

_Review comment by @charliermarsh on `.github/workflows/release.yaml`:406 on 2023-06-02 14:59_

Nit: use the "dry run" terminology here for consistency, like "If no tag was set, consider this a dry-run."

---

_@charliermarsh reviewed on 2023-06-02 15:00_

---

_Review comment by @charliermarsh on `How_to_Release.md`:1 on 2023-06-02 15:00_

`RELEASE.md` for title consistency? Or should we just put this in `CONTRIBUTING.md`, which is effectively our development guide? (It also includes benchmarking instructions, as an example.)

---

_@charliermarsh reviewed on 2023-06-02 15:00_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:226 on 2023-06-02 15:00_

What's the difference here?

---

_@charliermarsh reviewed on 2023-06-02 15:01_

---

_Review comment by @charliermarsh on `How_to_Release.md`:21 on 2023-06-02 15:01_

Does this also include promoting out of "draft"?

---

_Review comment by @MichaReiser on `.github/workflows/release.yaml`:11 on 2023-06-05 06:54_

This can be useful in cases where you want to re-run the release workflow on a specific commit. But you're right. We could create a branch pointing to that specific commit instead. 

---

_@MichaReiser reviewed on 2023-06-05 06:54_

---

_Review comment by @MichaReiser on `How_to_Release.md`:9 on 2023-06-05 06:56_

```suggestion
1. Run the release workflow with the version number (without starting `v`) as input. Make sure main has your
```

Nit: Rephrase to version number? I first thought I would need to create the tag myself and then provide its name. 

---

_Review comment by @MichaReiser on `How_to_Release.md`:17 on 2023-06-05 06:58_

```suggestion
   1. Create and push the git tag (from pyproject.toml). We create the git tag only here
      because we can't change it ([#4468](https://github.com/charliermarsh/ruff/issues/4468)), so
      we want to make sure everything up to and including publishing to pypi worked.
```

Nit: Document the tag name? What does *from pyproject.toml* mean?

---

_@MichaReiser approved on 2023-06-05 06:58_

---

_@konstin reviewed on 2023-06-13 12:14_

---

_Review comment by @konstin on `.github/workflows/ci.yaml`:226 on 2023-06-13 12:14_

one works by bash globbing, the other is the pip-provided way if installing from a directory which abstract wheel naming and such away and also works if there are multiple wheels (pip selects the right one)

---

_Converted to draft by @konstin on 2023-06-13 12:14_

---

_@konstin reviewed on 2023-06-13 13:43_

---

_Review comment by @konstin on `.github/workflows/release.yaml`:11 on 2023-06-13 13:43_

i've changed this to be an additional check you can opt in or ignore

---

_@konstin reviewed on 2023-06-13 13:45_

---

_Review comment by @konstin on `.github/workflows/release.yaml`:15 on 2023-06-13 13:45_

yes but i want tests for maturin updates :D

---

_Marked ready for review by @konstin on 2023-06-13 14:01_

---

_Comment by @konstin on 2023-06-13 14:01_

This is ready for another round of review

---

_Review comment by @charliermarsh on `RELEASING.md`:23 on 2023-06-20 17:17_

Can we put this in `CONTRIBUTING.md`? There's already a small section there on releases.

---

_@charliermarsh reviewed on 2023-06-20 17:17_

---

_@charliermarsh approved on 2023-06-20 17:20_

---

_Merged by @konstin on 2023-06-20 18:33_

---

_Closed by @konstin on 2023-06-20 18:33_

---

_Branch deleted on 2023-06-20 18:33_

---
