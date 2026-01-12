```yaml
number: 17445
title: "[red-knot] `typing.dataclass_transform`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dataclass_transform
created_at: 2025-04-17T08:37:59Z
updated_at: 2025-05-02T16:00:10Z
url: https://github.com/astral-sh/ruff/pull/17445
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] `typing.dataclass_transform`

---

_@sharkdp_

## Summary

* Add initial support for `typing.dataclass_transform`
  * Support decorating a function decorator with `@dataclass_transform(…)` (used by `attrs`, `strawberry`)
  * Support decorating a metaclass with `@dataclass_transform(…)` (used by `pydantic`, but doesn't work yet, because we don't seem to model `__new__` calls correctly?)
  * *No* support yet for decorating base classes with `@dataclass_transform(…)`. I haven't figured out how this even supposed to work. And haven't seen it being used.
* Add `strawberry` as an ecosystem project, as it makes heavy use of `@dataclass_transform`

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-04-17 08:37_

---

_Comment by @github-actions[bot] on 2025-04-17 08:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:64:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:65:9: Argument `stub_distribution_name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:66:9: Argument `upstream_url` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:67:9: Argument `extra_description` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:68:9: Argument `completeness_level` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:69:9: Argument `number_of_lines` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:70:9: Argument `package_status` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:71:9: Argument `upload_status` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:72:9: Argument `stubtest_settings` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:73:13: Argument `strictness` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:73:52: Argument `platforms` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:73:66: Argument `allowlist_length` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:75:9: Argument `pyright_setting` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/test_serialize.py:76:9: Argument `annotation_stats` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:159:9: Argument `annotated_parameters` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:160:9: Argument `unannotated_parameters` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:161:9: Argument `annotated_returns` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:162:9: Argument `unannotated_returns` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:163:9: Argument `explicit_Incomplete_parameters` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:164:9: Argument `explicit_Incomplete_returns` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:165:9: Argument `explicit_Any_parameters` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:166:9: Argument `explicit_Any_returns` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:167:9: Argument `annotated_variables` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:168:9: Argument `explicit_Any_variables` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:169:9: Argument `explicit_Incomplete_variables` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:170:9: Argument `classdefs` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:171:9: Argument `classdefs_with_Any` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:172:9: Argument `classdefs_with_Incomplete` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:241:9: Argument `package_name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:242:9: Argument `stub_distribution_name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:243:9: Argument `upstream_url` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:244:9: Argument `completeness_level` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:245:9: Argument `extra_description` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:246:9: Argument `number_of_lines` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:247:9: Argument `package_status` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:248:9: Argument `stubtest_settings` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:248:44: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:249:9: Argument `upload_status` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:250:9: Argument `pyright_setting` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:251:9: Argument `annotation_stats` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:258:9: Argument `file_path` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:261:9: Argument `parent_package` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:262:9: Argument `number_of_lines` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:263:9: Argument `pyright_setting` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/tests/conftest.py:264:9: Argument `annotation_stats` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:591:9: Argument `strictness` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:592:9: Argument `platforms` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:593:9: Argument `allowlist_length` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1176:9: Argument `package_name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1177:9: Argument `stub_distribution_name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1180:9: Argument `upstream_url` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1181:9: Argument `completeness_level` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1184:9: Argument `extra_description` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1187:9: Argument `number_of_lines` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1188:9: Argument `package_status` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1191:9: Argument `upload_status` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1192:9: Argument `stubtest_settings` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1195:9: Argument `pyright_setting` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1198:9: Argument `annotation_stats` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1320:9: Argument `file_path` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1321:9: Argument `parent_package` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1322:9: Argument `number_of_lines` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1323:9: Argument `pyright_setting` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/typeshed-stats/src/typeshed_stats/gather.py:1326:9: Argument `annotation_stats` does not match any known parameter of bound method `__init__`
- Found 88 diagnostics
+ Found 24 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:33:9: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:34:9: Argument `triangles` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/examples/attrs.py:36:17: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:36:25: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:36:31: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:37:25: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:37:31: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/examples/attrs.py:37:37: Argument `z` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/examples/attrs.py:38:25: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:372:20: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:377:20: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:377:31: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:383:17: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:384:20: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:469:25: Argument `foo` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:469:33: Argument `bar` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:469:41: Argument `foo` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:621:32: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:621:38: Argument `foo` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:647:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:662:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:469:25: Argument `foo` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:469:33: Argument `bar` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:469:41: Argument `foo` does not match any known parameter
- Found 300 diagnostics
+ Found 282 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/union.py:52:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/schema.py:298:25: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/object_type.py:61:13: Argument `fields` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/object_type.py:61:25: Argument `resolvable` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/object_type.py:72:34: Argument `policies` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/object_type.py:75:42: Argument `scopes` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/object_type.py:81:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:39:13: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:40:13: Argument `username` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:41:13: Argument `email` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:42:13: Argument `role` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:74:13: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:75:13: Argument `title` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:76:13: Argument `content` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:77:13: Argument `author` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:111:13: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:112:13: Argument `text` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:113:13: Argument `author` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:114:13: Argument `post` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:29:25: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:29:40: Argument `province` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:30:23: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:30:37: Argument `canton` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:74:17: Argument `message` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:75:17: Argument `field` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:76:17: Argument `fix` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:290:26: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:290:32: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:325:26: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:325:32: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:370:28: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:370:34: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:370:54: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:370:60: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:402:25: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:235:22: Argument `override_from` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:235:46: Argument `label` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:241:34: Argument `policies` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:244:36: Argument `fields` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:247:36: Argument `fields` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:250:42: Argument `scopes` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/field.py:256:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_name_converter.py:112:47: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_python_generics.py:99:38: Argument `data` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_python_generics.py:156:38: Argument `data` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/experimental/pydantic/test_basic.py:836:71: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/experimental/pydantic/test_basic.py:847:44: Argument `reason` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/relay/test_connection.py:163:26: Argument `id` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_extensions.py:27:50: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_generics_schema.py:1057:57: Argument `field` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_generics_schema.py:1057:69: Argument `fields` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_generics_schema.py:1103:34: Argument `obj` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_generics_schema.py:1141:30: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/experimental/pydantic/schema/test_federation.py:16:44: Argument `fields` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/experimental/pydantic/schema/test_federation.py:16:58: Argument `resolvable` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/enum.py:37:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/enum.py:111:34: Argument `policies` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/enum.py:114:42: Argument `scopes` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/enum.py:117:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:22:66: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:47:44: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:85:50: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:88:60: Argument `min` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:88:67: Argument `max` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:90:48: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:97:21: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:98:21: Argument `meta` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:163:35: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:163:50: Argument `real_age` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:163:64: Argument `real_age_2` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:186:66: Argument `real_age` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:210:66: Argument `real_age` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:234:66: Argument `real_age` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:258:66: Argument `real_age` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:282:60: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:284:68: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:311:45: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:311:65: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:342:35: Argument `config` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:371:66: Argument `config` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:402:35: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:431:66: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:450:60: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:477:44: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:512:46: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:565:28: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:565:51: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:616:64: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:657:64: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:660:64: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:691:63: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:695:64: Argument `reason` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:731:52: Argument `input` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:770:30: Argument `input` does not match any known parameter of bound method `__init__`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/strawberry/strawberry/tools/create_type.py:67:12: Return type does not match returned value: Expected `type`, found `@Todo(Support for `typing.TypeVar` instances in type expressions) | ((@Todo(Support for `typing.TypeVar` instances in type expressions), /) -> @Todo(Support for `typing.TypeVar` instances in type expressions))`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/strawberry/strawberry/tools/merge_types.py:35:12: Return type does not match returned value: Expected `type`, found `@Todo(Support for `typing.TypeVar` instances in type expressions) | ((@Todo(Support for `typing.TypeVar` instances in type expressions), /) -> @Todo(Support for `typing.TypeVar` instances in type expressions))`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/scalar.py:144:34: Argument `policies` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/scalar.py:147:42: Argument `scopes` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/scalar.py:150:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/strawberry/federation/argument.py:23:31: Argument `name` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/extensions/test_input_mutation.py:41:43: Argument `some` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/extensions/test_input_mutation.py:41:55: Argument `directive` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics.py:1058:57: Argument `field` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics.py:1058:69: Argument `fields` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics.py:1108:34: Argument `obj` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics.py:1148:30: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics.py:1203:44: Argument `data` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/relay/test_connection.py:163:26: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:238:30: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:238:53: Argument `total` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:238:62: Argument `items` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:239:30: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:239:53: Argument `total` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:239:62: Argument `items` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:240:27: Argument `id` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:240:50: Argument `data` does not match any known parameter of bound method `__init__`
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/strawberry/tests/federation/printer/test_additional_directives.py:1:1: Unused blanket `type: ignore` directive
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:29:40: Argument `province` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:30:37: Argument `canton` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:76:17: Argument `fix` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:290:32: Argument `name` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_interface.py:325:32: Argument `name` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:40:13: Argument `username` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:41:13: Argument `email` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:42:13: Argument `role` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:75:13: Argument `title` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:76:13: Argument `content` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:77:13: Argument `author` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:112:13: Argument `text` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:113:13: Argument `author` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/benchmarks/schema.py:114:13: Argument `post` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/python_312/test_generics_schema.py:1141:30: Argument `y` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics.py:1148:30: Argument `y` does not match any known parameter
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/strawberry/tests/federation/printer/test_override.py:1:1: Unused blanket `type: ignore` directive
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/test_printer/test_schema_directives.py:565:51: Argument `name` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:238:53: Argument `total` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:238:62: Argument `items` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:239:53: Argument `total` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:239:62: Argument `items` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/strawberry/tests/schema/test_generics_nested.py:240:50: Argument `data` does not match any known parameter
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/strawberry/tests/federation/printer/test_keys.py:1:1: Unused blanket `type: ignore` directive
- Found 2670 diagnostics
+ Found 2581 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-17 14:26_

