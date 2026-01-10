```yaml
number: 18382
title: "[ty] Argument type expansion for overload call evaluation"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/argument-type-expansion
created_at: 2025-05-30T03:06:18Z
updated_at: 2025-06-04T16:07:37Z
url: https://github.com/astral-sh/ruff/pull/18382
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Argument type expansion for overload call evaluation

---

_Pull request opened by @dhruvmanila on 2025-05-30 03:06_

## Summary

Part of astral-sh/ty#104, closes: astral-sh/ty#468

This PR implements the argument type expansion which is step 3 of the overload call evaluation algorithm.

Specifically, this step needs to be taken if type checking resolves to no matching overload and there are argument types that can be expanded.

## Test Plan

Add new test cases.

## Ecosystem analysis

This PR removes 174 `no-matching-overload` false positives -- I looked at a lot of them and they all are false positives.

One thing that I'm not able to understand is that in https://github.com/sphinx-doc/sphinx/blob/2b7e3adf27c158305acca9b5e4d0d93d3e4c6f09/sphinx/ext/autodoc/preserve_defaults.py#L179 the inferred type of `value` is `str | None` by ty and Pyright, which is correct, but it's only ty that raises `invalid-argument-type` error while Pyright doesn't. The constructor method of `DefaultValue` has declared type of `str` which is invalid.

There are few cases of false positives resulting due to the fact that ty doesn't implement narrowing on attribute expressions.

---

_Label `ty` added by @dhruvmanila on 2025-05-30 03:06_

---

_Comment by @github-actions[bot] on 2025-05-30 03:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
- error[no-matching-overload] pytest_robotframework/__init__.py:569:9: No overload of function `keyword` matches arguments
- Found 228 diagnostics
+ Found 227 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[no-matching-overload] src/anyio/_core/_sockets.py:279:12: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/anyio/_core/_sockets.py:543:19: No overload of function `fspath` matches arguments
- Found 111 diagnostics
+ Found 109 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[no-matching-overload] ignite/utils.py:345:14: No overload of bound method `__init__` matches arguments
- Found 2254 diagnostics
+ Found 2253 diagnostics

rich (https://github.com/Textualize/rich)
- error[no-matching-overload] rich/progress.py:496:14: No overload of bound method `open` matches arguments
- warning[unused-ignore-comment] rich/progress.py:506:44: Unused blanket `type: ignore` directive
- Found 394 diagnostics
+ Found 392 diagnostics

trio (https://github.com/python-trio/trio)
- error[no-matching-overload] src/trio/_highlevel_open_unix_stream.py:63:28: No overload of function `fspath` matches arguments
- Found 1094 diagnostics
+ Found 1093 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[no-matching-overload] src/check_jsonschema/cli/param_types.py:135:26: No overload of function `fspath` matches arguments
- Found 66 diagnostics
+ Found 65 diagnostics

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- error[no-matching-overload] src/pywinctl/_pywinctl_linux.py:161:21: No overload of function `compile` matches arguments
- error[no-matching-overload] src/pywinctl/_pywinctl_linux.py:215:20: No overload of function `compile` matches arguments
- Found 38 diagnostics
+ Found 36 diagnostics

nox (https://github.com/wntrblm/nox)
- error[no-matching-overload] nox/_options.py:160:12: No overload of function `fspath` matches arguments
- error[no-matching-overload] nox/command.py:84:33: No overload of function `fspath` matches arguments
- error[no-matching-overload] nox/command.py:160:22: No overload of function `fspath` matches arguments
- error[no-matching-overload] nox/command.py:161:17: No overload of function `fspath` matches arguments
- Found 37 diagnostics
+ Found 33 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[no-matching-overload] src/schemathesis/core/rate_limit.py:15:13: No overload of function `urlparse` matches arguments
- error[no-matching-overload] src/schemathesis/specs/openapi/checks.py:564:14: No overload of function `urlparse` matches arguments
- Found 414 diagnostics
+ Found 412 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[no-matching-overload] tornado/auth.py:1221:12: No overload of function `quote` matches arguments
- error[no-matching-overload] tornado/concurrent.py:177:9: No overload of function `future_add_done_callback` matches arguments
- error[no-matching-overload] tornado/curl_httpclient.py:386:43: No overload of function `to_unicode` matches arguments
- error[no-matching-overload] tornado/curl_httpclient.py:485:38: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/escape.py:59:24: No overload of function `to_unicode` matches arguments
- error[no-matching-overload] tornado/escape.py:77:26: No overload of function `to_unicode` matches arguments
- error[no-matching-overload] tornado/escape.py:128:12: No overload of function `quote_plus` matches arguments
- error[no-matching-overload] tornado/escape.py:128:12: No overload of function `quote` matches arguments
- error[no-matching-overload] tornado/escape.py:166:21: No overload of function `to_unicode` matches arguments
- error[no-matching-overload] tornado/escape.py:170:24: No overload of function `to_unicode` matches arguments
- error[no-matching-overload] tornado/httpclient.py:571:22: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/httputil.py:1162:12: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/httputil.py:1162:36: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/routing.py:535:33: No overload of function `compile` matches arguments
- error[no-matching-overload] tornado/routing.py:570:26: No overload of function `compile` matches arguments
- error[no-matching-overload] tornado/template.py:317:40: No overload of function `to_unicode` matches arguments
- error[no-matching-overload] tornado/web.py:685:17: No overload of function `to_unicode` matches arguments
- error[invalid-argument-type] tornado/web.py:2293:44: Argument to bound method `__init__` is incorrect: Expected `Pattern[Unknown]`, found `Unknown | str | Pattern[Unknown]`
+ error[invalid-argument-type] tornado/web.py:2293:44: Argument to bound method `__init__` is incorrect: Expected `Pattern[Unknown]`, found `Unknown | Pattern[str] | str | Pattern[Unknown]`
- error[no-matching-overload] tornado/web.py:3561:30: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/web.py:3583:43: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/web.py:3654:13: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/web.py:3764:13: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/web.py:3777:21: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/web.py:3779:21: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/web.py:3784:21: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/websocket.py:470:16: No overload of function `utf8` matches arguments
- error[no-matching-overload] tornado/websocket.py:919:21: No overload of function `utf8` matches arguments
- Found 353 diagnostics
+ Found 327 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[no-matching-overload] cki_lib/yaml.py:91:22: No overload of function `dirname` matches arguments
- Found 170 diagnostics
+ Found 169 diagnostics

vision (https://github.com/pytorch/vision)
- error[no-matching-overload] torchvision/datasets/eurosat.py:37:21: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/eurosat.py:53:21: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/folder.py:63:17: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/imagenet.py:53:28: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/imagenet.py:53:28: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:103:12: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:148:12: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:165:12: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:193:12: No overload of function `expanduser` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:312:32: No overload of function `fspath` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:353:19: No overload of function `dirname` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:359:35: No overload of function `basename` matches arguments
- error[no-matching-overload] torchvision/datasets/utils.py:382:21: No overload of function `expanduser` matches arguments
- Found 1546 diagnostics
+ Found 1533 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[no-matching-overload] src/urllib3/connectionpool.py:1150:12: No overload of function `_normalize_host` matches arguments
- error[no-matching-overload] src/urllib3/util/url.py:438:16: No overload of function `_normalize_host` matches arguments
- error[no-matching-overload] test/test_ssltransport.py:56:24: No overload of function `sample_request` matches arguments
- Found 509 diagnostics
+ Found 506 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[no-matching-overload] scrapy/mail.py:143:13: No overload of bound method `set_payload` matches arguments
- error[no-matching-overload] scrapy/mail.py:143:13: No overload of bound method `set_payload` matches arguments
- Found 1334 diagnostics
+ Found 1332 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[no-matching-overload] mitmproxy/io/har.py:111:24: No overload of function `encode` matches arguments
- error[no-matching-overload] mitmproxy/net/http/url.py:54:40: No overload of function `urlparse` matches arguments
- warning[unused-ignore-comment] mitmproxy/net/http/url.py:60:71: Unused blanket `type: ignore` directive
+ error[invalid-assignment] mitmproxy/net/http/url.py:54:5: Object of type `ParseResult | ParseResultBytes` is not assignable to `ParseResult`
+ warning[possibly-unbound-attribute] mitmproxy/net/http/url.py:58:16: Attribute `encode` on type `str | None` is possibly unbound
- error[no-matching-overload] mitmproxy/tools/web/app.py:102:21: No overload of function `always_str` matches arguments
- error[no-matching-overload] mitmproxy/tools/web/app.py:119:21: No overload of function `always_str` matches arguments
- error[no-matching-overload] test/mitmproxy/net/test_encoding.py:60:24: No overload of function `decode` matches arguments
- Found 2111 diagnostics
+ Found 2107 diagnostics

mypy (https://github.com/python/mypy)
- error[no-matching-overload] mypy/checker.py:762:22: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:2240:25: No overload of function `get_proper_type` matches arguments
+ error[invalid-argument-type] mypy/checker.py:2309:28: Argument to function `is_equivalent` is incorrect: Expected `Type`, found `None | (ProperType & ~AnyType)`
+ error[invalid-argument-type] mypy/checker.py:2317:33: Argument to function `is_subtype` is incorrect: Expected `Type`, found `None | (ProperType & ~AnyType)`
- error[no-matching-overload] mypy/checker.py:3749:15: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:4532:28: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:5310:42: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:6339:48: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:7618:25: No overload of function `conditional_types` matches arguments
- error[no-matching-overload] mypy/checker.py:7992:23: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:8742:9: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checker.py:8752:11: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checkexpr.py:438:24: No overload of function `get_proper_type` matches arguments
+ error[invalid-return-type] mypy/checkexpr.py:444:28: Return type does not match returned value: expected `Type`, found `Unknown | LiteralType | None`
- error[no-matching-overload] mypy/checkexpr.py:1032:23: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checkexpr.py:1469:23: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checkexpr.py:2134:29: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checkexpr.py:5346:19: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checkexpr.py:6672:15: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/checkmember.py:1494:21: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/join.py:894:16: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/meet.py:529:19: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/messages.py:839:27: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/messages.py:1069:23: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/plugins/common.py:180:28: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/plugins/dataclasses.py:795:27: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/plugins/dataclasses.py:919:13: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/plugins/functools.py:125:35: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/plugins/singledispatch.py:102:17: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/plugins/singledispatch.py:131:22: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/solve.py:306:16: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/stats.py:358:13: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/stubtest.py:1149:27: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/subtypes.py:1540:15: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/typeops.py:217:27: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/typeops.py:287:22: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/typeops.py:396:25: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/typeops.py:458:32: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/typeops.py:474:21: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypy/typeops.py:993:9: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypyc/irbuild/classdef.py:343:20: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypyc/irbuild/classdef.py:641:20: No overload of function `get_proper_type` matches arguments
- error[no-matching-overload] mypyc/irbuild/expression.py:224:11: No overload of function `get_proper_type` matches arguments
- Found 3262 diagnostics
+ Found 3225 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[no-matching-overload] src/_pytest/_code/code.py:392:43: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/_pytest/config/argparsing.py:113:20: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/_pytest/config/argparsing.py:170:20: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/_pytest/pathlib.py:1004:17: No overload of function `abspath` matches arguments
- error[no-matching-overload] src/_pytest/pytester.py:990:18: No overload of function `abspath` matches arguments
- error[no-matching-overload] src/_pytest/pytester.py:1405:25: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/_pytest/reports.py:371:32: No overload of function `fspath` matches arguments
- error[no-matching-overload] testing/python/collect.py:1232:16: No overload of function `fspath` matches arguments
- error[no-matching-overload] testing/python/collect.py:1247:16: No overload of function `fspath` matches arguments
- error[no-matching-overload] testing/test_recwarn.py:568:9: No overload of function `warn` matches arguments
- Found 787 diagnostics
+ Found 777 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[no-matching-overload] src/werkzeug/_reloader.py:285:20: No overload of function `abspath` matches arguments
- error[no-matching-overload] src/werkzeug/datastructures/accept.py:86:16: No overload of function `__getitem__` matches arguments
- error[no-matching-overload] src/werkzeug/datastructures/file_storage.py:194:28: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/werkzeug/utils.py:426:20: No overload of function `abspath` matches arguments
- error[no-matching-overload] src/werkzeug/utils.py:564:26: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/werkzeug/utils.py:564:48: No overload of function `fspath` matches arguments
- Found 478 diagnostics
+ Found 472 diagnostics

altair (https://github.com/vega/altair)
- error[no-matching-overload] tests/__init__.py:159:10: No overload of function `compile` matches arguments
- Found 1297 diagnostics
+ Found 1296 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- error[no-matching-overload] discord/ext/commands/help.py:668:13: No overload of bound method `sort` matches arguments
+ warning[unused-ignore-comment] discord/ext/commands/help.py:648:75: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] discord/ext/commands/help.py:652:75: Unused blanket `type: ignore` directive
- Found 619 diagnostics
+ Found 620 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[no-matching-overload] static_frame/core/util.py:1363:21: No overload of bound method `__init__` matches arguments
- Found 1943 diagnostics
+ Found 1942 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[no-matching-overload] mesonbuild/mlog.py:571:54: No overload of function `basename` matches arguments
- Found 1334 diagnostics
+ Found 1333 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[no-matching-overload] src/bokeh/command/subcommands/serve.py:1004:11: No overload of function `format_docstring` matches arguments
- error[no-matching-overload] src/bokeh/embed/server.py:408:27: No overload of function `format_docstring` matches arguments
- error[no-matching-overload] src/bokeh/embed/server.py:409:26: No overload of function `format_docstring` matches arguments
- error[no-matching-overload] src/bokeh/server/tornado.py:810:24: No overload of function `format_docstring` matches arguments
- error[no-matching-overload] src/bokeh/util/serialization.py:88:11: No overload of function `format_docstring` matches arguments
- error[no-matching-overload] src/bokeh/util/serialization.py:319:35: No overload of function `format_docstring` matches arguments
- Found 946 diagnostics
+ Found 940 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[no-matching-overload] sphinx/builders/html/__init__.py:1139:26: No overload of function `fspath` matches arguments
- error[no-matching-overload] sphinx/builders/html/__init__.py:1167:31: No overload of function `fspath` matches arguments
- error[no-matching-overload] sphinx/builders/html/_assets.py:166:16: No overload of function `fspath` matches arguments
- error[call-non-callable] sphinx/config.py:205:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` is not callable on object of type `tuple[Unknown, Unknown, Unknown]`
- error[no-matching-overload] sphinx/ext/autodoc/preserve_defaults.py:178:33: No overload of function `unparse` matches arguments
+ error[invalid-argument-type] sphinx/ext/autodoc/preserve_defaults.py:179:72: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
- error[no-matching-overload] sphinx/ext/autodoc/type_comment.py:96:52: No overload of function `unparse` matches arguments
- error[no-matching-overload] sphinx/search/__init__.py:497:36: No overload of function `fspath` matches arguments
- error[no-matching-overload] sphinx/util/__init__.py:33:12: No overload of function `fspath` matches arguments
- error[no-matching-overload] sphinx/util/docstrings.py:10:22: No overload of function `compile` matches arguments
- error[no-matching-overload] sphinx/util/i18n.py:320:17: No overload of function `splitext` matches arguments
- error[no-matching-overload] sphinx/util/i18n.py:320:17: No overload of function `splitext` matches arguments
- error[no-matching-overload] sphinx/util/i18n.py:320:17: No overload of function `splitext` matches arguments
- error[no-matching-overload] sphinx/util/inspect.py:981:25: No overload of function `unparse` matches arguments
- error[no-matching-overload] sphinx/util/inspect.py:995:18: No overload of function `unparse` matches arguments
- error[call-non-callable] sphinx/util/inventory.py:319:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]` is not callable on object of type `tuple[Unknown, Unknown, Unknown, Unknown]`
- error[no-matching-overload] sphinx/util/osutil.py:37:12: No overload of function `fspath` matches arguments
- error[no-matching-overload] sphinx/util/rst.py:28:17: No overload of function `compile` matches arguments
- Found 662 diagnostics
+ Found 646 diagnostics

rotki (https://github.com/rotki/rotki)
+ warning[unused-ignore-comment] rotkehlchen/externalapis/etherscan.py:443:63: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] rotkehlchen/tests/db/test_history_events.py:268:46: Unused blanket `type: ignore` directive
- error[no-matching-overload] rotkehlchen/tests/unit/test_calendar_reminders.py:145:25: No overload of function `get_decoded_events_of_transaction` matches arguments
- Found 2043 diagnostics
+ Found 2044 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[no-matching-overload] tests/appsec/iast/aspects/aspect_utils.py:33:24: No overload of function `sub` matches arguments
- Found 6912 diagnostics
+ Found 6911 diagnostics

zulip (https://github.com/zulip/zulip)
- error[no-matching-overload] zerver/tests/test_slack_importer.py:101:26: No overload of function `urlsplit` matches arguments
- error[no-matching-overload] zerver/tornado/django_api.py:55:26: No overload of function `urlsplit` matches arguments
- error[no-matching-overload] zproject/config.py:59:12: No overload of function `get_config` matches arguments
- Found 6944 diagnostics
+ Found 6941 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[no-matching-overload] src/prefect/_internal/concurrency/calls.py:84:26: No overload of function `get_deadline` matches arguments
- error[no-matching-overload] src/prefect/_internal/schemas/validators.py:297:12: No overload of function `fspath` matches arguments
- error[no-matching-overload] src/prefect/client/schemas/actions.py:128:16: No overload of function `validate_schedule_max_scheduled_runs` matches arguments
- error[no-matching-overload] src/prefect/client/schemas/actions.py:196:16: No overload of function `validate_schedule_max_scheduled_runs` matches arguments
- error[no-matching-overload] src/prefect/client/schemas/objects.py:733:16: No overload of function `list_length_50_or_less` matches arguments
- error[no-matching-overload] src/prefect/client/schemas/objects.py:738:16: No overload of function `validate_not_negative` matches arguments
- error[no-matching-overload] src/prefect/client/schemas/objects.py:1594:16: No overload of function `validate_max_metadata_length` matches arguments
- error[no-matching-overload] src/prefect/client/schemas/schedules.py:139:16: No overload of function `default_timezone` matches arguments
- error[no-matching-overload] src/prefect/serializers.py:243:16: No overload of function `cast_type_names_to_serializers` matches arguments
- error[no-matching-overload] src/prefect/server/schemas/actions.py:521:16: No overload of function `validate_cache_key_length` matches arguments
- error[no-matching-overload] src/prefect/server/schemas/core.py:377:16: No overload of function `list_length_50_or_less` matches arguments
- error[no-matching-overload] src/prefect/server/schemas/core.py:382:16: No overload of function `validate_not_negative` matches arguments
- error[no-matching-overload] src/prefect/tasks.py:1067:16: No overload of function `run_task` matches arguments
- error[no-matching-overload] src/prefect/utilities/collections.py:401:20: No overload of function `visit_collection` matches arguments
- error[no-matching-overload] src/prefect/workers/base.py:140:16: No overload of function `return_v_or_none` matches arguments
- Found 4499 diagnostics
+ Found 4484 diagnostics

sympy (https://github.com/sympy/sympy)
- error[no-matching-overload] sympy/utilities/exceptions.py:271:13: No overload of function `warn_explicit` matches arguments
- Found 18573 diagnostics
+ Found 18572 diagnostics

scipy (https://github.com/scipy/scipy)
- error[no-matching-overload] scipy/_lib/_array_api.py:378:16: No overload of function `norm` matches arguments
- error[no-matching-overload] scipy/sparse/linalg/tests/test_norm.py:134:59: No overload of function `norm` matches arguments
- error[no-matching-overload] scipy/sparse/linalg/tests/test_norm.py:137:41: No overload of function `norm` matches arguments
- error[no-matching-overload] subprojects/highs/src/highspy/highs.py:1032:16: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] subprojects/highs/src/highspy/highs.py:1182:16: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] subprojects/highs/src/highspy/highs.py:1604:16: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] subprojects/highs/src/highspy/highs.py:1905:78: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] subprojects/highs/src/highspy/highs.py:1973:78: No overload of bound method `__init__` matches arguments
- error[no-matching-overload] subprojects/highs/src/highspy/highs.py:2065:16: No overload of bound method `__init__` matches arguments
- Found 7766 diagnostics
+ Found 7757 diagnostics

```
</details>


