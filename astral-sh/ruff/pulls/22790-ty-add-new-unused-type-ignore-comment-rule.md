```yaml
number: 22790
title: "[ty] Add new `unused-type-ignore-comment` rule"
type: pull_request
state: open
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
base: main
head: micha/unused-type-ignore-comment
created_at: 2026-01-21T17:07:17Z
updated_at: 2026-01-21T17:28:19Z
url: https://github.com/astral-sh/ruff/pull/22790
synced_at: 2026-01-21T18:05:28Z
```

# [ty] Add new `unused-type-ignore-comment` rule

---

_@MichaReiser_

## Summary

This PR splits the `unused-ignore-comment` into two rules:

* `unused-ignore-comment` same as today, but it now only reports unused `ty: ignore` comments
* `unused-type-ignore-comment`: New, reports unused `type: ignore` comment


The separation allows projects using multiple type checkers to disable `unused-type-ignore-comments` 
so that ty doesn't complain about `type: ignore` directies that are needed by another type checker. 
But they can still use `unused-ignore-comments `to be warned about unused `ty: ignore` comments.


Fixes https://github.com/astral-sh/ty/issues/2494


## Test Plan

Updated tests


---

_Review requested from @carljm by @MichaReiser on 2026-01-21 17:07_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-21 17:07_

---

_Label `configuration` added by @MichaReiser on 2026-01-21 17:07_

---

_Label `ty` added by @MichaReiser on 2026-01-21 17:07_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-21 17:07_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-21 17:07_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-21 17:07_

---

_Comment by @astral-sh-bot[bot] on 2026-01-21 17:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-21 17:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
- pyp.py:20:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyp.py:20:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyp.py:637:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyp.py:637:63: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyp.py:639:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyp.py:639:53: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

