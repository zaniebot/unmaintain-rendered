```yaml
number: 5704
title: Use permalinks in ecosystem diff references
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: ci/ecosystem-permalinks
created_at: 2023-07-12T05:02:02Z
updated_at: 2023-07-12T07:05:00Z
url: https://github.com/astral-sh/ruff/pull/5704
synced_at: 2026-01-12T15:55:19Z
```

# Use permalinks in ecosystem diff references

---

_@zanieb_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Closes https://github.com/astral-sh/ruff/issues/5702

cc @dhruvmanila as I see you assigned yourself to the issue

## Test Plan

<!-- How was it tested? -->

Ran locally e.g.

`<a href='https://github.com/zulip/zulip/blob/fcede324202ac5bc3de0691d2d6e168d0f18c120/zproject/prod_settings_template.py#L144'>zproject/prod_settings_template.py:144:26:</a>`

Should succeed in CI as well.


---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:95 on 2023-07-12 05:06_

nit
```suggestion
            *["git", "rev-parse", "HEAD"],
```

---

_@zanieb reviewed on 2023-07-12 05:06_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:06_

So... because `Repository` is immutable (and I liked that it was immutable) we can't just get the commit SHA on clone and stash it on the `Repository` object. Instead, we yield it and it's attached to the `Diff` object for later display. It also turns out that the previously yielded `Path` was identical to the input argument so I removed that.

We probably ought to just have the ecosystem checks pinned to a specific SHA that's updated on a schedule by a bot and then we can skip the retrieval and just assign it to the `Repository` at creation time. That seems pretty low priority though.

---

_@zanieb reviewed on 2023-07-12 05:07_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:73 on 2023-07-12 05:07_

I renamed these before extracting `_get_commit` into a separate function. I don't think the extra clarity hurts though.

---

_Review requested from @konstin by @zanieb on 2023-07-12 05:07_

---

_Review requested from @dhruvmanila by @zanieb on 2023-07-12 05:07_

---

_@zanieb reviewed on 2023-07-12 05:11_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:11_

Also happy to hear alternative approaches if anyone has ideas! This seemed liked the simplest way to retain immutability but this was my first time looking at this script.

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:11_

Can we add the commit SHA to the `Repository` object as that's where I think it should be logically? I think adding it to `ref` should be the same meaning as branches are just pointers to specific commits.

---

_@dhruvmanila reviewed on 2023-07-12 05:13_

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:13_

This will also mean that the `repo.url_for` function/call need not be changed.

---

_@dhruvmanila reviewed on 2023-07-12 05:13_

---

_@zanieb reviewed on 2023-07-12 05:15_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:15_

I tried that first but we cannot because it's immutable. We definitely could decide to make it _mutable_, but I liked the immutable design. More commentary at https://github.com/astral-sh/ruff/pull/5704#discussion_r1260592025

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:95 on 2023-07-12 05:16_

Interestingly this worked locally but failed with `ambiguous argument 'head': unknown revision or path not in the working tree` in CI — curious to see if this changes anything but I doubt it?

---

_@zanieb reviewed on 2023-07-12 05:16_

---

_@dhruvmanila reviewed on 2023-07-12 05:16_

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:16_

Oh, I missed that comment. Give me a minute.

---

_@zanieb reviewed on 2023-07-12 05:19_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:87 on 2023-07-12 05:19_

idk how this got formatted like this but will fix -.-

---

_@zanieb reviewed on 2023-07-12 05:21_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:95 on 2023-07-12 05:21_

Failure at https://github.com/astral-sh/ruff/actions/runs/5527736701/jobs/10083885033

---

_@dhruvmanila reviewed on 2023-07-12 05:24_

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:24_

