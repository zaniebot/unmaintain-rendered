```yaml
number: 18017
title: "[ty] Rename `call-possibly-unbound-method` to `possibly-unbound-implicit-call`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ty-possibly-unbound-dunder-call
created_at: 2025-05-11T14:20:21Z
updated_at: 2025-05-22T15:29:47Z
url: https://github.com/astral-sh/ruff/pull/18017
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Rename `call-possibly-unbound-method` to `possibly-unbound-implicit-call`

---

_Pull request opened by @InSyncWithFoo on 2025-05-11 14:20_

## Summary

Follow-up to #17981 as per [this comment](https://github.com/astral-sh/ruff/pull/17981#discussion_r2081711279), part of [#226](https://github.com/astral-sh/ty/issues/226).

`call-possibly-unbound-method` is now called `possibly-unbound-implicit-call` and has its documentation clarified.

## Test Plan

None.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-11 14:20_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-11 14:20_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-11 14:20_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-11 14:20_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-05-11 14:20_

---

_Comment by @github-actions[bot] on 2025-05-11 14:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- warning[call-possibly-unbound-method] beartype/_check/error/_pep/errpep484604.py:135:29: Method `__getitem__` of type `str | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] beartype/_check/error/_pep/errpep484604.py:135:29: Method `__getitem__` of type `str | None` is possibly unbound

