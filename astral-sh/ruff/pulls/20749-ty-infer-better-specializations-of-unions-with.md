```yaml
number: 20749
title: "[ty] Infer better specializations of unions with `None` (etc)"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/union-none
created_at: 2025-10-07T15:34:23Z
updated_at: 2025-10-07T17:33:44Z
url: https://github.com/astral-sh/ruff/pull/20749
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Infer better specializations of unions with `None` (etc)

---

_Pull request opened by @dcreager on 2025-10-07 15:34_

This PR adds a specialization inference special case that lets us handle the following examples better:

```py
def f[T](t: T | None) -> T: ...
def g[T](t: T | int | None) -> T | int: ...

def _(x: str | None):
    reveal_type(f(x))  # revealed: str (previously str | None)

def _(y: str | int | None):
    reveal_type(g(x))  # revealed: str | int (previously str | int | None)
```

We already have a special case for when the formal is a union where one element is a typevar, but it maps the entire actual type to the typevar (as you can see in the "previously" results above).

The new special case kicks in when the actual is also a union. Now, we filter out any actual union elements that are already subtypes of the formal, and only bind whatever types remain to the typevar. (The `| None` pattern appears quite often in the ecosystem results, but it's more general and works with any number of non-typevar union elements.)

The new constraint solver should handle this case as well, but it's worth adding this heuristic now with the old solver because it eliminates some false positives from the ecosystem report, and makes the ecosystem report less noisy on the other constraint solver PRs.

---

_Label `ty` added by @dcreager on 2025-10-07 15:34_

---

_Comment by @github-actions[bot] on 2025-10-07 15:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 15:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_subprocesses.py:125:12: error[invalid-return-type] Return type does not match returned value: expected `CompletedProcess[bytes]`, found `CompletedProcess[bytes | None]`
- Found 226 diagnostics
+ Found 225 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/frames.py:196:20: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/resolvelib/resolvers/resolution.py:48:35: error[invalid-argument-type] Argument to function `_has_route_to_root` is incorrect: Expected `Mapping[KT@_build_result | @Todo | None, Criterion[RT@_build_result, CT@_build_result]]`, found `dict[KT@_build_result, Criterion[RT@_build_result, CT@_build_result]]`
- src/pip/_vendor/resolvelib/resolvers/resolution.py:597:16: error[invalid-return-type] Return type does not match returned value: expected `Result[RT@Resolver, CT@Resolver, KT@Resolver]`, found `Result[Unknown, Unknown | None, Unknown]`
- Found 467 diagnostics
+ Found 465 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_scope.py:51:16: error[invalid-return-type] Return type does not match returned value: expected `ScopeVarToken[T@AbstractScopeVar, Any]`, found `ScopeVarToken[T@AbstractScopeVar | Missing, Unknown]`
- Found 305 diagnostics
+ Found 304 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/core/option.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `Option[_TSource@of_optional]`, found `Option[_TSource@of_optional | None]`
- Found 213 diagnostics
+ Found 212 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/ext/apidoc/_generate.py:50:12: error[no-matching-overload] No overload of bound method `join` matches arguments
- sphinx/ext/graphviz.py:377:14: error[no-matching-overload] No overload of bound method `join` matches arguments
- Found 504 diagnostics
+ Found 502 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/models/detection/fcos.py:770:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- torchvision/models/detection/ssd.py:677:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- torchvision/models/detection/ssdlite.py:323:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- torchvision/models/detection/ssdlite.py:324:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- torchvision/models/segmentation/deeplabv3.py:276:41: error[invalid-argument-type] Argument to function `_deeplabv3_resnet` is incorrect: Expected `int`, found `int | None`
- torchvision/models/segmentation/deeplabv3.py:332:41: error[invalid-argument-type] Argument to function `_deeplabv3_resnet` is incorrect: Expected `int`, found `int | None`
- torchvision/models/segmentation/deeplabv3.py:386:46: error[invalid-argument-type] Argument to function `_deeplabv3_mobilenetv3` is incorrect: Expected `int`, found `int | None`
- torchvision/models/segmentation/fcn.py:168:35: error[invalid-argument-type] Argument to function `_fcn_resnet` is incorrect: Expected `int`, found `int | None`
- torchvision/models/segmentation/fcn.py:227:35: error[invalid-argument-type] Argument to function `_fcn_resnet` is incorrect: Expected `int`, found `int | None`
- torchvision/models/segmentation/lraspp.py:173:43: error[invalid-argument-type] Argument to function `_lraspp_mobilenetv3` is incorrect: Expected `int`, found `int | None`
- Found 1491 diagnostics
+ Found 1481 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:246:26: error[invalid-argument-type] Argument to function `reschedule` is incorrect: Expected `Task`, found `Task | None`
- src/trio/_core/_tests/test_run.py:253:26: error[invalid-argument-type] Argument to function `reschedule` is incorrect: Expected `Task`, found `Task | None`
- src/trio/_core/_tests/test_run.py:291:16: warning[possibly-missing-attribute] Attribute `parent_task` on type `Nursery | None` may be missing
- src/trio/_core/_tests/test_run.py:299:12: warning[possibly-missing-attribute] Attribute `parent_nursery` on type `Task | None` may be missing
- src/trio/_core/_tests/test_run.py:299:35: warning[possibly-missing-attribute] Attribute `eventual_parent_nursery` on type `Task | None` may be missing
- src/trio/_core/_tests/test_run.py:1553:13: warning[possibly-missing-attribute] Attribute `run_sync_soon` on type `TrioToken | None` may be missing
- src/trio/_core/_tests/test_run.py:1755:16: warning[possibly-missing-attribute] Attribute `parent_task` on type `Nursery | None` may be missing
- src/trio/_core/_tests/test_run.py:2491:17: error[invalid-argument-type] Argument to function `reattach_detached_coroutine_object` is incorrect: Expected `Task`, found `Task | None`
- src/trio/_subprocess.py:783:16: error[invalid-return-type] Return type does not match returned value: expected `CompletedProcess[bytes]`, found `CompletedProcess[bytes | None]`
- Found 656 diagnostics
+ Found 647 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/computation.py:149:12: error[invalid-return-type] Return type does not match returned value: expected `T_DataArray@cov`, found `T_DataArray@cov | None`
- xarray/computation/computation.py:252:12: error[invalid-return-type] Return type does not match returned value: expected `T_DataArray@corr`, found `T_DataArray@corr | None`
- xarray/tests/test_computation.py:1816:9: error[unsupported-operator] Operator `+` is unsupported between objects of type `DataArray | None` and `DataArray | None`
- xarray/tests/test_computation.py:1820:9: error[unsupported-operator] Operator `+` is unsupported between objects of type `DataArray | None` and `DataArray | None`
- Found 1615 diagnostics
+ Found 1611 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-redis/prefect_redis/lease_storage.py:143:16: error[invalid-return-type] Return type does not match returned value: expected `ResourceLease[ConcurrencyLimitLeaseMetadata]`, found `ResourceLease[None | ConcurrencyLimitLeaseMetadata]`
- src/integrations/prefect-redis/prefect_redis/lease_storage.py:198:20: error[invalid-return-type] Return type does not match returned value: expected `ResourceLease[ConcurrencyLimitLeaseMetadata]`, found `ResourceLease[ConcurrencyLimitLeaseMetadata | None]`
- src/prefect/server/concurrency/lease_storage/filesystem.py:120:16: error[invalid-return-type] Return type does not match returned value: expected `ResourceLease[ConcurrencyLimitLeaseMetadata]`, found `ResourceLease[None | ConcurrencyLimitLeaseMetadata]`
- src/prefect/server/concurrency/lease_storage/filesystem.py:143:16: error[invalid-return-type] Return type does not match returned value: expected `ResourceLease[ConcurrencyLimitLeaseMetadata]`, found `ResourceLease[ConcurrencyLimitLeaseMetadata | None]`
- src/prefect/server/concurrency/lease_storage/memory.py:49:16: error[invalid-return-type] Return type does not match returned value: expected `ResourceLease[ConcurrencyLimitLeaseMetadata]`, found `ResourceLease[ConcurrencyLimitLeaseMetadata | None]`
- Found 3203 diagnostics
+ Found 3198 diagnostics

jax (https://github.com/google/jax)
- jax/_src/shard_map.py:499:54: warning[possibly-missing-attribute] Attribute `str_short` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:500:27: warning[possibly-missing-attribute] Attribute `ndim` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:500:44: warning[possibly-missing-attribute] Attribute `ndim` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:511:14: warning[possibly-missing-attribute] Attribute `ndim` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:538:10: warning[possibly-missing-attribute] Attribute `shape` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:543:50: warning[possibly-missing-attribute] Attribute `str_short` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:545:51: warning[possibly-missing-attribute] Attribute `shape` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:547:16: warning[possibly-missing-attribute] Attribute `shape` on type `ShapedArray | NoFail` may be missing
- jax/_src/shard_map.py:573:44: error[unsupported-operator] Operator `in` is not supported for types `Hashable` and `NoFail`, in comparing `Hashable` with `set[Unknown] | NoFail`
- jax/_src/tree_util.py:456:22: error[invalid-argument-type] Argument is incorrect: Expected `T@_parallel_reduce`, found `T@_parallel_reduce | Unspecified`
- jax/_src/tree_util.py:456:25: error[invalid-argument-type] Argument is incorrect: Expected `T@_parallel_reduce`, found `T@_parallel_reduce | Unspecified`
- jax/_src/tree_util.py:469:10: error[invalid-return-type] Return type does not match returned value: expected `T@tree_reduce_associative`, found `T@tree_reduce_associative | Unspecified`
- jax/_src/tree_util.py:469:37: error[invalid-argument-type] Argument to function `_parallel_reduce` is incorrect: Expected `(T@tree_reduce_associative | Unspecified, T@tree_reduce_associative | Unspecified, /) -> T@tree_reduce_associative | Unspecified`, found `(T@tree_reduce_associative, T@tree_reduce_associative, /) -> T@tree_reduce_associative`
- Found 2460 diagnostics
+ Found 2447 diagnostics

colour (https://github.com/colour-science/colour)
- colour/characterisation/aces_it.py:239:8: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/characterisation/aces_it.py:240:33: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Argument type `SpectralDistribution | None | Any` does not satisfy upper bound `SpectralDistribution` of type variable `TypeSpectralDistribution`
- colour/characterisation/aces_it.py:243:11: warning[possibly-missing-attribute] Attribute `values` on type `SpectralDistribution | None | Any` may be missing
- colour/characterisation/aces_it.py:273:17: warning[possibly-missing-attribute] Attribute `name` on type `SpectralDistribution | None | Any` may be missing
- colour/characterisation/aces_it.py:1114:8: warning[possibly-missing-attribute] Attribute `shape` on type `MultiSpectralDistributions | None` may be missing
- colour/characterisation/aces_it.py:1116:26: warning[possibly-missing-attribute] Attribute `name` on type `MultiSpectralDistributions | None` may be missing
- colour/characterisation/aces_it.py:1118:38: error[invalid-argument-type] Argument to function `reshape_msds` is incorrect: Argument type `MultiSpectralDistributions | None` does not satisfy upper bound `MultiSpectralDistributions` of type variable `TypeMultiSpectralDistributions`
- colour/characterisation/aces_it.py:1122:43: error[invalid-argument-type] Argument to function `training_data_sds_to_RGB` is incorrect: Expected `MultiSpectralDistributions`, found `MultiSpectralDistributions | None | Unknown`
- colour/characterisation/aces_it.py:1125:9: error[invalid-argument-type] Argument to function `training_data_sds_to_XYZ` is incorrect: Expected `MultiSpectralDistributions`, found `MultiSpectralDistributions | None | Unknown`
- colour/colorimetry/lefs.py:116:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/colorimetry/lefs.py:116:53: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/colorimetry/lefs.py:577:14: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/lefs.py:577:40: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/lefs.py:578:14: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/lefs.py:578:38: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/lefs.py:579:14: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/lefs.py:579:43: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/photometry.py:77:9: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Argument type `SpectralDistribution | None | Any` does not satisfy upper bound `SpectralDistribution` of type variable `TypeSpectralDistribution`
- colour/colorimetry/photometry.py:128:9: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Argument type `SpectralDistribution | None | Any` does not satisfy upper bound `SpectralDistribution` of type variable `TypeSpectralDistribution`
- colour/colorimetry/spectrum.py:540:13: error[call-non-callable] Object of type `None` is not callable
- colour/colorimetry/spectrum.py:541:13: error[call-non-callable] Object of type `None` is not callable
- colour/colorimetry/spectrum.py:542:13: error[call-non-callable] Object of type `None` is not callable
- colour/colorimetry/tristimulus_values.py:194:57: warning[possibly-missing-attribute] Attribute `shape` on type `MultiSpectralDistributions | None | Any` may be missing
- colour/colorimetry/tristimulus_values.py:197:8: warning[possibly-missing-attribute] Attribute `shape` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/tristimulus_values.py:197:28: warning[possibly-missing-attribute] Attribute `shape` on type `MultiSpectralDistributions | None | Any` may be missing
- colour/colorimetry/tristimulus_values.py:199:26: warning[possibly-missing-attribute] Attribute `name` on type `SpectralDistribution | None | Any` may be missing
- colour/colorimetry/tristimulus_values.py:199:66: warning[possibly-missing-attribute] Attribute `name` on type `MultiSpectralDistributions | None | Any` may be missing
- colour/colorimetry/tristimulus_values.py:203:33: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Argument type `SpectralDistribution | None | Any` does not satisfy upper bound `SpectralDistribution` of type variable `TypeSpectralDistribution`
- colour/colorimetry/tristimulus_values.py:203:45: warning[possibly-missing-attribute] Attribute `shape` on type `MultiSpectralDistributions | None | Any` may be missing
- colour/colorimetry/tristimulus_values.py:205:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[MultiSpectralDistributions, SpectralDistribution]`, found `tuple[MultiSpectralDistributions | None | Any, SpectralDistribution | None | Any]`
- colour/models/cam16_ucs.py:112:17: warning[possibly-missing-attribute] Attribute `replace` on type `str | None` may be missing
- colour/models/jzazbz.py:248:15: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/jzazbz.py:248:38: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/jzazbz.py:249:15: warning[possibly-missing-attribute] Attribute `g` on type `Structure | None` may be missing
- colour/models/jzazbz.py:249:38: warning[possibly-missing-attribute] Attribute `g` on type `Structure | None` may be missing
- colour/models/jzazbz.py:262:27: warning[possibly-missing-attribute] Attribute `d_0` on type `Structure | None` may be missing
- colour/models/jzazbz.py:350:27: warning[possibly-missing-attribute] Attribute `d_0` on type `Structure | None` may be missing
- colour/models/jzazbz.py:358:25: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/jzazbz.py:358:55: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/jzazbz.py:359:25: warning[possibly-missing-attribute] Attribute `g` on type `Structure | None` may be missing
- colour/models/jzazbz.py:359:53: warning[possibly-missing-attribute] Attribute `g` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/aces.py:182:14: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:183:14: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:184:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:185:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:186:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:268:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:269:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:270:22: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/models/rgb/transfer_functions/aces.py:443:20: warning[possibly-missing-attribute] Attribute `X_BRK` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/aces.py:444:9: warning[possibly-missing-attribute] Attribute `A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/aces.py:444:33: warning[possibly-missing-attribute] Attribute `B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/aces.py:500:19: warning[possibly-missing-attribute] Attribute `Y_BRK` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/aces.py:502:20: warning[possibly-missing-attribute] Attribute `B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/aces.py:502:35: warning[possibly-missing-attribute] Attribute `A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:110:11: warning[possibly-missing-attribute] Attribute `R_0` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:111:11: warning[possibly-missing-attribute] Attribute `R_t` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:112:13: warning[possibly-missing-attribute] Attribute `sigma` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:113:12: warning[possibly-missing-attribute] Attribute `beta` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:114:13: warning[possibly-missing-attribute] Attribute `gamma` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:115:13: warning[possibly-missing-attribute] Attribute `delta` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:184:11: warning[possibly-missing-attribute] Attribute `R_0` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:185:11: warning[possibly-missing-attribute] Attribute `R_t` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:186:13: warning[possibly-missing-attribute] Attribute `sigma` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:187:12: warning[possibly-missing-attribute] Attribute `beta` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:188:13: warning[possibly-missing-attribute] Attribute `gamma` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/apple_log_profile.py:189:13: warning[possibly-missing-attribute] Attribute `delta` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arib_std_b67.py:114:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arib_std_b67.py:115:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arib_std_b67.py:116:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arib_std_b67.py:179:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arib_std_b67.py:180:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arib_std_b67.py:181:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:755:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:756:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:757:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:758:9: warning[possibly-missing-attribute] Attribute `s` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:759:9: warning[possibly-missing-attribute] Attribute `t` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:817:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:818:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:819:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:820:9: warning[possibly-missing-attribute] Attribute `s` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/arri.py:821:9: warning[possibly-missing-attribute] Attribute `t` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:99:9: warning[possibly-missing-attribute] Attribute `A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:100:9: warning[possibly-missing-attribute] Attribute `B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:101:9: warning[possibly-missing-attribute] Attribute `C` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:102:9: warning[possibly-missing-attribute] Attribute `D` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:103:9: warning[possibly-missing-attribute] Attribute `E` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:104:15: warning[possibly-missing-attribute] Attribute `LIN_CUT` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:163:9: warning[possibly-missing-attribute] Attribute `A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:164:9: warning[possibly-missing-attribute] Attribute `B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:165:9: warning[possibly-missing-attribute] Attribute `C` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:166:9: warning[possibly-missing-attribute] Attribute `D` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:167:9: warning[possibly-missing-attribute] Attribute `E` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/blackmagic_design.py:168:15: warning[possibly-missing-attribute] Attribute `LIN_CUT` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:100:18: warning[possibly-missing-attribute] Attribute `DI_LIN_CUT` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:101:12: warning[possibly-missing-attribute] Attribute `DI_A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:102:12: warning[possibly-missing-attribute] Attribute `DI_B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:103:12: warning[possibly-missing-attribute] Attribute `DI_C` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:104:12: warning[possibly-missing-attribute] Attribute `DI_M` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:163:18: warning[possibly-missing-attribute] Attribute `DI_LOG_CUT` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:164:12: warning[possibly-missing-attribute] Attribute `DI_A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:165:12: warning[possibly-missing-attribute] Attribute `DI_B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:166:12: warning[possibly-missing-attribute] Attribute `DI_C` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/davinci_intermediate.py:167:12: warning[possibly-missing-attribute] Attribute `DI_M` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:136:9: warning[possibly-missing-attribute] Attribute `A` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:137:9: warning[possibly-missing-attribute] Attribute `B` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:138:9: warning[possibly-missing-attribute] Attribute `C` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:139:9: warning[possibly-missing-attribute] Attribute `D` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:140:9: warning[possibly-missing-attribute] Attribute `E` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:141:9: warning[possibly-missing-attribute] Attribute `F` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:142:9: warning[possibly-missing-attribute] Attribute `G` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:143:9: warning[possibly-missing-attribute] Attribute `H` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:144:9: warning[possibly-missing-attribute] Attribute `I` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:220:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:221:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:222:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:223:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:224:9: warning[possibly-missing-attribute] Attribute `e` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:225:9: warning[possibly-missing-attribute] Attribute `f` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:226:9: warning[possibly-missing-attribute] Attribute `g` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:227:9: warning[possibly-missing-attribute] Attribute `h` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:228:9: warning[possibly-missing-attribute] Attribute `k` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/dicom_gsdf.py:229:9: warning[possibly-missing-attribute] Attribute `m` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:141:12: warning[possibly-missing-attribute] Attribute `cut1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:142:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:143:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:144:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:145:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:146:9: warning[possibly-missing-attribute] Attribute `e` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:147:9: warning[possibly-missing-attribute] Attribute `f` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:220:12: warning[possibly-missing-attribute] Attribute `cut2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:221:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:222:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:223:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:224:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:225:9: warning[possibly-missing-attribute] Attribute `e` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/fujifilm_f_log.py:226:9: warning[possibly-missing-attribute] Attribute `f` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/itur_bt_2020.py:128:9: warning[possibly-missing-attribute] Attribute `alpha` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/itur_bt_2020.py:129:9: warning[possibly-missing-attribute] Attribute `beta` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/itur_bt_2020.py:189:9: warning[possibly-missing-attribute] Attribute `alpha` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/itur_bt_2020.py:190:9: warning[possibly-missing-attribute] Attribute `beta` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/itur_bt_2100.py:591:42: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `int | float | None`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1141:41: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float | None` and `Literal[1]`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1142:41: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float | None` and `Literal[1]`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1143:41: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float | None` and `Literal[1]`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1228:41: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float | None` and `Literal[1]`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1229:41: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float | None` and `Literal[1]`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1230:41: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float | None` and `Literal[1]`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1410:50: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[1]` and `int | float | None`
- colour/models/rgb/transfer_functions/itur_bt_2100.py:1512:42: error[unsupported-operator] Operator `-` is unsupported between objects of type `Literal[1]` and `int | float | None`
- colour/models/rgb/transfer_functions/leica_l_log.py:116:12: warning[possibly-missing-attribute] Attribute `cut1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:117:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:118:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:119:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:120:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:121:9: warning[possibly-missing-attribute] Attribute `e` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:122:9: warning[possibly-missing-attribute] Attribute `f` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:194:12: warning[possibly-missing-attribute] Attribute `cut2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:195:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:196:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:197:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:198:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:199:9: warning[possibly-missing-attribute] Attribute `e` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/leica_l_log.py:200:9: warning[possibly-missing-attribute] Attribute `f` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:116:12: warning[possibly-missing-attribute] Attribute `cut1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:117:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:118:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:119:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:120:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:193:12: warning[possibly-missing-attribute] Attribute `cut2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:194:9: warning[possibly-missing-attribute] Attribute `a` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:195:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:196:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/nikon_n_log.py:197:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:122:12: warning[possibly-missing-attribute] Attribute `cut1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:123:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:124:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:125:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:198:12: warning[possibly-missing-attribute] Attribute `cut2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:199:9: warning[possibly-missing-attribute] Attribute `b` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:200:9: warning[possibly-missing-attribute] Attribute `c` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/panasonic_v_log.py:201:9: warning[possibly-missing-attribute] Attribute `d` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:129:11: warning[possibly-missing-attribute] Attribute `c_1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:130:11: warning[possibly-missing-attribute] Attribute `c_2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:131:11: warning[possibly-missing-attribute] Attribute `c_3` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:132:11: warning[possibly-missing-attribute] Attribute `m_1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:133:11: warning[possibly-missing-attribute] Attribute `m_2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:207:11: warning[possibly-missing-attribute] Attribute `c_1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:208:11: warning[possibly-missing-attribute] Attribute `c_2` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:209:11: warning[possibly-missing-attribute] Attribute `c_3` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:210:11: warning[possibly-missing-attribute] Attribute `m_1` on type `Structure | None` may be missing
- colour/models/rgb/transfer_functions/st_2084.py:211:11: warning[possibly-missing-attribute] Attribute `m_2` on type `Structure | None` may be missing
- colour/plotting/colorimetry.py:1141:24: error[not-iterable] Object of type `SpectralShape | None` may not be iterable
- colour/plotting/colorimetry.py:1157:15: warning[possibly-missing-attribute] Attribute `interval` on type `SpectralShape | None` may be missing
- colour/plotting/colorimetry.py:1157:43: warning[possibly-missing-attribute] Attribute `interval` on type `SpectralShape | None` may be missing
- colour/plotting/common.py:1353:12: error[unsupported-operator] Operator `%` is unsupported between objects of type `int` and `int | None`
- colour/plotting/diagrams.py:474:51: error[not-iterable] Object of type `Sequence[Unknown] | None | Any` may not be iterable
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `str | Colormap | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `str | Colormap | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `str | Normalize | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `str | Normalize | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | Sequence[int | float] | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `int | float | Sequence[int | float] | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `Colorizer | None`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `Colorizer | None`, found `Unknown | int | float | str`
- colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `bool`, found `Unknown | int | float | str | None`
+ colour/plotting/models.py:442:9: error[invalid-argument-type] Argument to bound method `scatter` is incorrect: Expected `bool`, found `Unknown | int | float | str`
- colour/plotting/models.py:1979:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- colour/plotting/section.py:202:54: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/section.py:243:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/section.py:362:54: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/section.py:388:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/tm3018/report.py:347:40: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:358:40: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:372:47: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:383:47: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:425:30: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `str`, found `T@optional | None | str`
- colour/plotting/tm3018/report.py:527:36: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/tm3018/report.py:622:36: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/tm3018/report.py:706:36: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/plotting/volume.py:525:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- colour/plotting/volume.py:600:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- colour/plotting/volume.py:787:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- colour/recovery/jakob2019.py:678:36: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | None`
- colour/recovery/jiang2013.py:284:45: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Expected `SpectralShape`, found `SpectralShape | None`
- colour/recovery/jiang2013.py:290:51: error[invalid-argument-type] Argument to function `reshape_msds` is incorrect: Expected `SpectralShape`, found `SpectralShape | None`
- colour/recovery/jiang2013.py:300:36: warning[possibly-missing-attribute] Attribute `wavelengths` on type `SpectralShape | None` may be missing
- colour/recovery/jiang2013.py:413:45: error[invalid-argument-type] Argument to function `reshape_sd` is incorrect: Expected `SpectralShape`, found `SpectralShape | None`
- colour/recovery/jiang2013.py:419:51: error[invalid-argument-type] Argument to function `reshape_msds` is incorrect: Expected `SpectralShape`, found `SpectralShape | None`
- colour/recovery/otsu2018.py:1597:21: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/recovery/otsu2018.py:1597:53: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/recovery/otsu2018.py:1601:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/recovery/otsu2018.py:1601:54: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/recovery/otsu2018.py:1602:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- colour/temperature/ohno2013.py:218:39: error[invalid-argument-type] Argument to function `planckian_table` is incorrect: Expected `int | float`, found `int | float | None`
- colour/temperature/ohno2013.py:218:46: error[invalid-argument-type] Argument to function `planckian_table` is incorrect: Expected `int | float`, found `int | float | None`
- colour/temperature/ohno2013.py:218:51: error[invalid-argument-type] Argument to function `planckian_table` is incorrect: Expected `int | float`, found `int | float | None`
- colour/utilities/array.py:646:12: error[call-non-callable] Object of type `None` is not callable
- colour/utilities/array.py:708:12: error[call-non-callable] Object of type `None` is not callable
- Found 706 diagnostics
+ Found 476 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/runtime/fragment.py:437:12: error[invalid-return-type] Return type does not match returned value: expected `((F@fragment, /) -> F@fragment) | F@fragment`, found `((F@fragment | None, /) -> F@fragment | None) | F@fragment | None`
- Found 195 diagnostics
+ Found 194 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/bigquery/__init__.py:1019:16: error[no-matching-overload] No overload of bound method `join` matches arguments
- ibis/backends/snowflake/tests/test_client.py:409:16: error[no-matching-overload] No overload of bound method `join` matches arguments
- ibis/expr/types/joins.py:321:31: error[invalid-argument-type] Argument to bound method `asof_join` is incorrect: Expected `Sequence[str | Column] | Column`, found `list[Unknown | tuple[()]]`
- ibis/expr/types/relations.py:560:16: error[no-matching-overload] No overload of bound method `join` matches arguments
- ibis/expr/types/relations.py:4857:37: error[invalid-argument-type] Argument to function `_to_selector` is incorrect: Expected `Sequence[str | Selector | Column] | Selector | Column`, found `list[Iterable[str] | Selector]`
- Found 3288 diagnostics
+ Found 3283 diagnostics