We could [replace](https://docs.python.org/3/library/collections.html#collections.somenamedtuple._replace) the value:
```python
self._replace(ref=await self._get_commit(checkout_dir))
```

But, otherwise I think this is fine.

---

_@dhruvmanila approved on 2023-07-12 05:25_

---

_@dhruvmanila reviewed on 2023-07-12 05:29_

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:95 on 2023-07-12 05:29_

Interesting! https://stackoverflow.com/a/56346962

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:29_

I'm not sure how we'd get the new `Repository` object back to the relevant owner without a bunch of changes.

---

_@zanieb reviewed on 2023-07-12 05:29_

---

_@dhruvmanila reviewed on 2023-07-12 05:32_

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:48 on 2023-07-12 05:32_

Ah yeah, didn't think about that (`_replace` returns a new object). I think this is fine then. We can iterate it later.

---

_Review comment by @dhruvmanila on `scripts/check_ecosystem.py`:89 on 2023-07-12 05:33_

```suggestion
        url = (
            f"https://github.com/{self.org}/{self.repo}"
            f"/blob/{commit_sha}/{path}"
        )
```

---

_@dhruvmanila reviewed on 2023-07-12 05:33_

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:95 on 2023-07-12 05:36_

That resolved it :)

---

_@zanieb reviewed on 2023-07-12 05:36_

---

_Comment by @dhruvmanila on 2023-07-12 05:38_

Oh shoot! Sorry, I fixed the wrong long line :p 

---

_Comment by @zanieb on 2023-07-12 05:39_

No problem :)

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:96 on 2023-07-12 05:49_

you can use `subprocess.check_output` with `text=True` here, it will also check the output status automatically 

https://github.com/astral-sh/ruff/blob/0666added9902990b6ea22d1a48f6fa1ae5ea605/scripts/update_schemastore.py#L18

---

_@konstin approved on 2023-07-12 05:49_

---

_Comment by @github-actions[bot] on 2023-07-12 05:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      8.0±0.04ms     5.1 MB/sec    1.00      7.9±0.01ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1874.2±4.39µs     8.9 MB/sec    1.00   1857.7±3.28µs     9.0 MB/sec
formatter/numpy/globals.py                 1.01    208.7±0.42µs    14.1 MB/sec    1.00    207.3±0.52µs    14.2 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.02ms     6.3 MB/sec    1.00      4.0±0.03ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.17ms     3.0 MB/sec    1.00     13.6±0.24ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.04ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    433.1±0.94µs     6.8 MB/sec    1.00    429.4±3.15µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.08ms     4.2 MB/sec    1.00      6.0±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.04ms     6.0 MB/sec    1.00      6.7±0.07ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1473.1±2.64µs    11.3 MB/sec    1.00   1457.8±2.44µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    168.6±0.25µs    17.5 MB/sec    1.00    164.7±0.26µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.04ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.5±0.74ms     3.2 MB/sec    1.01     12.7±0.56ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.16ms     5.8 MB/sec    1.00      2.8±0.19ms     5.9 MB/sec
formatter/numpy/globals.py                 1.00   322.7±27.64µs     9.1 MB/sec    1.02   328.1±25.67µs     9.0 MB/sec
formatter/pydantic/types.py                1.00      6.4±0.42ms     4.0 MB/sec    1.00      6.3±0.40ms     4.0 MB/sec
linter/all-rules/large/dataset.py          1.00     21.7±1.01ms  1918.4 KB/sec    1.03     22.3±0.91ms  1865.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.7±0.28ms     2.9 MB/sec    1.00      5.6±0.32ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   672.7±51.92µs     4.4 MB/sec    1.01   679.1±42.70µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.03     10.0±0.57ms     2.6 MB/sec    1.00      9.7±0.46ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.57ms     3.7 MB/sec    1.00     10.9±0.62ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.4±0.27ms     7.1 MB/sec    1.00      2.3±0.34ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   270.8±19.50µs    10.9 MB/sec    1.01   274.8±20.89µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.29ms     5.2 MB/sec    1.02      5.0±0.28ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @zanieb on `scripts/check_ecosystem.py`:96 on 2023-07-12 05:54_

Hm I'd rather not make a blocking call in an async function though 👿 

---

_@zanieb reviewed on 2023-07-12 05:54_

---

_Merged by @zanieb on 2023-07-12 06:26_

---

_Closed by @zanieb on 2023-07-12 06:26_

---

_Branch deleted on 2023-07-12 06:26_

---

_Review comment by @konstin on `scripts/check_ecosystem.py`:96 on 2023-07-12 07:04_

fair

---

_@konstin reviewed on 2023-07-12 07:05_

---
