```yaml
number: 18115
title: "[ty] Make dataclass instances adhere to DataclassInstance"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-400
created_at: 2025-05-15T08:46:02Z
updated_at: 2025-05-16T02:34:55Z
url: https://github.com/astral-sh/ruff/pull/18115
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Make dataclass instances adhere to DataclassInstance

---

_Pull request opened by @sharkdp on 2025-05-15 08:46_

## Summary

Make dataclass instances adhere to the `DataclassInstance` protocol.

fixes astral-sh/ty#400

## Test Plan

New Markdown tests


---

_Review requested from @carljm by @sharkdp on 2025-05-15 08:46_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-15 08:46_

---

_Review requested from @dcreager by @sharkdp on 2025-05-15 08:46_

---

_Label `ty` added by @sharkdp on 2025-05-15 08:46_

---

_Comment by @github-actions[bot] on 2025-05-15 08:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
com2ann (https://github.com/ilevkivskyi/com2ann)
- error[no-matching-overload] src/test_cli.py:234:53: No overload of function `asdict` matches arguments
- Found 13 diagnostics
+ Found 12 diagnostics

kopf (https://github.com/nolar/kopf)
- error[invalid-argument-type] kopf/_core/intents/handlers.py:35:45: Argument to function `replace` is incorrect: Argument type `CauseT & ChangingCause` does not satisfy upper bound of type variable `_DataclassT`
- Found 184 diagnostics
+ Found 183 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-argument-type] pydantic/_internal/_generate_schema.py:2073:38: Argument to function `replace` is incorrect: Argument type `ComputedFieldInfo` does not satisfy upper bound of type variable `_DataclassT`
- Found 773 diagnostics
+ Found 772 diagnostics

black (https://github.com/psf/black)
- error[invalid-argument-type] src/black/__init__.py:899:32: Argument to function `replace` is incorrect: Argument type `Mode` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] src/black/__init__.py:901:32: Argument to function `replace` is incorrect: Argument type `Mode` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] src/black/__init__.py:942:24: Argument to function `replace` is incorrect: Argument type `Mode` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] src/black/__init__.py:944:24: Argument to function `replace` is incorrect: Argument type `Mode` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] src/black/linegen.py:922:46: Argument to function `replace` is incorrect: Argument type `Mode` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] src/black/linegen.py:1053:32: Argument to function `replace` is incorrect: Argument type `Mode` does not satisfy upper bound of type variable `_DataclassT`
- Found 141 diagnostics
+ Found 135 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[invalid-argument-type] mkosi/__init__.py:1373:34: Argument to function `replace` is incorrect: Argument type `Config` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] mkosi/config.py:5605:73: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type[Args] | type[Config]`
- Found 318 diagnostics
+ Found 316 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[invalid-argument-type] boostedblob/google_auth.py:77:31: Argument to function `replace` is incorrect: Argument type `Request` does not satisfy upper bound of type variable `_DataclassT`
- Found 60 diagnostics
+ Found 59 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[invalid-argument-type] tests/test_tagged_union.py:80:31: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `Shape`
- error[no-matching-overload] tests/test_tagged_union.py:81:12: No overload of function `asdict` matches arguments
- error[no-matching-overload] tests/test_tagged_union.py:198:12: No overload of function `asdict` matches arguments
- Found 318 diagnostics
+ Found 315 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-argument-type] test/mitmproxy/addons/test_next_layer.py:662:17: Argument to function `replace` is incorrect: Argument type `TConf` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] test/mitmproxy/addons/test_next_layer.py:726:13: Argument to function `replace` is incorrect: Argument type `TConf` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] test/mitmproxy/addons/test_next_layer.py:768:13: Argument to function `replace` is incorrect: Argument type `TConf` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] test/mitmproxy/addons/test_next_layer.py:776:13: Argument to function `replace` is incorrect: Argument type `TConf` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] test/mitmproxy/addons/test_next_layer.py:803:13: Argument to function `replace` is incorrect: Argument type `TConf` does not satisfy upper bound of type variable `_DataclassT`
- error[invalid-argument-type] test/mitmproxy/addons/test_next_layer.py:819:13: Argument to function `replace` is incorrect: Argument type `TConf` does not satisfy upper bound of type variable `_DataclassT`
- Found 2169 diagnostics
+ Found 2163 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-argument-type] psycopg/psycopg/_tpc.py:111:24: Argument to function `replace` is incorrect: Argument type `Xid` does not satisfy upper bound of type variable `_DataclassT`
- Found 1201 diagnostics
+ Found 1200 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[no-matching-overload] mesonbuild/mformat.py:186:27: No overload of function `asdict` matches arguments
- error[no-matching-overload] mesonbuild/mformat.py:186:27: No overload of function `asdict` matches arguments
- error[no-matching-overload] mesonbuild/mformat.py:186:27: No overload of function `asdict` matches arguments
- error[invalid-argument-type] mesonbuild/mformat.py:849:29: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `EditorConfig`
- error[invalid-argument-type] mesonbuild/mformat.py:877:86: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `FormatterConfig`
- error[invalid-argument-type] mesonbuild/mformat.py:881:29: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `FormatterConfig`
- Found 1465 diagnostics
+ Found 1459 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:928:24: Return type does not match returned value: expected `DataClass_`, found `(((...) -> Any) & type[HydraConf]) | (DataClass_ & type[HydraConf]) | (list[Any] & type[HydraConf]) | (dict[Any, Any] & type[HydraConf])`
- Found 643 diagnostics
+ Found 642 diagnostics