zulip (https://github.com/zulip/zulip)
- corporate/lib/stripe.py:5342:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `int | float`, found `int | None`
- zerver/lib/test_classes.py:2746:13: error[invalid-assignment] Object of type `dict[str | None, list[str | None]]` is not assignable to `dict[str, list[str]]`
- zerver/lib/typed_endpoint.py:271:12: error[invalid-return-type] Return type does not match returned value: expected `FuncParam[T@parse_single_parameter]`, found `FuncParam[(Any & ~<class '_empty'>) | _NotSpecified]`
+ zerver/lib/typed_endpoint.py:271:12: error[invalid-return-type] Return type does not match returned value: expected `FuncParam[T@parse_single_parameter]`, found `FuncParam[Any & ~<class '_empty'>]`
- zerver/tests/test_bots.py:624:42: error[invalid-argument-type] Argument to function `do_change_default_sending_stream` is incorrect: Expected `UserProfile`, found `Unknown | None`
- zerver/tests/test_bots.py:625:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `bot_owner_id` on type `Unknown | None`
- zerver/tests/test_bots.py:626:9: warning[possibly-missing-attribute] Attribute `save` on type `Unknown | None` may be missing
- zerver/tests/test_bots.py:628:51: warning[possibly-missing-attribute] Attribute `id` on type `Unknown | None` may be missing
- zerver/tests/test_bots.py:2041:42: error[invalid-argument-type] Argument to function `do_change_default_sending_stream` is incorrect: Expected `UserProfile`, found `Unknown | None`
- zerver/tests/test_bots.py:2042:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `bot_owner_id` on type `Unknown | None`
- zerver/tests/test_bots.py:2043:9: warning[possibly-missing-attribute] Attribute `save` on type `Unknown | None` may be missing
- zerver/tests/test_bots.py:2045:45: error[invalid-argument-type] Argument to function `bot_owner_user_ids` is incorrect: Expected `UserProfile`, found `Unknown | None`
- zerver/views/auth.py:125:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | AnyStr@urljoin | None`
- Found 2721 diagnostics
+ Found 2710 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/matrices/matrixbase.py:5501:16: error[invalid-return-type] Return type does not match returned value: expected `Self@pinv_solve`, found `Unknown | None`
- Found 14014 diagnostics
+ Found 14013 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:97:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/history/events/structures/evm_event.py:204:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(str | None, /) -> Any`, found `def string_to_evm_address(value: str) -> @Todo`
- rotkehlchen/history/events/structures/evm_swap.py:103:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(str | None, /) -> Any`, found `def string_to_evm_address(value: str) -> @Todo`
- Found 1705 diagnostics
+ Found 1704 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/core.py:1546:48: error[invalid-argument-type] Argument to function `_event_repr` is incorrect: Expected `EventType[_DataT@async_fire_internal | None] | str`, found `EventType[_DataT@async_fire_internal] | str`
- homeassistant/core.py:1566:17: error[invalid-assignment] Object of type `Event[Mapping[str, Any]]` is not assignable to `Event[_DataT@async_fire_internal] | None`
- homeassistant/core.py:1566:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Event[_DataT@Event]`, found `Event[_DataT@async_fire_internal | None]`
- homeassistant/core.py:1567:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `EventType[_DataT@async_fire_internal | None] | str`, found `EventType[_DataT@async_fire_internal] | str`
- homeassistant/helpers/entity_registry.py:1048:13: error[invalid-argument-type] Argument is incorrect: Expected `Mapping[str, Any] | None`, found `Mapping[str, Any] | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1049:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `UndefinedType | None | str`
- homeassistant/helpers/entity_registry.py:1050:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1053:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1055:13: error[invalid-argument-type] Argument is incorrect: Expected `EntityCategory | None`, found `EntityCategory | UndefinedType | None`
- homeassistant/helpers/entity_registry.py:1058:13: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `bool | (UndefinedType & ~AlwaysFalsy)`
- homeassistant/helpers/entity_registry.py:1064:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1065:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1066:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1069:13: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `(int & ~AlwaysFalsy) | (UndefinedType & ~AlwaysFalsy) | Literal[0]`
- homeassistant/helpers/entity_registry.py:1070:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- homeassistant/helpers/entity_registry.py:1072:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | None | UndefinedType`
- Found 13740 diagnostics
+ Found 13724 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-07 15:38_

---

_Comment by @github-actions[bot] on 2025-10-07 15:43_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 0 | 182 | 0 |
| `invalid-argument-type` | 7 | 67 | 8 |
| `invalid-return-type` | 0 | 17 | 1 |
| `non-subscriptable` | 0 | 15 | 0 |
| `unsupported-operator` | 0 | 13 | 0 |
| `no-matching-overload` | 0 | 10 | 0 |
| `call-non-callable` | 0 | 5 | 0 |
| `invalid-assignment` | 0 | 4 | 0 |
| `not-iterable` | 0 | 2 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **8** | **315** | **9** |

**[Full report with detailed diff](https://dcreager-union-none.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-union-none.ecosystem-663.pages.dev/timing))


---

_Comment by @dcreager on 2025-10-07 16:44_

Lots of false positives removed in the ecosystem report. The only additions involve splatting a `**kwargs` argument, which is covered by https://github.com/astral-sh/ty/issues/1248.

---

_Marked ready for review by @dcreager on 2025-10-07 16:44_

---

_Review requested from @carljm by @dcreager on 2025-10-07 16:44_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-07 16:44_

---

_Review requested from @sharkdp by @dcreager on 2025-10-07 16:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1182 on 2025-10-07 16:58_

slightly simpler might be to do this? it would also mean you wouldn't need the `UnionBuilder` import up at the top

```diff
diff --git a/crates/ty_python_semantic/src/types/generics.rs b/crates/ty_python_semantic/src/types/generics.rs
index 14388f0518..e53bd41333 100644
--- a/crates/ty_python_semantic/src/types/generics.rs
+++ b/crates/ty_python_semantic/src/types/generics.rs
@@ -1170,14 +1170,11 @@ impl<'db> SpecializationBuilder<'db> {
                 if (actual_union.elements(self.db).iter()).any(|ty| ty.is_type_var()) {
                     return Ok(());
                 }
-                let actual_non_subtypes = (actual_union.elements(self.db).iter().copied())
-                    .filter(|ty| !ty.is_subtype_of(self.db, formal))
-                    .fold(UnionBuilder::new(self.db), |builder, element| {
-                        builder.add(element)
-                    });
-                let Some(remaining_actual) = actual_non_subtypes.try_build() else {
+                let remaining_actual =
+                    actual_union.filter(self.db, |ty| !ty.is_subtype_of(self.db, formal));
+                if remaining_actual.is_never() {
                     return Ok(());
-                };
+                }
                 self.add_type_mapping(formal_bound_typevar, remaining_actual);
             }
```

---

_@AlexWaygood approved on 2025-10-07 16:58_

Nice!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:1182 on 2025-10-07 17:02_

Yeah I had thought about that, but was feeling mildly icky about using `Never` in that way. I guess for unions it's okay, since that really is equivalent to "no elements in the union".

---

_@dcreager reviewed on 2025-10-07 17:02_

---

_@AlexWaygood reviewed on 2025-10-07 17:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1182 on 2025-10-07 17:04_

I don't have a strong opinion FWIW, but your current spelling _did_ make me do a "Huh, don't we have a `UnionType::filter()` method yet... oh! We do" 😄

---

_Merged by @dcreager on 2025-10-07 17:33_

---

_Closed by @dcreager on 2025-10-07 17:33_

---

_Branch deleted on 2025-10-07 17:33_

---
