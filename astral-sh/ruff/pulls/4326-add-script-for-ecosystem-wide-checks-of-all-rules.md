```yaml
number: 4326
title: Add script for ecosystem wide checks of all rules and fixes
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: add_ecosystem_all_fix_checker
created_at: 2023-05-09T19:53:05Z
updated_at: 2023-05-22T13:23:27Z
url: https://github.com/astral-sh/ruff/pull/4326
synced_at: 2026-01-12T03:50:02Z
```

# Add script for ecosystem wide checks of all rules and fixes

---

_Pull request opened by @konstin on 2023-05-09 19:53_

This adds my personal script for checking an entire checkout of ~2.1k packages for panics, autofix errors and similar problems. It's not really meant to be used by anybody else but i thought it's better if it lives in the repo than if it doesn't.

For reference, this is the current output of failing autofixes: https://gist.github.com/konstin/c3fada0135af6cacec74f166adf87a00. Trimmed down to the useful information: https://gist.github.com/konstin/c864f4c300c7903a24fdda49635c5da9

---

_Review requested from @charliermarsh by @konstin on 2023-05-09 19:53_

---

_Comment by @github-actions[bot] on 2023-05-09 20:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.03ms     2.9 MB/sec    1.00     13.9±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.9±1.48µs     7.0 MB/sec    1.00    423.3±0.56µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.01      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1451.6±3.25µs    11.5 MB/sec    1.00   1456.4±2.43µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.9±1.48µs    18.3 MB/sec    1.03    165.5±1.01µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.1±0.00ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.6 MB/sec    1.00      5.4±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1057.6±0.84µs    15.7 MB/sec    1.00   1052.7±0.54µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    108.4±0.18µs    27.2 MB/sec    1.00    108.6±1.10µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.7±0.23ms     2.6 MB/sec    1.00     15.6±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.06ms     4.3 MB/sec    1.00      3.9±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.2±8.49µs     6.5 MB/sec    1.01    459.4±9.31µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.7±0.19ms     3.8 MB/sec    1.00      6.4±0.10ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.02      7.8±0.17ms     5.2 MB/sec    1.00      7.6±0.08ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1626.7±27.69µs    10.2 MB/sec    1.00  1601.6±29.96µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.1±5.35µs    16.3 MB/sec    1.02    185.5±6.06µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.05ms     7.4 MB/sec    1.00      3.4±0.09ms     7.4 MB/sec
parser/large/dataset.py                    1.00      6.0±0.07ms     6.7 MB/sec    1.00      6.0±0.06ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1149.8±19.10µs    14.5 MB/sec    1.00  1145.8±18.89µs    14.5 MB/sec
parser/numpy/globals.py                    1.01    118.6±3.52µs    24.9 MB/sec    1.00    117.8±2.49µs    25.0 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.09ms     9.8 MB/sec    1.00      2.6±0.05ms    10.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `.gitignore`:151 on 2023-05-12 01:11_

My only critique here is that this `.gitignore` is the GitHub-standard, so this deviates from it. Should we add this separately? Or, is it even necessary?

---

_Review comment by @charliermarsh on `scripts/ecosystem_fix_all_check.sh`:18 on 2023-05-12 01:13_

I think it'd be useful to briefly document the expected workflow for running this, or at least the assumptions. At a glance, I see that:

1. It assumes you have a `.venv-3.11` installed in your root directory.
2. It assumes that you have `ripgrep` installed.
3. ...anything else? `checkouts`?

---

_@charliermarsh reviewed on 2023-05-12 01:32_

Nice!

Question: if we don't intend others to run these, what do you see as the tradeoffs in checking them in vs. merely archiving them on GitHub in some way (e.g., with this PR, but not merging it)?


---

_Comment by @konstin on 2023-05-18 17:40_

> Question: if we don't intend others to run these, what do you see as the tradeoffs in checking them in vs. merely archiving them on GitHub in some way (e.g., with this PR, but not merging it)?

I expect that i want to change it from time to time and i think it's nice to have our infrastructure in a place where it's findable externally 

---

_@konstin reviewed on 2023-05-18 17:51_

---

_Review comment by @konstin on `.gitignore`:151 on 2023-05-18 17:51_

github names them templates rather than standards and they do have a lot of content that doesn't apply to us, i normally only added something to gitignore once we were using it

---

_@konstin reviewed on 2023-05-18 18:27_

---

_Review comment by @konstin on `scripts/ecosystem_fix_all_check.sh`:18 on 2023-05-18 18:27_

The venv is created by the script, i replace ripgrep with grep. The checkouts need to come from the ecosystem ci, so it's a bit more involved to run

---

_Comment by @konstin on 2023-05-19 09:47_

Once we remove the forks from https://github.com/akx/ruff-usage-aggregate/blob/master/data/known-github-tomls.jsonl i think we can also just document how to run this, it has become a well usable script

---

_Review requested from @charliermarsh by @konstin on 2023-05-22 13:05_

---

_Review comment by @charliermarsh on `scripts/ecosystem_fix_all_check_entrypoint.sh`:9 on 2023-05-22 13:17_

Can we instead add `tqdm` to the `pyproject.toml` dependencies, and remove this script? This script seems generic rather than specific to `ecosystem_fix_all_check.py`.

---

_@charliermarsh reviewed on 2023-05-22 13:17_

---

_@charliermarsh approved on 2023-05-22 13:17_

---

_@konstin reviewed on 2023-05-22 13:23_

---

_Review comment by @konstin on `scripts/ecosystem_fix_all_check_entrypoint.sh`:9 on 2023-05-22 13:23_

unfortunately not since we have to keep the docker venv separate

---

_Merged by @konstin on 2023-05-22 13:23_

---

_Closed by @konstin on 2023-05-22 13:23_

---

_Branch deleted on 2023-05-22 13:23_

---
