```yaml
number: 22593
title: "[ty] Try fixing conformance workflow"
type: pull_request
state: merged
author: WillDuke
labels:
  - ci
  - testing
assignees: []
merged: true
base: main
head: wld/conformance-bugfix
created_at: 2026-01-15T08:56:01Z
updated_at: 2026-01-15T21:16:38Z
url: https://github.com/astral-sh/ruff/pull/22593
synced_at: 2026-01-15T22:02:06Z
```

# [ty] Try fixing conformance workflow

---

_@WillDuke_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I'm guessing that the funky results are a path normalization issue that wasn't apparent locally since the keys never match.

## Test Plan

<!-- How was it tested? -->

Bit of a chicken and egg problem on this one.


---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Label `ci` added by @MichaReiser on 2026-01-15 09:05_

---

_Label `testing` added by @MichaReiser on 2026-01-15 09:05_

---

_Comment by @MichaReiser on 2026-01-15 09:05_

You'll need to add the `conformance.py` here or it won't run as part of your PR

https://github.com/astral-sh/ruff/blob/522a5ce85c677ac975981d0724cec0c96cda2396/.github/workflows/typing_conformance.yaml#L6-L20

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Typing conformance

No changes



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Review requested from @carljm by @WillDuke on 2026-01-15 09:22_

---

_Review requested from @AlexWaygood by @WillDuke on 2026-01-15 09:22_

---

_Review requested from @sharkdp by @WillDuke on 2026-01-15 09:22_

---

_Review requested from @dcreager by @WillDuke on 2026-01-15 09:22_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 09:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- zipp/compat/py310.py:10:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 4 diagnostics
+ Found 3 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- pyp.py:20:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyp.py:637:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyp.py:639:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5 diagnostics
+ Found 2 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/web.py:37:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 48 diagnostics
+ Found 47 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:1128:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2439:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:2616:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_trio.py:187:86: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_trio.py:361:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_fileio.py:502:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_fileio.py:693:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_streams.py:36:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:295:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:298:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:342:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:356:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:363:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:423:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_tempfile.py:451:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/from_thread.py:276:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:213:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:230:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:234:13: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:262:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/functools.py:319:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/to_process.py:44:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 92 diagnostics
+ Found 70 diagnostics

