```yaml
number: 18058
title: "[ty] only issue redundant-cast on fully static types"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
base: main
head: cjm/redundant-cast
created_at: 2025-05-12T23:09:57Z
updated_at: 2025-05-15T02:38:54Z
url: https://github.com/astral-sh/ruff/pull/18058
synced_at: 2026-01-12T15:56:11Z
```

# [ty] only issue redundant-cast on fully static types

---

_@carljm_

## Summary

I initially was just going to silence redundant-cast from Unknown to Unknown (or Any to Any), since we keep seeing this crop up as a false positive, and it trivially violates the gradual guarantee to error on this.

But then I realized that the gradual-guarantee argument, and the entire theoretical basis for this change, applies to _any_ non-fully-static type.

So I think we should just not issue this diagnostic at all, unless the casted type is fully static.

## Test Plan

Adjusted/added mdtests.


---

_Review requested from @AlexWaygood by @carljm on 2025-05-12 23:09_

---

_Review requested from @sharkdp by @carljm on 2025-05-12 23:09_

---

_Review requested from @dcreager by @carljm on 2025-05-12 23:09_

---

_Label `ty` added by @carljm on 2025-05-12 23:09_

---

_Comment by @github-actions[bot] on 2025-05-12 23:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- warning[redundant-cast] pytest_robotframework/_internal/pytest/plugin.py:193:9: Value is already of type `Unknown`
- warning[redundant-cast] pytest_robotframework/_internal/pytest/plugin.py:571:16: Value is already of type `Unknown`
- warning[redundant-cast] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:151:40: Value is already of type `Unknown`
- warning[redundant-cast] tests/conftest.py:52:12: Value is already of type `Unknown`
- warning[redundant-cast] tests/conftest.py:347:20: Value is already of type `Unknown`
- Found 234 diagnostics
+ Found 229 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- warning[redundant-cast] nion/utils/StructuredModel.py:124:20: Value is already of type `Unknown`
- Found 22 diagnostics
+ Found 21 diagnostics

