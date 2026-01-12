```yaml
number: 4098
title: "Document that `--diff` implies `--fix-only`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/diff-requires-fix-arg
created_at: 2023-04-25T17:58:13Z
updated_at: 2023-04-26T03:45:38Z
url: https://github.com/astral-sh/ruff/pull/4098
synced_at: 2026-01-12T15:55:14Z
```

# Document that `--diff` implies `--fix-only`

---

_@dhruvmanila_

fixes: #4093 

---

_Comment by @github-actions[bot] on 2023-04-25 18:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     18.0±0.67ms     2.3 MB/sec    1.00     17.6±1.01ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.15ms     3.8 MB/sec    1.00      4.3±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   567.3±24.83µs     5.2 MB/sec    1.00   566.9±36.67µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.06      7.6±0.35ms     3.4 MB/sec    1.00      7.2±0.32ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.9±0.29ms     4.6 MB/sec    1.00      8.7±0.38ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1984.9±48.58µs     8.4 MB/sec    1.00  1971.1±68.64µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    242.7±7.63µs    12.2 MB/sec    1.00   237.9±18.62µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.1±0.11ms     6.2 MB/sec    1.00      4.0±0.10ms     6.4 MB/sec
parser/large/dataset.py                    1.01      7.1±0.21ms     5.8 MB/sec    1.00      7.0±0.47ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.05  1403.4±72.50µs    11.9 MB/sec    1.00  1340.3±48.84µs    12.4 MB/sec
parser/numpy/globals.py                    1.01    143.4±8.94µs    20.6 MB/sec    1.00    141.7±8.52µs    20.8 MB/sec
parser/pydantic/types.py                   1.06      3.1±0.12ms     8.2 MB/sec    1.00      2.9±0.12ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.24ms     2.4 MB/sec    1.01     17.2±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.02      4.4±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   538.6±18.67µs     5.5 MB/sec    1.00    540.9±6.92µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.08ms     3.5 MB/sec    1.00      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.09ms     4.6 MB/sec    1.00      8.8±0.06ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1912.0±26.79µs     8.7 MB/sec    1.02  1947.2±22.27µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.0±3.42µs    13.8 MB/sec    1.03    221.3±6.48µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.05ms     6.4 MB/sec    1.01      4.0±0.06ms     6.3 MB/sec
parser/large/dataset.py                    1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.9±0.05ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1303.7±14.14µs    12.8 MB/sec    1.01  1318.2±16.11µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    131.8±2.04µs    22.4 MB/sec    1.00    131.7±2.15µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.04ms     8.6 MB/sec    1.00      3.0±0.04ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-04-25 20:37_

It could also work if fix is set in the toml and not the CLI.

---

_Comment by @charliermarsh on 2023-04-25 23:59_

Hmm yeah, we may have to add error-handling outside of Clap, at runtime.

---

_Comment by @MichaReiser on 2023-04-26 00:23_

Thanks for looking into this issue.

If I understand this correctly, then there's no use case for using `--diff` without also using `--fix`. Do I understand this correctly? 

If so, wouldn't it be easier for user if we change the semantics of `--diff` to either automatically imply `--fix` and/or rename the option to e.g. `--fix-dry-run` (similar to [ESLint](https://eslint.org/docs/latest/use/command-line-interface#--fix-dry-run)) to make it clear that it computes the fixes? 

---

_Comment by @charliermarsh on 2023-04-26 00:28_

Hm yeah, at the very least, I think `--diff` should imply `--fix` or perhaps `--fix-only`.

I did look at `--fix-dry-run` at the time, but I think I opted for `--diff` to match Black's `--diff` option.

---

_Comment by @dhruvmanila on 2023-04-26 02:46_

> It could also work if fix is set in the toml and not the CLI.

I don't understand this as there's no `diff` option in configuration. This is (or was) about the CLI _only_ flag `--diff` 

---

> If I understand this correctly, then there's no use case for using `--diff` without also using `--fix`. Do I understand this correctly?

Yes

> If so, wouldn't it be easier for user if we change the semantics of `--diff` to either automatically imply `--fix` and/or rename the option to e.g. `--fix-dry-run` (similar to [ESLint](https://eslint.org/docs/latest/use/command-line-interface#--fix-dry-run)) to make it clear that it computes the fixes?

There's a `TODO` comment to enable it, but I think it's out of scope for this PR.

https://github.com/charliermarsh/ruff/blob/bff5d98121394c93bc9f82c45285517f85f71f41/crates/ruff_cli/src/lib.rs#L134-L136

---

> Hm yeah, at the very least, I think `--diff` should imply `--fix` or perhaps `--fix-only`.

Yes, I think it should imply `--fix-only` as it's to only show the fix diff and no violations are reported.

---

**tl;dr**, this should basically just be a document change to specify that `--diff` option implies `--fix-only`

---

_Renamed from "Make `--fix` required when `--diff` is specified" to "Document that `--diff` implies `--fix-only`" by @dhruvmanila on 2023-04-26 02:59_

---

_Merged by @charliermarsh on 2023-04-26 03:19_

---

_Closed by @charliermarsh on 2023-04-26 03:19_

---

_Comment by @charliermarsh on 2023-04-26 03:20_

Ahh right, sorry, you're correct. `--diff` _does_ imply `--fix-only`, it's just not documented.

---

_Branch deleted on 2023-04-26 03:45_

---
