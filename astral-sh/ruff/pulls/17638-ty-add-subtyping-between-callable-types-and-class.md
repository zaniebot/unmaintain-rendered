```yaml
number: 17638
title: "[ty] Add subtyping between Callable types and class literals with `__init__`"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: class-literal-init-callable-subtyping
created_at: 2025-04-25T23:51:34Z
updated_at: 2025-06-28T23:12:36Z
url: https://github.com/astral-sh/ruff/pull/17638
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Add subtyping between Callable types and class literals with `__init__`

---

_Pull request opened by @MatthewMckee4 on 2025-04-25 23:51_

## Summary

Allow classes with `__init__` to be subtypes of `Callable`

Fixes https://github.com/astral-sh/ty/issues/358

## Test Plan

Update is_subtype_of.md

---

_Renamed from "[red-knot] Add initial subtyping with for class literals with init" to "[red-knot] Add subtyping with for class literals with init" by @MatthewMckee4 on 2025-04-25 23:51_

---

_Comment by @github-actions[bot] on 2025-04-26 00:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
nionutils (https://github.com/nion-software/nionutils)
- error[no-matching-overload] nion/utils/test/ListModel_test.py:305:40: No overload of function `__new__` matches arguments
- error[no-matching-overload] nion/utils/test/ListModel_test.py:315:40: No overload of function `__new__` matches arguments
- error[no-matching-overload] nion/utils/test/ListModel_test.py:326:40: No overload of function `__new__` matches arguments
- Found 20 diagnostics
+ Found 17 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[no-matching-overload] chess/pgn.py:1850:12: No overload of function `read_game` matches arguments
- Found 37 diagnostics
+ Found 36 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[no-matching-overload] more_itertools/more.py:937:42: No overload of function `__new__` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[no-matching-overload] tests/test_converters.py:194:29: No overload of function `Factory` matches arguments
- Found 618 diagnostics
+ Found 617 diagnostics

kornia (https://github.com/kornia/kornia)
- error[invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:336:26: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:350:26: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:364:26: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:378:26: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[invalid-parameter-default] kornia/feature/dedode/transformer/layers/block.py:68:9: Default value of type `<class 'Attention'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] kornia/feature/dedode/transformer/layers/block.py:69:9: Default value of type `<class 'Mlp'>` is not assignable to annotated parameter type `(...) -> Unknown`
- Found 947 diagnostics
+ Found 941 diagnostics