kopf (https://github.com/nolar/kopf)
- warning[redundant-cast] kopf/_cogs/structs/bodies.py:160:16: Value is already of type `Unknown`
- Found 191 diagnostics
+ Found 190 diagnostics

kornia (https://github.com/kornia/kornia)
- warning[redundant-cast] kornia/augmentation/container/augment.py:367:23: Value is already of type `Unknown`
- warning[redundant-cast] kornia/augmentation/container/augment.py:375:27: Value is already of type `Unknown`
- warning[redundant-cast] kornia/augmentation/container/augment.py:398:46: Value is already of type `Unknown`
- warning[redundant-cast] kornia/augmentation/container/video.py:146:16: Value is already of type `Unknown`
- warning[redundant-cast] kornia/contrib/extract_patches.py:35:15: Value is already of type `Unknown`
- warning[redundant-cast] kornia/contrib/extract_patches.py:48:15: Value is already of type `Unknown`
- warning[redundant-cast] kornia/geometry/boxes.py:267:22: Value is already of type `Unknown`
- warning[redundant-cast] kornia/geometry/boxes.py:600:30: Value is already of type `Unknown`
- warning[redundant-cast] kornia/geometry/keypoints.py:216:15: Value is already of type `Unknown`
- warning[redundant-cast] kornia/nerf/nerf_solver.py:211:32: Value is already of type `Unknown`
- warning[redundant-cast] kornia/nerf/nerf_solver.py:212:18: Value is already of type `Unknown`
- warning[redundant-cast] kornia/nerf/nerf_solver.py:214:43: Value is already of type `Unknown`
- Found 963 diagnostics
+ Found 951 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
- warning[redundant-cast] jwt/algorithms.py:813:20: Value is already of type `Unknown`
- Found 185 diagnostics
+ Found 184 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[redundant-cast] strawberry/aiohttp/views.py:59:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/asgi/__init__.py:63:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/chalice/views.py:35:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/django/views.py:86:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/django/views.py:117:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/exceptions/utils/source_finder.py:111:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/ext/mypy_plugin.py:400:22: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/ext/mypy_plugin.py:408:23: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/fastapi/router.py:79:23: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/flask/views.py:48:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/flask/views.py:142:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/litestar/controller.py:166:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/quart/views.py:30:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/sanic/views.py:51:16: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/schema/schema_converter.py:289:25: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/schema/schema_converter.py:418:22: Value is already of type `Unknown`
- warning[redundant-cast] strawberry/schema/schema_converter.py:453:22: Value is already of type `Unknown`
- Found 458 diagnostics
+ Found 441 diagnostics

starlette (https://github.com/encode/starlette)
+ warning[unused-ignore-comment] starlette/testclient.py:382:47: Unused blanket `type: ignore` directive
- Found 185 diagnostics
+ Found 186 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[redundant-cast] tests/utilities/test_print_schema.py:558:45: Value is already of type `dict[Unknown, Unknown]`
- Found 411 diagnostics
+ Found 410 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- warning[redundant-cast] pydantic/functional_validators.py:77:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:80:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:135:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:142:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:229:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:236:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:304:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/functional_validators.py:311:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/json_schema.py:402:15: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/types.py:1299:21: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/types.py:1300:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/types.py:1301:20: Value is already of type `Unknown`
- warning[redundant-cast] pydantic/types.py:1302:23: Value is already of type `Unknown`
- Found 787 diagnostics
+ Found 774 diagnostics

paasta (https://github.com/yelp/paasta)
- warning[redundant-cast] paasta_tools/cli/cmds/spark_run.py:1379:23: Value is already of type `list[Unknown]`
- warning[redundant-cast] paasta_tools/setup_prometheus_adapter_config.py:913:18: Value is already of type `Unknown`
- warning[redundant-cast] paasta_tools/spark_tools.py:120:41: Value is already of type `list[Unknown]`
- Found 1009 diagnostics
+ Found 1006 diagnostics

ignite (https://github.com/pytorch/ignite)
- warning[redundant-cast] ignite/distributed/comp_models/base.py:371:16: Value is already of type `list[Unknown]`
- warning[redundant-cast] ignite/distributed/comp_models/native.py:648:16: Value is already of type `dict[Unknown, Unknown]`
- warning[redundant-cast] ignite/metrics/precision.py:155:20: Value is already of type `Unknown`
- warning[redundant-cast] ignite/metrics/precision.py:157:20: Value is already of type `Unknown`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:34:88: Value is already of type `list[Unknown]`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:37:64: Value is already of type `list[Unknown]`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:39:84: Value is already of type `list[Unknown]`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:137:31: Value is already of type `Unknown`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:255:26: Value is already of type `Unknown`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:264:61: Value is already of type `Unknown`
- warning[redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:335:63: Value is already of type `Unknown`
- Found 2276 diagnostics
+ Found 2265 diagnostics

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
- warning[redundant-cast] relays/helpers.py:52:15: Value is already of type `Unknown`
- Found 144 diagnostics
+ Found 143 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- warning[redundant-cast] dragonchain/lib/faas.py:54:20: Value is already of type `dict[Unknown, Unknown]`
- Found 317 diagnostics
+ Found 316 diagnostics

twine (https://github.com/pypa/twine)
- warning[redundant-cast] twine/__main__.py:36:20: Value is already of type `Unknown`
- Found 20 diagnostics
+ Found 19 diagnostics

poetry (https://github.com/python-poetry/poetry)
- warning[redundant-cast] src/poetry/console/application.py:375:26: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/console/application.py:428:30: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/factory.py:324:21: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/installation/installer.py:362:23: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/packages/locker.py:489:30: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/packages/locker.py:496:30: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/packages/locker.py:500:30: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/packages/locker.py:504:30: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/puzzle/provider.py:241:26: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/puzzle/provider.py:245:26: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/puzzle/provider.py:249:26: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/puzzle/provider.py:253:26: Value is already of type `Unknown`
- warning[redundant-cast] src/poetry/utils/dependency_specification.py:37:22: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_add.py:852:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_add.py:1199:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_add.py:1231:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_add.py:1344:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:93:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:157:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:217:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:276:15: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:294:15: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:326:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/commands/test_remove.py:389:17: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/test_application.py:240:16: Value is already of type `Unknown`
- warning[redundant-cast] tests/console/test_application.py:246:16: Value is already of type `Unknown`
- Found 1056 diagnostics
+ Found 1030 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- warning[redundant-cast] dummyserver/asgi_proxy.py:110:22: Value is already of type `Unknown`
- Found 465 diagnostics
+ Found 464 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- warning[redundant-cast] src/aiortc/contrib/media.py:457:22: Value is already of type `Unknown`
- Found 132 diagnostics
+ Found 131 diagnostics

operator (https://github.com/canonical/operator)
- warning[redundant-cast] ops/model.py:3624:20: Value is already of type `Unknown`
- warning[redundant-cast] ops/model.py:3731:20: Value is already of type `Unknown`
- warning[redundant-cast] ops/pebble.py:2844:39: Value is already of type `Unknown`
- warning[redundant-cast] ops/pebble.py:2870:39: Value is already of type `Unknown`
- Found 203 diagnostics
+ Found 199 diagnostics

colour (https://github.com/colour-science/colour)
- warning[redundant-cast] colour/appearance/ciecam02.py:780:12: Value is already of type `Unknown`
- warning[redundant-cast] colour/appearance/ciecam02.py:828:12: Value is already of type `Unknown`
- warning[redundant-cast] colour/examples/characterisation/examples_aces_it.py:66:9: Value is already of type `Unknown`
- warning[redundant-cast] colour/examples/characterisation/examples_aces_it.py:77:5: Value is already of type `Unknown`
- warning[redundant-cast] colour/examples/characterisation/examples_aces_it.py:77:5: Value is already of type `Unknown`
- warning[redundant-cast] colour/examples/characterisation/examples_aces_it.py:77:5: Value is already of type `Unknown`
- warning[redundant-cast] colour/io/luts/lut.py:330:36: Value is already of type `list[Unknown]`
- warning[redundant-cast] colour/io/tests/test_fichet2021.py:193:13: Value is already of type `Unknown`
- warning[redundant-cast] colour/io/tests/test_fichet2021.py:243:13: Value is already of type `Unknown`
- warning[redundant-cast] colour/plotting/colorimetry.py:530:12: Value is already of type `list[Unknown]`
- warning[redundant-cast] colour/plotting/colorimetry.py:695:19: Value is already of type `list[Unknown]`
- warning[redundant-cast] colour/plotting/common.py:1320:37: Value is already of type `list[Unknown]`
- warning[redundant-cast] colour/plotting/graph.py:91:40: Value is already of type `Unknown`
- warning[redundant-cast] colour/plotting/models.py:537:20: Value is already of type `list[Unknown]`
- warning[redundant-cast] colour/plotting/volume.py:505:20: Value is already of type `list[Unknown]`
- warning[redundant-cast] colour/recovery/otsu2018.py:416:33: Value is already of type `Unknown`
- warning[redundant-cast] colour/recovery/otsu2018.py:417:23: Value is already of type `Unknown`
- warning[redundant-cast] colour/recovery/otsu2018.py:418:32: Value is already of type `Unknown`
- warning[redundant-cast] colour/recovery/otsu2018.py:890:41: Value is already of type `Unknown`
- Found 655 diagnostics
+ Found 636 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- warning[redundant-cast] arviz/stats/stats.py:406:14: Value is already of type `Unknown`
- warning[redundant-cast] arviz/stats/stats.py:416:17: Value is already of type `Unknown`
- Found 852 diagnostics
+ Found 850 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[redundant-cast] mitmproxy/proxy/layers/tls.py:382:37: Value is already of type `Unknown`
- Found 2168 diagnostics
+ Found 2167 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- warning[redundant-cast] scrapy/commands/crawl.py:36:13: Value is already of type `Unknown`
- warning[redundant-cast] scrapy/http/request/form.py:204:30: Value is already of type `Unknown`
- warning[redundant-cast] scrapy/spiders/crawl.py:133:25: Value is already of type `Unknown`
- warning[redundant-cast] scrapy/spiders/crawl.py:136:23: Value is already of type `Unknown`
- warning[redundant-cast] scrapy/spiders/crawl.py:142:13: Value is already of type `Unknown`
- warning[redundant-cast] scrapy/utils/testproc.py:75:25: Value is already of type `Unknown`
- warning[redundant-cast] tests/utils/testproc.py:66:25: Value is already of type `Unknown`
- Found 1430 diagnostics
+ Found 1423 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- warning[redundant-cast] dedupe/datamodel.py:94:26: Value is already of type `Unknown`
- Found 69 diagnostics
+ Found 68 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- warning[redundant-cast] backend/models/core_models.py:288:34: Value is already of type `Unknown`
- warning[redundant-cast] backend/models/core_models.py:346:34: Value is already of type `Unknown`
- warning[redundant-cast] backend/models/core_models.py:379:29: Value is already of type `Unknown`
- warning[redundant-cast] backend/models/core_models.py:414:40: Value is already of type `Unknown`
- warning[redundant-cast] backend/models/core_models.py:445:38: Value is already of type `Unknown`
- warning[redundant-cast] backend/models/core_models.py:488:38: Value is already of type `Unknown`
- warning[redundant-cast] backend/services/utils.py:197:26: Value is already of type `Unknown`
- Found 125 diagnostics
+ Found 118 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- warning[redundant-cast] src/hydra_zen/_launch.py:92:12: Value is already of type `T`
- warning[redundant-cast] src/hydra_zen/_utils/coerce.py:107:16: Value is already of type `_T`
- warning[redundant-cast] src/hydra_zen/_utils/coerce.py:178:16: Value is already of type `_T`
- warning[redundant-cast] src/hydra_zen/third_party/beartype.py:130:12: Value is already of type `_T`
- warning[redundant-cast] src/hydra_zen/third_party/pydantic.py:111:16: Value is already of type `_T`
- Found 669 diagnostics
+ Found 664 diagnostics

mypy (https://github.com/python/mypy)
- warning[redundant-cast] mypy/types.py:2146:20: Value is already of type `Unknown`
- Found 3371 diagnostics
+ Found 3370 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- warning[redundant-cast] tests/integration_tests/test_instance_id.py:55:16: Value is already of type `Unknown`
- Found 733 diagnostics
+ Found 732 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- warning[redundant-cast] cwltool/cwlviewer.py:163:16: Value is already of type `list[Unknown]`
- warning[redundant-cast] cwltool/process.py:496:40: Value is already of type `Unknown`
- Found 338 diagnostics
+ Found 336 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
- warning[redundant-cast] src/capture_method/Screenshot using QT attempt.py:33:31: Value is already of type `Unknown`
- Found 53 diagnostics
+ Found 52 diagnostics

rclip (https://github.com/yurijmikhalevich/rclip)
- warning[redundant-cast] rclip/model.py:66:41: Value is already of type `Unknown`
- warning[redundant-cast] rclip/model.py:97:12: Value is already of type `Unknown`
- warning[redundant-cast] rclip/model.py:116:12: Value is already of type `Unknown`
- warning[redundant-cast] rclip/model.py:124:12: Value is already of type `Unknown`
- Found 20 diagnostics
+ Found 16 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[redundant-cast] static_frame/core/index_hierarchy.py:2751:16: Value is already of type `Unknown`
- Found 2764 diagnostics
+ Found 2763 diagnostics

meson (https://github.com/mesonbuild/meson)
- warning[redundant-cast] docs/extensions/refman_links.py:102:19: Value is already of type `Unknown`
- warning[redundant-cast] mesonbuild/envconfig.py:241:16: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/envconfig.py:282:19: Value is already of type `dict[Unknown, Unknown]`
- warning[redundant-cast] mesonbuild/modules/pkgconfig.py:267:32: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/modules/pkgconfig.py:269:22: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/modules/rust.py:197:19: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/modules/wayland.py:70:29: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/options.py:705:35: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/options.py:772:35: Value is already of type `list[Unknown]`
- warning[redundant-cast] mesonbuild/utils/universal.py:1620:16: Value is already of type `list[Unknown]`
- Found 1448 diagnostics
+ Found 1438 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- warning[redundant-cast] mypy_django_plugin/transformers/fields.py:142:27: Value is already of type `Unknown`
- warning[redundant-cast] mypy_django_plugin/transformers/models.py:61:20: Value is already of type `Unknown`
- Found 1173 diagnostics
+ Found 1171 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- warning[redundant-cast] src/bokeh/plotting/contour.py:335:22: Value is already of type `Unknown`
- warning[redundant-cast] src/bokeh/plotting/contour.py:348:21: Value is already of type `Unknown`
- Found 1007 diagnostics
+ Found 1005 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- warning[redundant-cast] openlibrary/solr/updater/list.py:88:28: Value is already of type `Unknown`
- Found 750 diagnostics
+ Found 749 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- warning[redundant-cast] lib/streamlit/auth_util.py:82:24: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/auth_util.py:141:53: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/connections/snowflake_connection.py:560:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/connections/snowpark_connection.py:94:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/connections/sql_connection.py:219:20: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/connections/sql_connection.py:220:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/alert.py:317:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/arrow.py:797:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/balloons.py:47:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/bokeh_chart.py:115:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/code.py:129:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/deck_gl_json_chart.py:538:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/doc_string.py:135:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/empty.py:130:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/exception.py:88:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/form.py:354:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/graphviz_chart.py:122:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/heading.py:247:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/html.py:124:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/iframe.py:190:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/image.py:195:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/json.py:152:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/layouts.py:887:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/lib/built_in_chart_utils.py:306:12: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/lib/built_in_chart_utils.py:320:10: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/map.py:258:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/markdown.py:384:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/media.py:383:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/media.py:670:12: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/metric.py:211:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/plotly_chart.py:546:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/progress.py:177:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/pyplot.py:138:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/pyplot.py:162:15: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/snow.py:47:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/text.py:76:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/toast.py:98:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/vega_charts.py:746:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/vega_charts.py:987:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/vega_charts.py:1253:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/vega_charts.py:1463:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/vega_charts.py:1985:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/audio_input.py:287:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/button.py:1073:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/button_group.py:1032:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/camera_input.py:280:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/chat.py:644:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/checkbox.py:352:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/color_picker.py:265:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/data_editor.py:1017:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/file_uploader.py:508:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/multiselect.py:515:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/number_input.py:656:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/radio.py:403:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/select_slider.py:454:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/selectbox.py:570:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/slider.py:962:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/text_widgets.py:692:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/widgets/time_widgets.py:1007:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/elements/write.py:580:16: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/runtime/state/session_state.py:701:24: Value is already of type `Unknown`
- warning[redundant-cast] lib/streamlit/watcher/event_based_path_watcher.py:339:21: Value is already of type `Unknown`
- Found 3313 diagnostics
+ Found 3251 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[redundant-cast] ddtrace/_trace/utils_botocore/span_pointers/dynamodb.py:666:16: Value is already of type `Unknown`
- warning[redundant-cast] ddtrace/debugging/_debugger.py:419:53: Value is already of type `Unknown`
- warning[redundant-cast] ddtrace/debugging/_debugger.py:477:40: Value is already of type `Unknown`
- warning[redundant-cast] ddtrace/debugging/_signal/model.py:223:27: Value is already of type `Unknown`
- Found 8602 diagnostics
+ Found 8598 diagnostics

zulip (https://github.com/zulip/zulip)
- warning[redundant-cast] zerver/lib/test_classes.py:753:15: Value is already of type `Unknown`
- warning[redundant-cast] zerver/lib/test_classes.py:763:22: Value is already of type `Unknown`
- warning[redundant-cast] zerver/management/commands/deactivate_realm.py:51:33: Value is already of type `Any`
- warning[redundant-cast] zerver/migrations/0257_fix_has_link_attribute.py:42:29: Value is already of type `Unknown`
- Found 4940 diagnostics
+ Found 4936 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:146:17: Value is already of type `Unknown`
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:147:17: Value is already of type `Unknown`
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:155:24: Value is already of type `Unknown`
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:159:17: Value is already of type `Unknown`
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:164:27: Value is already of type `Unknown`
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:285:19: Value is already of type `Unknown`
- warning[redundant-cast] misc/python/materialize/output_consistency/validation/result_comparator.py:292:28: Value is already of type `Unknown`
- Found 3319 diagnostics
+ Found 3312 diagnostics

scipy (https://github.com/scipy/scipy)
- warning[redundant-cast] scipy/_lib/array_api_compat/array_api_compat/numpy/_aliases.py:137:14: Value is already of type `Any`
- warning[redundant-cast] scipy/_lib/array_api_extra/tests/test_at.py:212:9: Value is already of type `Unknown`
+ warning[unused-ignore-comment] scipy/_lib/array_api_extra/tests/test_helpers.py:153:36: Unused blanket `type: ignore` directive
- warning[redundant-cast] scipy/signal/tests/test_short_time_fft.py:510:59: Value is already of type `Unknown`
- Found 7733 diagnostics
+ Found 7731 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-05-12 23:15_

> But then I realized that the gradual-guarantee argument, and the entire theoretical basis for this change, applies to _any_ non-fully-static type.

I feel like the situation for type-checker directives is a bit of a special case? If I'm a user and I do a big refactor and I accidentally end up with something like

```py
def f(x: tuple[Any, ...]):
    # several hundred lines of code
    y = cast(tuple[Any, ...], x)
```

I'd _definitely_ want a warning from my type checker telling me that I can remove the `cast()` call now. That's both because it's good to use `cast()` as little as possible from a type-safety perspective (it's unsound!), and because having unnecessary type-checker directives like this clutters the codebase and makes it harder to navigate the core logic of the code

---

_Comment by @AlexWaygood on 2025-05-12 23:17_

The vast majority of the false positives in the primer diff seem related to `Unknown`. I think I'd prefer to limit the special cases to types that (recursively) contain `Unknown` or `Todo`, not other gradual types.

---

_Comment by @carljm on 2025-05-12 23:52_

Hmm. I don't like introducing behavior distinctions between `Any` and `Unknown`...

Also that would mean recursively walking the types twice, or else introducing a new `contains_unknown_or_todo` set of methods that we have to implement for a bunch of types.

---

_Comment by @AlexWaygood on 2025-05-12 23:57_

> Also that would mean recursively walking the types twice, or else introducing a new `contains_unknown_or_todo` set of methods that we have to implement for a bunch of types.

nah, we could change the existing `contains_todo` method to be a generic `any_over_type` method, similar to this one here for walking AST trees: https://github.com/astral-sh/ruff/blob/41fa082414c7277082d7e19b307baea0a4771d4c/crates/ruff_python_ast/src/helpers.rs#L129-L131

and then we could change the call site so that it becomes something like

```rs
ty.any_over_type(|atom| matches!(atom, Type::Dynamic(DynamicType::Todo | DynamicType::Unknown))
```

---

_Comment by @AlexWaygood on 2025-05-12 23:58_

> Hmm. I don't like introducing behavior distinctions between `Any` and `Unknown`...

I do agree that this isn't great. I think the _best_ fix for the underlying issue here would be to figure out why we're inferring `Unknown` for various things where we probably shouldn't be inferring `Unknown`, and add explicit `Todo` branches for those missing features instead. This has the added benefit that it records explicitly in our codebase what our missing features are.

---

_Comment by @carljm on 2025-05-13 00:16_

> figure out why we're inferring `Unknown` for various things where we probably shouldn't be inferring `Unknown`

Of course that's great as far as it goes, but I don't think it's a solution to this issue. There will always be reasons for things to be inferred as `Unknown` that are out of our control (e.g. a dependency isn't found), and I don't think we should be emitting this as a cascading diagnostic in those cases -- it really is a problematic violation of the gradual guarantee, IMO, that won't go away no matter how many specific cases of lacking inference we fix.

I agree that it seems like less of an issue with `Any`, so maybe handling them differently here is OK.

---

_Closed by @AlexWaygood on 2025-05-15 02:38_

---
