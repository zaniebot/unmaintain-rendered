```yaml
number: 3689
title: "Enable `pycodestyle` rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pycodestyle
created_at: 2023-03-23T18:26:25Z
updated_at: 2024-01-21T07:07:25Z
url: https://github.com/astral-sh/ruff/pull/3689
synced_at: 2026-01-12T15:55:13Z
```

# Enable `pycodestyle` rules

---

_@charliermarsh_

## Summary

This PR enables the `pycodestyle` rules by removing the `logical_lines` feature flag. Based on @MichaReiser's great work, these rules only increase the CPython benchmark by about 25% in my local testing (let's see what the CI benchmarks say), and some 10s of milliseconds of that is a result of "more violations" (rather than actual analysis).

One major decision we should make is whether we want to include these rules in the default ruleset. Right now, the default ruleset is `["E", "F"]`. But we could change it to `["E4", "E5", "E7", "E9", "F"]` to represent "Those Pyflakes rules that are obviated by Black", which would also avoid any performance regressions for users on the default settings (and in theory for those that configure Ruff by starting with the defaults and iterating from there).


---

_Comment by @github-actions[bot] on 2023-03-23 18:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.5±0.46ms  2036.2 KB/sec    1.05     21.6±0.61ms  1930.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.17ms     3.5 MB/sec    1.05      5.0±0.22ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   593.9±19.77µs     5.0 MB/sec    1.03   613.7±30.33µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.29ms     3.0 MB/sec    1.06      9.0±0.35ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.21ms     4.1 MB/sec    1.15     11.4±0.43ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.06ms     7.7 MB/sec    1.10      2.4±0.08ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   251.2±23.80µs    11.7 MB/sec    1.11   279.0±19.50µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.15ms     5.7 MB/sec    1.15      5.1±0.23ms     5.0 MB/sec
parser/large/dataset.py                    1.04      7.6±0.28ms     5.4 MB/sec    1.00      7.3±0.18ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.04  1467.6±73.73µs    11.3 MB/sec    1.00  1417.7±47.15µs    11.7 MB/sec
parser/numpy/globals.py                    1.03   147.8±14.74µs    20.0 MB/sec    1.00    142.9±7.64µs    20.6 MB/sec
parser/pydantic/types.py                   1.03      3.2±0.11ms     8.0 MB/sec    1.00      3.1±0.07ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.0±0.60ms     2.0 MB/sec    1.00     20.1±0.74ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.21ms     3.3 MB/sec    1.01      5.1±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   591.1±27.63µs     5.0 MB/sec    1.01   598.1±27.73µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.42ms     3.0 MB/sec    1.01      8.6±0.44ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.33ms     4.1 MB/sec    1.03     10.2±0.43ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.03      2.2±0.13ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   260.4±10.59µs    11.3 MB/sec    1.00   261.6±15.16µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.17ms     5.7 MB/sec    1.01      4.5±0.17ms     5.6 MB/sec
parser/large/dataset.py                    1.00      8.0±0.22ms     5.1 MB/sec    1.01      8.0±0.28ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.02  1545.7±66.04µs    10.8 MB/sec    1.00  1513.5±58.55µs    11.0 MB/sec
parser/numpy/globals.py                    1.02   162.8±19.65µs    18.1 MB/sec    1.00    159.0±9.89µs    18.6 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.11ms     7.5 MB/sec    1.00      3.4±0.15ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-03-29 15:31_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-29 15:35_

---

_Comment by @MichaReiser on 2023-03-29 19:52_

Hmm, this is still kind of a lot. If I remember correctly than half of the regression is just due to the fact that we need to build the locator index.

Let me have a stab at adding a byte-location feature to RustPython tomorrow afternoon. Do you know if we make much use of the location row/column information outside of printing diagnostics?

Note: using byte offsets can regress performance for projects with many diagnostics because we have to compute the line index for every file with diagnostics, regardless if any rule uses the source. But it should give us better performance for projects that have only a few diagnostics, which is the majority of projects that are using Ruff

---

_Comment by @MichaReiser on 2023-04-26 18:27_

Let's see how this does in terms of performance after rebasing on top of #3931 

---

_Comment by @MichaReiser on 2023-04-26 18:58_

Nice! A 10% regression (compared to up to 50% before) is way more reasonable. 

---

_@MichaReiser approved on 2023-04-26 19:17_

Should we announce the new rules as experimental or do we feel confident they match pycodestyle? If we announce them as experimental, should we remove them from the pycodestyle group for now and ask users to explicitly opt-in?

---

_Comment by @MichaReiser on 2023-05-08 10:44_

I'm considering creating a new `nursery` group where we can add any new rules that we aren't confident to roll out directly and must be enabled explicitly (they are not included in `ALL`). 

---

_Merged by @charliermarsh on 2023-05-16 20:39_

---

_Closed by @charliermarsh on 2023-05-16 20:39_

---

_Branch deleted on 2023-05-16 20:39_

---

_Comment by @MichaReiser on 2023-05-16 20:40_

This is huge. 🎸🎸🎸

---

_Comment by @danieleades on 2024-01-21 06:53_

i see `E704` is ticked off the list, but i don't see it mentioned [in the docs](https://docs.astral.sh/ruff/rules/#error-e). what's the status of this rule?

---

_Comment by @AA-Turner on 2024-01-21 07:07_

E704 was removed in #2773, and remains gone as far as I can tell -- or I have previously missed it in the preview rules.

A

---
