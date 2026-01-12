```yaml
number: 18280
title: "[ty] use `__getattribute__` to lookup unknown members on a type"
type: pull_request
state: merged
author: felixscherz
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/call-dunder-getattribute
created_at: 2025-05-23T16:09:25Z
updated_at: 2025-05-26T10:59:45Z
url: https://github.com/astral-sh/ruff/pull/18280
synced_at: 2026-01-12T15:56:16Z
```

# [ty] use `__getattribute__` to lookup unknown members on a type

---

_@felixscherz_

Hi, this is in regards to https://github.com/astral-sh/ty/issues/441.
<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

`Type::member_lookup_with_policy` now falls back to calling `__getattribute__` when a member cannot be found as a second fallback after `__getattr__`.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added markdown tests.
<!-- How was it tested? -->


---

_Review requested from @carljm by @felixscherz on 2025-05-23 16:09_

---

_Review requested from @AlexWaygood by @felixscherz on 2025-05-23 16:09_

---

_Review requested from @sharkdp by @felixscherz on 2025-05-23 16:09_

---

_Review requested from @dcreager by @felixscherz on 2025-05-23 16:09_

---

_Comment by @github-actions[bot] on 2025-05-23 16:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- warning[possibly-unbound-attribute] pyinstrument/stack_sampler.py:278:16: Attribute `stack_sampler` on type `Unknown | _local` is possibly unbound
- Found 90 diagnostics
+ Found 89 diagnostics

anyio (https://github.com/agronholm/anyio)
- warning[possibly-unbound-attribute] src/anyio/_backends/_asyncio.py:2474:37: Attribute `current_cancel_scope` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/_backends/_asyncio.py:2506:32: Attribute `current_cancel_scope` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/from_thread.py:52:25: Attribute `current_async_backend` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/from_thread.py:53:17: Attribute `current_token` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/from_thread.py:74:25: Attribute `current_async_backend` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/from_thread.py:75:17: Attribute `current_token` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] src/anyio/from_thread.py:528:39: Attribute `current_async_backend` on type `Unknown | _local` is possibly unbound
- Found 119 diagnostics
+ Found 112 diagnostics