zipp (https://github.com/jaraco/zipp)
- zipp/compat/py310.py:10:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ zipp/compat/py310.py:10:23: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/web.py:37:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/pegen/web.py:37:32: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

packaging (https://github.com/pypa/packaging)
- src/packaging/pylock.py:106:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:106:52: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:179:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:179:51: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:191:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:191:60: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:278:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:278:21: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:346:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:346:32: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:374:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:374:79: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:388:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:388:32: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:416:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:416:79: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:429:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:429:32: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:457:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:457:79: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:517:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:517:70: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:525:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:525:90: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:526:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:526:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:567:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:567:22: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:568:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:568:34: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:606:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/pylock.py:606:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:94:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/client.py:94:21: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/client.py:4116:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/client.py:4116:38: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/client.py:4152:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/client.py:4152:38: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:136:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/connection.py:136:50: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:806:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/connection.py:806:63: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/utils.py:45:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ aioredis/utils.py:45:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

parso (https://github.com/davidhalter/parso)
- parso/grammar.py:109:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ parso/grammar.py:109:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:116:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ parso/grammar.py:116:37: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:135:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ parso/grammar.py:135:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:147:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ parso/grammar.py:147:34: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:163:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ parso/grammar.py:163:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

dacite (https://github.com/konradhalas/dacite)
- dacite/config.py:8:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/config.py:8:44: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/config.py:11:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/config.py:11:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/data.py:2:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/data.py:2:39: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/data.py:4:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/data.py:4:50: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/dataclasses.py:17:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/dataclasses.py:17:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/dataclasses.py:18:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/dataclasses.py:18:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/generics.py:10:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/generics.py:10:55: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/generics.py:12:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/generics.py:12:66: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:5:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/types.py:5:36: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:7:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/types.py:7:47: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:47:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/types.py:47:38: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:62:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dacite/types.py:62:37: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:1128:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_asyncio.py:1128:64: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2439:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_asyncio.py:2439:43: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2616:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_asyncio.py:2616:72: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_trio.py:187:86: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_trio.py:187:86: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_trio.py:361:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_trio.py:361:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_fileio.py:502:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_fileio.py:502:20: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_fileio.py:693:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_fileio.py:693:21: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_streams.py:36:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_streams.py:36:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:295:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:295:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:298:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:298:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:342:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:342:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:356:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:356:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:363:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:363:43: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:423:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:423:40: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:451:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_core/_tempfile.py:451:49: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/from_thread.py:276:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/from_thread.py:276:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:213:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/functools.py:213:20: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:230:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/functools.py:230:82: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:234:13: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/functools.py:234:13: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:262:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/functools.py:262:17: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:319:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/functools.py:319:20: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/to_process.py:44:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/to_process.py:44:22: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

com2ann (https://github.com/ilevkivskyi/com2ann)
- src/com2ann.py:668:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/com2ann.py:668:26: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

paroxython (https://github.com/laowantong/paroxython)
- paroxython/parse_program.py:288:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ paroxython/parse_program.py:288:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

nionutils (https://github.com/nion-software/nionutils)
- nion/utils/StructuredModel.py:247:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ nion/utils/StructuredModel.py:247:56: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- nion/utils/ThreadPool.py:30:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ nion/utils/ThreadPool.py:30:39: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/__main__.py:284:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/__main__.py:284:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/__main__.py:612:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/__main__.py:612:77: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/frame.py:346:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/frame.py:346:89: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/magic/magic.py:54:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/magic/magic.py:54:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/magic/magic.py:289:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/magic/magic.py:289:91: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/middleware.py:37:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/middleware.py:37:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/middleware.py:108:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/middleware.py:108:50: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/middleware.py:112:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/middleware.py:112:50: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/processors.py:90:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/processors.py:90:63: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/renderers/jsonrenderer.py:17:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/renderers/jsonrenderer.py:17:81: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/stack_sampler.py:102:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/stack_sampler.py:102:83: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive

kornia (https://github.com/kornia/kornia)
- kornia/augmentation/_2d/mix/base.py:194:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/_2d/mix/base.py:194:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_2d/mix/transplantation.py:196:119: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/_2d/mix/transplantation.py:196:119: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_2d/mix/transplantation.py:202:118: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/_2d/mix/transplantation.py:202:118: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_2d/mix/transplantation.py:299:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/_2d/mix/transplantation.py:299:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_3d/mix/transplantation.py:24:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/_3d/mix/transplantation.py:24:76: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/auto/base.py:44:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/auto/base.py:44:89: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:301:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:301:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:322:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:322:85: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:324:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:324:90: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:337:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:337:51: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:344:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:344:99: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:428:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:428:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:444:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:444:85: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:446:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:446:90: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:466:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:466:53: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:474:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/augment.py:474:99: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/base.py:323:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/base.py:323:51: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/ops.py:151:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/ops.py:151:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/ops.py:153:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/ops.py:153:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/ops.py:512:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/ops.py:512:30: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:170:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:170:55: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:262:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:262:85: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:337:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:337:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:347:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:347:26: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:355:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:355:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:363:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:363:25: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:371:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:371:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:379:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:379:25: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:387:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:387:31: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:395:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:395:29: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:403:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:403:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:419:109: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/patch.py:419:109: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:288:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/video.py:288:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:308:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/video.py:308:25: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:328:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/video.py:328:31: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:351:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/container/video.py:351:29: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/presets/ada.py:246:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/presets/ada.py:246:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/presets/ada.py:290:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/presets/ada.py:290:35: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/random_generator/base.py:111:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/random_generator/base.py:111:72: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/random_generator/base.py:117:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/augmentation/random_generator/base.py:117:71: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/constants.py:35:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/constants.py:35:44: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/constants.py:38:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/constants.py:38:59: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/boxmot_tracker.py:128:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/boxmot_tracker.py:128:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/boxmot_tracker.py:141:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/boxmot_tracker.py:141:44: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/edge_detection.py:123:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/edge_detection.py:123:71: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/edge_detection.py:166:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/edge_detection.py:166:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/edge_detection.py:174:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/edge_detection.py:174:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:189:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/object_detection.py:189:71: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:224:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/object_detection.py:224:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:232:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/object_detection.py:232:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:281:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/object_detection.py:281:38: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:79:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:79:73: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:123:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:123:19: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:130:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:130:30: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:171:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:171:58: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:175:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:175:58: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:179:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:179:58: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:183:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/super_resolution.py:183:58: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:356:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/visual_prompter.py:356:44: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:366:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/visual_prompter.py:366:52: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:376:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/visual_prompter.py:376:51: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:388:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/visual_prompter.py:388:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:397:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/visual_prompter.py:397:51: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:406:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/contrib/visual_prompter.py:406:53: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:69:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:69:40: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:72:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:72:60: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:117:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:117:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:119:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:119:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:141:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:141:40: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:145:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:145:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:146:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:146:82: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:149:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:149:48: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:161:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:161:40: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:163:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:163:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:164:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:164:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:167:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:167:49: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:181:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:181:59: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:184:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:184:74: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:187:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:187:40: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:189:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:189:41: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:199:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:199:99: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:226:108: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:226:108: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:229:108: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/image_module.py:229:108: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:76:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:76:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:133:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:133:29: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:134:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:134:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:135:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:135:27: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:139:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:139:46: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:147:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:147:47: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:172:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:172:31: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:174:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:174:66: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:175:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:175:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:190:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:190:50: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:191:101: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:191:101: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:192:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:192:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:199:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:199:68: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:209:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:209:53: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:274:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:274:67: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:296:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:296:44: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:298:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:298:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:311:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:311:47: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:317:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:317:46: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:319:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:319:34: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:339:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:339:40: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:341:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:341:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:360:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:360:92: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:363:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:363:87: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:368:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:368:91: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:374:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:374:31: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:387:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:387:45: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:391:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:391:31: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:393:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:393:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:408:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:408:31: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:411:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:411:28: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:423:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:423:47: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:424:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:424:81: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:426:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:426:73: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:429:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/core/mixin/onnx.py:429:53: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/decoder.py:33:121: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/decoder.py:33:121: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/decoder.py:65:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/decoder.py:65:20: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/decoder.py:101:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/decoder.py:101:24: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/dedode.py:234:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/dedode.py:234:50: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/dedode.py:235:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/dedode.py:235:54: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/descriptor.py:31:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/descriptor.py:31:91: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/detector.py:31:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/detector.py:31:91: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/detector.py:55:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/detector.py:55:34: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:36:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/encoder.py:36:64: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:41:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/encoder.py:41:52: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:79:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/encoder.py:79:42: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:92:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/encoder.py:92:63: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/transformer/layers/attention.py:93:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/transformer/layers/attention.py:93:62: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/transformer/layers/drop_path.py:39:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/transformer/layers/drop_path.py:39:34: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/utils.py:42:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/utils.py:42:75: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/utils.py:43:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/dedode/utils.py:43:87: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/integrated.py:482:93: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/integrated.py:482:93: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:42:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/lightglue.py:42:33: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:158:88: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/feature/lightglue.py:158:88: warning[unused-type-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:167:72: warning[unused-ignore-comment] Unused blanket `type

... (truncated 17995 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Converted to draft by @MichaReiser on 2026-01-21 17:17_

---

_Marked ready for review by @MichaReiser on 2026-01-21 17:28_

---
