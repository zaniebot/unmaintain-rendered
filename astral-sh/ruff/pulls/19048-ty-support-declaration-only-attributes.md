```yaml
number: 19048
title: "[ty] Support declaration-only attributes"
type: pull_request
state: merged
author: iyakushev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: support-decl-only
created_at: 2025-06-30T15:42:53Z
updated_at: 2025-07-07T14:55:40Z
url: https://github.com/astral-sh/ruff/pull/19048
synced_at: 2026-01-12T15:56:30Z
```

# [ty] Support declaration-only attributes

---

_@iyakushev_

## Summary

Following ty issue [#698](https://github.com/astral-sh/ty/issues/698) this PR adds support for declarations.   
I'd say the implementation is rather naive, so I would use it as a starting point for a gradual improvement. I tried to check declarations before bindings and it seemed to resolve the issue. 

closes #698

## Test Plan

Tested against mdtest (specifically attributes). 


---

_Review requested from @carljm by @iyakushev on 2025-06-30 15:42_

---

_Review requested from @AlexWaygood by @iyakushev on 2025-06-30 15:42_

---

_Review requested from @sharkdp by @iyakushev on 2025-06-30 15:42_

---

_Review requested from @dcreager by @iyakushev on 2025-06-30 15:42_

---

_Label `ty` added by @ntBre on 2025-06-30 15:47_

---

_Comment by @github-actions[bot] on 2025-06-30 16:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- error[unresolved-attribute] src/trio/_tests/test_ssl.py:106:8: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] src/trio/_tests/test_ssl.py:108:10: Type `FixtureRequest` has no attribute `param`
- Found 801 diagnostics
+ Found 799 diagnostics

kornia (https://github.com/kornia/kornia)
- error[unresolved-attribute] kornia/nerf/samplers.py:230:13: Unresolved attribute `_x` on type `Points2D_FlatTensors`.
- error[unresolved-attribute] kornia/nerf/samplers.py:231:13: Unresolved attribute `_y` on type `Points2D_FlatTensors`.
- error[unresolved-attribute] kornia/nerf/samplers.py:233:13: Unresolved attribute `_x` on type `Points2D_FlatTensors`.
- error[unresolved-attribute] kornia/nerf/samplers.py:233:57: Type `Points2D_FlatTensors` has no attribute `_x`
- error[unresolved-attribute] kornia/nerf/samplers.py:234:13: Unresolved attribute `_y` on type `Points2D_FlatTensors`.
- error[unresolved-attribute] kornia/nerf/samplers.py:234:57: Type `Points2D_FlatTensors` has no attribute `_y`
- error[unresolved-attribute] kornia/nerf/samplers.py:259:30: Type `Points2D_FlatTensors` has no attribute `_x`
- error[unresolved-attribute] kornia/nerf/samplers.py:259:58: Type `Points2D_FlatTensors` has no attribute `_y`
- Found 800 diagnostics
+ Found 792 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

yarl (https://github.com/aio-libs/yarl)
- error[unresolved-attribute] tests/test_quoting.py:18:16: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] tests/test_quoting.py:22:16: Type `FixtureRequest` has no attribute `param`
+ warning[unused-ignore-comment] tests/test_quoting.py:32:31: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] tests/test_quoting.py:36:31: Unused blanket `type: ignore` directive

flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~66MB
+     memo fields = ~60MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~54MB
+     memo fields = ~49MB

websockets (https://github.com/aaugustin/websockets)
- error[unresolved-attribute] src/websockets/asyncio/router.py:89:13: Unresolved attribute `handler` on type `ServerConnection`.
- error[unresolved-attribute] src/websockets/asyncio/router.py:89:33: Unresolved attribute `handler_kwargs` on type `ServerConnection`.
- error[unresolved-attribute] src/websockets/asyncio/router.py:94:26: Type `ServerConnection` has no attribute `handler`
- error[unresolved-attribute] src/websockets/asyncio/router.py:94:59: Type `ServerConnection` has no attribute `handler_kwargs`
- error[unresolved-attribute] src/websockets/asyncio/server.py:980:9: Unresolved attribute `username` on type `ServerConnection`.
- error[unresolved-attribute] src/websockets/sync/router.py:89:13: Unresolved attribute `handler` on type `ServerConnection`.
- error[unresolved-attribute] src/websockets/sync/router.py:89:33: Unresolved attribute `handler_kwargs` on type `ServerConnection`.
- error[unresolved-attribute] src/websockets/sync/router.py:94:20: Type `ServerConnection` has no attribute `handler`
- error[unresolved-attribute] src/websockets/sync/router.py:94:53: Type `ServerConnection` has no attribute `handler_kwargs`
- error[unresolved-attribute] src/websockets/sync/server.py:762:9: Unresolved attribute `username` on type `ServerConnection`.
- Found 65 diagnostics
+ Found 55 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[unresolved-attribute] tests/console/commands/test_init.py:316:31: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] tests/installation/test_installer.py:117:48: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] tests/installation/test_installer.py:118:11: Type `FixtureRequest` has no attribute `param`
- Found 939 diagnostics
+ Found 936 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
-     memo fields = ~129MB
+     memo fields = ~142MB

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[unresolved-attribute] tests/conftest.py:283:12: Type `FixtureRequest` has no attribute `param`
+ warning[unused-ignore-comment] tests/test___all__.py:26:27: Unused blanket `type: ignore` directive
- error[unresolved-attribute] tests/test__cli.py:204:20: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] tests/test_gather.py:174:24: Type `FixtureRequest` has no attribute `param`
- Found 26 diagnostics
+ Found 24 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[unresolved-attribute] src/urllib3/connectionpool.py:553:13: Type `BaseHTTPResponse` has no attribute `length_remaining`
- error[unresolved-attribute] test/conftest.py:289:28: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] test/conftest.py:373:8: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] test/conftest.py:383:15: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] test/conftest.py:385:12: Type `FixtureRequest` has no attribute `param`
- Found 438 diagnostics
+ Found 433 diagnostics