---

_Review requested from @carljm by @sharkdp on 2025-04-17 14:27_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-17 14:27_

---

_Review requested from @dcreager by @sharkdp on 2025-04-17 14:27_

---

_Comment by @sharkdp on 2025-04-17 14:32_

Ecosystem analysis:

There are a few new false positives.

We do not understand unannotated dataclass fields:

```py
@attr.define
class Foo:
    foo = attr.field()

Foo(foo=1)  # Argument `foo` does not match any known parameter
```

The false positives in `strawberry` from from the fact that we don't understand `@dataclass_transform` in combination with overloads yet. This affects [`@strawberry.type`](https://github.com/strawberry-graphql/strawberry/blob/be6caa05f86f236061d1bf3d3ff77a8bb64a003d/strawberry/types/object_type.py#L188-L228), but not some of the other "transformers" like [`@strawberry.interface`](https://github.com/strawberry-graphql/strawberry/blob/be6caa05f86f236061d1bf3d3ff77a8bb64a003d/strawberry/types/object_type.py#L388-L422), because the latter also decorates the implementation itself with `@dataclass_transform`.

---

_Converted to draft by @sharkdp on 2025-04-17 18:36_

---

_@sharkdp reviewed on 2025-04-18 18:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:347 on 2025-04-18 18:44_

