```yaml
number: 4766
title: Replace deletion-tracking with enforced isolation levels
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/deletion
created_at: 2023-05-31T21:08:35Z
updated_at: 2023-06-02T03:10:27Z
url: https://github.com/astral-sh/ruff/pull/4766
synced_at: 2026-01-12T03:50:03Z
```

# Replace deletion-tracking with enforced isolation levels

---

_Pull request opened by @charliermarsh on 2023-05-31 21:08_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We tend to apply all of our edits in one pass, similar to ESLint's model. However, there's one common case where it's not safe to apply edits in this way: deleting statements from indented bodies, where we run the risk of leading behind an empty body (and, instead, should leave behind a `pass` statement).

For example, if we remove both imports here independently, we'll generate a syntax error:

```py
if True:
  import os
  import sys
```

Historically, the way we solved this problem was by tracking statement deletions in the AST checker. So, when we generate the edit to delete `import os`, we'll mark that statement as deleted. When we go to delete `import sys`, we'll note that deleting this statement will leave the body empty (assuming all edits are applied), and instead create an edit to replace the statement with a `pass` (rather than deleting it outright).

This makes the edit process stateful, since we're generating edits based on assumptions about how the code will change when _other_ edits are applied. It also leads to subtle bugs when edits end up being applied in isolation, as in the LSP -- e.g., removing the second import here erroneously leaves the user with a `pass`:

https://github.com/charliermarsh/ruff/assets/1309177/a4d743cc-138d-46ae-8b41-89ba9602c740

One alternative solution would be to only apply one edit at a time, then re-run Ruff, etc. We tried this previously, and it can really hurt performance for large codebases, since your we have to run Ruff as up to the maximum number of fixable errors in your file.

This PR proposes a middle-ground solution, whereby fixes can declare themselves as require isolation. We only apply one isolation-requiring fix per run (though we will batch it with other, non-isolation-requiring fixes). It's rare that fixes require isolation, but in short, any fix that involves a statement deletion within a nested block must be isolated. (We already tracked these anyway, since we have a common helper for it.)


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-31 21:08_

---

_Review requested from @konstin by @charliermarsh on 2023-05-31 21:08_

---

_Comment by @MichaReiser on 2023-05-31 21:14_

How does this affect performance. Do you know how rare is i practice?

---

_Comment by @charliermarsh on 2023-05-31 21:17_

I'm not sure yet, but one thing I'm going to do to improve performance is add a key to the isolation groups, so that we can fix both of these in one pass:

```py
if False:
  import os

if False:
  import sys
```

So the maximum number of runs will be equal to the maximum number of deletions within any indent block (as opposed to the total number of deletions across all indented blocks in the file).


---

_Comment by @github-actions[bot] on 2023-05-31 21:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.08ms     2.9 MB/sec    1.01     14.4±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.18ms     4.8 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    422.7±0.46µs     7.0 MB/sec    1.00    420.2±0.51µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.03      7.0±0.03ms     5.8 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1517.6±3.71µs    11.0 MB/sec    1.00   1491.8±2.43µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    172.3±0.63µs    17.1 MB/sec    1.00    168.8±0.36µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.02ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1008.9±0.44µs    16.5 MB/sec    1.00   1012.3±1.19µs    16.4 MB/sec
parser/numpy/globals.py                    1.00    105.3±0.18µs    28.0 MB/sec    1.00    105.1±0.26µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.35ms     2.4 MB/sec    1.00     16.6±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.12ms     3.9 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   514.4±17.81µs     5.7 MB/sec    1.00    500.6±9.34µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.16ms     3.6 MB/sec    1.00      7.0±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.13ms     4.9 MB/sec    1.00      8.3±0.14ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1766.1±35.76µs     9.4 MB/sec    1.00  1772.4±34.86µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    208.0±6.54µs    14.2 MB/sec    1.00    207.6±6.95µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.07ms     6.8 MB/sec    1.00      3.8±0.08ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.4 MB/sec    1.02      6.5±0.09ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.01  1229.8±31.22µs    13.5 MB/sec    1.00  1217.9±24.88µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    124.4±2.08µs    23.7 MB/sec    1.01    125.9±2.48µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.06ms     9.2 MB/sec    1.01      2.8±0.07ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-01 03:12_