starlette (https://github.com/encode/starlette)
- error[invalid-argument-type] tests/conftest.py:20:9: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'TestClient'>`
- Found 176 diagnostics
+ Found 175 diagnostics

isort (https://github.com/pycqa/isort)
- error[invalid-argument-type] isort/identify.py:105:17: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Import'>`
- Found 54 diagnostics
+ Found 53 diagnostics

kopf (https://github.com/nolar/kopf)
- error[invalid-argument-type] kopf/_cogs/clients/auth.py:49:56: Argument to bound method `extended` is incorrect: Expected `(ConnectionInfo, /) -> Unknown`, found `<class 'APIContext'>`
- error[invalid-argument-type] kopf/_cogs/clients/auth.py:49:56: Argument to bound method `extended` is incorrect: Expected `(ConnectionInfo, /) -> Unknown`, found `<class 'APIContext'>`
- error[invalid-argument-type] kopf/_cogs/clients/auth.py:49:56: Argument to bound method `extended` is incorrect: Expected `(ConnectionInfo, /) -> Unknown`, found `<class 'APIContext'>`
- error[invalid-argument-type] kopf/_cogs/clients/auth.py:49:56: Argument to bound method `extended` is incorrect: Expected `(ConnectionInfo, /) -> Unknown`, found `<class 'APIContext'>`
- Found 167 diagnostics
+ Found 163 diagnostics

trio (https://github.com/python-trio/trio)
- error[no-matching-overload] src/trio/_core/_asyncgens.py:54:51: No overload of function `Factory` matches arguments
- error[no-matching-overload] src/trio/_core/_asyncgens.py:64:47: No overload of function `Factory` matches arguments
- Found 1092 diagnostics
+ Found 1090 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[no-matching-overload] strawberry/types/union.py:157:36: No overload of function `__new__` matches arguments
- Found 433 diagnostics
+ Found 432 diagnostics

websockets (https://github.com/aaugustin/websockets)
- error[invalid-assignment] src/websockets/legacy/auth.py:179:9: Object of type `<class 'BasicAuthWebSocketServerProtocol'>` is not assignable to `((...) -> BasicAuthWebSocketServerProtocol) | None`
- error[invalid-assignment] src/websockets/legacy/client.py:459:13: Object of type `(Any & ~None) | <class 'WebSocketClientProtocol'>` is not assignable to `((...) -> WebSocketClientProtocol) | None`
- error[invalid-assignment] src/websockets/legacy/server.py:1028:13: Object of type `(Any & ~None) | <class 'WebSocketServerProtocol'>` is not assignable to `((...) -> WebSocketServerProtocol) | None`
- Found 112 diagnostics
+ Found 109 diagnostics

jinja (https://github.com/pallets/jinja)
- error[no-matching-overload] tests/test_filters.py:499:34: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/test_filters.py:503:34: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/test_filters.py:551:31: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/test_filters.py:571:31: No overload of function `__new__` matches arguments
- Found 234 diagnostics
+ Found 230 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[invalid-argument-type] src/typeshed_stats/gather.py:993:13: Argument to bound method `from_lines` is incorrect: Expected `str | ((Unknown, /) -> Pattern)`, found `<class 'GitWildMatchPattern'>`
- Found 26 diagnostics
+ Found 25 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[invalid-argument-type] cki_lib/inttests/remote_responses.py:248:48: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class '_MockRequestHandler'>`
- Found 171 diagnostics
+ Found 170 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[no-matching-overload] tests/test_sql.py:182:36: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/test_sql.py:203:36: No overload of function `__new__` matches arguments
- error[no-matching-overload] tools/update_error_prefixes.py:27:14: No overload of bound method `__init__` matches arguments
- Found 1118 diagnostics
+ Found 1115 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[no-matching-overload] tests/test_http_response.py:267:17: No overload of function `__new__` matches arguments
- error[no-matching-overload] tests/test_http_response.py:310:17: No overload of function `__new__` matches arguments
- Found 1344 diagnostics
+ Found 1342 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[no-matching-overload] comtypes/test/test_DISPPARAMS.py:32:41: No overload of function `__new__` matches arguments
- Found 599 diagnostics
+ Found 598 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-argument-type] references/depth/stereo/parsing.py:21:26: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'CREStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:22:30: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'CarlaStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:23:27: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'InStereo2k'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:24:23: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SintelStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:25:33: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SceneFlowStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:26:39: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SceneFlowStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:27:34: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SceneFlowStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:28:30: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'FallingThingsStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:29:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ETH3DStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:30:27: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ETH3DStereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:31:32: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Kitti2015Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:32:31: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Kitti2015Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:33:32: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Kitti2012Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:34:31: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Kitti2012Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:36:9: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Middlebury2014Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:38:37: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Middlebury2014Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:39:36: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Middlebury2014Stereo'>`
- error[invalid-argument-type] references/depth/stereo/parsing.py:41:9: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Middlebury2014Stereo'>`
- error[invalid-argument-type] test/test_transforms_v2.py:1870:65: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'RandomApply'>`
- error[invalid-argument-type] torchvision/models/alexnet.py:58:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-assignment] torchvision/models/convnext.py:111:13: Object of type `<class 'CNBlock'>` is not assignable to `((...) -> Unknown) | None`
- error[invalid-argument-type] torchvision/models/convnext.py:114:34: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'LayerNorm2d'>`
- error[call-non-callable] torchvision/models/convnext.py:141:30: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/convnext.py:213:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/convnext.py:233:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/convnext.py:253:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/convnext.py:273:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/densenet.py:270:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/densenet.py:290:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/densenet.py:310:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/densenet.py:330:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-parameter-default] torchvision/models/detection/backbone_utils.py:72:5: Default value of type `<class 'FrozenBatchNorm2d'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] torchvision/models/detection/backbone_utils.py:190:5: Default value of type `<class 'FrozenBatchNorm2d'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-argument-type] torchvision/models/detection/faster_rcnn.py:384:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/faster_rcnn.py:405:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/faster_rcnn.py:426:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/faster_rcnn.py:447:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/fcos.py:658:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/keypoint_rcnn.py:322:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/keypoint_rcnn.py:343:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/mask_rcnn.py:365:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/mask_rcnn.py:387:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/retinanet.py:687:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/retinanet.py:708:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/ssd.py:31:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-argument-type] torchvision/models/detection/ssdlite.py:190:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'ObjectDetection'>`
- error[invalid-assignment] torchvision/models/efficientnet.py:80:13: Object of type `<class 'MBConv'>` is not assignable to `((...) -> Unknown) | None`
- error[invalid-assignment] torchvision/models/efficientnet.py:101:13: Object of type `<class 'FusedMBConv'>` is not assignable to `((...) -> Unknown) | None`
- error[invalid-parameter-default] torchvision/models/efficientnet.py:111:9: Default value of type `<class 'SqueezeExcitation'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-argument-type] torchvision/models/efficientnet.py:445:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:469:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:488:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:517:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:541:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:565:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:589:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:613:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:637:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:660:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:690:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/efficientnet.py:721:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-assignment] torchvision/models/googlenet.py:198:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/googlenet.py:199:24: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/googlenet.py:202:13: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/googlenet.py:202:63: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/googlenet.py:206:13: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/googlenet.py:209:13: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/googlenet.py:214:13: Object of type `None` is not callable
- error[invalid-assignment] torchvision/models/googlenet.py:241:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/googlenet.py:242:21: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/googlenet.py:281:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-assignment] torchvision/models/inception.py:182:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/inception.py:183:26: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:185:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:186:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:188:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:189:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:190:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:192:28: Object of type `None` is not callable
- error[invalid-assignment] torchvision/models/inception.py:219:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/inception.py:220:26: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:222:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:223:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:224:31: Object of type `None` is not callable
- error[invalid-assignment] torchvision/models/inception.py:249:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/inception.py:250:26: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:253:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:254:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:255:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:257:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:258:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:259:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:260:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:261:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:263:28: Object of type `None` is not callable
- error[invalid-assignment] torchvision/models/inception.py:293:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/inception.py:294:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:295:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:297:30: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:298:30: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:299:30: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:300:30: Object of type `None` is not callable
- error[invalid-assignment] torchvision/models/inception.py:324:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/inception.py:325:26: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:327:28: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:328:29: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:329:29: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:331:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:332:31: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:333:32: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:334:32: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:336:28: Object of type `None` is not callable
- error[invalid-assignment] torchvision/models/inception.py:373:13: Object of type `<class 'BasicConv2d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/inception.py:374:22: Object of type `None` is not callable
- error[call-non-callable] torchvision/models/inception.py:375:22: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/inception.py:413:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/maxvit.py:777:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mnasnet.py:224:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mnasnet.py:245:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mnasnet.py:270:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mnasnet.py:291:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-assignment] torchvision/models/mobilenetv2.py:96:13: Object of type `<class 'InvertedResidual'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/mobilenetv2.py:133:33: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/mobilenetv2.py:187:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mobilenetv2.py:204:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mobilenetv3.py:59:54: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SqueezeExcitation'>`
- error[invalid-assignment] torchvision/models/mobilenetv3.py:152:13: Object of type `<class 'InvertedResidual'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/mobilenetv3.py:174:27: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/mobilenetv3.py:300:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mobilenetv3.py:318:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/mobilenetv3.py:344:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:558:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:578:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:599:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:625:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:652:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:675:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:716:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/optical_flow/raft.py:735:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'OpticalFlow'>`
- error[invalid-argument-type] torchvision/models/quantization/googlenet.py:112:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/inception.py:172:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/mobilenetv2.py:69:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/mobilenetv3.py:164:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/resnet.py:167:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/resnet.py:188:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/resnet.py:205:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/resnet.py:226:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/resnet.py:243:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/resnet.py:264:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/shufflenetv2.py:131:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/shufflenetv2.py:152:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/shufflenetv2.py:173:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/quantization/shufflenetv2.py:195:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-assignment] torchvision/models/regnet.py:311:13: Object of type `<class 'SimpleStemIN'>` is not assignable to `((...) -> Unknown) | None`
- error[invalid-assignment] torchvision/models/regnet.py:315:13: Object of type `<class 'ResBottleneckBlock'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/regnet.py:320:21: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/regnet.py:345:25: Argument to bound method `__init__` is incorrect: Expected `(...) -> Unknown`, found `(((...) -> Unknown) & ~None) | ((...) -> Unknown) | None`
- error[invalid-argument-type] torchvision/models/regnet.py:420:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:438:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:464:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:482:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:508:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:526:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:552:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:570:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:596:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:614:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:640:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:658:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:681:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:703:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:729:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:747:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:770:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:792:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:819:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:841:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:867:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:885:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:911:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:929:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:955:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:973:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:999:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1017:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1043:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1061:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1087:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1105:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1131:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/regnet.py:1149:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:315:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:337:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:359:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:377:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:402:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:420:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:445:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:463:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:488:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:506:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:531:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:549:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:574:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:599:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:617:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:642:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/resnet.py:660:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/segmentation/deeplabv3.py:145:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SemanticSegmentation'>`
- error[invalid-argument-type] torchvision/models/segmentation/deeplabv3.py:166:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SemanticSegmentation'>`
- error[invalid-argument-type] torchvision/models/segmentation/deeplabv3.py:187:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SemanticSegmentation'>`
- error[invalid-argument-type] torchvision/models/segmentation/fcn.py:63:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SemanticSegmentation'>`
- error[invalid-argument-type] torchvision/models/segmentation/fcn.py:84:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SemanticSegmentation'>`
- error[invalid-argument-type] torchvision/models/segmentation/lraspp.py:99:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SemanticSegmentation'>`
- error[invalid-parameter-default] torchvision/models/shufflenetv2.py:110:9: Default value of type `<class 'InvertedResidual'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-argument-type] torchvision/models/shufflenetv2.py:197:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/shufflenetv2.py:219:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/shufflenetv2.py:240:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/shufflenetv2.py:265:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/squeezenet.py:127:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/squeezenet.py:148:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-parameter-default] torchvision/models/swin_transformer.py:428:9: Default value of type `<class 'ShiftedWindowAttention'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] torchvision/models/swin_transformer.py:485:9: Default value of type `<class 'ShiftedWindowAttentionV2'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] torchvision/models/swin_transformer.py:542:9: Default value of type `<class 'PatchMerging'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-assignment] torchvision/models/swin_transformer.py:549:13: Object of type `<class 'SwinTransformerBlock'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/swin_transformer.py:575:21: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/swin_transformer.py:656:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/swin_transformer.py:681:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/swin_transformer.py:706:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/swin_transformer.py:731:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/swin_transformer.py:756:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/swin_transformer.py:781:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:120:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:140:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:160:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:180:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:200:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:218:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:249:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:269:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vgg.py:289:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-assignment] torchvision/models/video/mvit.py:478:13: Object of type `<class 'MultiscaleBlock'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/video/mvit.py:509:17: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/video/mvit.py:606:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/mvit.py:639:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/resnet.py:326:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/resnet.py:346:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/resnet.py:366:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/resnet.py:413:9: Argument to function `_video_resnet` is incorrect: Expected `(...) -> Unknown`, found `<class 'BasicStem'>`
- error[invalid-argument-type] torchvision/models/video/resnet.py:450:9: Argument to function `_video_resnet` is incorrect: Expected `(...) -> Unknown`, found `<class 'BasicStem'>`
- error[invalid-argument-type] torchvision/models/video/resnet.py:487:9: Argument to function `_video_resnet` is incorrect: Expected `(...) -> Unknown`, found `<class 'R2Plus1dStem'>`
- error[invalid-argument-type] torchvision/models/video/s3d.py:158:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-parameter-default] torchvision/models/video/swin_transformer.py:400:9: Default value of type `<class 'PatchMerging'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-argument-type] torchvision/models/video/swin_transformer.py:408:29: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'SwinTransformerBlock'>`
- error[invalid-assignment] torchvision/models/video/swin_transformer.py:414:13: Object of type `<class 'PatchEmbed3d'>` is not assignable to `((...) -> Unknown) | None`
- error[call-non-callable] torchvision/models/video/swin_transformer.py:417:28: Object of type `None` is not callable
- error[invalid-argument-type] torchvision/models/video/swin_transformer.py:516:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/swin_transformer.py:547:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/swin_transformer.py:578:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/video/swin_transformer.py:605:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'VideoClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:354:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:377:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:403:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:433:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:459:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:483:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:509:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:539:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:566:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-argument-type] torchvision/models/vision_transformer.py:592:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ImageClassification'>`
- error[invalid-parameter-default] torchvision/prototype/models/depth/stereo/crestereo.py:496:9: Default value of type `<class 'LinearAttention'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] torchvision/prototype/models/depth/stereo/crestereo.py:569:9: Default value of type `<class 'LinearAttention'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:708:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'Conv2dNormActivation'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:723:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'IterativeCorrelationLayer'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:732:13: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'AttentionOffsetCorrelationLayer'>`
- error[invalid-assignment] torchvision/prototype/models/depth/stereo/crestereo.py:1025:5: Object of type `(Unknown & ~AlwaysFalsy) | <class 'LinearAttention'>` is not assignable to `(...) -> Unknown`
- error[invalid-assignment] torchvision/prototype/models/depth/stereo/crestereo.py:1033:5: Object of type `(Unknown & ~AlwaysFalsy) | <class 'LinearAttention'>` is not assignable to `(...) -> Unknown`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:1081:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:1186:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:1293:9: Argument is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:1438:39: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'ResidualBlock'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:1456:9: Argument to function `_crestereo` is incorrect: Expected `(...) -> Unknown`, found `<class 'LinearAttention'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/crestereo.py:1457:9: Argument to function `_crestereo` is incorrect: Expected `(...) -> Unknown`, found `<class 'LinearAttention'>`
- error[invalid-parameter-default] torchvision/prototype/models/depth/stereo/raft_stereo.py:36:9: Default value of type `<class 'ResidualBlock'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] torchvision/prototype/models/depth/stereo/raft_stereo.py:65:9: Default value of type `<class 'ResidualBlock'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-parameter-default] torchvision/prototype/models/depth/stereo/raft_stereo.py:111:9: Default value of type `<class 'ResidualBlock'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:637:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:657:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:690:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:709:28: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'StereoMatching'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:763:9: Argument to function `_raft_stereo` is incorrect: Expected `(...) -> Unknown`, found `<class 'ResidualBlock'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:768:9: Argument to function `_raft_stereo` is incorrect: Expected `(...) -> Unknown`, found `<class 'ResidualBlock'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:821:9: Argument to function `_raft_stereo` is incorrect: Expected `(...) -> Unknown`, found `<class 'ResidualBlock'>`
- error[invalid-argument-type] torchvision/prototype/models/depth/stereo/raft_stereo.py:826:9: Argument to function `_raft_stereo` is incorrect: Expected `(...) -> Unknown`, found `<class 'ResidualBlock'>`
- Found 1840 diagnostics
+ Found 1546 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[invalid-argument-type] pwndbg/gdblib/tui/control.py:47:48: Argument to function `register_window_type` is incorrect: Expected `(TuiWindow, /) -> _Window`, found `<class 'ControlTUIWindow'>`
- Found 2250 diagnostics
+ Found 2249 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[no-matching-overload] src/_pytest/_io/pprint.py:239:18: No overload of function `sorted` matches arguments
- Found 883 diagnostics
+ Found 882 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-argument-type] test/helper_tools/passive_close.py:23:40: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'service'>`
- Found 2114 diagnostics
+ Found 2113 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-argument-type] mypy/report.py:213:32: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'LineCountReporter'>`
- error[invalid-argument-type] mypy/report.py:312:32: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'AnyExpressionsReporter'>`
- error[invalid-argument-type] mypy/report.py:445:35: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'LineCoverageReporter'>`
- error[invalid-argument-type] mypy/report.py:575:33: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'MemoryXmlReporter'>`
- error[invalid-argument-type] mypy/report.py:704:36: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'CoberturaXmlReporter'>`
- error[invalid-argument-type] mypy/report.py:758:26: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'XmlReporter'>`
- error[invalid-argument-type] mypy/report.py:805:32: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'XsltHtmlReporter'>`
- error[invalid-argument-type] mypy/report.py:838:31: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'XsltTxtReporter'>`
- error[invalid-argument-type] mypy/report.py:922:36: Argument to function `register_reporter` is incorrect: Expected `(Reports, str, /) -> AbstractReporter`, found `<class 'LinePrecisionReporter'>`
- Found 3335 diagnostics
+ Found 3326 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/profile/__main__.py:2097:41: Argument to function `__new__` is incorrect: Expected `(...) -> Unknown`, found `<class 'FunctionMetaData'>`
- error[invalid-argument-type] static_frame/test/unit/test_bus.py:738:17: Argument to bound method `__init__` is incorrect: Expected `((str, /) -> Unknown) | None`, found `<class 'datetime64'>`
- error[invalid-argument-type] static_frame/test/unit/test_bus.py:818:17: Argument to bound method `__init__` is incorrect: Expected `((str, /) -> Unknown) | None`, found `<class 'datetime64'>`
- error[invalid-argument-type] static_frame/test/unit/test_bus.py:1142:17: Argument to bound method `__init__` is incorrect: Expected `((str, /) -> Unknown) | None`, found `<class 'datetime64'>`
- error[invalid-argument-type] static_frame/test/unit/test_bus.py:2277:58: Argument to bound method `from_dict` is incorrect: Expected `((...) -> IndexBase) | None`, found `<class 'IndexYearMonth'>`
- error[invalid-argument-type] static_frame/test/unit/test_bus.py:2873:13: Argument to bound method `__init__` is incorrect: Expected `((str, /) -> Unknown) | None`, found `<class 'datetime64'>`
- error[invalid-argument-type] static_frame/test/unit/test_quilt.py:1222:13: Argument to bound method `from_dict` is incorrect: Expected `((...) -> IndexBase) | None`, found `<class 'IndexDate'>`
- error[invalid-argument-type] static_frame/test/unit/test_series.py:1487:17: Argument to bound method `from_items` is incorrect: Expected `((...) -> IndexBase) | None`, found `<class 'IndexYearMonth'>`
- Found 2067 diagnostics
+ Found 2059 diagnostics

altair (https://github.com/vega/altair)
- error[invalid-argument-type] altair/utils/_show.py:72:57: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'OneShotRequestHandler'>`
- Found 1300 diagnostics
+ Found 1299 diagnostics