---

_Comment by @flying-sheep on 2025-05-30 12:54_

Maybe something like this would make a good test case:

```py
from typing import Self, Literal, Never, assert_type, overload
from ty_extensions import Not, Intersection

class MyTupleLike:
    __match_args__ = ("_0", "_1")
    _0: int
    _1: str

    def __new__(cls, _0: int, _1: str, /) -> Self:
        i = object.__new__(cls)
        i._0 = _0
        i._1 = _1
        return i
    def __len__(self) -> Literal[2]:
        return 2
    @overload
    def __getitem__(self, key: Literal[0]) -> int: ...
    @overload
    def __getitem__(self, key: Literal[1]) -> str: ...
    @overload
    def __getitem__(self, key: Intersection[int, Not[Literal[0, 1]]]) -> Never: ...
    
    def __getitem__(self, key: int) -> int | str:
        match key:
            case 0:
                return self._0
            case 1:
                return self._1
            case _:
                raise IndexError(key)


x = MyTupleLike(0, "")
a, b = x

assert_type(a, int)
assert_type(b, str)
```

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:135 on 2025-05-30 16:18_

This mainly an optimization to avoid cloning the `self.types` vector to initialize the `std::iter::successors` iterator. This would be useful in cases where no argument types can be expanded.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:225 on 2025-05-30 16:19_

