```yaml
number: 20776
title: "[ty] Type-context aware literal promotion"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/literal-promotion
created_at: 2025-10-08T21:36:00Z
updated_at: 2025-10-09T20:59:08Z
url: https://github.com/astral-sh/ruff/pull/20776
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Type-context aware literal promotion

---

_@ibraheemdev_

## Summary

Avoid literal promotion when a literal type annotation is provided, e.g.,
```py
x: list[Literal[1]] = [1]
```

Resolves https://github.com/astral-sh/ty/issues/1198. This does not fix issue https://github.com/astral-sh/ty/issues/1284, but it does make it more relevant because after this change, it is possible to directly instantiate a generic type with a literal specialization.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-08 21:36_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-08 21:36_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-08 21:36_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-08 21:36_

---

_Label `ty` added by @ibraheemdev on 2025-10-08 21:36_

---

_Comment by @github-actions[bot] on 2025-10-08 21:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-09 20:41:23.943326536 +0000
+++ new-output.txt	2025-10-09 20:41:27.257340205 +0000
@@ -643,9 +643,7 @@
 literals_literalstring.py:75:5: error[invalid-assignment] Object of type `Literal[b"test"]` is not assignable to `LiteralString`
 literals_literalstring.py:79:21: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bool`
 literals_literalstring.py:120:22: error[invalid-argument-type] Argument to function `literal_identity` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `TLiteral`
-literals_literalstring.py:130:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Container[T@Container]`, found `Container[str]`
 literals_literalstring.py:134:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `str` does not satisfy upper bound `LiteralString` of type variable `T`
-literals_literalstring.py:138:1: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[LiteralString]`
 literals_literalstring.py:167:1: error[type-assertion-failure] Argument does not have asserted type `A`
 literals_literalstring.py:171:5: error[invalid-assignment] Object of type `list[LiteralString]` is not assignable to `list[str]`
 literals_parameterizations.py:41:15: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -900,5 +898,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 901 diagnostics
+Found 899 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-08 21:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/longlinemarker.py:56:17: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[Literal["bgcolor", "color", "border"]]`
- Found 24 diagnostics
+ Found 23 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_generate_schema.py:1967:9: error[invalid-assignment] Object of type `dict[Unknown | _ParameterKind, Unknown | str]` is not assignable to `dict[_ParameterKind, Literal["positional_only", "positional_or_keyword", "keyword_only"]]`
- pydantic/_internal/_generate_schema.py:2041:9: error[invalid-assignment] Object of type `dict[Unknown | _ParameterKind, Unknown | str]` is not assignable to `dict[_ParameterKind, Literal["positional_only", "positional_or_keyword", "var_args", "keyword_only"]]`
- Found 768 diagnostics
+ Found 766 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/exchange/binance.py:54:37: error[invalid-argument-type] Invalid argument to key "order_props_in_contracts" with declared type `list[Literal["amount", "cost", "filled", "remaining"]]` on TypedDict `FtHas`: value of type `list[str]`
- freqtrade/exchange/exchange.py:161:37: error[invalid-argument-type] Invalid argument to key "order_props_in_contracts" with declared type `list[Literal["amount", "cost", "filled", "remaining"]]` on TypedDict `FtHas`: value of type `list[str]`
- Found 404 diagnostics
+ Found 402 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/modules/_qt.py:941:9: error[invalid-assignment] Object of type `list[Unknown | bool]` is not assignable to `list[str | Literal[False]]`
- Found 887 diagnostics
+ Found 886 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ui/select.py:1227:13: error[invalid-assignment] Object of type `dict[Unknown | <class 'UserSelect'> | <class 'RoleSelect'> | <class 'MentionableSelect'> | <class 'ChannelSelect'>, Unknown | ComponentType]` is not assignable to `dict[type[BaseSelect[Unknown]], Literal[ComponentType.user_select, ComponentType.channel_select, ComponentType.role_select, ComponentType.mentionable_select]]`
- Found 509 diagnostics
+ Found 508 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/environment/__init__.py:94:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | bool | ((node: Node) -> bool)]` is not assignable to `dict[str, Literal[False] | ((Node, /) -> bool)]`
- Found 493 diagnostics
+ Found 492 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- pyodide-build/pyodide_build/recipe/skeleton.py:359:47: error[invalid-argument-type] Argument to function `_find_dist` is incorrect: Expected `list[Literal["wheel", "sdist"]]`, found `list[Unknown | str]`
- Found 986 diagnostics
+ Found 985 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/book_providers.py:747:9: error[invalid-argument-type] Argument to function `multisort_best` is incorrect: Expected `list[tuple[Literal["min", "max"], (@Todo, /) -> int | float]]`, found `list[Unknown | tuple[str, (rec) -> Unknown]]`
- Found 850 diagnostics
+ Found 849 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/observers/ecs.py:44:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `dict[str, Literal["task", "container-instance", "deployment"]]`
- Found 3203 diagnostics
+ Found 3202 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/lib/zig.py:17:1: error[invalid-assignment] Object of type `dict[Unknown | tuple[str, str, int], Unknown | str]` is not assignable to `dict[tuple[@Todo, Literal["little", "big"], int], str]`
- Found 2581 diagnostics
+ Found 2580 diagnostics