paasta (https://github.com/yelp/paasta)
- error[invalid-assignment] paasta_tools/cli/cmds/mark_for_deployment.py:507:5: Object of type `<class 'NoMetrics'>` is not assignable to `(str, /) -> BaseMetrics`
- error[invalid-argument-type] paasta_tools/firewall_logging.py:133:57: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'SyslogUDPHandler'>`
- Found 943 diagnostics
+ Found 941 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[invalid-parameter-default] cwltool/main.py:405:5: Default value of type `<class 'StdFsAccess'>` is not assignable to annotated parameter type `(str, /) -> StdFsAccess`
- error[invalid-argument-type] cwltool/main.py:1281:21: Argument to function `init_job_order` is incorrect: Expected `(str, /) -> StdFsAccess`, found `Unknown | <class 'StdFsAccess'>`
- Found 302 diagnostics
+ Found 300 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[invalid-argument-type] cloudinit/log/loggers.py:222:33: Argument to function `setLogRecordFactory` is incorrect: Expected `(...) -> LogRecord`, found `<class 'CloudInitLogRecord'>`
- error[invalid-argument-type] tests/integration_tests/assets/echo_server.py:34:36: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'Server'>`
- Found 715 diagnostics
+ Found 713 diagnostics

zulip (https://github.com/zulip/zulip)
- error[no-matching-overload] corporate/lib/activity.py:89:17: No overload of function `__new__` matches arguments
- error[no-matching-overload] zerver/lib/send_email.py:608:33: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] zerver/management/commands/email_mirror.py:71:23: No overload of bound method `__init__` matches arguments
- error[invalid-return-type] zerver/management/commands/send_to_email_mirror.py:135:24: Return type does not match returned value: expected `EmailMessage`, found `Message[str, str]`
- error[no-matching-overload] zerver/management/commands/send_to_email_mirror.py:135:24: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] zerver/tests/test_email_mirror.py:1762:28: No overload of bound method `__init__` matches arguments
- error[invalid-argument-type] zerver/tests/test_email_mirror.py:1775:25: Argument to function `process_message` is incorrect: Expected `EmailMessage`, found `Message[str, str]`
- error[no-matching-overload] zerver/tests/test_email_mirror.py:1788:28: No overload of bound method `__init__` matches arguments
- error[invalid-argument-type] zerver/tests/test_email_mirror.py:1801:25: Argument to function `process_message` is incorrect: Expected `EmailMessage`, found `Message[str, str]`
- error[no-matching-overload] zerver/worker/email_mirror.py:26:15: No overload of bound method `__init__` matches arguments
- error[invalid-argument-type] zerver/worker/email_mirror.py:30:26: Argument to function `process_message` is incorrect: Expected `EmailMessage`, found `Message[str, str]`
- Found 6987 diagnostics
+ Found 6976 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-argument-type] benchmarks/bm/iast_fixtures/str_methods.py:1128:62: Argument to bound method `__init__` is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'WebServerHandler'>`
- error[invalid-argument-type] ddtrace/contrib/internal/psycopg/extensions....*[Comment body truncated]*

---

_Comment by @MatthewMckee4 on 2025-04-26 00:10_

I think i still have some thinking to do regarding the logic here, and perhaps without `Self` and `self` typing im not sure this should land, since the logic depends on that: "callable should be the concrete value of `Self`"

---

_Label `red-knot` added by @dhruvmanila on 2025-05-02 14:20_

---

_Marked ready for review by @MatthewMckee4 on 2025-05-04 09:12_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-05-04 09:12_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-05-04 09:12_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-05-04 09:12_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-05-04 09:12_

---

_Renamed from "[red-knot] Add subtyping with for class literals with init" to "[red-knot] Add subtyping with for class literals with `__init__`" by @MatthewMckee4 on 2025-05-04 09:12_

---

_Renamed from "[red-knot] Add subtyping with for class literals with `__init__`" to "[red-knot] Add subtyping between Callable types and class literals with `__init__`" by @AlexWaygood on 2025-05-04 09:13_

---

_@MatthewMckee4 reviewed on 2025-05-04 09:14_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1278 on 2025-05-04 09:14_

As per the spec this "shouldn't" pass, but there's maybe an argument that it should pass

---

_@MatthewMckee4 reviewed on 2025-05-04 09:14_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1278 on 2025-05-04 09:14_

(As is the test without "not" should pass)

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/class.rs`:919 on 2025-05-04 09:28_

