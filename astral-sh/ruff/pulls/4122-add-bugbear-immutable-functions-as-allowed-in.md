```yaml
number: 4122
title: Add bugbear immutable functions as allowed in dataclasses
type: pull_request
state: merged
author: mosauter
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-04-26T17:48:57Z
updated_at: 2023-04-28T01:23:06Z
url: https://github.com/astral-sh/ruff/pull/4122
synced_at: 2026-01-12T04:28:19Z
```

# Add bugbear immutable functions as allowed in dataclasses

---

_Pull request opened by @mosauter on 2023-04-26 17:48_

Fixes #4091 

Adds the "allowed" calls from `flake8-bugbear` to the allowed functions in dataclass defaults.

Compared to the things mentioned in #4091 it misses the type-alias for `MyId`. In code that means the following:

```python
MyId = int

@dataclass
class A:
    id: MyId = MyId(5)
```

... will still cause a RUF009 error. Mainly because Ruff can (AFAIR) not resolve the type-alias. Which is the same reason why I don't see a variant where we could detect if the call is a frozen and slotted dataclass, which would be semi-immutable. 

---

_Comment by @mosauter on 2023-04-26 17:51_

Should it also be configurable? In a sense that the user could supply more "allowed" functions via options? 

The bugbear variant has that ability but I'm not sure I like the doubled option for almost the same thing. (Although I have to say, I'm also not sure if I like it to be one option for both rules). Maybe it's time for merging those two rules...

---

_Marked ready for review by @mosauter on 2023-04-26 17:52_

---

_Comment by @github-actions[bot] on 2023-04-26 17:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.05ms     2.8 MB/sec    1.01     14.9±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.3±0.82µs     8.2 MB/sec    1.00    361.4±1.65µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.4 MB/sec    1.01      7.6±0.03ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1573.0±3.59µs    10.6 MB/sec    1.01   1592.7±3.29µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.1±0.30µs    17.2 MB/sec    1.01    172.6±0.26µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.6 MB/sec    1.02      3.4±0.02ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1147.4±1.10µs    14.5 MB/sec    1.00   1151.8±3.19µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    116.6±0.36µs    25.3 MB/sec    1.00    116.7±0.73µs    25.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.02ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     25.0±1.00ms  1667.4 KB/sec    1.02     25.4±1.07ms  1636.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      6.1±0.32ms     2.7 MB/sec    1.00      5.9±0.31ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.07   714.3±60.39µs     4.1 MB/sec    1.00   670.6±33.62µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.02     10.1±0.41ms     2.5 MB/sec    1.00      9.8±0.46ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.03     12.6±0.68ms     3.2 MB/sec    1.00     12.3±0.46ms     3.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06      2.6±0.13ms     6.3 MB/sec    1.00      2.5±0.10ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   299.5±19.14µs     9.9 MB/sec    1.03   308.2±22.22µs     9.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.5±0.26ms     4.6 MB/sec    1.00      5.5±0.23ms     4.6 MB/sec
parser/large/dataset.py                    1.02     10.0±0.39ms     4.1 MB/sec    1.00      9.8±0.28ms     4.2 MB/sec
parser/numpy/ctypeslib.py                  1.04  1914.8±99.39µs     8.7 MB/sec    1.00  1833.0±82.61µs     9.1 MB/sec
parser/numpy/globals.py                    1.04   196.2±17.77µs    15.0 MB/sec    1.00   189.5±14.80µs    15.6 MB/sec
parser/pydantic/types.py                   1.03      4.2±0.17ms     6.1 MB/sec    1.00      4.1±0.18ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-27 04:04_

Hmm, it looks like people _do_ use the bugbear setting to extend immutable calls: https://cs.github.com/?q=%22extend-immutable-calls%22%20language%3ATOML&scopeName=All%20repos&scope=.

My instinct then is to just respect that same setting here, even though it's a bit odd to have a "cross-linter" setting like that. In practice, it seems simplest, and most likely to be right for existing users.

---

_Comment by @mosauter on 2023-04-27 19:54_

> Hmm, it looks like people _do_ use the bugbear setting to extend immutable calls: https://cs.github.com/?q=%22extend-immutable-calls%22%20language%3ATOML&scopeName=All%20repos&scope=.
> 
> My instinct then is to just respect that same setting here, even though it's a bit odd to have a "cross-linter" setting like that. In practice, it seems simplest, and most likely to be right for existing users.

Added the option-recognition for that, so should be ready for review :)

---

_Merged by @charliermarsh on 2023-04-28 01:23_

---

_Closed by @charliermarsh on 2023-04-28 01:23_

---