colour (https://github.com/colour-science/colour)
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/cam16.py:445:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam02.py:520:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/ciecam16.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/hellwig2022.py:532:48: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/kim2009.py:476:35: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/appearance/zcam.py:644:66: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:498:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- error[no-matching-overload] colour/difference/delta_e.py:773:59: No overload of function `astuple` matches arguments
- Found 613 diagnostics
+ Found 538 diagnostics

optuna (https://github.com/optuna/optuna)
- error[no-matching-overload] optuna/artifacts/_upload.py:107:70: No overload of function `asdict` matches arguments
- error[no-matching-overload] optuna/artifacts/_upload.py:110:70: No overload of function `asdict` matches arguments
- Found 1187 diagnostics
+ Found 1185 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[no-matching-overload] src/_pytest/reports.py:494:16: No overload of function `asdict` matches arguments
- error[no-matching-overload] src/_pytest/reports.py:502:18: No overload of function `asdict` matches arguments
- error[no-matching-overload] src/_pytest/reports.py:512:20: No overload of function `asdict` matches arguments
- Found 934 diagnostics
+ Found 931 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[invalid-argument-type] aiohttp/client.py:909:46: Argument to function `replace` is incorrect: Argument type `ClientWSTimeout | (Unknown & ClientWSTimeout)` does not satisfy upper bound of type variable `_DataclassT`
- Found 175 diagnostics
+ Found 174 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-argument-type] lib/streamlit/runtime/scriptrunner_utils/script_requests.py:200:25: Argument to function `replace` is incorrect: Argument type `RerunData` does not satisfy upper bound of type variable `_DataclassT`
- Found 3249 diagnostics
+ Found 3248 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[no-matching-overload] ddtrace/internal/remoteconfig/_connectors.py:93:31: No overload of function `asdict` matches arguments
- Found 8711 diagnostics
+ Found 8710 diagnostics

zulip (https://github.com/zulip/zulip)
- error[no-matching-overload] corporate/views/portico.py:137:17: No overload of function `asdict` matches arguments
- error[no-matching-overload] corporate/views/portico.py:209:17: No overload of function `asdict` matches arguments
- error[no-matching-overload] corporate/views/portico.py:277:17: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/actions/message_flags.py:93:13: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/actions/message_flags.py:134:13: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/actions/message_flags.py:177:13: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/lib/user_groups.py:1211:12: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/tests/test_thumbnail.py:654:48: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/tests/test_thumbnail.py:694:17: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/tests/test_thumbnail.py:706:17: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/tests/test_thumbnail.py:718:17: No overload of function `asdict` matches arguments
- error[no-matching-overload] zerver/worker/thumbnail.py:121:17: No overload of function `asdict` matches arguments
- Found 4893 diagnostics
+ Found 4881 diagnostics

rotki (https://github.com/rotki/rotki)
- error[invalid-argument-type] rotkehlchen/tests/api/test_settings.py:60:25: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `DBSettings`
- error[invalid-argument-type] rotkehlchen/tests/api/test_settings.py:119:25: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `DBSettings`
- error[invalid-argument-type] rotkehlchen/tests/db/test_db.py:527:37: Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `DBSettings`
- error[no-matching-overload] rotkehlchen/tests/utils/database.py:72:32: No overload of function `asdict` matches arguments
- error[no-matching-overload] rotkehlchen/tests/utils/database.py:72:32: No overload of function `asdict` matches arguments
- error[no-matching-overload] rotkehlchen/tests/utils/database.py:72:32: No overload of function `asdict` matches arguments
- Found 2113 diagnostics
+ Found 2107 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[invalid-argument-type] misc/python/materialize/mzexplore/common.py:306:20: Argument to function `replace` is incorrect: Argument type `ExplainFile` does not satisfy upper bound of type variable `_DataclassT`
- Found 3280 diagnostics
+ Found 3279 diagnostics

```
</details>


---

_@sharkdp reviewed on 2025-05-15 08:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:646 on 2025-05-15 08:57_

This just passes "by accident" at the moment (because we still think that class objects themselves are also instances of the `DataclassInstance` protocol, and `fields` accepts `DataclassInstance | type[DataclassInstance]`), but I formulated it in such a way that it will be correct eventually.

---

_@AlexWaygood approved on 2025-05-15 12:20_

Thank you!

---

_Merged by @sharkdp on 2025-05-15 12:27_

---

_Closed by @sharkdp on 2025-05-15 12:27_

---

_Branch deleted on 2025-05-15 12:27_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:1146 on 2025-05-15 13:08_

After moving this here, the test `static_assert(is_subtype_of(Foo, DataclassInstance))` will (incorrectly) start passing, right?
Should that be added as a TODO, along with the `asdict` TODO in the tests?

---

_@abhijeetbodas2001 reviewed on 2025-05-15 13:08_

---

_@abhijeetbodas2001 reviewed on 2025-05-15 13:08_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:658 on 2025-05-15 13:08_

There's also `astuple` with a similar signature. Should that also be included in the tests?

---

_@sharkdp reviewed on 2025-05-15 18:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1146 on 2025-05-15 18:48_

> After moving this here, the test `static_assert(is_subtype_of(Foo, DataclassInstance))` will (incorrectly) start passing, right?

Yes, I think so. I did not add this test, but a similar one (passing `Foo` to `asdict`). Feel free to suggest this as a follow-up (with a TODO), if you think it makes sense to add.

---

_@sharkdp reviewed on 2025-05-15 18:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses.md`:658 on 2025-05-15 18:48_

Sure, that would make sense.

---

_@carljm reviewed on 2025-05-16 02:34_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:1146 on 2025-05-16 02:34_

Maybe I'm missing something here, but I think `static_assert(is_subtype_of(Foo, DataclassInstance))` is correct and should pass. `is_subtype_of` takes type expressions, and the type expression `Foo` does not describe a class literal type, it describes the type of all instances of `Foo`. That type is a subtype of `DataclassInstance`.

(In the previous PR on this topic I made a comment about a wrong test, but that comment was wrong :) I thought the test `fields(Foo)` should fail because the class object `Foo` does not inhabit `DataclassInstance`. The latter is right, but `fields(Foo)` should still succeed because `fields` also accepts `type[DataclassInstance]`.)

The test which should eventually not pass would be `is_subtype_of(type[Foo], DataclassInstance)` or `is_subtype_of(ty_extensions.TypeOf[Foo], DataclassInstance)`. (The former tests what we call a `SubclassOf` type, the latter a `ClassLiteral` type.)

---
