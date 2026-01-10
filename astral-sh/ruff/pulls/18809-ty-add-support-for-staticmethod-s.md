```yaml
number: 18809
title: "[ty] Add support for `@staticmethod`s"
type: pull_request
state: merged
author: med1844
labels:
  - ty
assignees: []
merged: true
base: main
head: staticmethod
created_at: 2025-06-20T01:17:19Z
updated_at: 2025-06-20T08:40:36Z
url: https://github.com/astral-sh/ruff/pull/18809
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Add support for `@staticmethod`s

---

_Pull request opened by @med1844 on 2025-06-20 01:17_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add support for `@staticmethod`s. Overall, the changes are very similar to #16305.

#18587 will be dependent on this PR for a potential fix of https://github.com/astral-sh/ty/issues/207.

mypy_primer will look bad since the new code allows ty to check more code.

## Test Plan

<!-- How was it tested? -->

Added new markdown tests. Please comment if there's any missing tests that I should add in, thank you.

---

_Review requested from @carljm by @med1844 on 2025-06-20 01:17_

---

_Review requested from @AlexWaygood by @med1844 on 2025-06-20 01:17_

---

_Review requested from @sharkdp by @med1844 on 2025-06-20 01:17_

---

_Review requested from @dcreager by @med1844 on 2025-06-20 01:17_

---

_Comment by @github-actions[bot] on 2025-06-20 01:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
+ error[invalid-argument-type] tests/test_array.py:322:49: Argument to function `unfold` is incorrect: Expected `(Literal[0], /) -> Option[tuple[Unknown, Literal[0]]]`, found `def unfolder(state: int) -> Option[tuple[int, int]]`
+ error[invalid-argument-type] tests/test_block.py:289:39: Argument to function `unfold` is incorrect: Expected `(Literal[0], /) -> Option[tuple[Unknown, Literal[0]]]`, found `def unfolder(state: int) -> Option[tuple[int, int]]`
- error[invalid-type-form] tests/test_tagged_union.py:331:23: Variable of type `staticmethod` is not allowed in a type expression
+ error[invalid-type-form] tests/test_tagged_union.py:331:23: Variable of type `def Face(suit: Suit, face: Unknown) -> Card` is not allowed in a type expression
- error[invalid-type-form] tests/test_tagged_union.py:336:32: Variable of type `staticmethod` is not allowed in a type expression
+ error[invalid-type-form] tests/test_tagged_union.py:336:32: Variable of type `def Face(suit: Suit, face: Unknown) -> Card` is not allowed in a type expression
- Found 252 diagnostics
+ Found 254 diagnostics

alerta (https://github.com/alerta/alerta)
+ warning[possibly-unbound-attribute] alerta/auth/decorators.py:114:29: Attribute `id` on type `User | None` is possibly unbound
+ warning[possibly-unbound-attribute] alerta/auth/decorators.py:115:27: Attribute `login` on type `User | None` is possibly unbound
+ warning[possibly-unbound-attribute] alerta/auth/decorators.py:116:45: Attribute `login` on type `User | None` is possibly unbound
+ warning[possibly-unbound-attribute] alerta/auth/decorators.py:116:64: Attribute `get_groups` on type `User | None` is possibly unbound
+ warning[possibly-unbound-attribute] alerta/auth/decorators.py:117:46: Attribute `login` on type `User | None` is possibly unbound
- error[invalid-argument-type] alerta/models/heartbeat.py:88:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/heartbeat.py:88:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `datetime | None`
- error[invalid-argument-type] alerta/models/key.py:54:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `Unknown | None`
+ error[invalid-argument-type] alerta/models/key.py:54:13: Argument to bound method `__init__` is incorrect: Expected `datetime`, found `datetime | None`
+ warning[possibly-unbound-attribute] alerta/views/customers.py:47:72: Attribute `customer` on type `Customer | None` is possibly unbound
+ warning[possibly-unbound-attribute] alerta/views/customers.py:48:55: Attribute `serialize` on type `Customer | None` is possibly unbound
+ error[invalid-assignment] tests/plugins/test_reject.py:27:5: Implicit shadowing of function `get_config`
- Found 477 diagnostics
+ Found 485 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ error[unresolved-attribute] sockeye/knn.py:171:9: Unresolved attribute `index_type` on type `Config`.
+ error[unresolved-attribute] sockeye/knn.py:173:9: Unresolved attribute `train_data_size` on type `Config`.
+ error[unresolved-attribute] sockeye/knn.py:175:50: Type `Config` has no attribute `state_data_type`
+ error[unresolved-attribute] sockeye/knn.py:176:39: Type `Config` has no attribute `index_size`
+ error[unresolved-attribute] sockeye/knn.py:176:58: Type `Config` has no attribute `dimension`
+ error[invalid-argument-type] sockeye/knn.py:177:33: Argument to bound method `__init__` is incorrect: Expected `KNNConfig`, found `Config`
+ error[unresolved-attribute] sockeye/knn.py:180:68: Type `Config` has no attribute `state_data_type`
+ error[unresolved-attribute] sockeye/knn.py:181:51: Type `Config` has no attribute `index_size`
+ error[unresolved-attribute] sockeye/knn.py:181:70: Type `Config` has no attribute `dimension`
+ error[unresolved-attribute] test/unit/test_config.py:66:16: Type `Config` has no attribute `param`
+ error[unresolved-attribute] test/unit/test_config.py:67:16: Type `Config` has no attribute `config`
+ error[unresolved-attribute] test/unit/test_config.py:96:12: Type `Config` has no attribute `new_attribute`
+ error[missing-argument] test/unit/test_inference.py:447:24: No arguments provided for required parameters `sequence`, `length`, `seq_scores`, `estimated_reference_length` of function `_assemble_translation`
+ error[unresolved-attribute] test/unit/test_knn.py:126:16: Type `Config` has no attribute `index_size`
+ error[unresolved-attribute] test/unit/test_knn.py:127:16: Type `Config` has no attribute `dimension`
+ error[unresolved-attribute] test/unit/test_knn.py:128:16: Type `Config` has no attribute `state_data_type`
+ error[unresolved-attribute] test/unit/test_knn.py:129:16: Type `Config` has no attribute `word_data_type`
+ error[unresolved-attribute] test/unit/test_knn.py:130:16: Type `Config` has no attribute `index_type`
+ error[unresolved-attribute] test/unit/test_knn.py:131:16: Type `Config` has no attribute `train_data_size`
- Found 346 diagnostics
+ Found 365 diagnostics

isort (https://github.com/pycqa/isort)
+ error[invalid-argument-type] isort/io.py:45:55: Argument to function `detect_encoding` is incorrect: Expected `() -> bytes`, found `bound method TextIOWrapper[_WrappedBuffer].readline(size: int = Literal[-1], /) -> str`
- Found 46 diagnostics
+ Found 47 diagnostics

optuna (https://github.com/optuna/optuna)
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:53:60: Argument to bound method `where_study_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:301:55: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:316:56: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:321:56: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:357:13: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:376:68: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:381:68: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:399:62: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:408:51: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
+ error[invalid-argument-type] tests/storages_tests/rdb_tests/test_models.py:413:51: Argument to bound method `where_trial_id` is incorrect: Expected `int`, found `Unknown | Column[Unknown]`
- error[unresolved-attribute] tests/test_experimental.py:74:39: Type `(...) -> Unknown` has no attribute `__name__`
- error[unresolved-attribute] tests/test_experimental.py:83:12: Type `(...) -> Unknown` has no attribute `__qualname__`
- Found 1005 diagnostics
+ Found 1013 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[missing-argument] src/hydra_zen/wrapper/_implementations.py:2111:13: No arguments provided for required parameters `name`, `node` of bound method `store`
- Found 598 diagnostics
+ Found 599 diagnostics