I can avoid using `itertools` here and have a custom implementation for this but it would also require allocation so I thought that we might as well utilize an existing implementation.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1253 on 2025-05-30 16:20_

I need to spend some time thinking about this.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1173 on 2025-05-30 16:21_

There's a possibility that this can be merged into a single loop that would also consider the expanded argument lists but for now I decided to just duplicate this part as it isn't too complex.

---

_@dhruvmanila reviewed on 2025-05-30 16:22_

---

_Comment by @dhruvmanila on 2025-05-30 16:25_

I think there are few more cases to test and I also need to think a bit more around the bindings state at the end of a successful evaluation of an expanded argument lists but I'm marking this as ready for review to get some initial feedback, specifically on the structure of the implementation and if there's something obvious that I've missed or mis-interpreted based on my reading of the spec.

---

_Marked ready for review by @dhruvmanila on 2025-05-30 16:25_

---

_Review requested from @carljm by @dhruvmanila on 2025-05-30 16:25_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-30 16:25_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-30 16:25_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-30 16:25_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:262 on 2025-05-30 17:49_

I think it should work to compare the vec and array directly:

```suggestion
        assert_eq!(&expanded, &types);
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:272 on 2025-05-30 17:49_

Ditto here:

```suggestion
        assert_eq!(expanded, [
            Type::BooleanLiteral(true),
            Type::BooleanLiteral(false),
        ]);
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:316 on 2025-05-30 17:50_

