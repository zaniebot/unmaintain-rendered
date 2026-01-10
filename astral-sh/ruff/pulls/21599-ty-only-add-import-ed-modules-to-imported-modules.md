```yaml
number: 21599
title: "[ty] Only add `import`ed modules to `imported_modules` if the `import` statement is in the global scope"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/submodule-attr-global-scope
created_at: 2025-11-23T22:31:01Z
updated_at: 2025-11-23T22:41:30Z
url: https://github.com/astral-sh/ruff/pull/21599
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Only add `import`ed modules to `imported_modules` if the `import` statement is in the global scope

---

_Pull request opened by @AlexWaygood on 2025-11-23 22:31_

## Summary

@Gankra pointed out in https://github.com/astral-sh/ruff/pull/21583#discussion_r2554196826 that we're currently inconsistent here between how we treat `import` statements and `from ... import ...` statements. We should probably be consistent!

## Test Plan

Added an mdtest


---

_Label `ty` added by @AlexWaygood on 2025-11-23 22:31_

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 22:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-23 22:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-chess (https://github.com/niklasf/python-chess)
+ chess/__init__.py:668:16: error[unresolved-attribute] Submodule `svg` may not be available as an attribute on module `chess`
+ chess/__init__.py:1477:16: error[unresolved-attribute] Submodule `svg` may not be available as an attribute on module `chess`
+ chess/__init__.py:3883:16: error[unresolved-attribute] Submodule `svg` may not be available as an attribute on module `chess`
+ chess/__init__.py:4324:16: error[unresolved-attribute] Submodule `svg` may not be available as an attribute on module `chess`
- Found 16 diagnostics
+ Found 20 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/cli/req_command.py:206:20: error[unresolved-attribute] Submodule `_internal` may not be available as an attribute on module `pip`
+ src/pip/_internal/cli/req_command.py:221:16: error[unresolved-attribute] Submodule `_internal` may not be available as an attribute on module `pip`
- Found 603 diagnostics
+ Found 605 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/llnl/util/lang.py:815:12: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ lib/spack/spack/llnl/util/lang.py:816:14: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- lib/spack/spack/llnl/util/lang.py:816:46: error[invalid-argument-type] Argument to function `module_from_spec` is incorrect: Expected `ModuleSpec`, found `ModuleSpec | None`
- lib/spack/spack/llnl/util/lang.py:821:17: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `ModuleSpec | None`
- lib/spack/spack/llnl/util/lang.py:823:9: warning[possibly-missing-attribute] Attribute `loader` may be missing on object of type `ModuleSpec | None`
- lib/spack/spack/llnl/util/lang.py:823:9: warning[possibly-missing-attribute] Attribute `exec_module` may be missing on object of type `Loader | None`
- lib/spack/spack/llnl/util/lang.py:826:29: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `ModuleSpec | None`
+ lib/spack/spack/util/path.py:274:11: error[unresolved-attribute] Submodule `parse` may not be available as an attribute on module `urllib`
+ lib/spack/spack/util/path.py:275:16: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
- Found 7957 diagnostics
+ Found 7956 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/main.py:1195:27: error[unresolved-attribute] Submodule `pool` may not be available as an attribute on module `multiprocessing`
+ isort/main.py:1196:17: error[unresolved-attribute] Submodule `pool` may not be available as an attribute on module `multiprocessing`
- Found 33 diagnostics
+ Found 35 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/serving.py:812:44: error[unresolved-attribute] Submodule `metadata` may not be available as an attribute on module `importlib`
- Found 398 diagnostics
+ Found 399 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/__init__.py:55:16: error[unresolved-attribute] Submodule `metadata` may not be available as an attribute on module `importlib`
- Found 179 diagnostics
+ Found 180 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_utils_spider.py:29:30: error[unresolved-attribute] Submodule `test_utils_spider` may not be available as an attribute on module `tests`
- Found 1727 diagnostics
+ Found 1728 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/config/__init__.py:1308:24: error[unresolved-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
+ src/_pytest/debugging.py:168:26: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/debugging.py:252:18: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:138:31: error[unresolved-attribute] Submodule `python` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:140:31: error[unresolved-attribute] Submodule `python` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:142:31: error[unresolved-attribute] Submodule `python` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:448:45: error[unresolved-attribute] Submodule `python` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:464:42: error[unresolved-attribute] Submodule `python` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:1962:10: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/fixtures.py:2021:10: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/mark/__init__.py:136:14: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/pytester.py:1246:18: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/terminal.py:393:20: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ src/_pytest/unittest.py:542:19: error[unresolved-attribute] Submodule `metadata` may not be available as an attribute on module `importlib`
+ testing/acceptance_test.py:932:18: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ testing/acceptance_test.py:933:16: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
+ testing/test_assertrewrite.py:1930:20: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`
+ testing/test_pastebin.py:113:29: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ testing/test_pastebin.py:134:29: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ testing/test_pastebin.py:150:25: error[unresolved-attribute] Submodule `error` may not be available as an attribute on module `urllib`
+ testing/test_pastebin.py:167:25: error[unresolved-attribute] Submodule `error` may not be available as an attribute on module `urllib`
+ testing/test_pastebin.py:193:29: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ testing/test_pathlib.py:423:13: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ testing/test_subtests.py:879:13: error[unresolved-attribute] Submodule `subtests` may not be available as an attribute on module `_pytest`
+ testing/test_unittest.py:440:29: error[unresolved-attribute] Submodule `unittest` may not be available as an attribute on module `_pytest`
+ testing/test_unittest.py:441:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 459 diagnostics
+ Found 485 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/diff.py:548:20: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ dulwich/porcelain.py:367:16: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `email`
- Found 183 diagnostics
+ Found 185 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ test/integration/test_backwards_compatibility.py:47:21: error[unresolved-attribute] Submodule `translate` may not be available as an attribute on module `sockeye`
+ test/integration/test_backwards_compatibility.py:54:13: error[unresolved-attribute] Submodule `translate` may not be available as an attribute on module `sockeye`
- Found 413 diagnostics
+ Found 415 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ tests/console/test_application.py:170:5: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `poetry`
+ tests/utils/env/test_env.py:519:26: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `poetry`
+ tests/utils/test_helpers.py:279:5: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `poetry`
- Found 968 diagnostics
+ Found 971 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/core/transport.py:170:18: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ src/schemathesis/engine/phases/unit/_executor.py:153:12: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:154:38: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:157:30: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:158:32: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:161:40: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:170:12: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:173:12: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:226:41: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:230:12: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/engine/phases/unit/_executor.py:264:13: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `hypothesis`
+ src/schemathesis/specs/openapi/auths.py:61:22: error[unresolved-attribute] Submodule `auth` may not be available as an attribute on module `requests`
- Found 290 diagnostics
+ Found 302 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/test/circlerefs_test.py:200:14: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
- Found 320 diagnostics
+ Found 321 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/api/dataframe/container.py:1302:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/dataframe/container.py:1315:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/dataframe/container.py:1326:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/dataframe/container.py:1339:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/dataframe/container.py:1364:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/pyspark/container.py:488:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/pyspark/container.py:501:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/pyspark/container.py:512:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/pyspark/container.py:525:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
+ pandera/api/pyspark/container.py:550:16: error[unresolved-attribute] Submodule `io` may not be available as an attribute on module `pandera`
- Found 1635 diagnostics
+ Found 1645 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ test/contrib/emscripten/test_emscripten.py:40:16: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:75:16: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:147:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ test/contrib/emscripten/test_emscripten.py:168:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ test/contrib/emscripten/test_emscripten.py:189:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ test/contrib/emscripten/test_emscripten.py:243:16: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:358:20: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
- test/contrib/emscripten/test_emscripten.py:379:9: error[invalid-assignment] Implicit shadowing of function `is_cross_origin_isolated`
+ test/contrib/emscripten/test_emscripten.py:379:9: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:402:16: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:1037:16: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:1056:16: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:1160:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ test/contrib/emscripten/test_emscripten.py:1163:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ test/contrib/emscripten/test_emscripten.py:1186:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ test/contrib/emscripten/test_emscripten.py:1215:24: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
- test/contrib/emscripten/test_emscripten.py:1221:13: error[invalid-assignment] Implicit shadowing of function `_run_sync_with_timeout`
+ test/contrib/emscripten/test_emscripten.py:1219:23: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:1221:13: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:1223:9: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
- test/contrib/emscripten/test_emscripten.py:1231:13: error[invalid-assignment] Implicit shadowing of function `_run_sync_with_timeout`
+ test/contrib/emscripten/test_emscripten.py:1229:23: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
+ test/contrib/emscripten/test_emscripten.py:1231:13: error[unresolved-attribute] Submodule `contrib` may not be available as an attribute on module `urllib3`
- Found 332 diagnostics
+ Found 349 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/fix_psycopg.py:97:16: error[unresolved-attribute] Submodule `generators` may not be available as an attribute on module `psycopg`
+ tests/test_connection_info.py:60:25: error[unresolved-attribute] Submodule `_pq_ctypes` may not be available as an attribute on module `psycopg.pq`
- tests/test_dns.py:13:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 663 diagnostics
+ Found 664 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ schema_salad/tests/test_ref_resolver.py:168:15: error[unresolved-attribute] Submodule `ref_resolver` may not be available as an attribute on module `schema_salad`
+ schema_salad/tests/test_ref_resolver.py:169:14: error[unresolved-attribute] Submodule `ref_resolver` may not be available as an attribute on module `schema_salad`
- Found 213 diagnostics
+ Found 215 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ comtypes/_post_coinit/misc.py:170:21: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `comtypes`
+ comtypes/automation.py:820:38: error[unresolved-attribute] Submodule `typeinfo` may not be available as an attribute on module `comtypes`
+ comtypes/clear_cache.py:47:20: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `comtypes`
+ comtypes/client/_code_cache.py:102:45: error[unresolved-attribute] Submodule `gen` may not be available as an attribute on module `comtypes`
- comtypes/client/_code_cache.py:121:13: error[invalid-assignment] Object of type `ModuleType` is not assignable to attribute `gen` of type `<module 'comtypes.gen'>`
+ comtypes/client/_code_cache.py:121:13: error[unresolved-attribute] Unresolved attribute `gen` on type `<module 'comtypes'>`.
+ comtypes/client/_code_cache.py:122:13: error[unresolved-attribute] Submodule `gen` may not be available as an attribute on module `comtypes`
+ comtypes/test/__init__.py:257:22: error[unresolved-attribute] Submodule `test` may not be available as an attribute on module `comtypes`
+ comtypes/test/test_client.py:138:20: error[unresolved-attribute] Submodule `stream` may not be available as an attribute on module `comtypes`
+ comtypes/test/test_client.py:143:20: error[unresolved-attribute] Submodule `automation` may not be available as an attribute on module `comtypes`
+ comtypes/test/test_client.py:148:20: error[unresolved-attribute] Submodule `typeinfo` may not be available as an attribute on module `comtypes`
+ comtypes/test/test_client.py:153:20: error[unresolved-attribute] Submodule `persist` may not be available as an attribute on module `comtypes`
+ comtypes/test/test_comserver.py:313:13: error[unresolved-attribute] Submodule `test_comserver` may not be available as an attribute on module `comtypes.test`
+ comtypes/test/test_showevents.py:11:41: error[unresolved-attribute] Submodule `test` may not be available as an attribute on module `comtypes`
+ comtypes/test/test_variant.py:321:11: error[unresolved-attribute] Submodule `automation` may not be available as an attribute on module `comtypes`
- Found 365 diagnostics
+ Found 378 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/maptype.py:33:28: error[unresolved-attribute] Submodule `typeops` may not be available as an attribute on module `mypy`
+ mypy/parse.py:27:12: error[unresolved-attribute] Submodule `fastparse` may not be available as an attribute on module `mypy`
+ mypy/subtypes.py:2054:24: error[unresolved-attribute] Submodule `solve` may not be available as an attribute on module `mypy`
+ mypyc/build.py:131:27: error[unresolved-attribute] Submodule `core` may not be available as an attribute on module `distutils`
- Found 1743 diagnostics
+ Found 1747 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/search/ja.py:90:23: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `ctypes`
+ sphinx/search/ja.py:92:23: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `ctypes`
- Found 573 diagnostics
+ Found 575 diagnostics