ignite (https://github.com/pytorch/ignite)
+ error[invalid-argument-type] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:77:24: Argument to function `print_results` is incorrect: Expected `list[list[str | int | float]]`, found `list[list[str | int | float | tuple[str | int | float, str | int | float]]]`
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_base.py:103:46: Attribute `dtype` on type `Unknown | int | float | str` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_base.py:103:75: Attribute `shape` on type `Unknown | int | float | str` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_base.py:112:46: Attribute `dtype` on type `Unknown | int | float | str` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_base.py:112:71: Attribute `shape` on type `Unknown | int | float | str` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_base.py:117:46: Attribute `dtype` on type `Unknown | int | float | str` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_base.py:117:71: Attribute `shape` on type `Unknown | int | float | str` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_horovod.py:97:12: Attribute `backend` on type `_HorovodDistModel | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_horovod.py:122:12: Attribute `backend` on type `_HorovodDistModel | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_xla.py:104:12: Attribute `backend` on type `_XlaDistModel | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_xla.py:125:12: Attribute `backend` on type `_XlaDistModel | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_xla.py:163:12: Attribute `device` on type `_XlaDistModel | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/comp_models/test_xla.py:184:63: Attribute `device` on type `_XlaDistModel | None` is possibly unbound
+ error[invalid-argument-type] tests/ignite/handlers/test_checkpoint.py:1077:37: Argument to function `load_objects` is incorrect: Expected `str | Mapping[Unknown, Unknown] | Path`, found `list[Unknown]`
+ error[invalid-argument-type] tests/ignite/handlers/test_time_profilers.py:858:40: Argument to function `print_results` is incorrect: Expected `list[list[str | int | float]]`, found `list[list[str | int | float | tuple[str | int | float, str | int | float]]]`
+ error[invalid-argument-type] tests/ignite/metrics/test_confusion_matrix.py:53:41: Argument to function `normalize` is incorrect: Expected `str`, found `None`
- Found 2193 diagnostics
+ Found 2209 diagnostics

vision (https://github.com/pytorch/vision)
+ warning[possibly-unbound-attribute] test/test_datasets_video_utils.py:92:22: Attribute `flatten` on type `list[slice[Any, Any, Any]] | Unknown` is possibly unbound
+ error[invalid-argument-type] test/test_transforms.py:323:53: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:53: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:53: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:53: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:53: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:66: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:66: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:66: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:66: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
+ error[invalid-argument-type] test/test_transforms.py:323:66: Argument to function `get_params` is incorrect: Expected `list[int | float]`, found `tuple[Unknown | float, Unknown]`
- Found 1530 diagnostics
+ Found 1541 diagnostics

pyppeteer (https://github.com/pyppeteer/pyppeteer)
+ error[invalid-argument-type] pyppeteer/launcher.py:171:40: Argument to function `create` is incorrect: Expected `Connection`, found `Connection | None`
- Found 100 diagnostics
+ Found 101 diagnostics

apprise (https://github.com/caronc/apprise)
+ error[unknown-argument] apprise/attachment/http.py:365:45: Argument `sanitize` does not match any known parameter of function `parse_url`
+ error[unresolved-attribute] test/helpers/rest.py:227:13: Unresolved attribute `request_rate_per_sec` on type `NotifyBase`.
+ error[unresolved-attribute] test/helpers/rest.py:231:31: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/helpers/rest.py:234:22: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/helpers/rest.py:238:37: Type `NotifyBase` has no attribute `url_identifier`
+ error[invalid-argument-type] test/helpers/rest.py:241:35: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/helpers/rest.py:245:17: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/helpers/rest.py:254:24: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/helpers/rest.py:258:29: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/helpers/rest.py:263:47: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/helpers/rest.py:267:43: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/helpers/rest.py:270:16: Type `NotifyBase` has no attribute `url_identifier`
+ error[unresolved-attribute] test/helpers/rest.py:270:38: Type `NotifyBase` has no attribute `url_identifier`
+ error[unresolved-attribute] test/helpers/rest.py:274:25: Type `NotifyBase` has no attribute `url_identifier`
+ error[unresolved-attribute] test/helpers/rest.py:274:49: Type `NotifyBase` has no attribute `url_identifier`
+ error[unresolved-attribute] test/helpers/rest.py:277:16: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/helpers/rest.py:277:32: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/helpers/rest.py:281:25: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/helpers/rest.py:281:43: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/helpers/rest.py:291:26: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/helpers/rest.py:295:20: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[invalid-argument-type] test/helpers/rest.py:295:32: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[invalid-argument-type] test/helpers/rest.py:297:25: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/helpers/rest.py:297:31: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/helpers/rest.py:299:25: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/helpers/rest.py:299:35: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/helpers/rest.py:311:37: Argument to function `hasattr` is incorrect: Expected `str`, found `NotifyBase`
+ error[no-matching-overload] test/helpers/rest.py:312:24: No overload of function `getattr` matches arguments
+ error[unresolved-attribute] test/test_api.py:407:12: Type `NotifyBase` has no attribute `socket_connect_timeout`
+ error[unresolved-attribute] test/test_api.py:408:12: Type `NotifyBase` has no attribute `socket_read_timeout`
+ error[unresolved-attribute] test/test_api.py:412:12: Type `NotifyBase` has no attribute `socket_connect_timeout`
+ error[unresolved-attribute] test/test_api.py:413:12: Type `NotifyBase` has no attribute `socket_read_timeout`
+ error[unresolved-attribute] test/test_api.py:430:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:432:26: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_api.py:441:16: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:857:12: Type `NotifyBase` has no attribute `url_identifier`
+ error[unresolved-attribute] test/test_api.py:857:35: Type `NotifyBase` has no attribute `url_identifier`
+ error[unresolved-attribute] test/test_api.py:858:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:858:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:860:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:860:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:862:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:862:39: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:867:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:867:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:870:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:870:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:873:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:873:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:876:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:876:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:879:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:879:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:885:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:885:39: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:890:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:890:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:926:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:931:5: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:934:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:936:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:941:5: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:943:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:946:5: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:947:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:948:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:948:29: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:951:5: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:952:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:955:5: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:956:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:959:5: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:960:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_api.py:965:9: Unresolved attribute `_url_identifier` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_api.py:966:16: Type `NotifyBase` has no attribute `url_id`
- error[unknown-argument] test/test_api.py:1283:38: Argument `show_disabled` does not match any known parameter of bound method `details`
+ error[invalid-argument-type] test/test_apprise_attachments.py:108:64: Argument to function `instantiate` is incorrect: Expected `bool | None`, found `Literal[100]`
+ error[invalid-argument-type] test/test_apprise_attachments.py:187:55: Argument to function `instantiate` is incorrect: Expected `bool | None`, found `Literal["invalid"]`
+ error[invalid-argument-type] test/test_attach_file.py:72:52: Argument to function `instantiate` is incorrect: Expected `bool | None`, found `Literal[30]`
+ error[unresolved-attribute] test/test_logger.py:214:17: Type `NotifyBase` has no attribute `send`
+ error[unresolved-attribute] test/test_plugin_africas_talking.py:146:12: Type `NotifyBase` has no attribute `notify`
+ error[invalid-argument-type] test/test_plugin_africas_talking.py:150:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_africas_talking.py:175:12: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_africas_talking.py:179:26: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_africas_talking.py:192:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_africas_talking.py:194:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_apprise_api.py:224:16: Type `NotifyAppriseAPI` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_apprise_api.py:231:16: Type `NotifyAppriseAPI` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_apprise_api.py:249:20: Type `NotifyAppriseAPI` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_apprise_api.py:255:20: Type `NotifyAppriseAPI` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_apprise_api.py:266:16: Type `NotifyAppriseAPI` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:194:23: Type `NotifyAprs` has no attribute `url_id`
+ error[unresolved-attribute] test/test_plugin_aprs.py:201:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:203:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:205:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:207:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:211:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:215:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:275:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:278:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:281:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:284:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:324:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_aprs.py:328:12: Type `NotifyAprs` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:411:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:443:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:473:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:504:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:544:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:584:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bluesky.py:621:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_bulksms.py:169:12: Type `NotifyBase` has no attribute `notify`
+ error[invalid-argument-type] test/test_plugin_bulksms.py:173:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_bulksms.py:202:12: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_bulksms.py:206:26: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_bulksms.py:212:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_bulkvs.py:155:12: Type `NotifyBase` has no attribute `notify`
+ error[invalid-argument-type] test/test_plugin_bulkvs.py:159:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_bulkvs.py:178:12: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_bulkvs.py:181:26: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_bulkvs.py:187:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:183:12: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:189:12: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:196:12: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:213:16: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:221:16: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:228:12: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:247:12: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:258:12: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:279:16: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_form.py:290:16: Type `NotifyForm` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_json.py:249:12: Type `NotifyJSON` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_json.py:255:12: Type `NotifyJSON` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_json.py:272:16: Type `NotifyJSON` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_json.py:282:12: Type `NotifyJSON` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_xml.py:194:12: Type `NotifyXML` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_xml.py:200:12: Type `NotifyXML` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_xml.py:217:16: Type `NotifyXML` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_custom_xml.py:227:12: Type `NotifyXML` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dapnet.py:165:12: Type `NotifyDapnet` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:171:5: Unresolved attribute `duration` on type `NotifyDBus`.
+ error[unresolved-attribute] test/test_plugin_dbus.py:180:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:185:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:198:12: Type `NotifyDBus` has no attribute `url_id`
+ error[unresolved-attribute] test/test_plugin_dbus.py:200:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:211:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:220:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:228:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:236:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:245:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:253:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:261:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:269:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:278:12: Type `NotifyDBus` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:380:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:396:5: Unresolved attribute `enabled` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_plugin_dbus.py:398:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:426:5: Unresolved attribute `duration` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_plugin_dbus.py:429:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_dbus.py:432:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:453:5: Unresolved attribute `duration` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_plugin_dbus.py:456:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_dbus.py:459:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:493:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_dbus.py:513:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_discord.py:538:12: Type `NotifyDiscord` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_discord.py:565:12: Type `NotifyDiscord` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_discord.py:738:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_discord.py:755:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_discord.py:774:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_discord.py:787:16: Type `NotifyBase` has no attribute `send`
+ error[unresolved-attribute] test/test_plugin_discord.py:794:16: Type `NotifyBase` has no attribute `send`
+ error[unresolved-attribute] test/test_plugin_discord.py:802:12: Type `NotifyBase` has no attribute `send`
+ error[unresolved-attribute] test/test_plugin_email.py:356:35: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_email.py:359:35: Type `NotifyBase` has no attribute `url_id`
+ error[invalid-argument-type] test/test_plugin_email.py:362:39: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_email.py:366:21: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_email.py:375:28: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_email.py:379:47: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_email.py:388:30: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_email.py:392:28: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[invalid-argument-type] test/test_plugin_email.py:392:40: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[invalid-argument-type] test/test_plugin_email.py:394:28: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_email.py:394:34: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_email.py:394:61: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_email.py:395:24: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_email.py:401:41: Argument to function `hasattr` is incorrect: Expected `str`, found `NotifyBase`
+ error[no-matching-overload] test/test_plugin_email.py:402:28: No overload of function `getattr` matches arguments
+ error[invalid-argument-type] test/test_plugin_email.py:407:35: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_email.py:410:28: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:423:36: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:491:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:493:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:494:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:495:12: Attribute `secure` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:496:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:503:12: Type `NotifyBase` has no attribute `user`
+ error[unresolved-attribute] test/test_plugin_email.py:520:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:525:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:547:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:555:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:564:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:572:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:584:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:609:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:699:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:735:12: Type `NotifyEmail` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:752:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:753:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:775:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:776:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:797:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:798:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:801:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:827:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:828:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:831:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:861:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:862:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:863:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:864:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:882:16: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:883:16: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:884:16: Attribute `host` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:885:36: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:938:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:986:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1036:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1062:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1088:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1110:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1111:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1113:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1118:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1138:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1138:25: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1139:12: Type `NotifyEmail` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1139:29: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1141:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1141:25: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1146:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1166:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1167:12: Type `NotifyEmail` has no attribute `password`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1169:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1174:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1199:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1224:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1250:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1251:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1271:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1294:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1295:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1304:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1328:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1339:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1363:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1374:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1412:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1462:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1468:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1513:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1519:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1574:12: Type `NotifyEmail` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1629:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_email.py:1633:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1678:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1722:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1858:16: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:1859:44: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:1860:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:1860:32: Type `NotifyBase` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1861:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:1862:12: Type `NotifyBase` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1863:12: Type `NotifyBase` has no attribute `user`
+ error[unresolved-attribute] test/test_plugin_email.py:1864:12: Type `NotifyBase` has no attribute `secure`
+ error[unresolved-attribute] test/test_plugin_email.py:1865:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:1866:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:1870:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1911:16: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:1912:44: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:1913:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:1913:32: Type `NotifyBase` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1914:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:1915:12: Type `NotifyBase` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1916:12: Type `NotifyBase` has no attribute `user`
+ error[unresolved-attribute] test/test_plugin_email.py:1917:12: Type `NotifyBase` has no attribute `secure`
+ error[unresolved-attribute] test/test_plugin_email.py:1918:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:1919:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:1923:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1986:16: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:1987:45: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:1988:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:1988:32: Type `NotifyBase` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1989:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:1990:12: Type `NotifyBase` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1991:12: Type `NotifyBase` has no attribute `user`
+ error[unresolved-attribute] test/test_plugin_email.py:1992:12: Type `NotifyBase` has no attribute `secure`
+ error[unresolved-attribute] test/test_plugin_email.py:1993:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:1994:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:1998:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:2050:16: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:2051:45: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:2053:12: Type `NotifyBase` has no attribute `from_addr`
+ error[unresolved-attribute] test/test_plugin_email.py:2054:12: Type `NotifyBase` has no attribute `user`
+ error[unresolved-attribute] test/test_plugin_email.py:2055:12: Type `NotifyBase` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:2056:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:2057:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:2058:12: Type `NotifyBase` has no attribute `targets`
+ error[unresolved-attribute] test/test_plugin_email.py:2084:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:2093:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2094:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2095:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2097:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2100:12: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2111:12: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2114:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2115:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2117:23: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2120:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2123:28: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2124:28: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2127:12: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2128:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2133:9: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2136:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2139:11: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2143:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2146:19: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2160:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2163:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2166:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2169:11: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2173:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2176:19: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2177:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2185:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2186:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2196:12: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2197:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2200:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2203:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2216:12: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2219:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2221:17: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2225:30: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2229:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2231:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:2234:28: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2236:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2237:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2239:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2240:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2241:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2244:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2246:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2248:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2251:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2252:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2255:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2257:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2261:28: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2262:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2264:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2265:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2268:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2270:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2271:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2274:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2275:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2279:28: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2282:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2284:28: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2285:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2298:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2302:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2305:20: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2308:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2312:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2314:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2318:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2322:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2326:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2329:16: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2331:16: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:2334:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2338:24: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2359:20: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2372:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2376:5: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:2380:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2384:5: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:2389:22: Type `NotifyBase` has no attribute `store`
+ error[unresolved-attribute] test/test_plugin_email.py:2393:5: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_emby.py:114:12: Type `NotifyEmby` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_emby.py:121:12: Type `NotifyEmby` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_emby.py:130:16: Type `NotifyEmby` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_emby.py:143:12: Type `NotifyEmby` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_emby.py:148:12: Type `NotifyEmby` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_emby.py:155:5: Attribute `port` on type `NotifyEmby` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_emby.py:156:12: Type `NotifyEmby` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_emby.py:161:12: Type `NotifyEmby` has no attribute `notify`
+ warning[possibly-unbound-attribute] test/test_plugin_emby.py:213:12: Attribute `port` on type `NotifyEmby` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_emby.py:219:5: Attribute `port` on type `NotifyEmby` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_emby.py:225:12: Attribute `port` on type `NotifyEmby` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_emby.py:338:5: Attribute `port` on type `NotifyEmby` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_emby.py:432:5: Attribute `port` on type `NotifyEmby` is possibly unbound
+ error[unresolved-attribute] test/test_plugin_fcm.py:259:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:296:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:329:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:359:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:390:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:414:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:433:16: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:454:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:514:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:549:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_fcm.py:581:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:123:5: Unresolved attribute `duration` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_plugin_gnome.py:126:12: Type `NotifyBase` has no attribute `enabled`
+ error[unresolved-attribute] test/test_plugin_gnome.py:159:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:165:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:178:12: Type `NotifyBase` has no attribute `urgency`
+ error[unresolved-attribute] test/test_plugin_gnome.py:179:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:185:12: Type `NotifyBase` has no attribute `urgency`
+ error[unresolved-attribute] test/test_plugin_gnome.py:186:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:192:12: Type `NotifyBase` has no attribute `urgency`
+ error[unresolved-attribute] test/test_plugin_gnome.py:193:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:205:12: Type `NotifyBase` has no attribute `urgency`
+ error[unresolved-attribute] test/test_plugin_gnome.py:207:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:212:12: Type `NotifyBase` has no attribute `urgency`
+ error[unresolved-attribute] test/test_plugin_gnome.py:214:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_gnome.py:220:12: Type `NotifyBase` has no attribute `urgency`
+ error[unresolved-attribute] test/test_plugin_gnome.py:221:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_google_chat.py:139:12: Type `NotifyGoogleChat` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_google_chat.py:160:12: Type `NotifyGoogleChat` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_growl.py:108:16: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_growl.py:126:16: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_growl.py:277:31: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_plugin_growl.py:281:35: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_growl.py:285:21: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_growl.py:289:55: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_growl.py:298:30: Type `NotifyBase` has no attribute `url`
+ error[invalid-argument-type] test/test_plugin_growl.py:305:41: Argument to function `hasattr` is incorrect: Expected `str`, found `NotifyBase`
+ error[no-matching-overload] test/test_plugin_growl.py:306:28: No overload of function `getattr` matches arguments
+ error[unresolved-attribute] test/test_plugin_growl.py:310:24: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_homeassistant.py:146:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_homeassistant.py:149:12: Type `NotifyBase` has no attribute `send`
+ error[unresolved-attribute] test/test_plugin_httpsms.py:147:12: Type `NotifyBase` has no attribute `notify`
+ error[invalid-argument-type] test/test_plugin_httpsms.py:151:16: Argument to function `len` is incorrect: Expected `Sized`, found `NotifyBase`
+ error[unresolved-attribute] test/test_plugin_httpsms.py:170:12: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_macosx.py:101:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_macosx.py:106:12: Type `NotifyBase` has no attribute `url_id`
+ error[unresolved-attribute] test/test_plugin_macosx.py:109:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:113:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:119:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:125:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_macosx.py:126:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:133:12: Type `NotifyBase` has no attribute `sound`
+ error[unresolved-attribute] test/test_plugin_macosx.py:134:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_macosx.py:135:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:142:12: Type `NotifyBase` has no attribute `click`
+ error[unresolved-attribute] test/test_plugin_macosx.py:143:23: Type `NotifyBase` has no attribute `url`
+ error[unresolved-attribute] test/test_plugin_macosx.py:144:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:160:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_macosx.py:173:5: Unresolved attribute `notify_path` on type `NotifyBase`.
+ error[unresolved-attribute] test/test_plugin_macosx.py:174:31: Type `NotifyBase` has no attribute `notify_path`
+ error[unresolved-attr...*[Comment body truncated]*

---

_Label `ty` added by @MichaReiser on 2025-06-20 06:10_

---

_@sharkdp approved on 2025-06-20 08:33_

This looks great, thank you! Good idea to pull this out of https://github.com/astral-sh/ruff/pull/18587

> mypy_primer will look bad since the new code allows ty to check more code.

Most of the [800+ new diagnostics](https://shark.fish/diff-staticmethod.html) come from the fact that we now understand [`Apprise.instantiate`](https://github.com/caronc/apprise/blob/ce90151051f630803cba755fc9f06e16f7d8590b/apprise/apprise.pyi#L21-L27), and properly return a `NotifyBase` instance (instead of `Unknown`). Unfortunately, the stubs for `NotifyBase` are a bit ... [minimalistic](https://github.com/caronc/apprise/blob/ce90151051f630803cba755fc9f06e16f7d8590b/apprise/plugins/base.pyi#L1). This is strange.. it would be better to have no stub file than to have this completely wrong stub? I'm afraid this is something that would have to be fixed in that project, but maybe @AlexWaygood has some suggestions here?


---

_Merged by @sharkdp on 2025-06-20 08:38_

---

_Closed by @sharkdp on 2025-06-20 08:38_

---

_Comment by @AlexWaygood on 2025-06-20 08:39_

> Unfortunately, the stubs for `NotifyBase` are a bit ... [minimalistic](https://github.com/caronc/apprise/blob/ce90151051f630803cba755fc9f06e16f7d8590b/apprise/plugins/base.pyi#L1). This is a bit strange.. it would be better to have no stub file than to have this completely wrong stub? I'm afraid this is something that would have to be fixed in that project, but maybe @AlexWaygood has some suggestions here?

Wow, that is odd. But yeah, I don't think there's anything we can do here unfortunately :-( that's something the Apprise project has to take responsibility for.

---