ditto

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:338 on 2025-05-30 17:50_

ditto

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:127 on 2025-05-30 17:50_

I like this implementation!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1131 on 2025-05-30 17:52_

Since you're unwrapping, here, you can just index directly

```suggestion
                self.overloads[index].check_types(
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1172 on 2025-05-30 17:56_

If you make this peekable you won't have to chain down below

```suggestion
        let mut expansions = argument_types.expand(db).peekable();

        let Some(first_expansion) = expansions.peek() else {
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1180 on 2025-05-30 17:57_

```suggestion
        for expanded_argument_lists in expansions {
```

---

_@dcreager reviewed on 2025-05-30 18:04_

Stepping out to pick up my son from school, here are some initial comments 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1095 on 2025-05-30 19:37_

It doesn't look like to have to do any fancy merging of these snapshots, they just exist to mark a state to return back to. Could you achieve the same thing by just cloning `self`? There are some extra fields in `CallableBinding` and `Binding` that you'd end up copying, which your snapshot types don't include. But I don't think those fields are substantially larger than the parts that you're already copying in the snapshots, and it might simplify things quite a bit.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1181 on 2025-05-30 19:39_

Instead of building up a vec, and then only constructing and using the union if all expansions succeeded, you could build up in a `UnionBuilder` directly, and use a `bool` flag to tell you if one of the expansions fails and you should bail out.

---

_@dcreager reviewed on 2025-05-30 19:39_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:27 on 2025-05-30 20:35_

```suggestion
# These match a single overload
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:56 on 2025-05-30 20:36_

```suggestion
Here, all of the calls below pass the arity check for all overloads, so we proceed to type checking
which filters out all but the matching overload:
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:1 on 2025-05-30 20:49_

It looks like the implementation supports tuple expansion as well, and there's a Rust test for it. Any reason not to include an mdtest showing it in action?

---

_@carljm reviewed on 2025-05-30 21:02_

Just glanced quickly at the tests for semantics; looks like @dcreager already has it covered for detailed review of the code. This is fantastic, thank you!

---

_@dhruvmanila reviewed on 2025-06-02 04:39_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1253 on 2025-06-02 04:39_

I think this will require some kind of "merge" logic to make sure the bindings state is correct at the end of a successful argument lists evaluation.

Consider the following abstract example which contains 4 overloads:
```py
@overload
def f(...) -> A: ...
@overload
def f(...) -> B: ...
@overload
def f(...) -> C: ...
@overload
def f(...) -> D: ...
```

After expanding the first argument, we get two argument lists which matches overload 1 and 3 respectively. This means that we now have the winning overload matches (1 and 3), so the return type would be `A | C`.

The logic in this PR is such that it restores the state of every binding after evaluating a single argument list, so at the end of evaluating the two argument lists (as stated above), the state would be as if we didn't perform type checking at all. I think this is incorrect.

In `Binding`, there are 6 fields which we need to consider that gets updated during type checking which is what the `BindingSnapshot` is recording:
```rs
struct BindingSnapshot<'db> {
    return_ty: Type<'db>,
    specialization: Option<Specialization<'db>>,
    inherited_specialization: Option<Specialization<'db>>,
    argument_parameters: Box<[Option<usize>]>,
    parameter_tys: Box<[Option<Type<'db>>]>,
    errors_position: usize,
}
```
Here, it's the `errors` field that represents whether an overload was "matched" or not while other fields represent additional information that type checking revealed.

Coming back to the above abstract example:
1. For overload 1, the first argument list evaluated successfully
2. For overload 3, the second argument list evaluated successfully
3. For overload 2 and 4, none of the argument lists matched

For (1) and (2), the `errors` field for the `Binding` corresponding to the matched overload should be empty and other fields should have the information from the successful evaluation.

For (3), I'm not exactly sure what would be the correct approach. My thinking is that:
* Combine all the `errors` from evaluating both argument lists
* Choose the `return_ty` from the last argument list
* Combine (?) the `specialization` and `inherited_specialization`
* Choose the `argument_parameters` from the last argument list
* Choose the `parameter_tys` from the last argument list

Quickly looking at what Pyright does, it seems that it re-evaluates it using the first argument list: https://github.com/microsoft/pyright/blob/321b6bf687c6c3ffa3eb627aeb8a143bc4740cde/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L9340-L9370

---

_@dhruvmanila reviewed on 2025-06-02 04:42_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1181 on 2025-06-02 04:42_

Yeah, I could do that. I did this to avoid the cost of [adding the type to the union](https://github.com/astral-sh/ruff/blob/a44db261387ffa44e7849b4d25e2ef3eed2524cf/crates/ty_python_semantic/src/types/builder.rs#L215-L410) in the cases where this expansion doesn't evaluate successfully and we need to move to the next argument type to expand or exit the expansion process because there are no argument type remaining.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1095 on 2025-06-02 04:43_

We _might_ have to do some kind of "merge" logic to determine the final state of the bindings when all the argument lists after expanding one or more types evaluates successfully. Refer to https://github.com/astral-sh/ruff/pull/18382#discussion_r2120026485 comment.

---

_@dhruvmanila reviewed on 2025-06-02 04:43_

---

_@dhruvmanila reviewed on 2025-06-02 06:02_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1095 on 2025-06-02 06:02_

I also think that cloning `Signature` multiple times would be expensive.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:1 on 2025-06-02 10:35_

Added a test case for tuples.

---

_@dhruvmanila reviewed on 2025-06-02 10:35_

---

_@dhruvmanila reviewed on 2025-06-02 10:59_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1891 on 2025-06-02 10:59_

I _might've_ overcomplicated a bit here, happy to revert / simplify here.

One way to simplify would be to re-check the overloads by using the first argument list instead of the current implementation that merges the state from _all_ argument lists.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1139 on 2025-06-02 11:00_

This TODO is important because we currently give `no-matching-overload` which is incorrect because we _did_ match an overload, it's type checking that failed.

---

_@dhruvmanila reviewed on 2025-06-02 11:00_

---

_@dcreager reviewed on 2025-06-02 18:17_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1181 on 2025-06-02 18:17_

:+1: sounds good

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1095 on 2025-06-02 18:19_

> We _might_ have to do some kind of "merge" logic to determine the final state of the bindings when all the argument lists after expanding one or more types evaluates successfully

Make sense! We're not merging yet, but we might need to, so set the types up now to allow that :+1: 

---

_@dcreager reviewed on 2025-06-02 18:19_

---

_Comment by @dhruvmanila on 2025-06-03 04:33_

**Post review changes:**

1. Add more test cases to mdtest consisting of generics, todos that are present in code (expanding enums, diagnostic reporting)
2. Merged bindings state after a successful evaluation of all argument lists into a final state as per https://github.com/astral-sh/ruff/pull/18382#discussion_r2120026485
3. Went through the ecosystem changes which all looks good

I'd appreciate a final review especially with regards to merging the bindings state which I'm not sure if it's worth keeping as it doesn't really have any user facing effect unless I'm missing something.

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-03 04:33_

---

_Review requested from @carljm by @dhruvmanila on 2025-06-03 04:33_

---

_Converted to draft by @dhruvmanila on 2025-06-03 14:48_

---

_Comment by @dhruvmanila on 2025-06-03 14:52_

I've put this in draft because I need to make some small tweaks with respect to the final state of the bindings that I've realized after talking with Carl in our 1:1 now. I'll quickly make those changes and will ask for review. Apologies if anyone was already looking at this PR.

---

_@dhruvmanila reviewed on 2025-06-03 15:39_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1253 on 2025-06-03 15:39_

For (3), the updated merged state would be something like the following:

```
    initial     arglist 1    arglist 2    final
    --------    ---------    ---------    -----
1   unmatched   matched      unmatched    matched
2   unmatched   unmatched    matched      matched
3   unmatched   unmatched    unmatched    unmatched
```

The "initial" is the state of each overloads before performing argument type expansion i.e., all three overloads are unmatched.

After evaluating the first argument list, it resulted in overload 1 being matched. Similarly, after evaluating the second argument list, it resulted in overload 2 being matched.

So, the final state of the bindings would indicate the overload 1 and 2 are matched while overload 3 is unmatched. This means that information about parameter types and specializations for the matched overload should be from the argument list that resulted in the match.

And, the return type would be the union of the "matched" overloads. This is a successful evaluation because both argument list resulted in a single matched overload.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1891 on 2025-06-03 15:39_

Simplified this part, so should be good now.

---

_@dhruvmanila reviewed on 2025-06-03 15:39_

---

_Comment by @dhruvmanila on 2025-06-03 15:43_

Ok, this is ready for a final review. I've commented [here](https://github.com/astral-sh/ruff/pull/18382#discussion_r2124275665) to visualize what the final state of the bindings look like. Happy to answer any questions that you might have. (cc @dcreager @carljm)

---

_Marked ready for review by @dhruvmanila on 2025-06-03 15:43_

---

_Comment by @dcreager on 2025-06-03 16:19_

> Ok, this is ready for a final review

I'm heading into an interview but will try to take a look at this later today

---

_@carljm approved on 2025-06-03 22:37_

Nice!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:148 on 2025-06-04 00:54_

nit: `expanded` is a slice iterator, so I think you should be able to replace `std::iter::once` with `std::slice::from_ref` and not have to use `Either`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/arguments.rs`:222 on 2025-06-04 00:55_

Same here

---

_@dcreager approved on 2025-06-04 00:55_

one last nit otherwise this looks great!

---

_@dhruvmanila reviewed on 2025-06-04 01:55_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:148 on 2025-06-04 01:55_

I was planning to change this when I saw your comment suggesting the same on the other PR ;)

Thank you for the suggestion!

---

_@dhruvmanila reviewed on 2025-06-04 02:05_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:222 on 2025-06-04 02:05_

This actually doesn't seem to be possible because I'm using `into_iter` here, I can't use `expanded.iter()` because `expanded` is created in this closure.

---

_@dhruvmanila reviewed on 2025-06-04 02:06_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/arguments.rs`:222 on 2025-06-04 02:06_

I could do this which requires allocating a vector for the single element:
```rs
                    if let Some(expanded) = expand_type(db, element) {
                        expanded.into_iter()
                    } else {
                        vec![element].into_iter()
                    }
```

---

_Merged by @dhruvmanila on 2025-06-04 02:12_

---

_Closed by @dhruvmanila on 2025-06-04 02:12_

---

_Branch deleted on 2025-06-04 02:12_

---

_Comment by @flying-sheep on 2025-06-04 12:36_

seems like this didn’t suffice to make the test case above work:

https://play.ty.dev/093e1321-5e0f-4b1c-b46e-26745c902c1d

> Object of type `MyTupleLike` is not iterable (not-iterable) [Ln 34, Col 8]

maybe another part of overload evaluation is responsible here. Should I file an issue?

---

_Comment by @dhruvmanila on 2025-06-04 12:48_

I'll need to look at your example a bit closely to see what you're trying to do but you're correct that this step of the algorithm wouldn't have solved it. Apologies for not noticing it earlier as this step is performing _argument_ type expansion and in your example overload, the argument type is just `int`. For reference, other steps of the algorithm are being tracked as sub-issues over at https://github.com/astral-sh/ty/issues/104.

---

_Comment by @flying-sheep on 2025-06-04 13:09_

The issue in my example is that ty knows that a class instance is iterable if it has a ~~`__getattr__`~~ `__getitem__` method that can be called with a single positional argument of type `int`. The issue with my example is that this is the case, but `ty` doesn’t understand that the overloaded method satisfies that interface.

---

_Comment by @AlexWaygood on 2025-06-04 13:13_

> if it has a `__getattr__` method

(I think you mean `__getitem__` :-)

---

_Comment by @flying-sheep on 2025-06-04 14:30_

Here’s a (failing) mdtest case:

````markdown
### Filling niches

`overloaded.pyi`:

```pyi
from typing import Literal, overload
from ty_extensions import Not, Intersection

class A: ...
class B: ...

@overload
def f(x: Literal[0]) -> A: ...
@overload
def f(x: Intersection[int, Not[Literal[0]]]) -> B: ...
```

```py
from overloaded import A, B, f

def _(x: int):
    reveal_type(f(0))  # revealed: A
    reveal_type(f(1))  # revealed: B
    reveal_type(f(x))  # revealed: A | B
```
````

---

_Comment by @carljm on 2025-06-04 14:50_

@flying-sheep You're right that a type checker _could_ expand `int` to a union of literal integer types and support this. It's not currently part of the typing spec to do this, and no other type checker does it, but it could certainly be done. It's a bit trickier than the [currently specified expansions](https://typing.python.org/en/latest/spec/overload.html#argument-type-expansion) because it's an infinite expansion, but I suspect practically limiting it to some small integer types would work fine in practice. (Though sooner or later someone would come along and wonder why it works for small integers but not large ones... maybe it would be possible to actually inspect the overloaded function and see which literal int types appear to be present in the overload signatures?)

I think we have higher priority things we need to do at the moment, though. It seems fairly easy to work around this issue: is there any downside to just adding an explicit third overload `def f(x: int) -> A | B: ...`? That [seems to be enough](https://play.ty.dev/24c39a0b-679f-46d6-9b99-496976549ffe) to make it behave the way you want.

---

_Comment by @flying-sheep on 2025-06-04 15:04_

If I read steps 3 of the [“overload call evaluation” part of the spec](https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation) correctly, that behavior is wrong:

> If all argument lists evaluate successfully, _combine their respective return types by union_ to determine the final return type for the call, and stop.

So since `int` overlaps with `Literal[0]`, given your definition, when passing `0`, `f(0)` should evaluate to `A|B`.

Or am I wrong? PyRight complains when overlapping overloads like this exist

---

_Comment by @carljm on 2025-06-04 15:09_

> So since `int` overlaps with `Literal[0]`, given your definition, when passing `0`, `f(0)` should evaluate to `A|B`.

That's not how overload evaluation is specified to work in Python. The first matching overload is used, not the union of the return types of all matching overloads.

(I personally think it would be better to treat overloads as an un-ordered intersection of signatures, and union the return types of all matched overloads, but we actually tried this in ty and the results from the ecosystem made it clear that it would be too incompatible with existing usage. Which isn't too surprising, given that the existing ecosystem doesn't have negation types available, and you really need negation types to make the "un-ordered intersection of signatures" interpretation usable. The ordered behavior effectively gives you implicit negation by having a more specific overload shadow a more general one.)

---

_Comment by @flying-sheep on 2025-06-04 15:20_

OK, is there some explicit mention of that in the spec? The part I’m quoting seems to contradict that (at least in the presence of expandable unions), while Step 6 agrees with you.

So PyRight (and maybe mypy) are just buggy?

PS: my original motivation is “simply” being able to type unpacking a heterogeneous collection: https://play.ty.dev/9817830f-c7c1-4a44-b4be-91704fe9500d

adding that overload will of course make that fail

---

_Comment by @carljm on 2025-06-04 15:29_

You need to consider the whole algorithm as presented in the spec, for any given case. Note that union expansion doesn't happen at all, unless zero overloads match after step 2. 

> If two or more candidate overloads remain, proceed to step 4.

In the case of passing `0` with the three-overload version here, two overloads match after step 2, which means we go directly to step 4, bypassing step 3. We end up reaching step 6 and choosing the first matching overload. The text you quoted about merging return types _only_ applies when trying multiple different union expansions.

> So PyRight (and maybe mypy) are just buggy?

I'm not sure which behavior you are referring to as buggy here.

> adding that overload will of course make that fail

I don't think so? It works fine for direct indexing: https://play.ty.dev/01df94d6-0b7e-4df9-9d85-f7d4dbc92224

It doesn't work for iteration/unpacking because we simply model a call to `__getitem__` with an argument of type `int` in order to determine the type returned by iterating using the legacy iteration protocol, we don't model it as calling with a sequence of literal integer types. This is a potential improvement we could make in the future, at least in the unpacking scenario.

---

_Comment by @flying-sheep on 2025-06-04 15:36_

Thanks for walking me through the algorithm, I wasn’t sure if I missed something!

> It works fine for direct indexing

yeah, which is why I specified that what I care about here is the unpacking lol

> I'm not sure which behavior you are referring to as buggy here.

See https://microsoft.github.io/pyright/#/configuration?id=type-check-rule-overrides

> `reportOverlappingOverload` [boolean or string, optional]: Generate or suppress diagnostics for function overloads that overlap in signature and obscure each other or have incompatible return types. **The default value for this setting is `"error"`.**

You’re saying that the spec says that overlapping overloads are an intentional part of the spec. PyRight flags them as error unless you ignore that. Should I report a bug?

---

_Comment by @carljm on 2025-06-04 15:42_

> You’re saying that the spec says that overlapping overloads are an intentional part of the spec. PyRight flags them as error unless you ignore that. Should I report a bug?

No; the second part "have incompatible return types" is critical there. The spec says that overlapping overloads are fine, as long as the later overload has a return type that is a super-type of the earlier overload. (This is what's necessary to make the overlapping overloads sound in the face of uncertain precision of the argument type.) That's the same logic that pyright's warning implements.

---

_Comment by @flying-sheep on 2025-06-04 16:01_

OK, so we’re at square 1:

Currently, the only way to have a chance at correctly typing the unpacking of a heterogeneous collection is to

1. do *exactly* what I’m doing in https://play.ty.dev/9817830f-c7c1-4a44-b4be-91704fe9500d (i.e. *do not* add the `int` overload)
3. add support for that to type checkers

right?

---

_Comment by @carljm on 2025-06-04 16:03_

I don't understand why (1) is required, or how the extra overload causes a problem. (It meets the requirement I mentioned that its return type is a super type of the prior overlapping overload return types.) My link above adds the extra `int` overload, and direct indexing works as expected. So it seems to me that all that is needed is (2) for us to model the legacy iteration protocol as a sequence of calls with literal ints, rather than as a single call with `int` argument.

---

_Comment by @flying-sheep on 2025-06-04 16:07_

Yeah, you’re right! That instead of “it’s an iterable over `T`, so unpacking yields a bunch of `T`s”.

---