pyodide (https://github.com/pyodide/pyodide)
+ src/tests/test_stack_switching.py:109:9: error[unresolved-attribute] Submodule `metadata` may not be available as an attribute on module `importlib`
+ src/tests/test_stack_switching.py:113:12: error[unresolved-attribute] Submodule `metadata` may not be available as an attribute on module `importlib`
- Found 992 diagnostics
+ Found 994 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_tests/test_threads.py:234:23: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `ctypes`
- Found 531 diagnostics
+ Found 532 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/dependencies/python.py:226:14: error[unresolved-attribute] Submodule `resources` may not be available as an attribute on module `importlib`
+ mesonbuild/mesonmain.py:248:38: error[unresolved-attribute] Submodule `options` may not be available as an attribute on module `mesonbuild`
+ mesonbuild/mesonmain.py:248:78: error[unresolved-attribute] Submodule `options` may not be available as an attribute on module `mesonbuild`
+ mesonbuild/mesonmain.py:249:19: error[unresolved-attribute] Submodule `options` may not be available as an attribute on module `mesonbuild`
+ mesonbuild/modules/python.py:420:21: error[unresolved-attribute] Submodule `resources` may not be available as an attribute on module `importlib`
- mesonbuild/scripts/python_info.py:5:1: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ packaging/createmsi.py:232:15: error[unresolved-attribute] Submodule `dom` may not be available as an attribute on module `xml`
+ packaging/createpkg.py:93:15: error[unresolved-attribute] Submodule `dom` may not be available as an attribute on module `xml`
+ run_project_tests.py:556:5: error[unresolved-attribute] Submodule `interpreterbase` may not be available as an attribute on module `mesonbuild`
- Found 1900 diagnostics
+ Found 1907 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/config/config_options.py:1194:16: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ mkdocs/config/config_options.py:1197:18: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- Found 226 diagnostics
+ Found 228 diagnostics

vision (https://github.com/pytorch/vision)
+ references/classification/presets.py:10:16: error[unresolved-attribute] Submodule `v2` may not be available as an attribute on module `torchvision.transforms`
+ references/detection/presets.py:13:16: error[unresolved-attribute] Submodule `v2` may not be available as an attribute on module `torchvision.transforms`
+ references/detection/presets.py:13:43: error[unresolved-attribute] Submodule `tv_tensors` may not be available as an attribute on module `torchvision`
+ references/segmentation/presets.py:11:16: error[unresolved-attribute] Submodule `v2` may not be available as an attribute on module `torchvision.transforms`
+ references/segmentation/presets.py:11:43: error[unresolved-attribute] Submodule `tv_tensors` may not be available as an attribute on module `torchvision`
+ torchvision/transforms/v2/functional/_utils.py:58:24: error[unresolved-attribute] Submodule `v2` may not be available as an attribute on module `torchvision.transforms`
- Found 1460 diagnostics
+ Found 1466 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/util.py:2119:24: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `ctypes`
- Found 1204 diagnostics
+ Found 1205 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/server/database/orm_models.py:1592:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/prefect/server/database/orm_models.py:1608:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ src/prefect/server/database/orm_models.py:1592:18: error[unresolved-attribute] Submodule `database` may not be available as an attribute on module `prefect.server`
+ src/prefect/server/database/orm_models.py:1608:18: error[unresolved-attribute] Submodule `database` may not be available as an attribute on module `prefect.server`

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/dist.py:767:24: error[unresolved-attribute] Submodule `command` may not be available as an attribute on module `distutils`
+ setuptools/_distutils/dist.py:793:24: error[unresolved-attribute] Submodule `command` may not be available as an attribute on module `distutils`
+ setuptools/_vendor/wheel/__main__.py:19:14: error[unresolved-attribute] Submodule `cli` may not be available as an attribute on module `wheel`
+ setuptools/tests/test_egg_info.py:179:32: error[unresolved-attribute] Submodule `errors` may not be available as an attribute on module `distutils`
+ setuptools/windows_support.py:23:34: error[unresolved-attribute] Submodule `wintypes` may not be available as an attribute on module `ctypes`
+ setuptools/windows_support.py:23:58: error[unresolved-attribute] Submodule `wintypes` may not be available as an attribute on module `ctypes`
+ setuptools/windows_support.py:24:33: error[unresolved-attribute] Submodule `wintypes` may not be available as an attribute on module `ctypes`
- Found 1274 diagnostics
+ Found 1281 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/utils/bulkimport.py:255:23: error[unresolved-attribute] Submodule `plugins` may not be available as an attribute on module `openlibrary`
- Found 1119 diagnostics
+ Found 1120 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/metadata.py:92:32: error[unresolved-attribute] Submodule `licenses` may not be available as an attribute on module `packaging`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ src/scikit_build_core/builder/sysconfig.py:174:22: error[unresolved-attribute] Submodule `sysconfig` may not be available as an attribute on module `distutils`
+ tests/test_pyproject_pep660.py:103:22: error[unresolved-attribute] Submodule `sysconfig` may not be available as an attribute on module `distutils`
- Found 44 diagnostics
+ Found 48 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/__init__.py:149:19: error[unresolved-attribute] Submodule `ptmalloc` may not be available as an attribute on module `pwndbg.aglib.heap`
+ pwndbg/aglib/heap/__init__.py:158:19: error[unresolved-attribute] Submodule `ptmalloc` may not be available as an attribute on module `pwndbg.aglib.heap`
+ pwndbg/aglib/proc.py:104:33: error[unresolved-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/proc.py:113:16: error[unresolved-attribute] Submodule `elf` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/aglib/proc.py:122:16: error[unresolved-attribute] Submodule `elf` may not be available as an attribute on module `pwndbg.aglib`
- pwndbg/commands/__init__.py:73:24: error[unresolved-attribute] Module `pwndbg.dbg` has no member `commands`
- pwndbg/commands/__init__.py:79:4: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
+ pwndbg/commands/__init__.py:208:52: error[invalid-argument-type] Argument to bound method `add_command` is incorrect: Expected `str`, found `Unknown | str | None`
- pwndbg/commands/__init__.py:208:29: error[unresolved-attribute] Module `pwndbg.dbg` has no member `add_command`
- pwndbg/commands/__init__.py:211:33: error[unresolved-attribute] Module `pwndbg.dbg` has no member `add_command`
- pwndbg/commands/__init__.py:312:16: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/commands/__init__.py:318:24: error[unresolved-attribute] Module `pwndbg.dbg` has no member `lex_args`
- pwndbg/commands/__init__.py:350:21: error[unresolved-attribute] Module `pwndbg.dbg` has no member `history`
+ pwndbg/commands/__init__.py:486:61: error[invalid-assignment] Object of type `(Frame & ~AlwaysFalsy) | Process | None` is not assignable to `Frame | Process`
- pwndbg/commands/__init__.py:435:48: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:439:55: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:444:51: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:448:55: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/commands/__init__.py:485:13: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
- pwndbg/commands/__init__.py:487:29: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/commands/__init__.py:774:26: warning[possibly-missing-attribute] Attribute `is_dynamically_linked` may be missing on object of type `Process | None`
- pwndbg/commands/__init__.py:585:12: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
- pwndbg/commands/__init__.py:774:26: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
+ pwndbg/commands/__init__.py:848:61: error[invalid-assignment] Object of type `(Frame & ~AlwaysFalsy) | Process | None` is not assignable to `Frame | Process`
- pwndbg/commands/__init__.py:847:13: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
- pwndbg/commands/__init__.py:849:29: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_inferior`
- pwndbg/commands/__init__.py:889:8: error[unresolved-attribute] Module `pwndbg.dbg` has no member `is_gdblib_available`
+ pwndbg/commands/__init__.py:856:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Unknown | Value | None`
- pwndbg/dbg/gdb/__init__.py:156:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`
+ pwndbg/dbg/gdb/__init__.py:156:24: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:416:16: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/gdb/__init__.py:467:20: error[unresolved-attribute] Submodule `remote` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/dbg/gdb/__init__.py:467:20: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:481:51: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:625:12: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:628:33: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/gdb/__init__.py:724:19: error[unresolved-attribute] Submodule `typeinfo` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/dbg/gdb/__init__.py:724:19: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/gdb/__init__.py:733:12: error[unresolved-attribute] Submodule `proc` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/dbg/gdb/__init__.py:733:12: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/gdb/__init__.py:759:56: error[unresolved-attribute] Object of type `None` has no attribute `xpsr`
+ pwndbg/dbg/gdb/__init__.py:759:56: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:909:17: error[unresolved-attribute] Submodule `info` may not be available as an attribute on module `pwndbg.gdblib`
+ pwndbg/dbg/gdb/__init__.py:952:33: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:961:21: error[unresolved-attribute] Submodule `info` may not be available as an attribute on module `pwndbg.gdblib`
+ pwndbg/dbg/gdb/__init__.py:1758:34: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/gdb/__init__.py:1759:25: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/lldb/__init__.py:147:12: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`
+ pwndbg/dbg/lldb/__init__.py:147:24: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:153:30: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:211:43: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/lldb/__init__.py:1020:21: error[unresolved-attribute] Submodule `vmmap` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/dbg/lldb/__init__.py:1020:21: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:1277:15: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:1365:13: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:1366:16: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:1386:26: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:1391:16: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/lldb/__init__.py:1724:19: error[unresolved-attribute] Module `pwndbg.dbg` has no member `selected_frame`
+ pwndbg/dbg/lldb/__init__.py:1724:19: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Frame | None`
+ pwndbg/dbg/lldb/__init__.py:1724:19: warning[possibly-missing-attribute] Attribute `pc` may be missing on object of type `Frame | None`
- pwndbg/dbg/lldb/__init__.py:1759:16: error[unresolved-attribute] Submodule `file` may not be available as an attribute on module `pwndbg.aglib`
+ pwndbg/dbg/lldb/__init__.py:1759:16: error[unresolved-attribute] Submodule `aglib` may not be available as an attribute on module `pwndbg`
+ pwndbg/dbg/lldb/__init__.py:1912:9: error[unresolved-attribute] Submodule `commands` may not be available as an attribute on module `pwndbg`
- pwndbg/dbg/lldb/__init__.py:1914:9: error[unresolved-attribute] Submodule `comments` may not be available as an attribute on module `pwndbg.commands`
+ pwndbg/dbg/lldb/__init__.py:1914:9: error[unresolved-attribute] Submodule `commands` may not be available as an attribute on module `pwndbg`
+ pwndbg/gdblib/events.py:314:13: error[unresolved-attribute] Submodule `exception` may not be available as an attribute on module `pwndbg`
- pwndbg/lib/tips.py:96:8: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- pwndbg/lib/tips.py:98:10: error[unresolved-attribute] Module `pwndbg.dbg` has no member `name`
- Found 2816 diagnostics
+ Found 2825 diagnostics

colour (https://github.com/colour-science/colour)
- colour/utilities/tests/test_deprecation.py:326:13: error[unresolved-attribute] Module `colour.utilities.tests.test_deprecated` has no member `OLD_NAME`
+ colour/utilities/tests/test_deprecation.py:321:16: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `colour.utilities`
+ colour/utilities/tests/test_deprecation.py:326:13: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `colour.utilities`
+ colour/utilities/tests/test_deprecation.py:343:13: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `colour.utilities`
+ colour/utilities/tests/test_deprecation.py:378:29: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `colour.utilities`
- Found 385 diagnostics
+ Found 388 diagnostics

altair (https://github.com/vega/altair)
+ altair/utils/schemapi.py:284:21: error[unresolved-attribute] Submodule `jsonschema` may not be available as an attribute on module `referencing`
- Found 1026 diagnostics
+ Found 1027 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/framework/editor/vss.py:74:15: error[unresolved-attribute] Module `win32com.client.gencache` has no member `EnsureModule`
+ Pythonwin/pywin/framework/editor/vss.py:74:15: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ Pythonwin/pywin/framework/editor/vss.py:88:28: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
- com/win32com/demos/connect.py:83:18: error[unresolved-attribute] Module `win32com.client` has no member `connect`
+ com/win32com/demos/connect.py:80:14: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/demos/connect.py:83:18: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/server/policy.py:684:23: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/server/policy.py:689:27: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/server/register.py:133:22: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ com/win32com/server/util.py:24:25: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ com/win32com/servers/interp.py:54:12: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ com/win32com/test/pippo_server.py:92:5: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
- com/win32com/test/testDynamic.py:61:51: error[invalid-argument-type] Argument to function `Dispatch` is incorrect: Expected `str | PyIDispatch | tuple[type[str], <class 'PyIID'>] | PyIUnknown`, found `PyIID`
+ com/win32com/test/testDynamic.py:50:10: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ com/win32com/test/testDynamic.py:51:37: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ com/win32com/test/testDynamic.py:61:18: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/test/testPyComTest.py:456:9: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/test/testPyComTest.py:459:15: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32com/test/testPyComTest.py:671:10: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
+ com/win32com/test/testPyComTest.py:674:15: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
+ com/win32com/test/testPyComTest.py:841:10: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
+ com/win32com/test/testPyComTest.py:844:14: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
+ com/win32com/test/testPyComTest.py:861:10: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
+ com/win32com/test/testPyComTest.py:895:14: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
+ com/win32com/test/testPyComTest.py:898:14: warning[possibly-missing-attribute] Attribute `client` may be missing on object of type `<module 'win32com'> | Unknown`
- com/win32com/test/testall.py:59:5: error[missing-argument] No argument provided for required parameter `name` of bound method `__init__`
+ com/win32com/test/testall.py:59:5: error[unresolved-attribute] Submodule `gencache` may not be available as an attribute on module `win32com.client`
- com/win32com/universal.py:49:20: error[unresolved-attribute] Module `win32com.client.build` has no member `VTableItem`
+ com/win32com/universal.py:49:20: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `win32com`
+ com/win32comext/adsi/demos/test.py:158:24: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `win32com`
+ win32/scripts/pywin32_postinstall.py:159:34: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`
+ win32/scripts/pywin32_postinstall.py:167:14: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`
+ win32/scripts/pywin32_postinstall.py:168:12: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`
+ win32/scripts/pywin32_postinstall.py:169:11: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ win32/scripts/pywin32_postinstall.py:215:16: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ win32/scripts/pywin32_postinstall.py:217:16: error[unresolved-attribute] Submodule `server` may not be available as an attribute on module `win32com`
+ win32/scripts/regsetup.py:40:32: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`
- Found 2656 diagnostics
+ Found 2683 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ lib-injection/sources/sitecustomize.py:341:16: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ setup.py:182:12: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ setup.py:185:11: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ tests/appsec/appsec/test_constants.py:11:16: error[unresolved-attribute] Submodule `constants` may not be available as an attribute on module `ddtrace`
+ tests/appsec/contrib_appsec/django_app/urls.py:108:35: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/django_app/urls.py:109:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/django_app/urls.py:114:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/django_app/urls.py:185:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/django_app/urls.py:189:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/django_app/urls.py:190:14: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:186:42: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:187:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:192:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:256:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:257:18: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:269:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/fastapi_app/app.py:272:18: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/flask_app/app.py:101:27: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/flask_app/app.py:102:26: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/flask_app/app.py:107:26: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/flask_app/app.py:178:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/flask_app/app.py:182:30: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/contrib_appsec/flask_app/app.py:183:14: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/iast/aspects/test_other_patching.py:96:29: error[unresolved-attribute] Submodule `parse` may not be available as an attribute on module `urllib`
+ tests/appsec/iast/fixtures/taint_sinks/ssrf.py:54:12: error[unresolved-attribute] Submodule `client` may not be available as an attribute on module `http`
+ tests/appsec/iast/fixtures/taint_sinks/ssrf.py:79:16: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ tests/appsec/iast/test_overhead_control_engine.py:228:10: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ tests/appsec/iast/test_overhead_control_engine.py:235:23: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ tests/appsec/iast_packages/packages/pkg_docutils.py:28:27: error[unresolved-attribute] Submodule `core` may not be available as an attribute on module `docutils`
+ tests/appsec/iast_packages/packages/pkg_docutils.py:59:27: error[unresolved-attribute] Submodule `core` may not be available as an attribute on module `docutils`
+ tests/coverage/test_coverage_multiprocessing.py:200:14: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ tests/coverage/test_coverage_multiprocessing.py:246:18: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ tests/coverage/test_coverage_threading.py:97:10: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ tests/coverage/test_coverage_threading.py:137:14: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ tests/debugging/test_config.py:18:22: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/debugging/test_config.py:19:25: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/debugging/test_config.py:22:13: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/debugging/test_config.py:24:13: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/debugging/test_config.py:26:19: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/debugging/test_config.py:29:13: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/debugging/test_config.py:30:13: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `ddtrace.internal`
+ tests/internal/crashtracker/utils.py:67:28: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `ctypes`
+ tests/internal/test_module.py:374:16: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ tests/internal/test_module.py:375:18: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ tests/internal/test_module.py:377:18: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
-

... (truncated 80 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-23 22:35_

---

_Comment by @AlexWaygood on 2025-11-23 22:41_

Ah, hmm, I suppose there actually is no inconsistency right now, because currently `from ... import ...` imports only ever create `Definition`s (never available submodule attributes). But https://github.com/astral-sh/ruff/pull/21583, as it currently stands, would _create_ an inconsistency (because it starts to add available submodule attributes based on `from ... import ...` imports, sometimes).

The number of ecosystem hits here makes me think I should just adjust #21583 rather than changing anything about how we do non-`from` imports.

---

_Closed by @AlexWaygood on 2025-11-23 22:41_

---

_Branch deleted on 2025-11-23 22:41_

---
