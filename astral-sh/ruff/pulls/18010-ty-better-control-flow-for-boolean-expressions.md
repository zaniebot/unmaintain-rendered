```yaml
number: 18010
title: "[ty] Better control flow for boolean expressions that are inside if"
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/better-if-bool-short-circuiting
created_at: 2025-05-10T20:54:10Z
updated_at: 2025-05-16T13:43:06Z
url: https://github.com/astral-sh/ruff/pull/18010
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Better control flow for boolean expressions that are inside if

---

_Pull request opened by @TomerBin on 2025-05-10 20:54_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
With this PR we now detect that x is always defined in `use`:
```py
if flag and (x := number):
    use(x)
```

When outside if, it's still detected as possibly not defined
```py
flag and (x := number)
# error: [possibly-unresolved-reference]
use(x)
```
In order to achieve that, I had to find a way to get access to the flow-snapshots of the boolean expression when analyzing the flow of the if statement. I did it by special casing the visitor of boolean expression to return flow control information, exporting two snapshots - `might_short_circuited` and `never_short_circuited`. When indexing boolean expression itself we must assume all possible flows, but when it's inside if statement, we can be smarter than that.

This PR is still kind of draft, but I'd like to get your overall opinion so opening it for review :)
Hope you'll get time to review this before the alpha release :)

**Best reviewed by commits**


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Fixed existing and added new mdtests.
I went through some of mypy primer results and they look fine


---

_Renamed from "[ty] Better control flow for boolean expression that are inside if" to "[ty] Better control flow for boolean expressions that are inside if" by @TomerBin on 2025-05-10 20:54_

---

_Comment by @github-actions[bot] on 2025-05-10 20:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- warning[possibly-unresolved-reference] bidict/_orderedbidict.py:157:24: Name `arg` used when possibly not defined
- Found 15 diagnostics
+ Found 14 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- warning[possibly-unresolved-reference] koda_validate/dataclasses.py:213:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dataclasses.py:265:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dataclasses.py:269:39: Name `result_async` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dictionary.py:917:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dictionary.py:950:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dictionary.py:954:39: Name `async_result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dictionary.py:1080:35: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dictionary.py:1111:35: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/dictionary.py:1115:35: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/namedtuple.py:206:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/namedtuple.py:257:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/namedtuple.py:261:39: Name `result_async` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/typeddict.py:199:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/typeddict.py:241:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] koda_validate/typeddict.py:245:39: Name `result_async` used when possibly not defined
- Found 50 diagnostics
+ Found 35 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- warning[possibly-unresolved-reference] src/async_utils/_qs.py:64:17: Name `unset` used when possibly not defined
- warning[possibly-unresolved-reference] src/async_utils/_qs.py:163:17: Name `unset` used when possibly not defined
- Found 16 diagnostics
+ Found 14 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:58:28: Name `rules` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:70:29: Name `metas` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:82:21: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:90:21: Name `a` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:90:31: Name `b` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:98:21: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:98:47: Name `string` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:107:29: Name `rules` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:128:55: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:128:67: Name `more_alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:128:89: Name `opt` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:139:51: Name `more_alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:139:67: Name `opt` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:148:51: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:148:62: Name `opt` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:157:34: Name `annotation` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:178:32: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:195:24: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:195:36: Name `more_alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:198:24: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:207:81: Name `action` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:213:38: Name `action` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:225:35: Name `items` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:244:43: Name `item` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:244:49: Name `annotation` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:255:43: Name `item` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:276:27: Name `atom` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:288:38: Name `atom` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:294:38: Name `atom` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:309:24: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:328:32: Name `node` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:341:26: Name `alts` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:364:20: Name `target_atoms` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:381:20: Name `target_atoms` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:392:40: Name `target_atoms` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:410:27: Name `atoms` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:421:27: Name `atoms` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:438:39: Name `m` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:438:44: Name `r` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_parser.py:451:20: Name `op` used when possibly not defined
- Found 56 diagnostics
+ Found 16 diagnostics

anyio (https://github.com/agronholm/anyio)
- warning[possibly-unresolved-reference] src/anyio/_backends/_asyncio.py:865:39: Name `closure` used when possibly not defined
- Found 119 diagnostics
+ Found 118 diagnostics

trio (https://github.com/python-trio/trio)
- warning[possibly-unresolved-reference] src/trio/testing/_raises_group.py:249:91: Name `stringified_exception` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/testing/_raises_group.py:250:42: Name `stringified_exception` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/testing/_raises_group.py:720:88: Name `expected` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/testing/_raises_group.py:720:136: Name `expected` used when possibly not defined
- Found 1096 diagnostics
+ Found 1092 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- warning[possibly-unresolved-reference] pydantic/_internal/_generate_schema.py:917:105: Name `validators` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/_internal/_generate_schema.py:2652:20: Name `inlining_behavior` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/_internal/_generate_schema.py:2659:22: Name `inlining_behavior` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/config.py:1192:41: Name `kwargs_conf` used when possibly not defined
+ warning[unused-ignore-comment] pydantic/json_schema.py:1127:50: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pydantic/json_schema.py:1055:40: Name `input_schema` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/json_schema.py:1080:40: Name `input_schema` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/json_schema.py:1096:40: Name `input_schema` used when possibly not defined
- warning[possibly-unresolved-reference] pydantic/json_schema.py:1525:51: Name `root_description` used when possibly not defined
- Found 772 diagnostics
+ Found 765 diagnostics

poetry (https://github.com/python-poetry/poetry)
- warning[possibly-unresolved-reference] src/poetry/console/application.py:273:41: Name `message` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/installation/chooser.py:204:32: Name `hash_name` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/installation/chooser.py:204:56: Name `hash_name` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/puzzle/transaction.py:153:36: Name `installed_package` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/puzzle/transaction.py:154:24: Name `installed_package` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/puzzle/transaction.py:155:53: Name `installed_package` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/repositories/http_repository.py:179:54: Name `hash_name` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/repositories/http_repository.py:182:62: Name `hash_name` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/repositories/http_repository.py:185:69: Name `hash_name` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/repositories/http_repository.py:360:32: Name `hash_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/repositories/http_repository.py:360:56: Name `hash_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/utils/env/env_manager.py:190:58: Name `minor` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/utils/env/python/manager.py:277:27: Name `active_python` used when possibly not defined
- warning[possibly-unresolved-reference] src/poetry/utils/env/python/manager.py:279:20: Name `active_python` used when possibly not defined
- Found 1027 diagnostics
+ Found 1013 diagnostics

mkosi (https://github.com/systemd/mkosi)
- warning[possibly-unresolved-reference] mkosi/__init__.py:434:39: Name `level` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:437:35: Name `version` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:2043:24: Name `root` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:3265:29: Name `uid` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:3468:77: Name `roothash` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:4327:37: Name `path` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:4328:38: Name `path` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:4345:36: Name `stat` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/__init__.py:4345:49: Name `stat` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:997:35: Name `r` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:997:35: Name `r` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:997:35: Name `r` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:5011:20: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:5011:25: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:5012:38: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:5012:56: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:5012:80: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:5194:25: Name `imagedir` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/distributions/fedora.py:39:42: Name `rhs` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/installer/__init__.py:75:37: Name `mirror` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/installer/__init__.py:75:45: Name `mirror` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/qemu.py:1418:64: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/qemu.py:1451:36: Name `initrd` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/qemu.py:1603:49: Name `status` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:542:43: Name `factory` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:542:52: Name `factory` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/vmspawn.py:91:38: Name `p` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/vmspawn.py:99:41: Name `initrd` used when possibly not defined
- Found 316 diagnostics
+ Found 288 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- warning[possibly-unresolved-reference] pwndbg/aglib/disasm/arch.py:855:93: Name `r_value` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/aglib/disasm/arch.py:855:93: Name `r_value` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/aglib/disasm/arch.py:855:93: Name `r_value` used when possibly not defined
- error[invalid-assignment] pwndbg/aglib/nearpc.py:117:9: Object of type `None` is not assignable to `int`
- Found 2219 diagnostics
+ Found 2215 diagnostics

vision (https://github.com/pytorch/vision)
- warning[possibly-unresolved-reference] setup.py:106:47: Name `version_pin_lt` used when possibly not defined
- Found 2049 diagnostics
+ Found 2048 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- warning[possibly-unresolved-reference] mkdocs/utils/rendering.py:85:16: Name `alt` used when possibly not defined
- Found 346 diagnostics
+ Found 345 diagnostics

yarl (https://github.com/aio-libs/yarl)
- warning[possibly-unresolved-reference] yarl/_url.py:1028:19: Name `old_segments` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1028:40: Name `old_segments` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1028:68: Name `old_segments` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:33: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:33: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:33: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:33: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:50: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:50: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:50: Name `invalid` used when possibly not defined
- warning[possibly-unresolved-reference] yarl/_url.py:1529:50: Name `invalid` used when possibly not defined
- Found 163 diagnostics
+ Found 152 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[possibly-unresolved-reference] mitmproxy/flowfilter.py:307:35: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/flowfilter.py:313:35: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/flowfilter.py:343:35: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/flowfilter.py:370:35: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/proxy/server.py:159:56: Name `e` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/proxy/server.py:161:36: Name `e` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/proxy/server.py:161:40: Name `e` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/proxy/server.py:161:43: Name `e` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/utils/asyncio_utils.py:47:38: Name `test` used when possibly not defined
- Found 2161 diagnostics
+ Found 2152 diagnostics

altair (https://github.com/vega/altair)
- warning[possibly-unresolved-reference] altair/utils/core.py:712:27: Name `categories` used when possibly not defined
- Found 1350 diagnostics
+ Found 1349 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- warning[possibly-unresolved-reference] cki_lib/yaml.py:128:32: Name `schema` used when possibly not defined
- warning[possibly-unresolved-reference] cki_lib/yaml.py:128:32: Name `schema` used when possibly not defined
- warning[possibly-unresolved-reference] cki_lib/yaml.py:128:32: Name `schema` used when possibly not defined
- Found 183 diagnostics
+ Found 180 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- warning[possibly-unresolved-reference] psycopg/psycopg/rows.py:136:71: Name `nfields` used when possibly not defined
- warning[possibly-unresolved-reference] psycopg/psycopg/rows.py:215:12: Name `nfields` used when possibly not defined
+ warning[unused-ignore-comment] psycopg/psycopg/rows.py:241:63: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] psycopg/psycopg/types/datetime.py:673:16: Name `ds` used when possibly not defined
- warning[possibly-unresolved-reference] psycopg/psycopg/types/datetime.py:680:16: Name `ints` used when possibly not defined
- Found 1199 diagnostics
+ Found 1196 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- warning[possibly-unresolved-reference] scrapy/core/engine.py:410:44: Name `d` used when possibly not defined
- Found 1420 diagnostics
+ Found 1419 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unresolved-reference] src/_pytest/_code/code.py:667:23: Name `subexc` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:518:46: Name `stringified_exception` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:527:24: Name `stringified_exception` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:529:42: Name `stringified_exception` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:1169:71: Name `expected` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:1171:47: Name `expected` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:1221:83: Name `expected` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/raises.py:1222:62: Name `expected` used when possibly not defined
- Found 930 diagnostics
+ Found 922 diagnostics

mypy (https://github.com/python/mypy)
- warning[possibly-unresolved-reference] mypy/checker.py:3687:29: Name `active_class` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/checker.py:4882:29: Name `defn` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/checker.py:7879:18: Name `deprecated` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/checker.py:7892:47: Name `candidate` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/checker.py:7893:24: Name `candidate` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/checkexpr.py:4112:33: Name `defn` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/checkmember.py:345:40: Name `items` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/expandtype.py:480:20: Name `cached` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/join.py:571:63: Name `item` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/semanal.py:1327:31: Name `deprecated` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/semanal.py:1330:44: Name `deprecated` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/semanal.py:1357:20: Name `deprecated` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/stubgen.py:810:46: Name `spec` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/suggestions.py:650:39: Name `call_tp` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/type_visitor.py:299:20: Name `cached` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/typeops.py:502:34: Name `arg_type` used when possibly not defined
- warning[possibly-unresolved-reference] mypy/typeops.py:514:24: Name `arg_type` used when possibly not defined
- Found 3336 diagnostics
+ Found 3319 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- warning[possibly-unresolved-reference] mypy_django_plugin/django/context.py:513:31: Name `lookup_type` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/django/context.py:513:58: Name `lookup_type` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/django/context.py:520:24: Name `lookup_type` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/managers.py:148:101: Name `model_arg` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/managers.py:150:94: Name `model_arg` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/manytomany.py:153:42: Name `_To` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/manytomany.py:153:47: Name `_Through` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/manytoone.py:31:20: Name `_To` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/orm_lookups.py:27:72: Name `model_type` used when possibly not defined
- warning[possibly-unresolved-reference] mypy_django_plugin/transformers/orm_lookups.py:50:101: Name `model_type` used when possibly not defined
- Found 1172 diagnostics
+ Found 1162 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- warning[possibly-unresolved-reference] openlibrary/catalog/add_book/load_book.py:166:40: Name `record` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/catalog/marc/parse.py:493:45: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/importapi/code.py:168:30: Name `staged_field` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/openlibrary/status.py:55:48: Name `contents` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/upstream/account.py:434:38: Name `ol_account` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/upstream/account.py:438:25: Name `ol_account` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/upstream/account.py:440:40: Name `ol_account` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/upstream/borrow.py:149:21: Name `acquisitions` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/upstream/borrow.py:150:31: Name `acquisitions` used when possibly not defined
- warning[possibly-unresolved-reference] openlibrary/plugins/upstream/models.py:469:32: Name `lccn` used when possibly not defined
- error[unresolved-reference] openlibrary/solr/query_utils.py:192:39: Name `next_word` used when not defined
- error[unresolved-reference] openlibrary/solr/query_utils.py:192:39: Name `next_word` used when not defined
- error[unresolved-reference] openlibrary/solr/query_utils.py:192:39: Name `next_word` used when not defined
- Found 761 diagnostics
+ Found 748 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- warning[possibly-unresolved-reference] sphinx/ext/autodoc/__init__.py:2714:51: Name `docstring` used when possibly not defined
- warning[possibly-unresolved-reference] sphinx/ext/autodoc/importer.py:125:19: Name `item` used when possibly not defined
- Found 675 diagnostics
+ Found 673 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- warning[possibly-unresolved-reference] aiohttp/client_ws.py:162:46: Name `exc` used when possibly not defined
- warning[possibly-unresolved-reference] aiohttp/connector.py:697:13: Name `conns` used when possibly not defined
- warning[possibly-unresolved-reference] aiohttp/connector.py:698:20: Name `conns` used when possibly not defined
- warning[possibly-unresolved-reference] aiohttp/http_writer.py:195:34: Name `compressed_chunk` used when possibly not defined
- warning[possibly-unresolved-reference] aiohttp/http_writer.py:196:31: Name `compressed_chunk` used when possibly not defined
- warning[possibly-unresolved-reference] aiohttp/web_ws.py:184:46: Name `exc` used when possibly not defined
- Found 173 diagnostics
+ Found 167 diagnostics

rotki (https://github.com/rotki/rotki)
- warning[possibly-unresolved-reference] rotkehlchen/api/rest.py:665:13: Name `gnosispay_decoder` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/api/rest.py:2806:21: Name `monerium` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/api/rest.py:2815:21: Name `gnosis_pay` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/api/v1/schemas.py:1279:28: Name `given_set` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/base/modules/echo/decoder.py:77:36: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/base/modules/echo/decoder.py:104:17: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/base/modules/echo/decoder.py:125:36: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/curve/decoder.py:89:28: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/curve/decoder.py:89:90: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/eigenlayer/utils.py:45:32: Name `eigenpod` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/eth2/beacon.py:286:52: Name `exit_epoch` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/fluence/decoder.py:109:77: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/fluence/decoder.py:115:61: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/lido/decoder.py:107:79: Name `sender` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/sky/decoder.py:294:45: Name `location_label` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/ethereum/modules/sky/decoder.py:302:45: Name `location_label` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/aave/v2/decoder.py:117:32: Name `from_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/aave/v2/decoder.py:118:25: Name `to_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/aave/v3/decoder.py:257:25: Name `balance_increase` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/aave/v3/decoder.py:266:32: Name `balance_increase` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/aave/v3/decoder.py:268:42: Name `balance_increase` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/aura_finance/decoder.py:168:55: Name `crypto_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/balancer/decoder.py:99:51: Name `evm_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/balancer/decoder.py:111:52: Name `evm_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/curve/decoder.py:808:41: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/curve/lend/decoder.py:464:69: Name `crypto_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:721:26: Name `input_rule` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:721:26: Name `input_rule` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:721:26: Name `input_rule` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:790:23: Name `eth_event` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1150:38: Name `token_kind_and_id` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1150:38: Name `token_kind_and_id` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1150:38: Name `token_kind_and_id` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1292:38: Name `token_kind_and_id` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1292:38: Name `token_kind_and_id` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/decoder.py:1292:38: Name `token_kind_and_id` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/extrafi/decoder.py:203:55: Name `token` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/giveth/decoder.py:111:45: Name `user` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/hop/balances.py:140:36: Name `balance_norm` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/hop/balances.py:141:53: Name `balance_norm` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/hop/balances.py:157:36: Name `reward_norm` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/hop/balances.py:158:55: Name `reward_norm` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/odos/common.py:73:30: Name `identifier` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/pendle/decoder.py:446:55: Name `crypto_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/rainbow/decoder.py:174:26: Name `possible_fee_amount` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/rainbow/decoder.py:181:26: Name `possible_fee_amount` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/rainbow/decoder.py:198:34: Name `possible_fee_amount` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/rainbow/decoder.py:200:33: Name `out_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/rainbow/decoder.py:210:34: Name `possible_fee_amount` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/rainbow/decoder.py:211:33: Name `in_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/stakedao/decoder.py:184:57: Name `deposited_asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/thegraph/balances.py:91:47: Name `delegator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/thegraph/balances.py:93:25: Name `beneficiary` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/utils.py:243:15: Name `underlying_token` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/utils.py:245:48: Name `underlying_token` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/evm/decoding/zerox/decoder.py:220:41: Name `used_router_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/gnosis/modules/gnosis_pay/decoder.py:107:35: Name `new_notes` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/gnosis/modules/gnosis_pay/decoder.py:144:35: Name `new_notes` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/polygon_pos/modules/polygon_pos_bridge/decoder.py:96:32: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/polygon_pos/modules/polygon_pos_bridge/decoder.py:134:32: Name `user_address` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/chain/polygon_pos/modules/polygon_pos_bridge/decoder.py:159:27: Name `event_token` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/constants/resolver.py:26:12: Name `id_parts` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:544:47: Name `v_index` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:576:20: Name `target_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:580:20: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:581:24: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:584:32: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:591:44: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:591:44: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:591:44: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:591:44: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:596:54: Name `source_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/eth2.py:597:47: Name `target_validator` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/db/upgrades/v45_v46.py:107:34: Name `label` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/bitmex.py:266:80: Name `error` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/bitstamp.py:261:41: Name `reference` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/coinbase.py:905:62: Name `event_currency` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/coinbase.py:906:45: Name `event_currency` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/coinbaseprime.py:89:42: Name `pair_data` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/coinbaseprime.py:90:43: Name `pair_data` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/coinbaseprime.py:406:42: Name `new_cursor` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/exchanges/iconomi.py:284:44: Name `timestamp` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/externalapis/defillama.py:155:35: Name `coins_result` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/history/price.py:181:32: Name `pool_token` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/history/skipped.py:101:31: Name `location_label` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/inquirer.py:716:48: Name `cache` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/inquirer.py:716:61: Name `cache` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/inquirer.py:1355:44: Name `price_and_oracle` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/rotkehlchen.py:1203:17: Name `eth2` used when possibly not defined
+ warning[unused-ignore-comment] rotkehlchen/tasks/calendar.py:333:84: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:323:25: Name `ens_name` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:324:37: Name `ens_expires` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:326:30: Name `counterparty` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:327:32: Name `ens_name` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:327:77: Name `ens_expires` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:332:21: Name `ens_name` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:336:30: Name `ens_name` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:363:37: Name `locktime` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:366:117: Name `locktime` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:435:20: Name `asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:438:30: Name `asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:444:21: Name `cutoff_time` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:456:31: Name `cutoff_time` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:457:93: Name `asset` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/calendar.py:457:163: Name `cutoff_time` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tasks/manager.py:574:21: Name `indices` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tests/utils/mock.py:141:92: Name `data` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tests/utils/mock.py:147:39: Name `data` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tests/utils/mock.py:149:33: Name `data` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tests/utils/mock.py:152:92: Name `data` used when possibly not defined
- warning[possibly-unresolved-reference] rotkehlchen/tests/utils/mock.py:175:12: Name `closest` used when possibly not defined
- Found 2104 diagnostics
+ Found 1995 diagnostics

zulip (https://github.com/zulip/zulip)
- warning[possibly-unresolved-reference] zerver/openapi/curl_param_value_generators.py:88:22: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] zerver/openapi/markdown_extension.py:344:22: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] zerver/openapi/openapi.py:385:22: Name `content` used when possibly not defined
- Found 4870 diagnostics
+ Found 4867 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- warning[possibly-unresolved-reference] lib/streamlit/elements/lib/pandas_styler_utils.py:267:37: Name `match` used when possibly not defined
- warning[possibly-unresolved-reference] lib/streamlit/elements/lib/pandas_styler_utils.py:267:37: Name `match` used when possibly not defined
- warning[possibly-unresolved-reference] lib/streamlit/elements/lib/pandas_styler_utils.py:267:37: Name `match` used when possibly not defined
- Found 3295 diagnostics
+ Found 3292 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- warning[possibly-unresolved-reference] src/bokeh/core/serialization.py:470:41: Name `arr` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/layouts.py:641:33: Name `tool` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/models/renderers/glyph_renderer.py:98:47: Name `field` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/models/renderers/glyph_renderer.py:99:46: Name `field` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/models/renderers/glyph_renderer.py:101:46: Name `field` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/util/compiler.py:233:28: Name `file` used when possibly not defined
- Found 995 diagnostics
+ Found 989 diagnostics

scipy (https://github.com/scipy/scipy)
- warning[possibly-unresolved-reference] scipy/signal/_signaltools.py:2934:20: Name `m` used when possibly not defined
- Found 7835 diagnostics
+ Found 7834 diagnostics

sympy (https://github.com/sympy/sympy)
- warning[possibly-unresolved-reference] sympy/functions/elementary/integers.py:49:26: Name `i` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/matrices/matrixbase.py:2043:25: Name `fzero` used when possibly not defined
- Found 17162 diagnostics
+ Found 17160 diagnostics

```
</details>


