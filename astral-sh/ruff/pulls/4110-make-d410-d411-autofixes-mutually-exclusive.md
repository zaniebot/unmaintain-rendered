```yaml
number: 4110
title: Make D410/D411 autofixes mutually exclusive
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: 3827_docstring_autofix
created_at: 2023-04-26T02:42:31Z
updated_at: 2023-04-28T18:56:38Z
url: https://github.com/astral-sh/ruff/pull/4110
synced_at: 2026-01-12T15:55:14Z
```

# Make D410/D411 autofixes mutually exclusive

---

_@evanrittenhouse_

This should close #3827.

I tested by having a `test.py` file (repro'd from the issue):
```
def pow(self, exponent: int | float):
    """
    Raise expression to the power of the given exponent.

    Parameters
    ----------
    exponent
        ...
    Examples
    --------
    >>> df = pl.DataFrame({"foo": [1, 2, 3, 4]})

    """
    ...

```

I then ran `cargo run -p ruff_cli -- check test.py --fix --select D410 --select D411` and ensured that only one new line was created between `...` and `Examples`. As a sanity check, I also reset the file and performed the same test with no whitespace after `>>> df...`.

---

_Comment by @github-actions[bot] on 2023-04-26 02:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.03ms     2.7 MB/sec    1.00     14.8±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.04    376.8±1.41µs     7.8 MB/sec    1.00    362.6±0.72µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.01ms     5.4 MB/sec    1.00      7.6±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1587.8±4.35µs    10.5 MB/sec    1.00   1587.7±6.45µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.8±0.64µs    17.2 MB/sec    1.00    172.6±0.33µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.00ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.02      6.2±0.00ms     6.6 MB/sec    1.00      6.1±0.00ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1157.0±1.06µs    14.4 MB/sec    1.02   1182.1±3.08µs    14.1 MB/sec
parser/numpy/globals.py                    1.00    117.9±0.27µs    25.0 MB/sec    1.02    120.4±0.42µs    24.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.01      2.6±0.00ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.4±0.60ms     2.3 MB/sec    1.00     17.3±0.29ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.09ms     3.9 MB/sec    1.00      4.2±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    496.3±6.22µs     5.9 MB/sec    1.00   493.6±20.83µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.16ms     3.5 MB/sec    1.00      7.2±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.12ms     4.7 MB/sec    1.00      8.7±0.10ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1822.5±28.57µs     9.1 MB/sec    1.01  1839.6±24.42µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    206.4±6.58µs    14.3 MB/sec    1.00    206.9±7.60µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.07ms     6.6 MB/sec    1.01      3.9±0.06ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.8±0.08ms     6.0 MB/sec    1.00      6.8±0.06ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1303.7±17.47µs    12.8 MB/sec    1.01  1319.7±17.39µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    132.4±2.00µs    22.3 MB/sec    1.01    133.7±3.04µs    22.1 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.7 MB/sec    1.01      3.0±0.05ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-04-26 02:56_

---

_Comment by @charliermarsh on 2023-04-26 15:15_

Can we think of any way to fix this by ensuring that the generated fixes are identical? That way, we'll deduplicate them when we go to apply the generated fixes.

The issue with the approach in here is that we won't generate a fix at all for one of the two violations, IIUC, which means that in the VS Code extension and elsewhere, we won't show a code action for users to automatically fix it.

---

_Comment by @evanrittenhouse on 2023-04-26 15:25_

Oh interesting, I didn't know we deduplicate fixes before applying. Sure. 

The ranges of the two insertions should be identical, so I could add fixes to both diagnostics, then check ranges and remove one before returning from the checking function. Does that approach make sense?

---

_Comment by @charliermarsh on 2023-04-26 15:39_

I think the goal should be that the two rules independently produce an identical fix in this case, and that we return both diagnostics with their respective fixes, and defer to the fix-applied to deduplicate. Is that possible to achieve?

---

_Comment by @evanrittenhouse on 2023-04-26 23:36_

Should be! I basically changed `NoBlankLineAfterSection` to insert the newline at the first character of the next line (where `NoBlankLineBeforeSection` does) to avoid conflicts. I'll have to rebase against `main` to fix conflicts with #3931, I think, which will change the implementation a bit.

---

_Comment by @evanrittenhouse on 2023-04-27 18:58_

@charliermarsh you're too quick, was going to finish it out after work today! :D

---

_Comment by @charliermarsh on 2023-04-27 18:59_

Haha no worries. Trying to see if I can remove the iterator clone, but I have a few meetings so I'll return to this later or you can beat me to it. Will merge tonight.

---

_Label `bug` added by @charliermarsh on 2023-04-28 01:20_

---

_Label `docstring` added by @charliermarsh on 2023-04-28 01:20_

---

_Merged by @charliermarsh on 2023-04-28 01:24_

---

_Closed by @charliermarsh on 2023-04-28 01:24_

---

_Comment by @evanrittenhouse on 2023-04-28 18:56_

Clean solution with the `peek()` @charliermarsh 

---

_Branch deleted on 2023-04-28 18:56_

---
