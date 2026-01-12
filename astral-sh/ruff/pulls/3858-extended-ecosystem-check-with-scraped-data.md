```yaml
number: 3858
title: Extended ecosystem check with scraped data
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: extended_ecosystem_checker
created_at: 2023-04-03T09:02:52Z
updated_at: 2023-04-06T22:54:41Z
url: https://github.com/astral-sh/ruff/pull/3858
synced_at: 2026-01-12T04:28:19Z
```

# Extended ecosystem check with scraped data

---

_Pull request opened by @konstin on 2023-04-03 09:02_

This allows running the ecosystem CI locally in an isolated docker container using the data from https://github.com/akx/ruff-usage-aggregate. It also changes the output format of the ecosystem CI a little to include the repository URL.

CC @akx @sciyoshi i merged your efforts in one locally runnable docker container

---

_Comment by @github-actions[bot] on 2023-04-03 09:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.0±0.03ms     2.7 MB/sec    1.00     14.9±0.01ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    412.6±1.61µs     7.2 MB/sec    1.00    407.9±1.11µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.06ms     3.9 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.03ms     5.2 MB/sec    1.01      7.9±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1722.0±1.90µs     9.7 MB/sec    1.01   1733.6±4.04µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.0±0.38µs    16.2 MB/sec    1.01    183.2±0.50µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.00ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     20.2±0.86ms     2.0 MB/sec    1.00     19.6±0.72ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.2±0.35ms     3.2 MB/sec    1.00      5.0±0.19ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   607.8±30.49µs     4.9 MB/sec    1.03   626.4±44.14µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.27ms     3.0 MB/sec    1.00      8.6±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.35ms     4.1 MB/sec    1.01     10.1±0.35ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.3±0.11ms     7.2 MB/sec    1.00      2.3±0.07ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   259.4±12.09µs    11.4 MB/sec    1.02   264.6±15.00µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.25ms     5.5 MB/sec    1.05      4.8±0.25ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@akx reviewed on 2023-04-03 10:53_

Cool! 😎 

---

_Comment by @akx on 2023-04-03 12:40_

I just refreshed https://github.com/akx/ruff-usage-aggregate/blob/master/data/known-github-tomls.jsonl with about 90 new repos.

---

_Review requested from @MichaReiser by @konstin on 2023-04-05 10:22_

---

_Review requested from @charliermarsh by @konstin on 2023-04-05 10:22_

---

_Review comment by @MichaReiser on `scripts/Dockerfile.ecosystem`:16 on 2023-04-06 06:26_

What's your reasoning behind running it in a docker container vs running the script locally? 

---

_Review comment by @MichaReiser on `scripts/check_ecosystem.py`:58 on 2023-04-06 06:27_

Nit: Add a newline to make it clearer that this statement does not belong to the if body?
```suggestion
                git_command.extend(["--branch", self.ref])

          	git_command.extend(
```

---

_@MichaReiser approved on 2023-04-06 06:28_

Nice!

Could we add a section to our Contribution.md that explains how to run the ecosystem checks?

---

_Review comment by @konstin on `scripts/Dockerfile.ecosystem`:16 on 2023-04-06 08:18_

https://moyix.blogspot.com/2022/09/someones-been-messing-with-my-subnormals.html or rather, running software not designed around working with arbitrary untrusted inputs with the largest set of untrusted inputs i could find

---

_@konstin reviewed on 2023-04-06 08:18_

---

_Comment by @konstin on 2023-04-06 08:30_

> Could we add a section to our Contribution.md that explains how to run the ecosystem checks?

done

---

_Merged by @charliermarsh on 2023-04-06 22:39_

---

_Closed by @charliermarsh on 2023-04-06 22:39_

---

_Branch deleted on 2023-04-06 22:39_

---