I am also not sure this is exactly correct. The spec states it can be a subtype or a union contain a subtype of the class. I should also add a test that includes a union containing subtype

---

_@MatthewMckee4 reviewed on 2025-05-04 09:28_

---

_Comment by @MatthewMckee4 on 2025-05-04 23:18_

Addressing some of the mypy primer issues
- https://github.com/niklasf/python-chess This looks correct (as of now), once self typing is implemented and we update the code to synthesize the `__init__` with the concrete type of `Self` this should work fine
- https://github.com/python-attrs/attrs This looks fine

---

_Closed by @AlexWaygood on 2025-05-04 23:23_

---

_Reopened by @AlexWaygood on 2025-05-04 23:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1278 on 2025-05-09 16:30_

Why does this not pass according to the spec? It seems like the spec would say in this case that the callable type of `C` would be the union of the `__new__` signature, and the `__init__` bound-method signature with replaced return type. That union seems like it should be a subtype of `Callable[[int], C]`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:940 on 2025-05-09 17:08_

I'm not actually sure what the spec means by "the concrete value of `Self`" here; I don't know what else that could be, other than what you have here (for non-generic classes, anyway).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:941 on 2025-05-09 17:09_

What about generic classes? Can we add a test for that case? Do we need a TODO?
 
Should this just be `self_ty.to_instance()` instead, so we don't have to worry about those distinctions here?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:976 on 2025-05-09 17:10_