colour (https://github.com/colour-science/colour)
- colour/notation/munsell.py:2037:5: error[invalid-assignment] Object of type `dict[Unknown | int, Unknown | None | str]` is not assignable to `dict[int, Literal["Linear", "Radial"] | None]`
- Found 479 diagnostics
+ Found 478 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/config.py:1189:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1205:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1220:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1235:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1250:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1265:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1287:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1309:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1331:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1353:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1375:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1396:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1418:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1439:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1460:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1481:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1502:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1523:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1544:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1565:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1586:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1607:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1628:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1649:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1670:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1691:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1712:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1727:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1743:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1763:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1778:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1807:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1838:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1852:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1866:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1893:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1932:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1969:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:1994:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2014:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2036:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2062:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2090:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2105:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2122:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2143:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2159:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2170:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- lib/streamlit/config.py:2213:5: error[invalid-argument-type] Argument to function `_create_theme_options` is incorrect: Expected `list[Literal["theme"] | CustomThemeCategories]`, found `list[Unknown | str]`
- Found 194 diagnostics
+ Found 145 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/frame.py:14046:5: error[invalid-assignment] Object of type `list[Unknown | str]` is not assignable to `list[Literal["index", "columns"]]`
- pandas/core/generic.py:5417:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | None]` is not assignable to `dict[Literal["index", "columns"], Any]`
- Found 3388 diagnostics
+ Found 3386 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/bitcoin/xpub.py:217:25: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/chain/bitcoin/xpub.py:223:25: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/chain/evm/decoding/balancer/constants.py:27:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | int]` is not assignable to `dict[str, Literal[1, 2, 3]]`
- rotkehlchen/db/dbhandler.py:1315:75: error[invalid-argument-type] Argument to function `insert_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/db/dbhandler.py:1370:13: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/db/dbhandler.py:1731:67: error[invalid-argument-type] Argument to function `insert_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/db/dbhandler.py:1765:68: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/db/dbhandler.py:3213:17: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/db/dbhandler.py:3222:17: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/rotkehlchen.py:939:17: error[invalid-argument-type] Argument to function `replace_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/tests/utils/xpubs.py:34:13: error[invalid-argument-type] Argument to function `insert_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- rotkehlchen/tests/utils/xpubs.py:52:13: error[invalid-argument-type] Argument to function `insert_tag_mappings` is incorrect: Expected `list[Literal["identifier", "chain", "address", "xpub.xpub", "derivation_path"]]`, found `list[Unknown | str]`
- Found 1714 diagnostics
+ Found 1702 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/recorder/statistics.py:1913:9: error[invalid-argument-type] Argument to function `_statistics_at_time` is incorrect: Expected `set[Literal["last_reset", "max", "mean", "min", "state", "sum"]]`, found `set[Unknown | str]`
- homeassistant/components/sensor/recorder.py:584:34: error[invalid-argument-type] Argument to function `get_latest_short_term_statistics_with_session` is incorrect: Expected `set[Literal["last_reset", "max", "mean", "min", "state", "sum"]]`, found `set[Unknown | str]`
- Found 13695 diagnostics
+ Found 13693 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- ci/deploy/pypi.py:23:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str]` is not assignable to `dict[str, Literal["setup.py", "pyproject.toml"]]`
- Found 3388 diagnostics
+ Found 3387 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/stats/_qmc.py:2609:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, ((...) -> Unknown) | ((best_sample: ndarray[@Todo, Unknown], n_iters: int, n_nochange: int, rng: GeneratorType@_random_cd, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown]) | ((sample: @Todo, *, tol: @Todo = float, maxiter: @Todo = int, qhull_options: str | None = None, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown])].__getitem__(key: str, /) -> ((...) -> Unknown) | ((best_sample: ndarray[@Todo, Unknown], n_iters: int, n_nochange: int, rng: GeneratorType@_random_cd, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown]) | ((sample: @Todo, *, tol: @Todo = float, maxiter: @Todo = int, qhull_options: str | None = None, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown])` cannot be called with key of type `Literal["random-cd", "lloyd"] | None` on object of type `dict[str, ((...) -> Unknown) | ((best_sample: ndarray[@Todo, Unknown], n_iters: int, n_nochange: int, rng: GeneratorType@_random_cd, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown]) | ((sample: @Todo, *, tol: @Todo = float, maxiter: @Todo = int, qhull_options: str | None = None, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown])]`
+ scipy/stats/_qmc.py:2609:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, ((...) -> Unknown) | ((best_sample: ndarray[@Todo, Unknown], n_iters: int, n_nochange: int, rng: GeneratorType@_random_cd, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown]) | ((sample: @Todo, *, tol: @Todo = float, maxiter: @Todo = Literal[10], qhull_options: str | None = None, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown])].__getitem__(key: str, /) -> ((...) -> Unknown) | ((best_sample: ndarray[@Todo, Unknown], n_iters: int, n_nochange: int, rng: GeneratorType@_random_cd, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown]) | ((sample: @Todo, *, tol: @Todo = float, maxiter: @Todo = Literal[10], qhull_options: str | None = None, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown])` cannot be called with key of type `Literal["random-cd", "lloyd"] | None` on object of type `dict[str, ((...) -> Unknown) | ((best_sample: ndarray[@Todo, Unknown], n_iters: int, n_nochange: int, rng: GeneratorType@_random_cd, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown]) | ((sample: @Todo, *, tol: @Todo = float, maxiter: @Todo = Literal[10], qhull_options: str | None = None, **kwargs: dict[Unknown, Unknown]) -> ndarray[@Todo, Unknown])]`

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-08 21:50_

---

_Comment by @github-actions[bot] on 2025-10-08 21:55_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 66 | 1 |
| `invalid-assignment` | 0 | 13 | 0 |
| **Total** | **0** | **79** | **1** |

**[Full report with detailed diff](https://ibraheem-literal-promotion.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-literal-promotion.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-08 21:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion)

### Merging #20776 will **not alter performance**

<sub>Comparing <code>ibraheem/literal-promotion</code> (41af679) with <code>main</code> (537ec5f)</sub>



### Summary

`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fliteral-promotion?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_@ibraheemdev reviewed on 2025-10-09 00:19_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1148 on 2025-10-09 00:19_