nionutils (https://github.com/nion-software/nionutils)
- warning[call-possibly-unbound-method] nion/utils/Event.py:106:68: Method `__getitem__` of type `Unknown | StackSummary | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] nion/utils/Event.py:106:68: Method `__getitem__` of type `Unknown | StackSummary | None` is possibly unbound

pyinstrument (https://github.com/joerick/pyinstrument)
- warning[call-possibly-unbound-method] pyinstrument/__main__.py:53:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] pyinstrument/__main__.py:53:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] pyinstrument/__main__.py:54:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] pyinstrument/__main__.py:54:9: Method `__getitem__` of type `list[str] | None` is possibly unbound

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- warning[call-possibly-unbound-method] backend/models/global_scoreboard_models.py:93:101: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] backend/models/global_scoreboard_models.py:93:101: Method `__getitem__` of type `Unknown | None` is possibly unbound

kornia (https://github.com/kornia/kornia)
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/affine.py:135:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/affine.py:135:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/center_crop.py:126:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/center_crop.py:126:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/center_crop.py:149:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/center_crop.py:149:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/crop.py:246:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/crop.py:246:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/resize.py:112:20: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/resize.py:112:20: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/resized_crop.py:155:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/resized_crop.py:155:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/rotation.py:114:30: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/rotation.py:216:30: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/shear.py:112:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/rotation.py:114:30: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/rotation.py:216:30: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/shear.py:112:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_2d/geometric/translate.py:108:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/translate.py:108:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_3d/geometric/affine.py:168:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_3d/geometric/affine.py:168:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/augmentation/_3d/geometric/rotation.py:131:32: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/augmentation/_3d/geometric/rotation.py:131:32: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] kornia/enhance/adjust.py:875:57: Method `__getitem__` of type `int | float | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:875:57: Method `__getitem__` of type `int | float | Unknown` is possibly unbound
- warning[call-possibly-unbound-method] kornia/enhance/normalize.py:261:12: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/normalize.py:261:12: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
- warning[call-possibly-unbound-method] kornia/enhance/normalize.py:262:11: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/normalize.py:262:11: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
- warning[call-possibly-unbound-method] kornia/filters/kernels_geometry.py:73:17: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/filters/kernels_geometry.py:73:17: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
- warning[call-possibly-unbound-method] kornia/filters/kernels_geometry.py:81:21: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/filters/kernels_geometry.py:81:21: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
- warning[call-possibly-unbound-method] kornia/filters/kernels_geometry.py:180:21: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/filters/kernels_geometry.py:180:21: Method `__getitem__` of type `Unknown | int | float` is possibly unbound

alerta (https://github.com/alerta/alerta)
- warning[call-possibly-unbound-method] alerta/plugins/escalate.py:47:34: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] alerta/plugins/escalate.py:47:34: Method `__getitem__` of type `Unknown | None` is possibly unbound

koda-validate (https://github.com/keithasaurus/koda-validate)
- warning[call-possibly-unbound-method] koda_validate/serialization/errors.py:179:16: Method `__getitem__` of type `str | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] koda_validate/serialization/errors.py:179:16: Method `__getitem__` of type `str | None` is possibly unbound

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[call-possibly-unbound-method] src/graphql/utilities/introspection_from_schema.py:50:15: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/graphql/utilities/introspection_from_schema.py:50:15: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/error/test_graphql_error.py:260:16: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/error/test_graphql_error.py:260:16: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/error/test_graphql_error.py:261:9: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/error/test_graphql_error.py:261:9: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/execution/test_resolve.py:316:17: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/execution/test_resolve.py:316:17: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/execution/test_resolve.py:322:20: Method `__getitem__` of type `list[SourceLocation] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/execution/test_resolve.py:322:20: Method `__getitem__` of type `list[SourceLocation] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/pyutils/test_description.py:202:30: Method `__getitem__` of type `dict[str, Any] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/pyutils/test_description.py:202:30: Method `__getitem__` of type `dict[str, Any] | None` is possibly unbound

trio (https://github.com/python-trio/trio)
- warning[call-possibly-unbound-method] src/trio/_tests/test_highlevel_serve_listeners.py:151:27: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/trio/_tests/test_highlevel_serve_listeners.py:151:27: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- warning[call-possibly-unbound-method] src/trio/_tests/test_highlevel_serve_listeners.py:152:16: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/trio/_tests/test_highlevel_serve_listeners.py:152:16: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound

pydantic (https://github.com/pydantic/pydantic)
- warning[call-possibly-unbound-method] pydantic/_internal/_generate_schema.py:291:23: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] pydantic/_internal/_generate_schema.py:291:23: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] pydantic/main.py:109:5: Method `__getitem__` of type `dict[Unknown, Unknown] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] pydantic/main.py:109:5: Method `__getitem__` of type `dict[Unknown, Unknown] | None` is possibly unbound

ignite (https://github.com/pytorch/ignite)
- warning[call-possibly-unbound-method] tests/ignite/conftest.py:125:34: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/conftest.py:125:34: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/distributed/test_launcher.py:281:44: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/test_launcher.py:281:44: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/distributed/test_launcher.py:282:42: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/test_launcher.py:282:42: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/distributed/utils/__init__.py:457:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:457:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/distributed/utils/__init__.py:459:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:459:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/distributed/utils/__init__.py:482:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:482:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/distributed/utils/__init__.py:484:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:484:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_fractional_bias.py:159:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:159:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:140:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:140:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:124:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:124:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:125:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:125:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_kendall_correlation.py:176:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_kendall_correlation.py:176:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_kendall_correlation.py:177:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_kendall_correlation.py:177:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_manhattan_distance.py:131:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_manhattan_distance.py:131:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_manhattan_distance.py:132:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_manhattan_distance.py:132:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_maximum_absolute_error.py:125:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_maximum_absolute_error.py:125:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_maximum_absolute_error.py:126:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_maximum_absolute_error.py:126:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:162:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:162:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:163:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:163:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_mean_error.py:116:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_mean_error.py:117:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_error.py:116:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_error.py:117:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_mean_normalized_bias.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_normalized_bias.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_mean_normalized_bias.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_normalized_bias.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_median_absolute_error.py:145:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_error.py:145:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_median_absolute_error.py:146:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_error.py:146:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:165:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:165:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:166:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:166:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_median_relative_absolute_error.py:154:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_relative_absolute_error.py:154:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_median_relative_absolute_error.py:155:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_relative_absolute_error.py:155:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_pearson_correlation.py:210:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_pearson_correlation.py:210:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_pearson_correlation.py:211:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_pearson_correlation.py:211:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_r2_score.py:137:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_r2_score.py:137:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_r2_score.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_r2_score.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_spearman_correlation.py:163:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_spearman_correlation.py:163:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_spearman_correlation.py:164:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_spearman_correlation.py:164:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_wave_hedges_distance.py:113:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/regression/test_wave_hedges_distance.py:114:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_wave_hedges_distance.py:113:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_wave_hedges_distance.py:114:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_accuracy.py:318:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_accuracy.py:318:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_accuracy.py:445:40: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_accuracy.py:445:40: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_classification_report.py:29:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_classification_report.py:29:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_cohen_kappa.py:231:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_cohen_kappa.py:231:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_cohen_kappa.py:232:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_cohen_kappa.py:232:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_cosine_similarity.py:97:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_cosine_similarity.py:97:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_cosine_similarity.py:98:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_cosine_similarity.py:98:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_entropy.py:102:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_entropy.py:102:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_entropy.py:103:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_entropy.py:103:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_epoch_metric.py:172:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_epoch_metric.py:172:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_fbeta.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_fbeta.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_js_divergence.py:122:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_js_divergence.py:122:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_js_divergence.py:123:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_js_divergence.py:123:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_kl_divergence.py:121:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_kl_divergence.py:121:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_kl_divergence.py:122:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_kl_divergence.py:122:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_maximum_mean_discrepancy.py:133:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_maximum_mean_discrepancy.py:133:24: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_maximum_mean_discrepancy.py:133:66: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_maximum_mean_discrepancy.py:133:66: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_mean_absolute_error.py:72:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_mean_absolute_error.py:72:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_mean_absolute_error.py:73:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_mean_absolute_error.py:73:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_mean_squared_error.py:71:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_mean_squared_error.py:71:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_mean_squared_error.py:72:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_mean_squared_error.py:72:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_metrics_lambda.py:490:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_metrics_lambda.py:490:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_mutual_information.py:108:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_mutual_information.py:108:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_mutual_information.py:109:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_mutual_information.py:109:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_precision.py:352:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_precision.py:352:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_precision_recall_curve.py:218:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_precision_recall_curve.py:219:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_precision_recall_curve.py:218:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_precision_recall_curve.py:219:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_psnr.py:84:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_psnr.py:84:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_psnr.py:85:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_psnr.py:85:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_recall.py:355:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_recall.py:355:21: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_roc_curve.py:149:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_roc_curve.py:149:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_roc_curve.py:150:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_roc_curve.py:150:13: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_root_mean_squared_error.py:71:20: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_root_mean_squared_error.py:71:20: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_root_mean_squared_error.py:71:68: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_root_mean_squared_error.py:71:68: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] tests/ignite/metrics/test_top_k_categorical_accuracy.py:73:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/ignite/metrics/test_top_k_categorical_accuracy.py:73:17: Method `__getitem__` of type `Unknown | int | float | list[Unknown]` is possibly unbound

sockeye (https://github.com/awslabs/sockeye)
- warning[call-possibly-unbound-method] sockeye/decoder.py:224:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] sockeye/decoder.py:224:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_data_io.py:367:20: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_data_io.py:367:20: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_data_io.py:414:25: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_data_io.py:414:25: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_data_io.py:415:25: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_data_io.py:415:25: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_data_io.py:420:28: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_data_io.py:420:28: Method `__getitem__` of type `Unknown | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:48:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:48:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:49:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:49:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:50:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:50:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:53:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:53:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:54:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:54:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:57:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:57:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/unit/test_knn.py:58:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/unit/test_knn.py:58:17: Method `__getitem__` of type `Unknown | None` is possibly unbound

flake8 (https://github.com/pycqa/flake8)
- warning[call-possibly-unbound-method] src/flake8/formatting/base.py:167:22: Method `__getitem__` of type `str | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/flake8/formatting/base.py:167:22: Method `__getitem__` of type `str | None` is possibly unbound

cki-lib (https://gitlab.com/cki-project/cki-lib)
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:232:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:232:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:233:54: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:233:54: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:239:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:239:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:240:46: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:240:46: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:246:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:246:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:247:36: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:247:36: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:253:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:253:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:257:58: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:257:58: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:262:54: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:262:54: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:268:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:268:41: Method `__getitem__` of type `Match[str] | None` is possibly unbound
- warning[call-possibly-unbound-method] cki_lib/gitlab.py:269:40: Method `__getitem__` of type `Match[str] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] cki_lib/gitlab.py:269:40: Method `__getitem__` of type `Match[str] | None` is possibly unbound

werkzeug (https://github.com/pallets/werkzeug)
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:34:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:34:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:35:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:35:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:36:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:36:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:39:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:39:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:40:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:40:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:41:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:41:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:42:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:42:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:46:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:46:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:47:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:47:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/middleware/test_http_proxy.py:51:12: Method `__getitem__` of type `Any | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/middleware/test_http_proxy.py:51:12: Method `__getitem__` of type `Any | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_routing.py:604:5: Method `__getitem__` of type `Unknown | Mapping[str, Any] | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_routing.py:604:5: Method `__getitem__` of type `Unknown | Mapping[str, Any] | None` is possibly unbound

stone (https://github.com/dropbox/stone)
- warning[call-possibly-unbound-method] test/test_backend.py:273:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/test_backend.py:273:17: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] test/test_python_types.py:173:9: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/test_python_types.py:173:9: Method `__getitem__` of type `Unknown | None` is possibly unbound

vision (https://github.com/pytorch/vision)
- warning[call-possibly-unbound-method] references/classification/utils.py:319:5: Method `__getitem__` of type `None | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] references/classification/utils.py:319:5: Method `__getitem__` of type `None | Unknown` is possibly unbound
- warning[call-possibly-unbound-method] torchvision/ops/roi_align.py:189:27: Method `__getitem__` of type `None | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] torchvision/ops/roi_align.py:189:27: Method `__getitem__` of type `None | Unknown` is possibly unbound
- warning[call-possibly-unbound-method] torchvision/ops/roi_align.py:190:27: Method `__getitem__` of type `None | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] torchvision/ops/roi_align.py:190:27: Method `__getitem__` of type `None | Unknown` is possibly unbound
- warning[call-possibly-unbound-method] torchvision/ops/roi_align.py:194:19: Method `__getitem__` of type `Unknown | Literal[1]` is possibly unbound
+ warning[possibly-unbound-implicit-call] torchvision/ops/roi_align.py:194:19: Method `__getitem__` of type `Unknown | Literal[1]` is possibly unbound

urllib3 (https://github.com/urllib3/urllib3)
- warning[call-possibly-unbound-method] test/with_dummyserver/test_socketlevel.py:2130:54: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] test/with_dummyserver/test_socketlevel.py:2130:54: Method `__getitem__` of type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound

twine (https://github.com/pypa/twine)
- warning[call-possibly-unbound-method] twine/__init__.py:44:13: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] twine/__init__.py:44:13: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
- warning[call-possibly-unbound-method] twine/__init__.py:45:15: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] twine/__init__.py:45:15: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
- warning[call-possibly-unbound-method] twine/__init__.py:51:15: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] twine/__init__.py:51:15: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
- warning[call-possibly-unbound-method] twine/__init__.py:52:47: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] twine/__init__.py:52:47: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
- warning[call-possibly-unbound-method] twine/__init__.py:52:47: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] twine/__init__.py:52:47: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
- warning[call-possibly-unbound-method] twine/__init__.py:52:47: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] twine/__init__.py:52:47: Method `__getitem__` of type `PackageMetadata | None` is possibly unbound

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- warning[call-possibly-unbound-method] src/hydra_zen/structured_configs/_implementations.py:2130:13: Method `__getitem__` of type `DataclassOptions | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/hydra_zen/structured_configs/_implementations.py:2130:13: Method `__getitem__` of type `DataclassOptions | None` is possibly unbound
- warning[call-possibly-unbound-method] src/hydra_zen/structured_configs/_implementations.py:2140:13: Method `__getitem__` of type `DataclassOptions | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/hydra_zen/structured_configs/_implementations.py:2140:13: Method `__getitem__` of type `DataclassOptions | None` is possibly unbound
- warning[call-possibly-unbound-method] src/hydra_zen/structured_configs/_implementations.py:3483:9: Method `__getitem__` of type `DataclassOptions | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/hydra_zen/structured_configs/_implementations.py:3483:9: Method `__getitem__` of type `DataclassOptions | None` is possibly unbound
- warning[call-possibly-unbound-method] src/hydra_zen/structured_configs/_make_custom_builds.py:333:9: Method `__getitem__` of type `(@Todo(dict comprehension type) & ~None) | DataclassOptions | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] src/hydra_zen/structured_configs/_make_custom_builds.py:333:9: Method `__getitem__` of type `(@Todo(dict comprehension type) & ~None) | DataclassOptions | None` is possibly unbound