Shouldn't this be `dunder_new_bound_method` instead of `dunder_new_function`? Otherwise we wrongly include the `cls` argument. I feel like that might be why the one test you commented on above is saying "not subtype", when it seems like it should say "subtype".

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:980 on 2025-05-09 17:13_

And similarly here I feel like we need to be returning the bound method version of `__new__`?

---

_@carljm reviewed on 2025-05-09 17:13_

This looks pretty solid! Sorry for the slow review. A few comments/questions.

---

_@MatthewMckee4 reviewed on 2025-05-10 10:34_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1278 on 2025-05-10 10:34_

Ah, so yeah you're right, but what is happening here (after i added ":Any" to args and kwargs) 
is we end up calling is_subtype with
self: (*args: Any, **kwargs: Any) -> C
target: (int, /) -> C
which returns false because self isnt fully static

---

_@MatthewMckee4 reviewed on 2025-05-10 10:56_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/class.rs`:941 on 2025-05-10 10:56_

I think generic cases need some more work, do you think `into_callable` belongs in `ClassLiteral` or should it maybe be in `ClassType`?

---

_@MatthewMckee4 reviewed on 2025-05-10 13:29_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/class.rs`:976 on 2025-05-10 13:29_

I already call `into_bound_method_type` further up

---

_Renamed from "[red-knot] Add subtyping between Callable types and class literals with `__init__`" to "[ty] Add subtyping between Callable types and class literals with `__init__`" by @MichaReiser on 2025-05-12 07:24_