Hmm.. this is incorrect, because we're promoting the type of each parameter, not the type of the generic class, so the type annotation specialization does not match. We might need to delay the promotion step for this to work.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:858 on 2025-10-09 11:21_

So far, I think we only required `object` to exist. It looks like this requires a custom typeshed to have all known classes?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1215 on 2025-10-09 11:32_

An interesting consequence here is that we would still perform promotion if something else in the annotation makes the promoted type assignable. For example:
```py
from typing import Any, Literal

xs: list[Any | Literal[1]] = [1, 1]

reveal_type(xs)  # list[Any | int]
```
Probably nothing to worry about, though...

---

_Comment by @AlexWaygood on 2025-10-09 11:32_

> ```diff
> + tests/core/test_scope.py:127:23: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `Box[str]`, found `Box[str]`
> + tests/core/test_scope.py:148:19: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `Box[str]`, found `Box[str]`
> + tests/core/test_scope.py:159:19: error[invalid-argument-type] Argument to bound method `set` is incorrect: Expected `Box[str]`, found `Box[str]`
> ```

These look quite odd -- any idea what's causing these diagnostics? `Box[str]` must refer to the same `Box[str]` across any single diagnostic in these instances, or we'd automatically use the fully qualified name of `Box` in the diagnostic to disambiguate it (e.g. https://github.com/astral-sh/ruff/pull/20756#issuecomment-3379038485)

This also doesn't look correct -- `dict[str, str]` is one of the union elements in the first type here, so it seems like it should be assignable to that union:

```diff
tornado (https://github.com/tornadoweb/tornado)
+ tornado/test/auth_test.py:563:69: error[invalid-argument-type] Argument to function `url_concat` is incorrect: Expected `None | dict[str, str] | list[tuple[str, str]] | tuple[tuple[str, str], ...]`, found `dict[str, str]`
```

---

_Comment by @AlexWaygood on 2025-10-09 11:37_

> ```diff
> - colour/graph/conversion.py:1121:30: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | str | ((sd: @Todo | SpectralDistribution | MultiSpectralDistributions, cmfs: MultiSpectralDistributions | None = None, illuminant: SpectralDistribution | None = None, k: @Todo | None = None, method: str = str, **kwargs: Any) -> @Todo) | ... omitted 60 union elements`
> + colour/graph/conversion.py:1121:30: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | Literal["Spectral Distribution", "CIE XYZ", "Luminous Flux", "Luminous Efficiency", "Luminous Efficacy", "Luminance", "Lightness", "Whiteness", "Yellowness", "CIE xy", "Colorimetric Purity", "Complementary Wavelength", "Dominant Wavelength", "Excitation Purity", "Wavelength", "CIE xyY", "CIE Lab", "CIE Luv", "CIE Luv uv", "CIE UCS", "CIE UCS uv", "CIE UVW", "DIN99", "hdr-CIELAB", "Hunter Lab", "Hunter Rdab", "ICaCb", "ICtCp", "IgPgTg", "IPT", "IPT Ragoo 2021", "Jzazbz", "hdr-IPT", "OSA UCS", "Oklab", "ProLab", "sUCS", "Yrg", "CIE 1931", "CIE 1960 UCS", "CIE 1976 UCS", "RGB", "Scene-Referred RGB", "HSV", "HSL", "HCL", "IHLS", "CMY", "CMYK", "RGB Luminance", "Prismatic", "Output-Referred RGB", "YCbCr", "YcCbcCrc", "YCoCg", "sRGB", "Hexadecimal", "CSS Color 3", "Munsell Colour", "Munsell Value", "CRI", "CQS", "CCT", "Mired", "ATD95", "CIECAM02", "CIECAM02 JMh", "CAM16", "CAM16 JMh", "CIECAM16", "CIECAM16 JMh", "Hellwig 2022", "Hellwig 2022 JMh", "Kim 2009", "Hunt", "LLAB", "Nayatani95", "RLAB", "sCAM", "sCAM JMh", "ZCAM", "CAM02LCD", "CAM02SCD", "CAM02UCS", "CAM16LCD", "CAM16SCD", "CAM16UCS"] | (def sd_to_XYZ(sd: @Todo | SpectralDistribution | MultiSpectralDistributions, cmfs: MultiSpectralDistributions | None = None, illuminant: SpectralDistribution | None = None, k: @Todo | None = None, method: str = Literal["ASTM E308"], **kwargs: Any) -> @Todo) | ... omitted 117 union elements`
> ```

Possibly not something for this PR, but we should look into why we're creating these huge unions in `colour` (the size of which apparently doubles with this PR!). It obviously really slows us down when we get unions that big. We might want to look into whether it's really correct/beneficial to be inferring these huge unions on that codebase. Even if it is, maybe there are some places where we handle unions in an inefficient way that we can optimise. (Operations on unions are inherently at least quadratic, but we might be able to add caching in some places, and there still might be some places where our handling is accidentally worse than quadratic unnecessarily.)

(The reason why we added `colour` as a benchmark originally is that both pyright and mypy used to have truly pathological performance on that codebase. I guess we're starting to see why that might be the case...!)

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:1148 on 2025-10-09 11:42_

I think I'd need an example to understand this better..

---

_@sharkdp approved on 2025-10-09 11:43_

Thank you — this looks great.

---

_@AlexWaygood reviewed on 2025-10-09 11:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:858 on 2025-10-09 11:47_

we also require `tuple`, here: <https://github.com/astral-sh/ruff/blob/f0d0b5790079cc38ec0f4b34d771a056f1cd1481/crates/ty_python_semantic/src/types/tuple.rs#L200-L217>

But yes, in general I think we should try to impose as few restrictions as possible on users of custom typesheds, and should try our hardest to gracefully fallback to `Unknown` in situations when we try to lookup classes in typeshed

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:531 on 2025-10-09 12:02_

there are some interesting examples above this for unannotated `frozenset`s (which don't need to be fixed in this PR, I'm just mentioning it because it might be something you'd be interested in fixing in future work). `frozenset`, like `tuple`, is covariant, so for an unannotated `x = frozenset((1, 2, 3))` call, we would ideally infer the type as `frozenset[Literal[1, 2, 3]]`, the same as we would infer `(1, 2, 3)` as `tuple[Literal[1], Literal[2], Literal[3]]` rather than `tuple[int, int, int]`. Unlike with invariant generics, `Literal` types don't really cause any ergonomic issues when they appear in specializations of covariant types.

But we need to be careful: `list(frozenset((1, 2, 3)))` should still be inferred as `list[int]` rather than `list[Literal[1, 2, 3]]` if there's no declared type! And `[frozenset((1, 2))]` should be `list[frozenset[int]]`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1215 on 2025-10-09 12:11_

Hrm, I think I _would_ prefer `list[Any | Literal[1]]` to be the revealed type there... but I do agree that this seems unlikely to come up in the real world, so it doesn't need to block this PR

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5216 on 2025-10-09 12:22_

It seems like we currently have only one use of `try_call_dunder` that passes a `tcx` which isn't `TypeContext::default()`, but all callsites become more verbose because of the new parameter. Is it worth maybe having two methods?

```rs
    fn try_call_dunder(
        self,
        db: &'db dyn Db,
        name: &str,
        mut argument_types: CallArguments<'_, 'db>,
    ) -> Result<Bindings<'db>, CallDunderError<'db>> {
        self.try_call_dunder_with_type_context(
	        self,
	        db: &'db dyn Db,
	        name: &str,
	        mut argument_types: CallArguments<'_, 'db>,
            TypeContext::default()
	    )
    }

    fn try_call_dunder_with_type_context(
        self,
        db: &'db dyn Db,
        name: &str,
        mut argument_types: CallArguments<'_, 'db>,
        tcx: TypeContext<'db>,
    ) -> Result<Bindings<'db>, CallDunderError<'db>> {
        // Has the body of the method called `try_call_dunder` on your branch
    }
```

Then most callsites could use the `try_call_dunder()` method and not have to worry about the `tcx` argument.

Or are we anticipating that we'll want to eventually plumb the type context through many more of these `try_call_dunder` calls in due course?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6364 on 2025-10-09 12:25_

same question here -- it seems like the vast majority of `apply_type_mapping()` calls currently just pass in `TypeContext::default()`, so I wonder if it would be more ergonomic to have separate `apply_type_mapping` and `apply_type_mapping_with_context` methods, where the former would just be a thin wrapper around the latter?

---

_@AlexWaygood reviewed on 2025-10-09 12:28_

Cool stuff!

---

_@T-256 reviewed on 2025-10-09 16:01_

---

_Review comment by @T-256 on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:133 on 2025-10-09 16:01_

is that mean `x: list[Literal[1, 2, 3]] = [2]` declaration is fine?

---

_@ibraheemdev reviewed on 2025-10-09 19:41_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1148 on 2025-10-09 19:41_

Nevermind, I just needed to map the type variable instances, instead of relying on the order of the parameters.

---

_@ibraheemdev reviewed on 2025-10-09 19:45_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:858 on 2025-10-09 19:45_

We also require this for `list`, `set`, and `dict`, which are the same classes that this method is used for: <https://github.com/astral-sh/ruff/blob/db91ac7dce312b24dc7d7adbf0f2857f6771fccd/crates/ty_python_semantic/src/types/infer/builder.rs#L5589-L5592>

It would be easy to fallback and ignore the type context though, if that's preferable.

---

_@ibraheemdev reviewed on 2025-10-09 19:48_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:6364 on 2025-10-09 19:48_

I think passing `TypeContext::default()` is a useful cue that we are ignoring a potential type context. There are probably cases where we currently have type context but are not making use of it.

---

_@ibraheemdev reviewed on 2025-10-09 19:49_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:133 on 2025-10-09 19:49_

Yes, that works fine, as it's equivalent to `x: list[Literal[1] | Literal[2] | Literal[3]] = [2]`.

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-09 19:51_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-09 19:51_

---

_Comment by @ibraheemdev on 2025-10-09 19:57_

> These look quite odd -- any idea what's causing these diagnostics?

I fixed these, we were passing incorrect `TypeContext` to generic constructors in some cases, the ecosystem report is much better now.

The diagnostics are interesting though — we currently reinfer the RHS of annotated assignments without any type-context before displaying the diagnostics. This was originally to avoid unioning the inferred type with the type annotation, but it now also means that we always perform literal promotion (as there is no type-context to prevent it). For example:
```py
x: list[Literal[3]] = [2]
```
```
error[invalid-assignment]: Object of type `list[Unknown | int]` is not assignable to `list[Literal[3]]`
 --> x.py:3:1
  |
1 | from typing import Literal
2 |
3 | x: list[Literal[3]] = [2]
  | ^
```
The type in the diagnostic is now *less* precise because of the reinference. I don't think the "`X[T]` is not assignable to `X[T]`" error is ever possible without a bug in ty, but this example also seems problematic. I'm not sure if we should avoid literal promotion entirely during reinference (which could lead to large unions in diagnostics), or just remove the reinference step entirely.

---

_@AlexWaygood reviewed on 2025-10-09 19:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:858 on 2025-10-09 19:58_

> It would be easy to fallback and ignore the type context though, if that's preferable.

I think that would be better, if it's easy. Even if it's unlikely that users would actually try to use custom typesheds that didn't include these classes, it makes it easier to write tests that have custom typesheds in them if we keep the requirements here to an absolute minimum

---

_@AlexWaygood reviewed on 2025-10-09 19:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6364 on 2025-10-09 19:58_

Okay, that's fine then 👍

---

_Comment by @AlexWaygood on 2025-10-09 20:01_

> The diagnostics are interesting though — we currently reinfer the RHS of annotated assignments without any type-context before displaying the diagnostics. This was originally to avoid unioning the inferred type with the type annotation, but it now also means that we always perform literal promotion (as there is no type-context to prevent it).

Those diagnostics don't seem _too_ bad to me tbh — I've seen worse from mypy/pyright for this kind of thing 😆 I wouldn't spend too much time on that right now, personally

---

_@T-256 reviewed on 2025-10-09 20:14_

---

_Review comment by @T-256 on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:133 on 2025-10-09 20:14_

it's worth to add such example to mdtests, At first, I thought all values in `Literal[` part of annotation must be as same as declared values.

---

_@AlexWaygood approved on 2025-10-09 20:40_

Nice!

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-09 20:41_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-09 20:41_

---

_Merged by @ibraheemdev on 2025-10-09 20:53_

---

_Closed by @ibraheemdev on 2025-10-09 20:53_

---

_Branch deleted on 2025-10-09 20:53_

---

_Comment by @ibraheemdev on 2025-10-09 20:59_

The regression on `colour` seemed to be because of a bug in the literal promotion code. This change now has no performance impact.

---
