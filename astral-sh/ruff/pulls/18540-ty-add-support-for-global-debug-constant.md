```yaml
number: 18540
title: "[ty] Add support for global `__debug__` constant"
type: pull_request
state: merged
author: suneettipirneni
labels:
  - ty
assignees: []
merged: true
base: main
head: feature/__debug__-support
created_at: 2025-06-07T20:28:53Z
updated_at: 2025-06-11T10:45:41Z
url: https://github.com/astral-sh/ruff/pull/18540
synced_at: 2026-01-12T15:56:21Z
```

# [ty] Add support for global `__debug__` constant

---

_@suneettipirneni_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes https://github.com/astral-sh/ty/issues/577. Make global `__debug__` a `bool` constant.

## Test Plan

<!-- How was it tested? -->

Mdtest `global-constants.md` was created to check if resolved type was `bool`.


---

_Review requested from @carljm by @suneettipirneni on 2025-06-07 20:28_

---

_Review requested from @AlexWaygood by @suneettipirneni on 2025-06-07 20:28_

---

_Review requested from @sharkdp by @suneettipirneni on 2025-06-07 20:28_

---

_Review requested from @dcreager by @suneettipirneni on 2025-06-07 20:28_

---

_Comment by @github-actions[bot] on 2025-06-07 21:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- error[unresolved-reference] beartype/_util/py/utilpyinterpreter.py:59:12: Name `__debug__` used when not defined
- Found 571 diagnostics
+ Found 570 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[unresolved-reference] comtypes/_post_coinit/unknwn.py:31:8: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/server/register.py:375:30: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:32:21: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:710:12: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:725:12: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/codegenerator.py:734:12: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/helpers.py:124:12: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/helpers.py:228:12: Name `__debug__` used when not defined
- error[unresolved-reference] comtypes/tools/codegenerator/helpers.py:287:12: Name `__debug__` used when not defined
- Found 595 diagnostics
+ Found 586 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[unresolved-reference] src/_pytest/assertion/rewrite.py:58:21: Name `__debug__` used when not defined
- Found 776 diagnostics
+ Found 775 diagnostics

jinja (https://github.com/pallets/jinja)
- error[unresolved-reference] src/jinja2/runtime.py:274:12: Name `__debug__` used when not defined
- Found 226 diagnostics
+ Found 225 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[unresolved-reference] mitmproxy/http.py:189:8: Name `__debug__` used when not defined
- error[unresolved-reference] mitmproxy/proxy/utils.py:17:12: Name `__debug__` used when not defined
- error[unresolved-reference] mitmproxy/utils/asyncio_utils.py:46:8: Name `__debug__` used when not defined
- Found 2050 diagnostics
+ Found 2047 diagnostics

rotki (https://github.com/rotki/rotki)
- error[unresolved-reference] rotkehlchen/api/server.py:435:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/api/server.py:501:16: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/chain/evm/decoding/base.py:70:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/chain/evm/decoding/base.py:79:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/chain/evm/decoding/base.py:92:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/chain/evm/decoding/base.py:104:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:346:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1468:8: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:47:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:57:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:61:16: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:65:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:86:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:95:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:104:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:107:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:115:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:118:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:137:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:140:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:145:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:150:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:155:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:158:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:191:8: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:209:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:212:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:284:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:287:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:292:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:295:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:303:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:306:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:312:16: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:317:20: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:322:16: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:327:20: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:490:16: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/drivers/gevent.py:496:16: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/db/updates.py:80:12: Name `__debug__` used when not defined
- error[unresolved-reference] rotkehlchen/history/events/structures/base.py:137:12: Name `__debug__` used when not defined
- Found 2034 diagnostics
+ Found 1993 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[unresolved-reference] tests/native/test_abi.py:482:12: Name `__debug__` used when not defined
- error[unresolved-reference] tests/native/test_armv7rf.py:32:12: Name `__debug__` used when not defined
- Found 1229 diagnostics
+ Found 1227 diagnostics

```
</details>


---

_Label `ty` added by @ntBre on 2025-06-08 03:36_

---

_@sharkdp approved on 2025-06-10 06:43_

Thank you!

---

_Merged by @sharkdp on 2025-06-10 06:48_

---

_Closed by @sharkdp on 2025-06-10 06:48_

---

_Renamed from "[ty] Add support for global __debug__ constant" to "[ty] Add support for global `__debug__` constant" by @dhruvmanila on 2025-06-11 10:45_

---