New name in analogy to the `__dataclass_params__` runtime introspection attribute, which contains the same information.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3578 on 2025-04-18 19:00_

I think it's fine to annotate this with `type`. We don't need the generic `type[_T]` form, as we're going to compute the return type "manually" anyway.

---

_@sharkdp reviewed on 2025-04-18 19:00_

---

_@sharkdp reviewed on 2025-04-18 19:31_

---

_Review comment by @sharkdp on `Cargo.toml`:236 on 2025-04-18 19:31_

I didn't find a way to only set this for `red_knot_python_semantic`, as it inherits this configuration, and it looks like I can't overwrite workspace settings for lints?

---

_Comment by @github-actions[bot] on 2025-04-18 19:39_

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

_@sharkdp reviewed on 2025-04-18 20:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:290 on 2025-04-18 20:44_

This is similar to the (working) test above, but now it's not the implementation that is decorated with `@dataclass_transform()`, but one of the overloads. This is explicitly allowed in the spec.

I haven't looked into this deeply, but I am assume this isn't working since we never see the decorator on the actual function definition. I am not sure how to handle this, as I'm not yet familiar with how overloads work. And how we would treat decorators on overloads. @dhruvmanila This is not urgent, but if you see an easy solution to this, I'd be glad to hear it.

---

_@sharkdp reviewed on 2025-04-18 20:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call/bind.rs`:705 on 2025-04-18 20:45_

Haven't invested more time into making this prettier, as I'm not sure if that one failing test case at the bottom if `dataclass_transform.md` will affect how we handle overloaded dataclass-transformers in general.

---

_Marked ready for review by @sharkdp on 2025-04-18 20:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3458 on 2025-04-21 18:41_

Probably just a TODO at most for now, but do we care at all about the annotated return type of a function decorated with `@dataclass_transform`, and whether it's consistent with this signature?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:290 on 2025-04-21 19:07_

The logic for collecting overloads is in `FunctionType::signature` method. I think you'd either need to have that method also return "dataclass transform params, if found" (which is a little weird), or extract the overload-walking logic into a separate method that iterates overloads as `FunctionLiteral` types, and use that both for your purposes and in `::signature`.

---

_@carljm approved on 2025-04-21 19:07_

Looks good!

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:368 on 2025-04-21 19:40_

nit: we could do `impl From<DataclassTransformerParams> for DataclassParams` ?

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:290 on 2025-04-21 21:39_

> extract the overload-walking logic into a separate method that iterates overloads as `FunctionLiteral` types, and use that both for your purposes and in `::signature`.

Yeah, I think this is what I'd do as well.

Initially, I thought we could have a method that returns an iterator over the overloaded `FunctionType` but that wouldn't work reliably because we need to follow through all of the symbols with the same name to find the boundary where the overload ends. This means the method would need to collect all of them, so maybe something like the following method:

```rs
struct OverloadedFunction<'db> {
    /// The overloads of this function.
    overloads: Vec<FunctionType<'db>>,
    /// The implementation of this overloaded function, if any.
    implementation: Option<FunctionType<'db>>,
}