---

_Marked ready for review by @TomerBin on 2025-05-10 21:16_

---

_Review requested from @carljm by @TomerBin on 2025-05-10 21:16_

---

_Review requested from @AlexWaygood by @TomerBin on 2025-05-10 21:16_

---

_Review requested from @sharkdp by @TomerBin on 2025-05-10 21:16_

---

_Review requested from @dcreager by @TomerBin on 2025-05-10 21:16_

---

_Label `ty` added by @AlexWaygood on 2025-05-10 21:18_

---

_Comment by @sharkdp on 2025-05-12 10:20_

Just adding a note that it looks like this fixes https://github.com/astral-sh/ty/issues/139

---

_Comment by @sharkdp on 2025-05-13 11:56_

> Hope you'll get time to review this before the alpha release :)

It's fantastic that you found a way to make this work, and I'm looking forward to reviewing this!

I'll just add another note here that merging this PR is not an immediate priority (anymore) because we recently decided to disable `possibly-unresolved-reference` by default for the alpha release. One of the reasons for this was the bug that you're fixing here, but unfortunately, there are a few other root causes that lead to false-positive `possibly-unresolved-reference` diagnostics, so we would likely keep that default setting for now, even if this would be integrated.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md`:86 on 2025-05-16 10:56_

I don't think this is important enough to try to implement in this PR, but it may be worth a TODO comment here noting that this could be `int & AlwaysFalsy` -- if we reach this point and `x` _is_ defined, it must be falsy.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md`:99 on 2025-05-16 11:02_

Similarly here, if `x` is defined it must be truthy, so this _could_ be `int & AlwaysTruthy` (but I don't think it's important that we implement that right now, it can just be a comment)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/boolean/short_circuit.md`:154 on 2025-05-16 11:06_

Nit:
```suggestion
### Nested boolean expression
```

---

_@carljm approved on 2025-05-16 11:36_

This looks really good! Thank you for the contribution. I will push a small commit with a few renaming adjustments and the TODO comments I mentioned inline, and then merge.

---

_Merged by @carljm on 2025-05-16 11:59_

---

_Closed by @carljm on 2025-05-16 11:59_

---