---

_Comment by @MatthewMckee4 on 2025-05-15 18:02_

@carljm is there any urgency for this? If anyone can provide any more insights on another review I'd be happy to implement them asap.

I'm aware you did give some comments but I'm still a bit unsure about them

---

_Comment by @carljm on 2025-05-16 02:37_

@MatthewMckee4 Sorry for the delay, I'm aware this is just waiting for me to look at it again! It's PyCon this week so a number of us are distracted :)

---

_Comment by @MatthewMckee4 on 2025-05-16 07:58_

All good, there's no urgency on my end, just wanted to make sure this wasn't forgotten. Thanks

---

_@carljm approved on 2025-05-28 20:41_

Made a few updates here, this looks good to me now, and the primer diff looks great. Gonna go ahead and merge!

---

_Merged by @carljm on 2025-05-28 20:43_

---

_Closed by @carljm on 2025-05-28 20:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/known_instance.rs`:330 on 2025-05-29 12:23_

@carljm, it looks like this change came from one of your commits. What's the motivation for this change? `_typeshed.Self` is just a legacy TypeVar -- it's very different to `typing(_extensions).Self`. You can see it defined here:

https://github.com/astral-sh/ruff/blob/04dc48e17c2518f97436d76284a509499087d81c/crates/ty_vendored/vendor/typeshed/stdlib/_typeshed/__init__.pyi#L36-L40

The only reason for its existence is that PEP 673 (somewhat inexplicably) has an arbitrary ban on using `typing(_extensions).Self` in any metaclass methods, so there's no other way for typeshed to annotate the return types of `__new__` or `__enter__` methods on metaclasses unless it defines a custom legacy typevar

---

_@AlexWaygood reviewed on 2025-05-29 12:23_

---

_@carljm reviewed on 2025-05-29 23:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/known_instance.rs`:330 on 2025-05-29 23:28_

Oops, my mistake, I mis-interpreted that comment. I'll roll this back and see if it causes a problem.

---

_@carljm reviewed on 2025-05-29 23:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/known_instance.rs`:330 on 2025-05-29 23:37_

https://github.com/astral-sh/ruff/pull/18377

---

_@AlexWaygood reviewed on 2025-05-30 06:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/known_instance.rs`:330 on 2025-05-30 06:34_

Thank you!

---

_Branch deleted on 2025-06-28 23:12_

---
