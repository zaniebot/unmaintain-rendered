```yaml
number: 18275
title: "[ty] Experiment: allow functions to be redefined (same signature)"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/allow-function-redefiniton
created_at: 2025-05-23T13:08:48Z
updated_at: 2025-06-23T06:48:30Z
url: https://github.com/astral-sh/ruff/pull/18275
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Experiment: allow functions to be redefined (same signature)

---

_Pull request opened by @sharkdp on 2025-05-23 13:08_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Explore another route towards solving https://github.com/astral-sh/ty/issues/350. The idea is to loosen the restrictions around function redefinitions, and make code like this pass without errors:
```py
def f(x: int) -> str:
    return "a"

def another_f(x: int) -> str:
    return "b"

f = another_f
```
The way this is implemented here is probably not how we would want to implement this, but the effect should be similar.

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-05-23 13:08_

---

_Comment by @github-actions[bot] on 2025-05-23 13:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- error[invalid-assignment] beartype/claw/_importlib/_clawimpload.py:377:9: Implicit shadowing of function `cache_from_source`
- Found 550 diagnostics
+ Found 549 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[invalid-assignment] pyinstrument/vendor/decorator.py:295:5: Implicit shadowing of function `__init__`
- error[invalid-assignment] pyinstrument/vendor/decorator.py:301:5: Implicit shadowing of function `__init__`
- Found 88 diagnostics
+ Found 86 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-assignment] pydantic/dataclasses.py:335:5: Implicit shadowing of function `__call__`
- Found 762 diagnostics
+ Found 761 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[invalid-assignment] dulwich/tests/utils.py:361:5: Implicit shadowing of function `showwarning`
- Found 128 diagnostics
+ Found 127 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[invalid-assignment] tornado/test/netutil_test.py:78:9: Implicit shadowing of function `getaddrinfo`
- error[invalid-assignment] tornado/test/netutil_test.py:125:9: Implicit shadowing of function `getaddrinfo`
- error[invalid-assignment] tornado/util.py:438:9: Implicit shadowing of function `websocket_mask`
- Found 351 diagnostics
+ Found 348 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[invalid-assignment] src/urllib3/util/wait.py:107:9: Implicit shadowing of function `wait_for_socket`
- error[invalid-assignment] src/urllib3/util/wait.py:109:9: Implicit shadowing of function `wait_for_socket`
- error[invalid-assignment] test/contrib/emscripten/test_emscripten.py:1178:13: Implicit shadowing of function `_run_sync_with_timeout`
- error[invalid-assignment] test/contrib/emscripten/test_emscripten.py:1188:13: Implicit shadowing of function `_run_sync_with_timeout`
- Found 449 diagnostics
+ Found 445 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-assignment] src/hydra_zen/structured_configs/_implementations.py:207:1: Implicit shadowing of function `get_yaml_loader`
- Found 638 diagnostics
+ Found 637 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-assignment] references/classification/utils.py:213:5: Implicit shadowing of function `print`
- error[invalid-assignment] references/depth/stereo/utils/distributed.py:18:5: Implicit shadowing of function `print`
- error[invalid-assignment] references/detection/utils.py:228:5: Implicit shadowing of function `print`
- error[invalid-assignment] references/optical_flow/utils.py:240:5: Implicit shadowing of function `print`
- error[invalid-assignment] references/segmentation/utils.py:233:5: Implicit shadowing of function `print`
- error[invalid-assignment] references/video_classification/utils.py:197:5: Implicit shadowing of function `print`
- Found 1840 diagnostics
+ Found 1834 diagnostics

paasta (https://github.com/yelp/paasta)
- error[invalid-assignment] paasta_tools/cli/fsm_cmd.py:44:5: Implicit shadowing of function `copyfile`
- error[invalid-assignment] paasta_tools/cli/fsm_cmd.py:45:5: Implicit shadowing of function `copymode`
- Found 934 diagnostics
+ Found 932 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[invalid-assignment] mkdocs/__main__.py:58:9: Implicit shadowing of function `showwarning`
- Found 208 diagnostics
+ Found 207 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[invalid-assignment] mesonbuild/minstall.py:183:13: Implicit shadowing of function `chown`
- error[invalid-assignment] unittests/failuretests.py:49:5: Implicit shadowing of function `which`
- error[invalid-assignment] unittests/failuretests.py:50:5: Implicit shadowing of function `_search`
- Found 1367 diagnostics
+ Found 1364 diagnostics

colour (https://github.com/colour-science/colour)
- error[invalid-assignment] colour/utilities/verbose.py:304:5: Implicit shadowing of function `showwarning`
- Found 536 diagnostics
+ Found 535 diagnostics

operator (https://github.com/canonical/operator)
- error[invalid-assignment] ops/log.py:77:5: Implicit shadowing of function `showwarning`
- Found 111 diagnostics
+ Found 110 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-assignment] src/_pytest/doctest.py:493:5: Implicit shadowing of function `unwrap`
- Found 882 diagnostics
+ Found 881 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-assignment] examples/contrib/ntlm_upstream_proxy.py:150:9: Implicit shadowing of function `start_handshake`
- error[invalid-assignment] examples/contrib/ntlm_upstream_proxy.py:151:9: Implicit shadowing of function `receive_handshake_data`
- Found 2111 diagnostics
+ Found 2109 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ warning[unused-ignore-comment] scrapy/commands/fetch.py:96:34: Unused blanket `type: ignore` directive
- Found 1303 diagnostics
+ Found 1304 diagnostics

optuna (https://github.com/optuna/optuna)
+ warning[unused-ignore-comment] tests/storages_tests/test_heartbeat.py:275:63: Unused blanket `type: ignore` directive
- Found 1067 diagnostics
+ Found 1068 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-assignment] src/bokeh/__init__.py:113:1: Implicit shadowing of function `formatwarning`
- Found 953 diagnostics
+ Found 952 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[invalid-assignment] openlibrary/plugins/openlibrary/code.py:952:1: Implicit shadowing of function `can_write`
- error[invalid-assignment] openlibrary/plugins/openlibrary/dev_instance.py:19:5: Implicit shadowing of function `query`
- error[invalid-assignment] openlibrary/plugins/openlibrary/dev_instance.py:20:5: Implicit shadowing of function `withKey`
- error[invalid-assignment] openlibrary/plugins/upstream/utils.py:1683:5: Implicit shadowing of function `get_markdown`
- Found 750 diagnostics
+ Found 746 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ warning[unused-ignore-comment] cloudinit/config/schema.py:518:48: Unused blanket `type: ignore` directive
- error[invalid-assignment] tests/unittests/early_patches.py:26:1: Implicit shadowing of function `lru_cache`
- error[invalid-assignment] tests/unittests/sources/test_configdrive.py:609:13: Implicit shadowing of function `find_devs_with`
- error[invalid-assignment] tests/unittests/sources/test_opennebula.py:66:9: Implicit shadowing of function `switch_user_cmd`
- Found 715 diagnostics
+ Found 713 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ warning[unused-ignore-comment] ddtrace/internal/compat.py:130:48: Unused blanket `type: ignore` directive
- error[invalid-assignment] ddtrace/internal/coverage/multiprocessing_coverage.py:184:5: Implicit shadowing of function `__init__`
- error[invalid-assignment] ddtrace/internal/coverage/multiprocessing_coverage.py:185:5: Implicit shadowing of function `close`
- error[invalid-assignment] ddtrace/internal/coverage/multiprocessing_coverage.py:186:5: Implicit shadowing of function `join`
- error[invalid-assignment] ddtrace/internal/coverage/multiprocessing_coverage.py:187:5: Implicit shadowing of function `kill`
- error[invalid-assignment] ddtrace/internal/coverage/multiprocessing_coverage.py:188:5: Implicit shadowing of function `terminate`
- error[invalid-assignment] ddtrace/internal/coverage/multiprocessing_coverage.py:208:9: Implicit shadowing of function `get_preparation_data`
- error[invalid-assignment] ddtrace/internal/coverage/threading_coverage.py:84:5: Implicit shadowing of function `__init__`
- error[invalid-assignment] ddtrace/internal/coverage/threading_coverage.py:86:5: Implicit shadowing of function `join`
- error[invalid-assignment] tests/appsec/architectures/mini.py:40:1: Implicit shadowing of function `_flush_events_queue`
- error[invalid-assignment] tests/appsec/contrib_appsec/utils.py:43:1: Implicit shadowing of function `finalize_asm_env`
- error[invalid-assignment] tests/appsec/utils.py:36:5: Implicit shadowing of function `finalize_asm_env`
- error[invalid-assignment] tests/cache/conftest.py:23:1: Implicit shadowing of function `cached`
- error[invalid-assignment] tests/contrib/djangorestframework/test_appsec.py:23:5: Implicit shadowing of function `finalize_asm_env`
- error[invalid-assignment] tests/contrib/integration_registry/registry_update_helpers/integration_registry_manager.py:171:9: Implicit shadowing of function `getattr`
- Found 6867 diagnostics
+ Found 6854 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-assignment] zerver/management/commands/makemessages.py:153:9: Implicit shadowing of function `templatize`
- Found 6980 diagnostics
+ Found 6979 diagnostics

sympy (https://github.com/sympy/sympy)
- error[invalid-assignment] sympy/interactive/printing.py:282:9: Implicit shadowing of function `_repr_latex_`
+ warning[unused-ignore-comment] sympy/testing/runtests.py:124:53: Unused blanket `type: ignore` directive

scipy (https://github.com/scipy/scipy)
- error[invalid-assignment] scipy/_lib/decorator.py:267:5: Implicit shadowing of function `__init__`
- error[invalid-assignment] scipy/_lib/decorator.py:273:5: Implicit shadowing of function `__init__`
- error[invalid-assignment] scipy/stats/_distribution_infrastructure.py:4296:9: Implicit shadowing of function `_moment_raw_formula`
- error[invalid-assignment] scipy/stats/_distribution_infrastructure.py:4297:9: Implicit shadowing of function `_moment_central_formula`
- error[invalid-assignment] scipy/stats/_distribution_infrastructure.py:4298:9: Implicit shadowing of function `_moment_standardized_formula`
- Found 7812 diagnostics
+ Found 7807 diagnostics

```
</details>


---

_Renamed from "Experiment: allow functions to be redefined (same signature)" to "[ty] Experiment: allow functions to be redefined (same signature)" by @sharkdp on 2025-06-23 06:48_

---