kornia (https://github.com/kornia/kornia)
- error[unresolved-attribute] kornia/feature/lightglue.py:429:12: Type `SimpleNamespace` has no attribute `input_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:429:30: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:430:41: Type `SimpleNamespace` has no attribute `input_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:430:57: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:434:20: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:434:43: Type `SimpleNamespace` has no attribute `num_heads`
- error[unresolved-attribute] kornia/feature/lightglue.py:436:21: Type `SimpleNamespace` has no attribute `add_scale_ori`
- error[unresolved-attribute] kornia/feature/lightglue.py:436:46: Type `SimpleNamespace` has no attribute `add_laf`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:19: Type `SimpleNamespace` has no attribute `num_heads`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:19: Type `SimpleNamespace` has no attribute `num_heads`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:19: Type `SimpleNamespace` has no attribute `num_heads`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:19: Type `SimpleNamespace` has no attribute `num_heads`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:35: Type `SimpleNamespace` has no attribute `n_layers`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:35: Type `SimpleNamespace` has no attribute `n_layers`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:35: Type `SimpleNamespace` has no attribute `n_layers`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:35: Type `SimpleNamespace` has no attribute `n_layers`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:50: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:50: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:50: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:441:50: Type `SimpleNamespace` has no attribute `descriptor_dim`
- error[unresolved-attribute] kornia/feature/lightglue.py:442:64: Type `SimpleNamespace` has no attribute `flash`
- error[unresolved-attribute] kornia/feature/lightglue.py:451:24: Type `SimpleNamespace` has no attribute `weights`
- Found 969 diagnostics
+ Found 947 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[unresolved-attribute] dulwich/cli.py:147:21: Type `FetchPackResult` has no attribute `items`
- Found 128 diagnostics
+ Found 127 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[unresolved-attribute] mkdocs/config/config_options.py:991:9: Type `SimpleNamespace` has no attribute `file`
- Found 208 diagnostics
+ Found 207 diagnostics

pyppeteer (https://github.com/pyppeteer/pyppeteer)
- warning[possibly-unbound-attribute] pyppeteer/browser.py:66:31: Attribute `Disconnected` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/browser.py:172:23: Attribute `TargetCreated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/browser.py:173:26: Attribute `TargetCreated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/browser.py:180:23: Attribute `TargetDestroyed` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/browser.py:181:40: Attribute `TargetDestroyed` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/browser.py:192:23: Attribute `TargetChanged` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/browser.py:193:40: Attribute `TargetChanged` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:79:19: Attribute `LifecycleEvent` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:86:19: Attribute `LifecycleEvent` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:120:19: Attribute `FrameAttached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:152:19: Attribute `FrameNavigated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:159:19: Attribute `FrameNavigatedWithinDocument` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:160:19: Attribute `FrameNavigated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/frame_manager.py:232:19: Attribute `FrameDetached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/navigator_watcher.py:32:17: Attribute `LifecycleEvent` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/navigator_watcher.py:37:17: Attribute `FrameNavigatedWithinDocument` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/navigator_watcher.py:42:17: Attribute `FrameDetached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/network_manager.py:240:19: Attribute `Response` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/network_manager.py:241:19: Attribute `RequestFinished` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/network_manager.py:258:19: Attribute `Request` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/network_manager.py:273:19: Attribute `Response` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/network_manager.py:286:19: Attribute `RequestFinished` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/network_manager.py:300:19: Attribute `RequestFailed` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:158:23: Attribute `WorkerCreated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:165:23: Attribute `WorkerDestroyed` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:172:16: Attribute `FrameAttached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:172:75: Attribute `FrameAttached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:173:16: Attribute `FrameDetached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:173:75: Attribute `FrameDetached` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:174:16: Attribute `FrameNavigated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:174:76: Attribute `FrameNavigated` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:177:16: Attribute `Request` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:177:71: Attribute `Request` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:178:16: Attribute `Response` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:178:72: Attribute `Response` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:179:16: Attribute `RequestFailed` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:179:77: Attribute `RequestFailed` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:180:16: Attribute `RequestFinished` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:180:79: Attribute `RequestFinished` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:182:72: Attribute `DOMContentLoaded` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:183:66: Attribute `Load` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:193:23: Attribute `Close` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:221:23: Attribute `Console` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:673:13: Attribute `Metrics` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:685:19: Attribute `PageError` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:716:31: Attribute `Console` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:730:19: Attribute `Console` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:736:26: Attribute `Alert` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:738:26: Attribute `Confirm` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:740:26: Attribute `Prompt` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:742:26: Attribute `BeforeUnload` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:744:19: Attribute `Dialog` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:825:73: Attribute `Request` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:900:13: Attribute `Response` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:943:35: Attribute `Request` on type `Unknown | SimpleNamespace` is possibly unbound
- warning[possibly-unbound-attribute] pyppeteer/page.py:977:35: Attribute `Response` on type `Unknown | SimpleNamespace` is possibly unbound
- Found 158 diagnostics
+ Found 102 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3317:5: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3368:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3369:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3370:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3373:13: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3377:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3378:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3379:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3381:13: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3399:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3400:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3401:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3405:13: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3407:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3408:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3409:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3413:13: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3414:9: Attribute `info` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3457:9: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3458:9: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3459:9: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3462:13: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3468:13: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3469:13: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- warning[possibly-unbound-attribute] ddtrace/vendor/ply/yacc.py:3477:17: Attribute `warning` on type `(Unknown & ~None) | PlyLogger | NullLogger` is possibly unbound
- Found 6843 diagnostics
+ Found 6818 diagnostics

zulip (https://github.com/zulip/zulip)
- warning[possibly-unbound-attribute] corporate/lib/stripe.py:413:23: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- warning[possibly-unbound-attribute] corporate/lib/stripe_event_handler.py:72:28: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- warning[possibly-unbound-attribute] corporate/views/upgrade.py:74:25: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- warning[possibly-unbound-attribute] corporate/views/upgrade.py:124:25: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- warning[possibly-unbound-attribute] corporate/views/upgrade.py:174:25: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- error[unresolved-attribute] zerver/actions/message_edit.py:346:17: Type `_StrPromise` has no attribute `format`
- error[unresolved-attribute] zerver/actions/message_edit.py:362:17: Type `_StrPromise` has no attribute `format`
- warning[possibly-unbound-attribute] zerver/forms.py:614:33: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- warning[possibly-unbound-attribute] zerver/lib/queue.py:430:12: Attribute `queue_client` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] zerver/tests/test_message_notification_emails.py:125:13: Attribute `strip` on type `str | _StrPromise` is possibly unbound
- warning[possibly-unbound-attribute] zerver/views/auth.py:836:48: Attribute `format` on type `Unknown | _StrPromise` is possibly unbound
- Found 6998 diagnostics
+ Found 6987 diagnostics

sympy (https://github.com/sympy/sympy)
- warning[possibly-unbound-attribute] sympy/combinatorics/permutations.py:3093:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/add.py:1273:16: Attribute `distribute` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/function.py:300:44: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/function.py:471:44: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/mul.py:304:26: Attribute `distribute` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/mul.py:729:13: Attribute `distribute` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:456:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:467:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:478:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:501:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1028:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1035:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1042:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1049:57: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1056:61: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1060:41: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1064:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1071:41: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1073:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1452:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1467:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1479:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1491:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1505:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1520:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1533:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1867:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1873:39: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1887:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1899:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1908:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1919:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1928:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1939:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1948:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:1957:12: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3054:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3063:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3075:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3086:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3215:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3224:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3236:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3247:42: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/numbers.py:3658:12: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/operations.py:95:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/operations.py:532:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/parameters.py:99:25: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/parameters.py:128:11: Attribute `distribute` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/parameters.py:153:11: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/power.py:139:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/power.py:1384:16: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/relational.py:636:44: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/relational.py:802:44: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/relational.py:868:44: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/sympify.py:431:20: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/core/tests/test_power.py:248:8: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/elementary/exponential.py:283:14: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/elementary/miscellaneous.py:382:48: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/elementary/piecewise.py:146:40: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/elementary/tests/test_exponential.py:29:8: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/elementary/tests/test_exponential.py:98:12: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/elementary/tests/test_exponential.py:187:12: Attribute `exp_is_pow` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/functions/special/hyper.py:211:35: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/geometry/ellipse.py:1542:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/geometry/point.py:110:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/logic/boolalg.py:1023:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/logic/boolalg.py:1315:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/series/sequences.py:1050:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/series/sequences.py:1162:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/sets/contains.py:36:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/sets/powerset.py:71:22: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/sets/sets.py:1357:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/sets/sets.py:1534:24: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/sets/sets.py:1983:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/simplify/radsimp.py:213:20: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/simplify/radsimp.py:544:20: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/simplify/simplify.py:406:20: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/stats/symbolic_probability.py:499:35: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- warning[possibly-unbound-attribute] sympy/tensor/functions.py:24:43: Attribute `evaluate` on type `Unknown | _global_parameters` is possibly unbound
- Found 18752 diagnostics
+ Found 18672 diagnostics

scipy (https://github.com/scipy/scipy)
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:54:18: Attribute `diff_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:124:18: Attribute `tilbert_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:170:18: Attribute `itilbert_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:237:18: Attribute `hilbert_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:275:18: Attribute `ihilbert_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:312:18: Attribute `cs_diff_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:367:18: Attribute `sc_diff_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:421:18: Attribute `ss_diff_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:479:18: Attribute `cc_diff_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/fftpack/_pseudo_diffs.py:524:18: Attribute `shift_cache` on type `(Unknown & _local) | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_dsolve/linsolve.py:97:8: Attribute `u` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_dsolve/linsolve.py:252:35: Attribute `u` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_dsolve/linsolve.py:576:8: Attribute `u` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_gcrotmk.py:33:5: Attribute `c` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_gcrotmk.py:40:5: Attribute `n` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_gcrotmk.py:53:5: Attribute `c` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_gcrotmk.py:57:15: Attribute `c` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_gcrotmk.py:69:9: Attribute `n` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_gcrotmk.py:75:16: Attribute `n` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_lgmres.py:35:5: Attribute `c` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_lgmres.py:42:5: Attribute `n` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_lgmres.py:53:5: Attribute `c` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_lgmres.py:58:15: Attribute `c` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_lgmres.py:70:9: Attribute `n` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/sparse/linalg/_isolve/tests/test_lgmres.py:76:16: Attribute `n` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_mannwhitneyu.py:470:9: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_mannwhitneyu.py:471:13: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_morestats.py:2821:37: Attribute `a` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_morestats.py:2822:37: Attribute `a` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_morestats.py:2826:20: Attribute `a` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_morestats.py:2828:20: Attribute `a` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_page_trend_test.py:413:5: Attribute `state` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/_page_trend_test.py:414:12: Attribute `state` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:377:17: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:378:33: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:383:33: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:384:35: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:385:35: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:388:23: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:392:17: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:393:24: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:635:9: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:638:17: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:641:25: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:646:9: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:648:17: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_hypotests.py:651:25: Attribute `s` on type `Unknown | _local` is possibly unbound
- warning[possibly-unbound-attribute] scipy/stats/tests/test_morestats.py:677:16: Attribute `a` on type `Unknown | _local` is possibly unbound
- Found 7811 diagnostics
+ Found 7763 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[unresolved-attribute] misc/python/materialize/scalability/executor/benchmark_executor.py:266:21: Type `_local` has no attribute `worker_id`
- Found 3217 diagnostics
+ Found 3216 diagnostics

```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1535 on 2025-05-23 16:14_

```suggestion
inferred type for all attributes, and matches other type checkers such as mypy and pyright.
```

---

_@AlexWaygood reviewed on 2025-05-23 16:15_

This looks excellent, thank you!

---

_Label `ty` added by @AlexWaygood on 2025-05-23 16:15_

---

_Comment by @AlexWaygood on 2025-05-23 16:28_

> ```diff
> paasta (https://github.com/yelp/paasta)
> - error[unresolved-attribute] paasta_tools/contrib/emit_allocated_cpu_metrics.py:36:16: Type `InstanceConfig & <Protocol with members 'get_instances'>` has no attribute `get_max_instances`
> - Found 934 diagnostics
> + Found 933 diagnostics
> ```

Hmm, this mypy_primer hit isn't correct. It looks like there's a pre-existing bug here (introduced by me!) in how we resolve member access on synthesized-protocol instances. This PR exposes the bug. Here's a minimal repro for the behaviour on your branch:

```py
from typing import reveal_type

def f(x: object):
    if hasattr(x, "foo"):
        reveal_type(x)  # revealed: <Protocol with members 'foo'>
        reveal_type(x.bar)  # revealed: Any
```

I think the issue is that we unconditionally fallback to attribute access on instances of `object` in the second branch here, regardless of what the attribute is: https://github.com/astral-sh/ruff/blob/a1399656c9109293781405c8e33f4bf27f1b8694/crates/ty_python_semantic/src/types/instance.rs#L297-L309

Since this bug hasn't been introduced by you, I don't think it should be your responsibility to fix it. This is especially true since it only affects edge cases that should be pretty rare. I can open a followup issue after this PR is merged to track fixing this bug.

---

_Comment by @felixscherz on 2025-05-23 16:37_

I will have a look at it in any case:)

---

_Comment by @AlexWaygood on 2025-05-23 16:39_

All the primer hits that don't involve synthesized protocols (which is most of them!) look great to me. They mostly consist of false-positive diagnostics going away because we didn't understand the effect of [`SimpleNamespace.__getattribute__`](https://github.com/python/typeshed/blob/ddf6c62482d328dba97fd8307399e63e81a41d18/stdlib/types.pyi#L330-L342) or [`_thread._local.__getattribute__`](https://github.com/python/typeshed/blob/ddf6c62482d328dba97fd8307399e63e81a41d18/stdlib/_thread.pyi#L108-L111) (`_thread._local` is the type returned by [`threading.local`](https://github.com/python/typeshed/blob/ddf6c62482d328dba97fd8307399e63e81a41d18/stdlib/threading.pyi#L73)) or [`django-stubs`'s `django.utils._StrPromise.__getattribute__`](https://github.com/typeddjango/django-stubs/blob/f9f8a19fb2ab5bb44ef1f90822ceb6a3aa3cd8df/django-stubs/utils/functional.pyi#L23-L38)

---

_Comment by @AlexWaygood on 2025-05-23 17:37_

This fixes the regression with attribute access on synthesized-protocol instances, but I'm not totally sure if it's the best fix:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index f674ed1b30..fcbef2a783 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -805,6 +805,13 @@ impl<'db> Type<'db> {
         matches!(self, Type::ClassLiteral(..))
     }
 
+    pub const fn into_protocol_instance(self) -> Option<ProtocolInstanceType<'db>> {
+        match self {
+            Type::ProtocolInstance(protocol_instance) => Some(protocol_instance),
+            _ => None,
+        }
+    }
+
     /// Turn a class literal (`Type::ClassLiteral` or `Type::GenericAlias`) into a `ClassType`.
     /// Since a `ClassType` must be specialized, apply the default specialization to any
     /// unspecialized generic class literal.
@@ -3202,6 +3209,13 @@ impl<'db> Type<'db> {
                         return Symbol::Unbound.into();
                     }
 
+                    if self
+                        .into_protocol_instance()
+                        .is_some_and(|proto| !proto.has_member(db, "__getattribute__"))
+                    {
+                        return Symbol::Unbound.into();
+                    }
+
                     // Typeshed has a `__getattribute__` method defined on `builtins.object` so we
                     // explicitly hide it here using `MemberLookupPolicy::MRO_NO_OBJECT_FALLBACK`.
                     self.try_call_dunder_with_policy(
diff --git a/crates/ty_python_semantic/src/types/instance.rs b/crates/ty_python_semantic/src/types/instance.rs
index e63a420cd1..075973bdd4 100644
--- a/crates/ty_python_semantic/src/types/instance.rs
+++ b/crates/ty_python_semantic/src/types/instance.rs
@@ -189,6 +189,10 @@ impl<'db> ProtocolInstanceType<'db> {
         }
     }
 
+    pub(super) fn has_member(self, db: &'db dyn Db, name: &str) -> bool {
+        self.inner.interface(db).has_member(db, name)
+    }
+
     /// Return the meta-type of this protocol-instance type.
     pub(super) fn to_meta_type(self, db: &'db dyn Db) -> Type<'db> {
         match self.inner {
diff --git a/crates/ty_python_semantic/src/types/protocol_class.rs b/crates/ty_python_semantic/src/types/protocol_class.rs
index 005ea95765..b09d745110 100644
--- a/crates/ty_python_semantic/src/types/protocol_class.rs
+++ b/crates/ty_python_semantic/src/types/protocol_class.rs
@@ -133,6 +133,13 @@ impl<'db> ProtocolInterface<'db> {
         }
     }
 
+    pub(super) fn has_member(self, db: &'db dyn Db, name: &str) -> bool {
+        match self {
+            Self::Members(members) => members.inner(db).contains_key(name),
+            Self::SelfReference => false,
+        }
+    }
+
     /// Return `true` if all members of this protocol are fully static.
     pub(super) fn is_fully_static(self, db: &'db dyn Db) -> bool {
         self.members(db).all(|member| member.ty.is_fully_static(db))
```

Curious if @sharkdp has any suggestions here -- he knows our member-access machinery better than anybody else 😄

---

_@AlexWaygood approved on 2025-05-23 17:38_

I'm in favour of landing this and fixing the synthesized-protocol regression as a followup, but I'll wait a bit before landing in case any other maintainers disagree!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3231 on 2025-05-26 09:57_

I think `__getattribute__` should take precedence over `__getattr__`?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3213 on 2025-05-26 10:00_

You also need to pass `MemberLookupPolicy::NO_INSTANCE_FALLBACK` here, because dunder methods should never be looked up on instances.

This is arguable a bad choice of API for `try_call_dunder_with_policy` (I will change that in a follow-up). `MemberLookupPolicy::NO_INSTANCE_FALLBACK` should always be implicitly added.

I also added a regression test that would have caught this.

---

_@sharkdp approved on 2025-05-26 10:04_

Thank you very much. I pushed some changes to address my review comments.

---

_Comment by @sharkdp on 2025-05-26 10:22_

> Curious if @sharkdp has any suggestions here

So... this was a bit of a rabbit hole :smile:.

> I think the issue is that we unconditionally fallback to attribute access on instances of `object` in the second branch here

This is problem number one. We should never look up dunder methods on instances. The problem was that `MemberLookupPolicy::NO_INSTANCE_FALLBACK` was not passed to `try_call_dunder_get_with_policy`.

After I fixed that, I ran into problem number two, which was that we still ended up retrieving `object.__getattribute__`, even though we were passing `MemberLookupPolicy::MRO_NO_OBJECT_FALLBACK`. I fixed that problem in https://github.com/astral-sh/ruff/pull/18312 (after fixing [another problem](https://github.com/astral-sh/ruff/pull/18313) that would have caused ecosystem false positives otherwise).

After that, the protocol-instance changes are gone from this PR.

By the way, the unconditional fallback to `object` for looking up instance members synthesized protocols looks fine to me.

---

_@felixscherz reviewed on 2025-05-26 10:42_

---

_Review comment by @felixscherz on `crates/ty_python_semantic/src/types.rs`:3231 on 2025-05-26 10:42_

Yes:)

---

_Comment by @felixscherz on 2025-05-26 10:43_

Thank you for the review!

---

_Merged by @sharkdp on 2025-05-26 10:59_

---

_Closed by @sharkdp on 2025-05-26 10:59_

---