scrapy (https://github.com/scrapy/scrapy)
-     memo fields = ~189MB
+     memo fields = ~207MB

psycopg (https://github.com/psycopg/psycopg)
- error[unresolved-attribute] psycopg_pool/psycopg_pool/base.py:201:9: Unresolved attribute `_expire_at` on type `BaseConnection[Any]`.
- Found 562 diagnostics
+ Found 561 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[unresolved-attribute] src/_pytest/python.py:1110:12: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] testing/test_pathlib.py:120:17: Type `FixtureRequest` has no attribute `param`
- Found 527 diagnostics
+ Found 525 diagnostics

altair (https://github.com/vega/altair)
-     memo fields = ~228MB
+     memo fields = ~251MB

rotki (https://github.com/rotki/rotki)
- error[no-matching-overload] rotkehlchen/tests/api/test_eth2.py:302:9: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_eth2.py:1091:12: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_eth2.py:1382:5: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_ethereum_transactions.py:1422:5: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_ethereum_transactions.py:1469:5: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_ethereum_transactions.py:1528:9: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_sushiswap.py:59:5: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/api/test_uniswap.py:72:5: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/data_migrations/test_migrations.py:550:9: No overload of function `get_decoded_events_of_transaction` matches arguments
- warning[possibly-unbound-attribute] rotkehlchen/tests/integration/test_eth2.py:414:96: Attribute `query_withdrawals` on type `Unknown | Blockscout | None` is possibly unbound
- error[no-matching-overload] rotkehlchen/tests/unit/decoders/test_degen.py:33:17: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/unit/decoders/test_degen.py:78:17: No overload of function `get_decoded_events_of_transaction` matches arguments
- error[no-matching-overload] rotkehlchen/tests/unit/test_evm_balances.py:43:5: No overload of function `get_decoded_events_of_transaction` matches arguments
- Found 1728 diagnostics
+ Found 1715 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
-     memo fields = ~539MB
+     memo fields = ~593MB

scipy (https://github.com/scipy/scipy)
- error[unresolved-attribute] scipy/_lib/array_api_extra/tests/conftest.py:33:26: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] scipy/_lib/array_api_extra/tests/conftest.py:205:16: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] scipy/_lib/array_api_extra/tests/conftest.py:215:18: Type `FixtureRequest` has no attribute `param`
- error[unresolved-attribute] scipy/conftest.py:443:10: Type `FixtureRequest` has no attribute `param`
- Found 6624 diagnostics
+ Found 6620 diagnostics
- TOTAL MEMORY USAGE: ~1271MB
+ TOTAL MEMORY USAGE: ~1399MB

```
</details>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-30 16:41_

---

_Comment by @sharkdp on 2025-06-30 21:06_

Thank you for working on this! I am planning to take a closer look tomorrow.

It looks like this leads to a lot new false positives on ecosystem projects (see the auto-generated mypy_primer comment). We should probably try to find out what causes them (+ fix them and write a few more test cases here).

---

_Comment by @iyakushev on 2025-07-01 09:01_

Thank you @sharkdp! I will try to add some test cases following mypy_primer results tomorrow. Is it okay to add them into mdtests? 
As for false positives, I think its due to [this line](https://github.com/astral-sh/ruff/pull/19048/files#diff-a4d813e6b59314ddbc2448d1133969548760611a9de2d07a0c36a0cbe2c93ed3R1819). I mostly copied the way bindings did it while getting familiar with the codebase. 

---

_Comment by @sharkdp on 2025-07-01 09:31_

> Thank you @sharkdp! I will try to add some test cases following mypy_primer results tomorrow. Is it okay to add them into mdtests?

Yes. Feel free to add some new sections in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/attributes.md

> As for false positives, I think its due to [this line](https://github.com/astral-sh/ruff/pull/19048/files#diff-a4d813e6b59314ddbc2448d1133969548760611a9de2d07a0c36a0cbe2c93ed3R1819). I mostly copied the way bindings did it while getting familiar with the codebase.

That particular part will change with https://github.com/astral-sh/ruff/pull/18955 where we basically decide not to track possible unboundness (and with your PR: undeclaredness) anymore for implicit instance attributes. You could maybe try rebasing your branch on top of that already.

---

_Review requested from @MichaReiser by @iyakushev on 2025-07-01 16:55_

---

_Converted to draft by @iyakushev on 2025-07-01 16:59_

---

_Comment by @iyakushev on 2025-07-01 17:45_

@sharkdp Please take a look when available. It seems to me that most of the errors (false positives) in the original implementation come from class instance access to its attributes via `self` if they have some form of annotation.
```python
class C:
    def __init__(self):
        self.foo: list[int] = []
        
    def bar(self):
        print(self.foo)  # Previously possibly-unbound 
```
I've added a few mdtest to validate this (and marked them with TODO). There's definitely something wrong; either its beyond the original scope of the issue, or I'm misusing something. Either way, your guidance would be very valuable 😃 
I've changed implementation as well, it only checks declarations at the very end if we have no defined bindings.

---

_Comment by @sharkdp on 2025-07-02 15:34_

> Please take a look when available. It seems to me that most of the errors (false positives) in the original implementation come from class instance access to its attributes via `self` if they have some form of annotation

> I've added a few mdtest to validate this (and marked them with TODO). 

I don't understand? Non of the new MD tests seems to show some kind of possibly-unbound false positive?

> I've changed implementation as well, it only checks declarations at the very end if we have no defined bindings.

I think we should switch that and check declarations first. If we have a declaration, we should prefer it over any bindings.

---

_Comment by @iyakushev on 2025-07-03 18:38_

> Non of the new MD tests seems to show some kind of possibly-unbound false positive?

Sorry for the ambiguity in my previous message. I've tried to replicate the warnings I saw in `mypy_primer` repos, but after I've pulled the new changes (The PR you mentioned + main), the issue seems to be resolved. I've double checked against the chess repo snippet from `primer`, but I did not contribute it to the mdtest. The changes in the mdtest are for broader testing.

I've moved the declaration check above bindings and fixed another potential issue with Class obj/instance access to attributes. In my previous attempt instance attributes were leaking into class objects even if they were declared only  in `__init__`.

Could we, perhaps, run `mypy_primer` once again?

---

_Comment by @codspeed-hq[bot] on 2025-07-04 09:59_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/iyakushev%3Asupport-decl-only?runnerMode=WallTime)

### Merging #19048 will **not alter performance**

<sub>Comparing <code>iyakushev:support-decl-only</code> (a6206f1) with <code>main</code> (b6edfbc)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Renamed from "[ty] Support scoped declarations " to "[ty] Support declarations-only attributes" by @iyakushev on 2025-07-04 10:06_

---

_Renamed from "[ty] Support declarations-only attributes" to "[ty] Support declaration-only attributes" by @iyakushev on 2025-07-04 10:07_

---

_Marked ready for review by @iyakushev on 2025-07-05 07:56_

---

_@sharkdp reviewed on 2025-07-07 08:41_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:26 on 2025-07-07 08:41_

It's true that this should be `list[Unknown]`, but this has nothing to do with this PR. We currently still infer `[]` as `list[Unknown]`, and we use that inferred type for the local use of `declared_and_bound_generic` here. I wouldn't expect generic types to play any special role here, so I think we can remove this test (and the type-assertions below) again.

---

_@sharkdp reviewed on 2025-07-07 08:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:36 on 2025-07-07 08:43_

I don't understand the purpose of the first test here. And the second test doesn't work yet because we don't infer the type of `self` yet. I think this can also be removed again.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1766 on 2025-07-07 08:44_

Do you see a chance to avoid the code duplication here? This looks almost identical to what we have for bindings below.

---

_@sharkdp reviewed on 2025-07-07 08:44_

---

_@sharkdp requested changes on 2025-07-07 08:44_

Thank you very much. This looks great. Just a few minor comments.

---

_Comment by @iyakushev on 2025-07-07 09:41_

Thank you for taking time to review this! 
I've pushed the changes addressing your comments. As for duplication, I've moved the code into a closure, if that's ok. The name might be a little off imho.

---

_Review requested from @sharkdp by @iyakushev on 2025-07-07 09:42_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-07 10:25_

---

_@sharkdp approved on 2025-07-07 10:47_

This looks great — thank you very much!

---

_Merged by @sharkdp on 2025-07-07 10:55_

---

_Closed by @sharkdp on 2025-07-07 10:55_

---

_Branch deleted on 2025-07-07 14:55_

---
