```yaml
number: 19234
title: "[ty] Deploy ecosystem diff to Cloudflare pages"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/deploy-ecosystem-diff-to-cloudflare
created_at: 2025-07-09T12:51:45Z
updated_at: 2025-07-10T13:58:55Z
url: https://github.com/astral-sh/ruff/pull/19234
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Deploy ecosystem diff to Cloudflare pages

---

_@sharkdp_

## Summary

Changes the ecosystem-analyzer workflow to deploy the diff to Cloudflare pages and post a link in the PR. Also adds a summary statistics to that PR comment.

## Test Plan

The comment below: https://github.com/astral-sh/ruff/pull/19234#issuecomment-3053205937. I previously had some dummy changes on this PR to see a non-zero diff. And I didn't reapply the label after I reverted that change, such that it's still visible for reviewers.


---

_Label `ci` added by @sharkdp on 2025-07-09 12:51_

---

_Label `ty` added by @sharkdp on 2025-07-09 12:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-09 12:52_

---

_Comment by @github-actions[bot] on 2025-07-09 12:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ warning[unused-ignore-comment] tests/fixtures/test_python/test_keywordify_context_manager.py:9:20: Unused `ty: ignore` directive: 'division-by-zero'
+ warning[unused-ignore-comment] tests/fixtures/test_python/test_keywordify_keyword_inside_context_manager.py:17:20: Unused `ty: ignore` directive: 'division-by-zero'
- Found 176 diagnostics
+ Found 178 diagnostics

attrs (https://github.com/python-attrs/attrs)
- warning[division-by-zero] tests/test_make.py:1481:71: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_validators.py:263:5: Cannot divide object of type `Literal[0]` by zero
- Found 640 diagnostics
+ Found 638 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- warning[division-by-zero] backend/services/user_updater_helpers.py:143:27: Cannot divide object of type `float` by zero
- Found 66 diagnostics
+ Found 65 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- warning[division-by-zero] dulwich/contrib/diffstat.py:160:27: Cannot divide object of type `float` by zero
- warning[division-by-zero] dulwich/contrib/diffstat.py:161:27: Cannot divide object of type `float` by zero
- Found 155 diagnostics
+ Found 153 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- warning[division-by-zero] tests/pool/test_pool.py:127:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/pool/test_pool.py:1030:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/pool/test_pool_async.py:128:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/pool/test_pool_async.py:1032:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/pool/test_sched.py:82:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/pool/test_sched_async.py:81:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_adapt.py:467:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_adapt.py:482:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_concurrency.py:491:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_concurrency_async.py:379:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection.py:241:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection.py:268:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection.py:286:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection.py:912:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection_async.py:236:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection_async.py:264:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection_async.py:282:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_connection_async.py:915:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_copy.py:384:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_copy.py:394:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_copy_async.py:396:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_copy_async.py:408:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common.py:590:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common.py:599:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common.py:773:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common.py:785:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common_async.py:594:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common_async.py:602:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common_async.py:781:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_cursor_common_async.py:793:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pipeline.py:78:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pipeline.py:89:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pipeline.py:478:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pipeline_async.py:75:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pipeline_async.py:86:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pipeline_async.py:477:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_transaction.py:103:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_transaction.py:122:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_transaction.py:636:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_transaction_async.py:103:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_transaction_async.py:122:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_transaction_async.py:643:13: Cannot divide object of type `Literal[1]` by zero
- Found 669 diagnostics
+ Found 627 diagnostics

trio (https://github.com/python-trio/trio)
- warning[division-by-zero] src/trio/_core/_run.py:161:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] src/trio/_core/_tests/test_guest_mode.py:434:29: Cannot divide object of type `Literal[1]` by zero
- Found 798 diagnostics
+ Found 796 diagnostics

rich (https://github.com/Textualize/rich)
- warning[division-by-zero] examples/suppress.py:17:5: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] rich/logging.py:289:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] rich/pretty.py:959:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_getfileno.py:22:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_logging.py:51:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_logging.py:82:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_logging.py:114:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pretty.py:287:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_pretty.py:298:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:21:16: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:65:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:96:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:117:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:144:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:171:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:227:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:239:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:288:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:319:16: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:338:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:351:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_traceback.py:369:9: Cannot divide object of type `Literal[1]` by zero
- Found 325 diagnostics
+ Found 303 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- warning[division-by-zero] sockeye/scoring.py:190:48: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] sockeye/utils.py:592:32: Cannot divide object of type `Literal[0]` by zero
- warning[division-by-zero] sockeye/utils.py:593:31: Cannot divide object of type `Literal[0]` by zero
- warning[division-by-zero] sockeye/utils.py:594:30: Cannot divide object of type `Literal[0]` by zero
- Found 321 diagnostics
+ Found 317 diagnostics

optuna (https://github.com/optuna/optuna)
- warning[division-by-zero] tests/study_tests/test_study.py:1029:19: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/study_tests/test_study.py:1042:34: Cannot divide object of type `Literal[1]` by zero
- Found 581 diagnostics
+ Found 579 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- warning[division-by-zero] tests/unittests/test_all_stages.py:202:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_gpg.py:188:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_gpg.py:206:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_url_helper.py:518:31: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_url_helper.py:527:31: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_url_helper.py:585:32: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_url_helper.py:593:32: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_util.py:3314:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/unittests/test_util.py:3333:17: Cannot divide object of type `Literal[1]` by zero
- Found 667 diagnostics
+ Found 658 diagnostics

asynq (https://github.com/quora/asynq)
- warning[division-by-zero] asynq/tests/test_recursion.py:54:29: Cannot divide object of type `int` by zero
- warning[division-by-zero] asynq/tests/test_recursion.py:54:29: Cannot divide object of type `float` by zero
- warning[division-by-zero] asynq/tests/test_recursion.py:61:29: Cannot divide object of type `int` by zero
- warning[division-by-zero] asynq/tests/test_recursion.py:61:29: Cannot divide object of type `float` by zero
- Found 188 diagnostics
+ Found 184 diagnostics

jinja (https://github.com/pallets/jinja)
- warning[division-by-zero] tests/test_debug.py:32:38: Cannot divide object of type `Literal[1]` by zero
- Found 197 diagnostics
+ Found 196 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- warning[division-by-zero] mkdocs/tests/livereload_tests.py:448:49: Cannot divide object of type `Literal[0]` by zero
- Found 190 diagnostics
+ Found 189 diagnostics

vision (https://github.com/pytorch/vision)
- warning[division-by-zero] test/test_ops.py:2034:24: Cannot divide object of type `Literal[0]` by zero
- Found 1475 diagnostics
+ Found 1474 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- warning[division-by-zero] tornado/test/gen_test.py:63:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/gen_test.py:71:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/gen_test.py:457:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/gen_test.py:472:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/gen_test.py:485:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/gen_test.py:505:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:347:43: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:358:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:371:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:380:43: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:386:45: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:588:43: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/ioloop_test.py:602:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/locks_test.py:373:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/tcpserver_test.py:27:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/twisted_test.py:63:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/web_test.py:1041:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/web_test.py:1048:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/web_test.py:1059:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/web_test.py:1936:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/web_test.py:1979:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/web_test.py:1982:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tornado/test/websocket_test.py:78:9: Cannot divide object of type `Literal[1]` by zero
- Found 244 diagnostics
+ Found 221 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- warning[division-by-zero] freqtrade/data/metrics.py:314:33: Cannot divide object of type `Literal[0]` by zero
- Found 399 diagnostics
+ Found 398 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- warning[division-by-zero] tests/pipelines.py:8:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/pipelines.py:16:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/spiders.py:332:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/spiders.py:339:17: Cannot divide object of type `Literal[2]` by zero
- warning[division-by-zero] tests/test_pipeline_crawl.py:204:24: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_request_attribute_binding.py:20:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_spidermiddleware.py:108:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_spidermiddleware.py:563:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_spidermiddleware_output_chain.py:146:39: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_utils_defer.py:123:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_utils_defer.py:150:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_utils_log.py:33:13: Cannot divide object of type `Literal[0]` by zero
- warning[division-by-zero] tests/test_utils_python.py:63:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/test_utils_signal.py:53:9: Cannot divide object of type `Literal[1]` by zero
- Found 1099 diagnostics
+ Found 1085 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- warning[division-by-zero] testing/code/test_excinfo.py:427:13: Cannot floor divide object of type `Literal[0]` by zero
- warning[division-by-zero] testing/code/test_excinfo.py:628:21: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] testing/code/test_excinfo.py:682:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] testing/python/raises.py:323:24: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] testing/test_runner.py:549:45: Cannot divide object of type `Literal[0]` by zero
- Found 525 diagnostics
+ Found 520 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- warning[division-by-zero] lib/Crypto/Util/number.py:469:27: Cannot floor divide object of type `int` by zero
- Found 1476 diagnostics
+ Found 1475 diagnostics

altair (https://github.com/vega/altair)
- warning[division-by-zero] tests/vegalite/v5/test_api.py:1784:71: Cannot divide object of type `Literal[1]` by zero
- Found 1277 diagnostics
+ Found 1276 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- warning[division-by-zero] openlibrary/utils/tests/test_retry.py:39:31: Cannot divide object of type `Literal[1]` by zero
- Found 698 diagnostics
+ Found 697 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[division-by-zero] benchmarks/bm/iast_utils/aspects_benchmarks_simple_report.py:33:28: Cannot divide object of type `float` by zero
- warning[division-by-zero] benchmarks/bm/iast_utils/aspects_benchmarks_simple_report.py:33:28: Cannot divide object of type `int` by zero
- warning[division-by-zero] ddtrace/internal/ci_visibility/api/_session.py:153:32: Cannot divide object of type `Literal[0]` by zero
- warning[division-by-zero] ddtrace/internal/ci_visibility/api/_session.py:153:32: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/contrib/aiohttp/app/web.py:72:12: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/contrib/cherrypy/web.py:60:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/contrib/falcon/app/resources.py:17:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/contrib/molten/test_molten.py:37:12: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/contrib/pyramid/app/web.py:27:9: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/contrib/sanic/run_server.py:34:5: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/tracer/test_span.py:195:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/tracer/test_span.py:242:13: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/tracer/test_span.py:274:17: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] tests/tracer/test_tracer.py:1227:17: Cannot divide object of type `Literal[1]` by zero
- Found 6853 diagnostics
+ Found 6839 diagnostics

scipy (https://github.com/scipy/scipy)
- warning[division-by-zero] scipy/integrate/tests/test_integrate.py:761:16: Cannot divide object of type `float` by zero
- warning[division-by-zero] scipy/integrate/tests/test_integrate.py:767:16: Cannot divide object of type `float` by zero
- warning[division-by-zero] scipy/integrate/tests/test_integrate.py:776:18: Cannot divide object of type `float` by zero
- warning[division-by-zero] scipy/signal/tests/test_filter_design.py:649:59: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] scipy/signal/tests/test_filter_design.py:826:64: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] scipy/sparse/linalg/_isolve/lsmr.py:256:14: Cannot divide object of type `Literal[1]` by zero
- warning[division-by-zero] scipy/sparse/linalg/_isolve/lsqr.py:393:14: Cannot divide object of type `Literal[1]` by zero
- Found 6620 diagnostics
+ Found 6613 diagnostics

sympy (https://github.com/sympy/sympy)
- warning[division-by-zero] sympy/assumptions/tests/test_context.py:27:13: Cannot divide object of type `Literal[1]` by zero
- Found 13300 diagnostics
+ Found 13299 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-09 13:05_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-09 13:05_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-09 15:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-09 15:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-09 16:02_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-09 16:02_

---

_Comment by @github-actions[bot] on 2025-07-09 16:03_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No changes detected ✅
**[Full report with detailed diff](https://david-deploy-ecosystem-diff.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-09 16:15_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-09 16:16_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-09 17:02_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-09 17:02_

---

_Renamed from "[ty] Deploy ecosystem diff to CF pages" to "[ty] Deploy ecosystem diff to Cloudflare pages" by @sharkdp on 2025-07-09 17:13_

---

_@sharkdp reviewed on 2025-07-09 17:23_

---

_Review comment by @sharkdp on `.github/zizmor.yml`:13 on 2025-07-09 17:23_

To be honest, I didn't fully understand what kind of problem this `cache-poisoning` lint tries to prevent. It felt like it wouldn't be relevant here(?), and then I saw that `publish-playground.yml` was also excluded here. Maybe I should ask our newest team member at some point :smile: 

---

_Marked ready for review by @sharkdp on 2025-07-09 17:24_

---

_Comment by @sharkdp on 2025-07-09 17:26_

> `unused-ignore-comment` 	4 	2 	0
> `invalid-argument-type` 	1 	1 	0

There's a race condition in ecosystem-analyzer where we get spurious changes if a project changes between computing the old diagnostics and the new ones. This should obviously be fixed (and would also be much faster), but has nothing to do with this PR.

---

_@AlexWaygood approved on 2025-07-09 17:59_

This is fantastic!

---

_@zanieb approved on 2025-07-09 20:29_

Doesn't this mean that the ecosystem check won't run on user-contributed pull requests because they won't have access to secrets? What's your plan for that?

---

_Comment by @sharkdp on 2025-07-09 20:54_

> Doesn't this mean that the ecosystem check won't run on user-contributed pull requests because they won't have access to secrets?

Oh. I was not aware of that limitation :-| That means I also don't have a plan on how to address this. Would it still work if the workflow is manually triggered by someone with the necessary permissions? I'll do some research tomorrow.

---

_Comment by @zanieb on 2025-07-09 21:03_

> Would it still work if the workflow is manually triggered by someone with the necessary permissions? I'll do some research tomorrow.

You can use the `pull_request_target` trigger, which would then run the workflow from the `main` branch on the pull request, but increases the danger of attacks. Dispatching the workflow manually seems reasonable as well. I'm not sure what the best way to do it is. @woodruffw might have suggestions.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-10 06:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 06:51_

---

_Comment by @sharkdp on 2025-07-10 06:53_

Merging this for now. It should still work on contributor PRs, but will skip deployment to Cloudflare pages. I added an upload step so that `diff.html` can still be downloaded from artifacts. As far as I can tell, the PR comment with the summary should still work, it will just not include a direct link.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-10 06:54_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-10 06:54_

---

_Merged by @sharkdp on 2025-07-10 07:03_

---

_Closed by @sharkdp on 2025-07-10 07:03_

---

_Branch deleted on 2025-07-10 07:03_

---

_@woodruffw reviewed on 2025-07-10 13:58_

---

_Review comment by @woodruffw on `.github/zizmor.yml`:13 on 2025-07-10 13:58_

`cache-poisoning` mostly tries to prevent dataflows where two conditions hold:

1. The workflow has a "publish" trigger (like `release`) **OR** uses a well-known "publishing" action (like `cloudflare/wrangler-action`)
2. The workflow contains a separate "cache-aware" action that has caching enabled, e.g. `actions/setup-python`.

The basic theory behind it is that (1) identifies publishing behavior, and that publishing behavior should generally be hermetic and not subject to a potentially poisoned cache entry. 

OTOH in practice this can be very noisy, as is seen here 😅 -- not all publishing behaviors actually _need_ to be hermetic. But it's hard to assert that in a blanket fashion, so for now `zizmor` mostly leaves it up to users to manually ignore a cache-poisoning finding that isn't actually relevant to them.

(More generally, none of this would be an issue if GitHub made poisoning the actions cache harder + eliminated trivial pivoting between workflow runs via the cache.)

---