Ok, I improved the performance (and added some commentary) in #4774. (I want to keep them as separate PRs, since they can both stand on their own.)

---

_Comment by @charliermarsh on 2023-06-01 04:01_

It looks like in CPython, there are 143 unused imports in nested blocks, but many of these are tied to the same statement (e.g., `crates/ruff/resources/test/cpython/Lib/subprocess.py` reports 18 unused imports in nested block, but they're all part of one import-from statement, and so are fixed in one pass).

The worst offender is `crates/ruff/resources/test/cpython/Lib/test/test_import/__init__.py`, which has 34 unused imports, all of which seem to come from separate statements. (It's a file that includes a bunch of imports within tests, to test the imports themselves.)

Most other files that have at least one unused import in a nested block have ~1-4 such imports. So it's pretty rare.

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_type_checking/rules/empty_type_checking_block.rs`:56 on 2023-06-01 06:31_

(not your code) i'm surprised this didn't trigger clippy

---

_@konstin approved on 2023-06-01 06:32_

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/edits.rs`:180 on 2023-06-01 06:33_

Nit: Does `matches!(vec.as_slice(), [value])` work?

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:71 on 2023-06-01 06:34_

Nit
```suggestion
        if fix.isolation().is_isolated() {
```

---

_Comment by @konstin on 2023-06-01 06:37_

Instead of setting isolation as a binary flag, would it be possible to set Isolation with a text range or a dummy edit on the parent node?
```rust
if True:
  import os
  import sys

if 1==1:
  import re
  import platform
```
The os and sys edits could mark the whole `if True` node as affected, the re and platform edits on the `if 1==1`. We would then apply the os and re edit in one pass (not inserting a pass) and the sys and platform edit in the second one (inserting a pass)

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:70 on 2023-06-01 06:40_

Oh, this is interesting. I expected that we run `isolated` fixes in full isolation -> We apply no other fixes in that pass. But that's not what we do. Is there another word that may describe this behavior better? 

I guess I expected `Serializable` isolation (using SQL terminology) and our default isolation level is `NO_OVERLAPPING`. The way I think about this new isolation level is similar to NON_CONCURRENT but that feels wrong too. Hmm

On naming:
* Should we rename `Isolation` to `IsolationLevel`
* Should we rename the default isolation to `NoOverlapping` (or similar) because all fixes have an isolation. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:5290 on 2023-06-01 06:40_

Yeeess! Weird magic `fix.content() == Some("pass")` is gone

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/rules/unused_variable.rs`:218 on 2023-06-01 06:42_

Thanks for categorizing the fixes

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:151 on 2023-06-01 06:43_

Hehe, `Isolation`... level ;)

---

_@MichaReiser approved on 2023-06-01 06:43_

Nice. I love how this removes complexity throughout the whole code base.

---

_Comment by @charliermarsh on 2023-06-01 20:35_

@konstin - Yes! I ended up doing this in https://github.com/charliermarsh/ruff/pull/4774.

---

_Comment by @charliermarsh on 2023-06-01 20:36_

We could probably go even further if we're willing to accept more heuristics. E.g., we could check if the body contains at least one statement that we would _never_ delete (huge hack), we could avoid isolation levels...

---

_@charliermarsh reviewed on 2023-06-02 02:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/edits.rs`:180 on 2023-06-02 02:34_

No, `matches!(vec, [v] if v == value)` does, though is that better?

---

_Merged by @charliermarsh on 2023-06-02 02:45_

---

_Closed by @charliermarsh on 2023-06-02 02:45_

---

_Branch deleted on 2023-06-02 02:45_

---