schema_salad (https://github.com/common-workflow-language/schema_salad)
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:23:57: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:23:57: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:24:33: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:24:33: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:25:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:25:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:69:60: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:69:60: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:70:32: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:70:32: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:71:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:71:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:123:57: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:123:57: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:124:33: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:124:33: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:125:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:125:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:144:57: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:144:57: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:145:12: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:145:12: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[call-possibly-unbound-method] schema_salad/tests/test_cg.py:146:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-implicit-call] schema_salad/tests/test_cg.py:146:24: Method `__getitem__` of type `Unknown | None` is possibly unbound

Expression (https://github.com/cognitedata/Expression)
- warning[call-possibly-unbound-method] tests/test_seq.py:15:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq.py:15:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq.py:27:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq.py:27:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq.py:38:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq.py:38:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq_builder.py:16:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:16:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq_builder.py:37:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:37:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq_builder.py:57:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:57:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq_builder.py:77:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:77:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_seq_builder.py:89:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
+ warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:89:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[call-possibly-un...*[Comment body truncated]*

---

_Label `ty` added by @dylwil3 on 2025-05-11 20:25_

---

_Comment by @MichaReiser on 2025-05-12 07:38_

Thank you.

This is a question for everyone. Are there any other implicit method calls to non-dunder methods? Should we rename the rule to `possibly-unbound-implicit-call` to make it more explicit that this is about implicit calls and less about calls to dunder methods (e.g. calling a possibly unbound dunder method gives a possibly-unbound-attribute` error). 

The other thing that isn't clear to me is why we have a call-possibly-undounb-method specifically for dunder calls but not for other call related errors. Should we just remove the error code altogether and instead use a custom message or note instead?


---

_Comment by @dscorbett on 2025-05-12 14:32_

In Python 3.14, `fractions.Fraction` implicitly calls `as_integer_ratio`. There are plenty of other implicit calls (search for `getattr` and `hasattr` in the CPython source) but that seems to be the most user-facing and well-documented one.

---

_Renamed from "[ty] Rename `call-possibly-unbound-method` to `possibly-unbound-dunder-call`" to "[ty] Rename `call-possibly-unbound-method` to `possibly-unbound-implicit-call`" by @InSyncWithFoo on 2025-05-14 00:04_

---

_Comment by @MichaReiser on 2025-05-14 05:00_

@AlexWaygood what do you think of the new name?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:107 on 2025-05-22 03:27_

```suggestion
    /// Expressions such as `x[y]` and `x * y` call methods
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:108 on 2025-05-22 03:27_

```suggestion
    /// under the hood (`__getitem__` and `__mul__` respectively).
```

---

_@carljm approved on 2025-05-22 03:28_

Barring a couple tweaks, this looks fine to me. @AlexWaygood since this was your suggestion, do you want to give it a final review and adjust/merge?

---

_Comment by @AlexWaygood on 2025-05-22 04:14_

Shoot, sorry, I missed the earlier pings here. I'll get this over the line tomorrow!

---

_@AlexWaygood approved on 2025-05-22 15:22_

This is great, thank you

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-22 15:22_

---

_Merged by @AlexWaygood on 2025-05-22 15:25_

---

_Closed by @AlexWaygood on 2025-05-22 15:25_

---

_Branch deleted on 2025-05-22 15:29_

---
