```yaml
number: 4891
title: "Make `C413` fix as suggested for `reversed` call"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/C413/autofix-with-key
created_at: 2023-06-06T04:32:01Z
updated_at: 2023-06-08T03:08:04Z
url: https://github.com/astral-sh/ruff/pull/4891
synced_at: 2026-01-12T03:43:29Z
```

# Make `C413` fix as suggested for `reversed` call

---

_Pull request opened by @dhruvmanila on 2023-06-06 04:32_

## Summary

~When the `key` keyword argument is present, the output for `sorted` and `reversed` will be different. This is especially true when the elements are itself a collection. The former will sort only as per the given key while the latter will sort using all the elements present in the collection. This could lead to different output for the suggested auto-fix.~

Look at the comments and the linked issue for description but the gist is that the fix for `reversed` call will be `Suggestion` while for others it'll be `Automatic`.

## Alternative

We could flag the auto-fix as `Suggested`, but I would rather not flag it as the diagnostic itself is incorrect.

## Test Plan

`cargo test`

fixes: #4879 


---

_Comment by @github-actions[bot] on 2023-06-06 04:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11      6.4±0.02ms     6.4 MB/sec    1.00      5.7±0.03ms     7.1 MB/sec
formatter/numpy/ctypeslib.py               1.03   1229.0±4.58µs    13.5 MB/sec    1.00  1196.4±14.61µs    13.9 MB/sec
formatter/numpy/globals.py                 1.00    132.1±0.22µs    22.3 MB/sec    1.04    137.5±0.18µs    21.5 MB/sec
formatter/pydantic/types.py                1.07      2.7±0.01ms     9.3 MB/sec    1.00      2.5±0.01ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.01     14.0±0.19ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    422.1±0.41µs     7.0 MB/sec    1.00    418.6±0.85µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.03      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1451.2±3.74µs    11.5 MB/sec    1.00   1457.1±5.27µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.9±0.32µs    18.3 MB/sec    1.01    162.4±0.63µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.08ms     6.1 MB/sec    1.02      6.9±0.06ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1297.4±23.27µs    12.8 MB/sec    1.08  1399.3±34.73µs    11.9 MB/sec
formatter/numpy/globals.py                 1.00    141.4±3.14µs    20.9 MB/sec    1.13    159.2±5.35µs    18.5 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.04ms     8.8 MB/sec    1.09      3.2±0.05ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.26ms     2.5 MB/sec    1.02     16.8±0.29ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.02      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    494.4±8.14µs     6.0 MB/sec    1.00    488.0±6.91µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.01      7.1±0.19ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.13ms     4.8 MB/sec    1.00      8.2±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1769.5±22.96µs     9.4 MB/sec    1.00  1719.4±12.77µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    197.8±3.34µs    14.9 MB/sec    1.00    195.7±3.91µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-06 14:38_

---

_Comment by @charliermarsh on 2023-06-07 01:21_

My understanding was that the bug wasn't related to `key`, but rather, was related to the difference in behavior between `reversed(...)` and sorting with `reverse=True` -- but I may be mistaken...

In the former case, you're just exactly reversing the order of elements in the list. In the latter, you're performing a stable sort. The difference comes down to how it breaks ties.

Notice in the issue:

```py
>>> original = [(1, 0), (2, 0), (3, 1), (4, 1)]
>>> print(list(reversed(sorted(original, key=lambda x: x[1]))))
[(4, 1), (3, 1), (2, 0), (1, 0)]
>>> print(sorted(original, key=lambda x: x[1], reverse=True))
[(3, 1), (4, 1), (1, 0), (2, 0)]
```

With `reverse=True`, `(3, 1)` comes before `(4, 1)` -- that is, elements with the same sort key (`1`) appear in the same order after sorting, i.e., it's a "stable" sort. With `reversed`, you're just flipping the entire list, so elements naturally appear out-of-relative-order.

My inclination here is actually to continue flagging this, but make it a suggested rather than an automatic fix with we rewrite `reversed` to `reverse=True`.


---

_Comment by @dhruvmanila on 2023-06-07 13:22_

Isn't the reason the sorting is different because of the `key` argument? If you remove the `key` argument, it results in the same order:

```python
>>> original = [(1, 0), (2, 0), (3, 1), (4, 1)]
>>> list(reversed(sorted(original)))
[(4, 1), (3, 1), (2, 0), (1, 0)]
>>> sorted(original, reverse=True)
[(4, 1), (3, 1), (2, 0), (1, 0)]
```

The fact that `key` is present means the sorting is done by comparing against whatever element the key function returns which, if it's a collection, could be any. This is the reason I thought the diagnostic itself is incorrect both could result in different order in the presence of `key`. I just realize that this should only be true in the `reversed` case and not with `list`.

I'm open to suggestion and if you think otherwise I'm happy to update the PR accordingly.

---

_Comment by @charliermarsh on 2023-06-07 13:25_

But the key is causing us to sort based on comparing the second element of each tuple — hence the introduction of ties, since multiple tuples have 0 and 1 as their second element. Without the key, the ties are broken based on the data alone, so the stable vs. unstable sort distinction is irrelevant. I think it still comes down to whether the sort is stable, not whether the key is customized.

---

_Comment by @dhruvmanila on 2023-06-07 16:33_

Oh, got it. I'll update the PR accordingly.

---

_Comment by @charliermarsh on 2023-06-07 17:20_

Does my explanation make sense though? Do you think it’s correct?

---

_Comment by @dhruvmanila on 2023-06-07 18:17_

> Does my explanation make sense though? Do you think it’s correct?

Yes, the fact that the code is correct but the output _might_ be different makes this a valid diagnostic but _could_ be an incorrect fix.

---

_Renamed from "Avoid flagging `C413` for `key` is present for `sorted`" to "Make `C413` fix as suggested for `reversed` call" by @dhruvmanila on 2023-06-07 18:28_

---

_Merged by @charliermarsh on 2023-06-07 22:23_

---

_Closed by @charliermarsh on 2023-06-07 22:23_

---

_Comment by @charliermarsh on 2023-06-07 22:23_

Thanks :)

---

_Branch deleted on 2023-06-08 03:08_

---