#[salsa::tracked]
impl<'db> FunctionType<'db> {
    /// Returns `self` as [`OverloadedFunction`] if it is overloaded, [`None`] otherwise.
    fn to_overloaded(self, db: &'db dyn Db) -> Option<OverloadedFunction<'db>> {
        todo!()
    }
}
```

This could be used to get the `DataclassTransformerParams` that's added to a `FunctionType` which is decorated with `@dataclass_transform`. Another way would be to add a method to return this and to keep the above method internal-only:

```rs
#[salsa::tracked]
impl<'db> FunctionType<'db> {
    /// Returns `self` as [`OverloadedFunction`] if it is overloaded, [`None`] otherwise.
    fn to_overloaded(self, db: &'db dyn Db) -> Option<OverloadedFunction<'db>> {
        todo!()
    }

	// TODO: What should be the name of this method? Or, should we change the field name?
    pub(crate) fn dataclass_transformer_params(self, ...) -> Option<DataclassTransformerParams> {
		if let Some(overloaded) = self.to_overloaded(db) {
			// get the `DataclassTransformerParams` from `OverloadedFunction`
		} else {
			self.dataclass_transformer_params(db)
		}
	}
}
```

We might then need to check whether "the `dataclass_transform` decorator is applied to any one, but not more than one, of the overloads" and raise diagnostic (although Pyright and mypy doesn't do that) within this function and then it might just be better to actually create the `DataclassTransformerParams` in this method instead of during inference.

I also thought about this for a bit and I would also need to somehow iterate over all the overloaded function definitions during the inference (at the end of `infer_region_scope`) to validate the overload usages. The information that I require would all exists on the `Signature` (would need to copy the `FunctionDecorators`) which can be fetched by the `signature` method so that might be ok.

---

_@dhruvmanila approved on 2025-04-21 21:39_

---

_@sharkdp reviewed on 2025-04-22 08:32_

---

_Review comment by @sharkdp on `Cargo.toml`:236 on 2025-04-22 08:32_

I'll merge this for now, but I will open a salsa ticket to improve this, and add a reminder for myself to remove this again.

---

_Merged by @sharkdp on 2025-04-22 08:33_

---

_Closed by @sharkdp on 2025-04-22 08:33_

---

_Branch deleted on 2025-04-22 08:33_

---

_@sharkdp reviewed on 2025-04-22 08:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:290 on 2025-04-22 08:52_

Thank you Carl and Dhruv. I left this open for now, but opened a new (post-alpha?) [ticket](https://github.com/astral-sh/ruff/issues/17541).

---

_@abhijeetbodas2001 reviewed on 2025-05-02 10:31_

---

_Review comment by @abhijeetbodas2001 on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:278 on 2025-05-02 10:31_

In this example, `versioned_class` takes in a parameter (`version`).
In that case, `versioned_class`, when called, should *return a decorator* which will accept a class, right?
If the default `version` value has to be picked up, even then, the invocation should be like this, isn't it?

```py
@versioned_class()   # notice ()
class D1:
    x: str
```

@sharkdp I did not understand how `@versioned_class` (without the call) will work.


---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:278 on 2025-05-02 10:47_

> In this example, `versioned_class` takes in a parameter (`version`).

Well... it takes a keyword-only argument `version`. But it can also take a positional `cls` argument (first overload).



> In that case, `versioned_class`, when called, should _return a decorator_ which will accept a class, right?

If it's called with `versioned_class(version=2)`, it should return a decorator, yes.

But if it's called with `versioned_class(some_class)`, where `some_class` is of type `T`, it should return a `T` (first overload). This lets `versioned_class` *itself* also act as a decorator.

> If the default `version` value has to be picked up, even then, the invocation should be like this, isn't it?

> `@versioned_class() # notice ()`

Using `@versioned_class()` should *also* work (it calls the second overload), but the purpose of this test was to check that it also works without the parens (in which case it is called with one argument, the `D1` class, and picks the first overload).


---

_@sharkdp reviewed on 2025-05-02 10:47_

---

_@abhijeetbodas2001 reviewed on 2025-05-02 12:11_

---

_Review comment by @abhijeetbodas2001 on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:278 on 2025-05-02 12:11_

Thanks for the explanation.

> If it's called with versioned_class(version=2), it should return a decorator, yes.

For cases where `versioned_class` itself is the decorator (first overload), there is no way to pass a custom `version` value right? If so, what is the point of having the `version` kwarg in the first overload's signature? (ie, why not just `versioned_class(cls: T) -> T`?)

---

_@sharkdp reviewed on 2025-05-02 15:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:278 on 2025-05-02 15:25_

I guess you could still use it with non-decorator syntax
```py
class C: …

C = versioned_class(C, version=2)
```
But yeah, I agree, it's probably not that useful. I added this test because a lot of libraries like `attrs`, `pydantic`, or `strawberry` use this exact pattern. They provide dataclass-like decorators which you can use with or without arguments. And using these two overloads is exactly how they are implemented.

---

_@abhijeetbodas2001 reviewed on 2025-05-02 16:00_

---

_Review comment by @abhijeetbodas2001 on `crates/red_knot_python_semantic/resources/mdtest/dataclass_transform.md`:278 on 2025-05-02 16:00_

Got it, thanks for the context!

---
