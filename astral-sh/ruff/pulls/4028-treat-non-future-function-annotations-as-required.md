```yaml
number: 4028
title: Treat non-future function annotations as required-at-runtime
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/func
created_at: 2023-04-19T18:24:43Z
updated_at: 2023-04-19T18:58:11Z
url: https://github.com/astral-sh/ruff/pull/4028
synced_at: 2026-01-12T04:28:19Z
```

# Treat non-future function annotations as required-at-runtime

---

_Pull request opened by @charliermarsh on 2023-04-19 18:24_

## Summary

It turns out that if `from __future__ import annotations` isn't enabled, then annotations on function arguments or return values _are_ required at runtime, as in this example:

```py
from collections.abc import Callable, Collection

def test():
    def nested(x: Callable) -> Callable[[Collection[str]], list[str]]:
        def double_nested(x: Collection[str]) -> list[str]:
            return list(x)

        return double_nested

    a = nested(None)
    return a(("a", "b"))

print(test())
```

To be consistent with our logic for this rule, we thus need to avoid raising `TCH` errors here.

We might want to change the semantics of the rule such that it tells you to move the types into a `TYPE_CHECKING` block _and_ quote them, but as of now, users that follow these suggestions will break their code. (As a corollary, these rules effectively require that you're using `from __future__ import annotations` to be useful.)

Closes #4020.


---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:602 on 2023-04-19 18:26_

(This applies to `AnnAssign` statements, not function definitions.)

---

_@charliermarsh reviewed on 2023-04-19 18:26_

---

_Comment by @github-actions[bot] on 2023-04-19 18:35_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -10, 0 error(s))

<details><summary>zulip (+0, -10)</summary>
<p>

```diff
- analytics/views/realm_activity.py:2:22: TCH003 Move standard library import `datetime.datetime` into a type-checking block
- tools/documentation_crawler/documentation_crawler/commands/crawl_with_status.py:5:28: TCH002 Move third-party import `scrapy.crawler.Crawler` into a type-checking block
- zerver/lib/display_recipient.py:3:30: TCH002 Move third-party import `django_stubs_ext.ValuesQuerySet` into a type-checking block
- zerver/lib/sqlalchemy_utils.py:8:27: TCH001 Move application import `zerver.lib.db.TimeTrackingConnection` into a type-checking block
- zerver/management/commands/process_queue.py:8:19: TCH003 Move standard library import `types.FrameType` into a type-checking block
- zerver/tests/test_auth_backends.py:89:30: TCH001 Move application import `zerver.lib.types.Validator` into a type-checking block
- zerver/tests/test_message_fetch.py:46:30: TCH001 Move application import `zerver.lib.types.DisplayRecipientT` into a type-checking block
- zerver/tests/test_subs.py:11:25: TCH002 Move third-party import `django.http.HttpResponse` into a type-checking block
- zerver/tests/test_webhooks_common.py:5:25: TCH002 Move third-party import `django.http.HttpRequest` into a type-checking block
- zerver/tests/test_webhooks_common.py:6:34: TCH002 Move third-party import `django.http.response.HttpResponse` into a type-checking block
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.01     14.3±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.8±1.74µs     6.5 MB/sec    1.01    458.2±0.87µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1611.8±1.48µs    10.3 MB/sec    1.00   1614.3±2.81µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.9±0.43µs    16.7 MB/sec    1.01    179.4±0.49µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.1 MB/sec    1.01      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1136.7±0.67µs    14.6 MB/sec    1.01   1146.1±6.68µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    112.5±0.16µs    26.2 MB/sec    1.01    113.4±0.37µs    26.0 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.3 MB/sec    1.01      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.08ms     2.5 MB/sec    1.01     16.2±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.04ms     3.9 MB/sec    1.00      4.2±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    447.0±5.02µs     6.6 MB/sec    1.00    438.1±6.18µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.04ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1819.3±11.98µs     9.2 MB/sec    1.00   1814.6±8.85µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.4±2.19µs    15.5 MB/sec    1.00    190.5±3.40µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.01      3.8±0.06ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.6±0.03ms     6.2 MB/sec    1.02      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1265.0±7.00µs    13.2 MB/sec    1.01   1281.4±9.12µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    127.7±1.00µs    23.1 MB/sec    1.01    129.1±1.34µs    22.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.1 MB/sec    1.01      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-19 18:43_

---

_Closed by @charliermarsh on 2023-04-19 18:43_

---

_Branch deleted on 2023-04-19 18:43_

---
