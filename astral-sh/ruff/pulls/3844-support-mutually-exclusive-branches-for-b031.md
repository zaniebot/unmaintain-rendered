```yaml
number: 3844
title: "Support mutually exclusive branches for `B031`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/b031-false-positive
created_at: 2023-04-01T05:03:58Z
updated_at: 2023-04-04T03:14:03Z
url: https://github.com/astral-sh/ruff/pull/3844
synced_at: 2026-01-12T15:55:13Z
```

# Support mutually exclusive branches for `B031`

---

_@dhruvmanila_

fixes: #3801 

---

_Comment by @github-actions[bot] on 2023-04-01 05:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     15.2±0.18ms     2.7 MB/sec    1.00     14.6±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    409.8±1.84µs     7.2 MB/sec    1.01    415.6±1.81µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.5±0.03ms     3.9 MB/sec    1.00      6.4±0.06ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.02ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1736.6±7.11µs     9.6 MB/sec    1.01   1750.4±2.65µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.2±0.89µs    16.3 MB/sec    1.01    183.2±0.50µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.00ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.1±0.14ms     2.5 MB/sec    1.00     15.7±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.05ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.5±7.28µs     6.0 MB/sec    1.01    499.2±7.28µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.09ms     3.7 MB/sec    1.00      6.7±0.12ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.06ms     4.9 MB/sec    1.00      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1802.4±20.99µs     9.2 MB/sec    1.00  1791.8±17.55µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    196.4±7.92µs    15.0 MB/sec    1.00    192.9±5.07µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.07ms     6.8 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-04-01 09:08_

The recursive case is not handled correctly.
```python
for _section, section_items in itertools.groupby(items, key=lambda p: p[1]):
    # Mutually exclusive branches shouldn't trigger the warning
    if _section == "greens":
        collect_shop_items(shopper, section_items)
        if _section == "greens":
            collect_shop_items(shopper, section_items)  # B031 false negative
        elif _section == "frozen items":
            collect_shop_items(shopper, section_items)  # B031 false negative
        else:
            collect_shop_items(shopper, section_items)  # B031 false negative
    elif _section == "frozen items":
        collect_shop_items(shopper, section_items)  # B031 false positive
    else:
        collect_shop_items(shopper, section_items)  # B031 false positive
    # Now, it should detect
    collect_shop_items(shopper, section_items)  # B031
```

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_bugbear/B031.py`:104 on 2023-04-01 14:20_

We may be missing "multiple usages within the same arm, top-level":

```py
for _section, section_items in itertools.groupby(items, key=lambda p: p[1]):
    match _section:
        case "greens":
            collect_shop_items(shopper, section_items)
            collect_shop_items(shopper, section_items)
```

This seems like it should trigger B031, but doesn't.

---

_@charliermarsh reviewed on 2023-04-01 14:20_

---

_@charliermarsh reviewed on 2023-04-01 14:21_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:69 on 2023-04-01 14:21_

My intuition (without reading the code deeply) is that this might need to be handled as a stack, that we push and pop from as we enter and leave branchable code?

---

_@dhruvmanila reviewed on 2023-04-02 04:17_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_bugbear/B031.py`:104 on 2023-04-02 04:17_

Yes, this should be triggered, let me take a look at that.

---

_@dhruvmanila reviewed on 2023-04-02 04:36_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:69 on 2023-04-02 04:36_

Yes, I'm thinking of using a stack but it's getting a bit complicated so might take some time to resolve this.

---

_Converted to draft by @dhruvmanila on 2023-04-02 04:36_

---

_Comment by @dhruvmanila on 2023-04-03 13:37_

I'll give an explanation of the logic behind this in a couple of hours.

---

_Marked ready for review by @dhruvmanila on 2023-04-03 13:37_

---

_Comment by @dhruvmanila on 2023-04-04 01:57_

***Using the stack model:***

A new stack element is pushed for every mutually exclusive statement (`if`/`match` statement, not `elif`/`else`/`case`) statements. This stack element is `Vec<u8>` where each number represents the count of group in individual branch. They're in the same order as that of branches or case blocks in case of `match` statements.

Once we're out of a mutually exclusive statement, the final count will be the max value from every branch/case. This will be added to either the parent statement or the global count if there isn't any.

![image](https://user-images.githubusercontent.com/67177269/229582657-b8f800dc-758c-45f4-8e1a-6717cb51e89f.png)


---

_Review requested from @charliermarsh by @dhruvmanila on 2023-04-04 01:58_

---

_Comment by @charliermarsh on 2023-04-04 02:27_

Thank you! I continue to be so impressed by your contributions, really appreciate your help improving Ruff.

---

_Merged by @charliermarsh on 2023-04-04 02:33_

---

_Closed by @charliermarsh on 2023-04-04 02:33_

---

_Comment by @dhruvmanila on 2023-04-04 03:13_

> Thank you! I continue to be so impressed by your contributions, really appreciate your help improving Ruff.

Hey, thanks! I really enjoy contributing to the project :)

---

_Branch deleted on 2023-04-04 03:14_

---