packaging (https://github.com/pypa/packaging)
- src/packaging/pylock.py:106:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:179:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:191:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:278:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:346:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:374:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:388:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:416:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:429:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:457:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:517:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:525:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:526:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:567:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:568:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/packaging/pylock.py:606:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 23 diagnostics
+ Found 7 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
- src/com2ann.py:668:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 10 diagnostics
+ Found 9 diagnostics

dacite (https://github.com/konradhalas/dacite)
- dacite/config.py:8:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/config.py:11:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/data.py:2:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/data.py:4:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/dataclasses.py:17:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/dataclasses.py:18:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/generics.py:10:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/generics.py:12:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:5:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:7:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:47:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dacite/types.py:62:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 22 diagnostics
+ Found 10 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:94:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/client.py:4116:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/client.py:4152:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:136:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:806:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/utils.py:45:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 30 diagnostics
+ Found 24 diagnostics

parso (https://github.com/davidhalter/parso)
- parso/grammar.py:109:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:116:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:135:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:147:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- parso/grammar.py:163:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 199 diagnostics
+ Found 194 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- paroxython/parse_program.py:288:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 7 diagnostics
+ Found 6 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- nion/utils/StructuredModel.py:247:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- nion/utils/ThreadPool.py:30:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 23 diagnostics
+ Found 21 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/__main__.py:284:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/__main__.py:612:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/frame.py:346:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/magic/magic.py:54:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/magic/magic.py:289:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/middleware.py:37:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/middleware.py:108:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/middleware.py:112:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/processors.py:90:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/renderers/jsonrenderer.py:17:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/stack_sampler.py:102:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 43 diagnostics
+ Found 32 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/augmentation/_2d/mix/base.py:194:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_2d/mix/transplantation.py:196:119: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_2d/mix/transplantation.py:202:118: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_2d/mix/transplantation.py:299:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/_3d/mix/transplantation.py:24:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/auto/base.py:44:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:301:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:322:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:324:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:337:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:344:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:428:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:444:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:446:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:466:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:474:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/base.py:323:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/ops.py:151:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/ops.py:153:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/ops.py:512:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:170:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:262:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:337:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:347:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:355:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:363:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:371:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:379:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:387:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:395:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:403:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/patch.py:419:109: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:288:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:308:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:328:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/video.py:351:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/presets/ada.py:246:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/presets/ada.py:290:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/random_generator/base.py:111:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/random_generator/base.py:117:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/constants.py:35:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/constants.py:38:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/boxmot_tracker.py:128:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/boxmot_tracker.py:141:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/edge_detection.py:123:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/edge_detection.py:166:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/edge_detection.py:174:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:189:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:224:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:232:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/object_detection.py:281:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:79:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:123:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:130:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:171:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:175:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:179:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/super_resolution.py:183:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:356:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:366:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:376:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:388:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:397:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/contrib/visual_prompter.py:406:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:69:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:70:93: warning[unused-ignore-comment] Unused `ty: ignore` directive
- kornia/core/mixin/image_module.py:72:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:117:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:119:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:141:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:145:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:146:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:149:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:161:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:163:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:164:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:167:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:181:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:184:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:187:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:189:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:199:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:226:108: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/image_module.py:229:108: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:76:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:133:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:134:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:135:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:139:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:147:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:172:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:174:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:175:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:190:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:191:101: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:192:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:199:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:209:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:274:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:296:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:298:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:311:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:317:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:319:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:339:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:341:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:360:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:363:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:368:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:374:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:387:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:391:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:393:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:408:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:411:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:423:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:424:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:426:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/core/mixin/onnx.py:429:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/decoder.py:33:121: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/decoder.py:65:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/decoder.py:101:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/dedode.py:234:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/dedode.py:235:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/descriptor.py:31:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/detector.py:31:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/detector.py:55:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:36:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:41:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:79:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/encoder.py:92:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/transformer/layers/attention.py:93:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/transformer/layers/drop_path.py:39:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/utils.py:42:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/utils.py:43:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/integrated.py:482:93: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:42:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:158:88: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:167:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:248:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:250:111: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:287:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:478:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:490:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:550:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:554:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:556:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:579:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:593:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:594:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:671:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue.py:679:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:32:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:68:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:149:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:150:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:152:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:153:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:155:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/lightglue_onnx/lightglue.py:156:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/loftr/loftr.py:208:95: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/loftr/utils/coarse_matching.py:272:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/matching.py:566:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/siftdesc.py:41:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/siftdesc.py:42:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/sold2/backbones.py:175:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/sold2/backbones.py:177:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/sold2/backbones.py:182:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/sold2/backbones.py:183:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/filters/dissolving.py:234:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/filters/dissolving.py:256:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/filters/dissolving.py:261:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/filters/dissolving.py:266:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/boxes.py:747:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/boxes.py:764:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/keypoints.py:217:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/liegroup/so3.py:176:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/transform/crop2d.py:470:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/transform/crop2d.py:472:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/transform/crop2d.py:476:118: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/vector.py:24:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:67:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:85:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:87:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:102:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:103:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:104:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/_hf_models/hf_onnx_community.py:142:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/base.py:45:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/depth_estimation/base.py:36:118: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/depth_estimation/base.py:73:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/efficient_vit/backbone.py:28:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/efficient_vit/nn/act.py:36:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/efficient_vit/nn/ops.py:22:1: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/efficient_vit/utils/network.py:30:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/rt_detr/model.py:254:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/segmentation/base.py:149:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/segmentation/segmentation_models.py:72:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/tiny_vit.py:551:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/module.py:48:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/module.py:50:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/module.py:65:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/module.py:71:110: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/module.py:75:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:54:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:56:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:74:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:77:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:103:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:108:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/sequential.py:114:110: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:71:121: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:104:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:125:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:128:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:142:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:171:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:173:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:206:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/onnx/utils.py:208:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/transpiler/transpiler.py:53:8: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/transpiler/transpiler.py:86:8: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/transpiler/transpiler.py:116:8: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 324 diagnostics
+ Found 101 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/builder.py:164:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/builder.py:165:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/builder.py:166:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/builder.py:177:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/builder.py:184:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/builder.py:234:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/ci/common.py:221:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/ci/common.py:239:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/cmd/unit_test.py:17:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/config.py:473:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/config.py:517:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/config.py:1481:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/database.py:1764:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/environment/environment.py:2018:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/fetch_strategy.py:523:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/install_test.py:259:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/install_test.py:514:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/installer.py:228:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/installer.py:1400:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/installer.py:2411:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/installer.py:2650:87: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/llnl/util/filesystem.py:3000:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/llnl/util/tty/__init__.py:204:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/llnl/util/tty/__init__.py:248:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/llnl/util/tty/__init__.py:263:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/patch.py:187:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/spec.py:3603:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/binary_distribution.py:631:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/binary_distribution.py:1212:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/binary_distribution.py:1319:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:35:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:36:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:53:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:54:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:58:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:59:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:60:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:71:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:72:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:76:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:77:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:78:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:338:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:339:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:348:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:349:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:383:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:403:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:404:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:460:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/cmd/dev_build.py:461:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/config.py:1815:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:706:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:707:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:708:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:799:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:800:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:801:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:802:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/repo.py:894:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/test/sbang.py:58:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/tokenize.py:104:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:220:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:225:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:226:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:228:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:229:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:230:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:233:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:234:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/elf.py:235:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/file_cache.py:159:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/file_cache.py:175:77: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/url.py:89:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/url.py:90:86: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/url.py:93:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/util/url.py:94:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/distro/distro.py:629:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/async_utils.py:11:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/async_utils.py:12:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/async_utils.py:27:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/compiler.py:120:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/compiler.py:1404:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/debug.py:76:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/debug.py:220:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/debug.py:226:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:499:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:659:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:662:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:693:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:696:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:912:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1171:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1186:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1243:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1289:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1346:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1495:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1529:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1561:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1609:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/environment.py:1656:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/exceptions.py:131:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:14:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:180:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:199:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:216:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:241:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:295:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/ext.py:302:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:221:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:619:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:645:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1084:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1242:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1283:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1312:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1478:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1523:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1559:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1599:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/filters.py:1637:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/idtracking.py:137:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/lexer.py:459:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/lexer.py:531:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/lexer.py:578:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/meta.py:79:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/nativetypes.py:104:63: warning[unused-ignore-c

... (truncated 9122 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @WillDuke on 2026-01-15 09:32_

Unfortunately, the workflow still references the version on main as before.

---

_@AlexWaygood reviewed on 2026-01-15 09:36_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:1 on 2026-01-15 09:36_

On line 85, could you checkout the PR branch again before executing the Python script (after you've built the binary on the target branch)? Then I think the workflow would use the version of the script from this PR branch rather than the version on `main`

---

_Review request for @dcreager removed by @AlexWaygood on 2026-01-15 09:49_

---

_Review request for @carljm removed by @AlexWaygood on 2026-01-15 09:49_

---

_Review request for @sharkdp removed by @AlexWaygood on 2026-01-15 09:49_

---

_@AlexWaygood reviewed on 2026-01-15 11:31_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:91 on 2026-01-15 11:31_

```suggestion
            git switch -
```

---

_@AlexWaygood reviewed on 2026-01-15 11:35_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:91 on 2026-01-15 11:35_

I guess this is what the git error message suggests 

```suggestion
            git switch - --detach
```

---

_Comment by @WillDuke on 2026-01-15 11:51_

Thanks for the guidance, @AlexWaygood. For some reason, it had not occurred to me to switch the branch in the workflow itself. The script passed the new assertion that I added, so I'm just waiting for the bot to update now to confirm.

---

_@AlexWaygood reviewed on 2026-01-15 12:01_

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:622 on 2026-01-15 12:01_

(I think it's useful to keep this print statement anyway, so that we can see the report in the logs for the CI job even if the commenting bot fails to do its job!)

---

_@AlexWaygood reviewed on 2026-01-15 12:03_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:89 on 2026-01-15 12:03_

If you move this a few lines higher so that it's inside the previous parenthesized expression, I think you wouldn't need to do any `cd`-ing

---

_Comment by @MichaReiser on 2026-01-15 12:21_

I'll take this from here as I consider it important to have the typing conformance tests back. Thanks for following up on this so quickly and identifying the root cause.

---

_@AlexWaygood approved on 2026-01-15 12:25_

---

_Review comment by @MichaReiser on `.github/workflows/typing_conformance.yaml`:89 on 2026-01-15 12:28_

I keep the parentheses because they help with readability but I switched to using relative paths in more places, to remove the need for cd-ing out of the directory

---

_@MichaReiser reviewed on 2026-01-15 12:28_

---

_@MichaReiser reviewed on 2026-01-15 12:29_

---

_Review comment by @MichaReiser on `scripts/conformance.py`:622 on 2026-01-15 12:29_

It's also useful when the bot has to truncate the comment

---

_Merged by @MichaReiser on 2026-01-15 12:38_

---

_Closed by @MichaReiser on 2026-01-15 12:38_

---

_Branch deleted on 2026-01-15 21:16_

---
