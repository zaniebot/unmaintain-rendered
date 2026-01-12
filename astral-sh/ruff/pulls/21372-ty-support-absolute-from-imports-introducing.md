```yaml
number: 21372
title: "[ty] support absolute `from` imports introducing local submodules in `__init__.py` files"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/abs-allowed-imp
created_at: 2025-11-11T01:11:11Z
updated_at: 2025-11-11T18:36:31Z
url: https://github.com/astral-sh/ruff/pull/21372
synced_at: 2026-01-12T15:57:22Z
```

# [ty] support absolute `from` imports introducing local submodules in `__init__.py` files

---

_@Gankra_

By resolving `.` and the LHS of the from import during semantic indexing, we can check if the LHS is a submodule of `.`, and handle `from whatever.thispackage.x.y import z` exactly like we do `from .x.y import z`.

Fixes https://github.com/astral-sh/ty/issues/1484

---

_Label `server` added by @Gankra on 2025-11-11 01:11_

---

_Review requested from @carljm by @Gankra on 2025-11-11 01:11_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-11 01:11_

---

_Review requested from @sharkdp by @Gankra on 2025-11-11 01:11_

---

_Label `ty` added by @Gankra on 2025-11-11 01:11_

---

_Review requested from @dcreager by @Gankra on 2025-11-11 01:11_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-11 01:11_

---

_Comment by @Gankra on 2025-11-11 01:12_

(This implementation isn't the prettiest, especially in the infer side... something I should iterate on tomorrow probably)

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 01:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-11 16:25:47.327737448 +0000
+++ new-output.txt	2025-11-11 16:25:50.696748604 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19384)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19385)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-11 01:13_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
- tests/ignite/contrib/engines/test_common.py:479:28: error[unresolved-attribute] Module `ignite.handlers` has no member `tensorboard_logger`
- tests/ignite/contrib/engines/test_common.py:480:32: error[unresolved-attribute] Module `ignite.handlers` has no member `tensorboard_logger`
- tests/ignite/contrib/engines/test_common.py:488:28: error[unresolved-attribute] Module `ignite.handlers` has no member `tensorboard_logger`
- tests/ignite/contrib/engines/test_common.py:489:32: error[unresolved-attribute] Module `ignite.handlers` has no member `tensorboard_logger`
- tests/ignite/contrib/engines/test_common.py:497:28: error[unresolved-attribute] Module `ignite.handlers` has no member `tensorboard_logger`
- tests/ignite/contrib/engines/test_common.py:498:32: error[unresolved-attribute] Module `ignite.handlers` has no member `tensorboard_logger`
- tests/ignite/contrib/engines/test_common.py:512:28: error[unresolved-attribute] Module `ignite.handlers` has no member `visdom_logger`
- tests/ignite/contrib/engines/test_common.py:513:32: error[unresolved-attribute] Module `ignite.handlers` has no member `visdom_logger`
- tests/ignite/contrib/engines/test_common.py:522:28: error[unresolved-attribute] Module `ignite.handlers` has no member `visdom_logger`
- tests/ignite/contrib/engines/test_common.py:523:32: error[unresolved-attribute] Module `ignite.handlers` has no member `visdom_logger`
- tests/ignite/contrib/engines/test_common.py:536:28: error[unresolved-attribute] Module `ignite.handlers` has no member `polyaxon_logger`
- tests/ignite/contrib/engines/test_common.py:537:32: error[unresolved-attribute] Module `ignite.handlers` has no member `polyaxon_logger`
- tests/ignite/contrib/engines/test_common.py:544:28: error[unresolved-attribute] Module `ignite.handlers` has no member `polyaxon_logger`
- tests/ignite/contrib/engines/test_common.py:545:32: error[unresolved-attribute] Module `ignite.handlers` has no member `polyaxon_logger`
- tests/ignite/contrib/engines/test_common.py:556:28: error[unresolved-attribute] Module `ignite.handlers` has no member `mlflow_logger`
- tests/ignite/contrib/engines/test_common.py:557:32: error[unresolved-attribute] Module `ignite.handlers` has no member `mlflow_logger`
- tests/ignite/contrib/engines/test_common.py:565:28: error[unresolved-attribute] Module `ignite.handlers` has no member `mlflow_logger`
- tests/ignite/contrib/engines/test_common.py:566:32: error[unresolved-attribute] Module `ignite.handlers` has no member `mlflow_logger`
- tests/ignite/contrib/engines/test_common.py:581:5: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:587:32: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:588:36: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:596:32: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:597:36: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:605:32: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:606:36: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:616:32: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:617:36: error[unresolved-attribute] Module `ignite.handlers` has no member `clearml_logger`
- tests/ignite/contrib/engines/test_common.py:628:28: error[unresolved-attribute] Module `ignite.handlers` has no member `neptune_logger`
- tests/ignite/contrib/engines/test_common.py:629:32: error[unresolved-attribute] Module `ignite.handlers` has no member `neptune_logger`
- tests/ignite/contrib/engines/test_common.py:637:28: error[unresolved-attribute] Module `ignite.handlers` has no member `neptune_logger`
- tests/ignite/contrib/engines/test_common.py:638:32: error[unresolved-attribute] Module `ignite.handlers` has no member `neptune_logger`
- Found 2155 diagnostics
+ Found 2124 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/convenience.py:109:25: error[unresolved-attribute] Module `dedupe` has no member `api`
- dedupe/convenience.py:122:28: error[unresolved-attribute] Module `dedupe` has no member `api`
- Found 59 diagnostics
+ Found 57 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/cli/commands/__init__.py:126:9: warning[possibly-missing-attribute] Attribute `params` may be missing on object of type `<module 'schemathesis.cli.commands.run'> | Unknown`
- Found 260 diagnostics
+ Found 261 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/python_codegen.py:61:18: error[unresolved-attribute] Module `black` has no member `mode`
- schema_salad/python_codegen.py:62:34: error[unresolved-attribute] Module `black` has no member `mode`
- Found 197 diagnostics
+ Found 195 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/study/_optimize.py:41:12: error[unresolved-attribute] Module `optuna.study` has no member `study`
- optuna/study/_optimize.py:129:12: error[unresolved-attribute] Module `optuna.study` has no member `study`
- optuna/study/_optimize.py:188:12: error[unresolved-attribute] Module `optuna.study` has no member `study`
- optuna/testing/storages.py:119:37: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- optuna/testing/storages.py:130:17: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- optuna/testing/storages.py:158:27: error[unresolved-attribute] Module `optuna.storages` has no member `_grpc`
- tests/samplers_tests/test_cmaes.py:456:12: error[unresolved-attribute] Module `optuna.samplers` has no member `_cmaes`
- tests/samplers_tests/test_cmaes.py:465:16: error[unresolved-attribute] Module `optuna.samplers` has no member `_cmaes`
- tests/samplers_tests/test_cmaes.py:474:16: error[unresolved-attribute] Module `optuna.samplers` has no member `_cmaes`
- tests/samplers_tests/test_cmaes.py:484:16: error[unresolved-attribute] Module `optuna.samplers` has no member `_cmaes`
- tests/storages_tests/journal_tests/test_journal.py:49:28: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/storages_tests/journal_tests/test_journal.py:54:24: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/storages_tests/journal_tests/test_journal.py:58:24: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/storages_tests/journal_tests/test_journal.py:63:20: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/storages_tests/journal_tests/test_journal.py:67:37: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/storages_tests/journal_tests/test_journal.py:118:20: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/storages_tests/journal_tests/test_journal.py:126:24: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/test_cli.py:1514:13: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/test_cli.py:1552:13: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tests/test_cli.py:1598:13: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- tutorial/20_recipes/011_journal_storage.py:24:5: error[unresolved-attribute] Module `optuna.storages` has no member `journal`
- Found 569 diagnostics
+ Found 548 diagnostics

xarray (https://github.com/pydata/xarray)
- asv_bench/benchmarks/coding.py:18:9: error[unresolved-attribute] Module `xarray` has no member `coding`
+ asv_bench/benchmarks/coding.py:18:9: error[unresolved-attribute] Module `xarray.coding` has no member `times`
- asv_bench/benchmarks/dataset_io.py:21:18: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:646:39: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:650:19: error[unresolved-attribute] Module `xarray` has no member `backends`
+ asv_bench/benchmarks/dataset_io.py:650:19: error[unresolved-attribute] Module `xarray.backends` has no member `locks`
- asv_bench/benchmarks/dataset_io.py:653:24: error[unresolved-attribute] Module `xarray` has no member `core`
+ asv_bench/benchmarks/dataset_io.py:653:24: error[unresolved-attribute] Module `xarray.core` has no member `indexing`
- asv_bench/benchmarks/dataset_io.py:656:21: error[unresolved-attribute] Module `xarray` has no member `core`
+ asv_bench/benchmarks/dataset_io.py:656:21: error[unresolved-attribute] Module `xarray.core` has no member `indexing`
- asv_bench/benchmarks/dataset_io.py:664:32: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:665:22: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:667:19: error[unresolved-attribute] Module `xarray` has no member `backends`
+ asv_bench/benchmarks/dataset_io.py:667:19: error[unresolved-attribute] Module `xarray.backends` has no member `locks`
- asv_bench/benchmarks/dataset_io.py:678:23: error[unresolved-attribute] Module `xarray` has no member `backends`
+ asv_bench/benchmarks/dataset_io.py:678:23: error[unresolved-attribute] Module `xarray.backends` has no member `locks`
- asv_bench/benchmarks/dataset_io.py:681:34: error[unresolved-attribute] Module `xarray` has no member `backends`
+ asv_bench/benchmarks/dataset_io.py:681:34: error[unresolved-attribute] Module `xarray.backends` has no member `locks`
- asv_bench/benchmarks/dataset_io.py:683:27: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:684:21: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:715:34: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:730:35: error[unresolved-attribute] Module `xarray` has no member `backends`
- asv_bench/benchmarks/dataset_io.py:733:36: error[unresolved-attribute] Module `xarray` has no member `backends`
- properties/test_encode_decode.py:29:13: error[unresolved-attribute] Module `xarray` has no member `coding`
+ properties/test_encode_decode.py:29:13: error[unresolved-attribute] Module `xarray.coding` has no member `variables`
- properties/test_encode_decode.py:40:13: error[unresolved-attribute] Module `xarray` has no member `coding`
+ properties/test_encode_decode.py:40:13: error[unresolved-attribute] Module `xarray.coding` has no member `variables`
- properties/test_encode_decode.py:48:13: error[unresolved-attribute] Module `xarray` has no member `coding`
+ properties/test_encode_decode.py:48:13: error[unresolved-attribute] Module `xarray.coding` has no member `variables`
- xarray/tests/test_accessor_dt.py:449:17: error[unresolved-attribute] Module `xarray` has no member `coding`
+ xarray/tests/test_accessor_dt.py:449:17: error[unresolved-attribute] Module `xarray.coding` has no member `cftimeindex`
- xarray/tests/test_accessor_dt.py:522:9: error[unresolved-attribute] Module `xarray` has no member `coding`
+ xarray/tests/test_accessor_dt.py:522:9: error[unresolved-attribute] Module `xarray.coding` has no member `cftimeindex`
- xarray/tests/test_accessor_dt.py:542:17: error[unresolved-attribute] Module `xarray` has no member `coding`
+ xarray/tests/test_accessor_dt.py:542:17: error[unresolved-attribute] Module `xarray.coding` has no member `cftimeindex`
- xarray/tests/test_accessor_dt.py:563:13: error[unresolved-attribute] Module `xarray` has no member `coding`
+ xarray/tests/test_accessor_dt.py:563:13: error[unresolved-attribute] Module `xarray.coding` has no member `cftimeindex`
- xarray/tests/test_backends.py:3652:20: error[unresolved-attribute] Module `xarray` has no member `coding`
+ xarray/tests/test_backends.py:3652:20: error[unresolved-attribute] Module `xarray.coding` has no member `times`
- xarray/tests/test_backends.py:6551:13: error[unresolved-attribute] Module `xarray` has no member `backends`
- xarray/tests/test_backends.py:7060:15: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends.py:7067:15: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends.py:7073:15: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends.py:7081:14: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends.py:7086:14: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends.py:7091:14: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends.py:7096:18: error[unresolved-attribute] Module `xarray.backends` has no member `zarr`
- xarray/tests/test_backends_api.py:123:25: error[unresolved-attribute] Module `xarray` has no member `backends`
- xarray/tests/test_backends_api.py:142:29: error[unresolved-attribute] Module `xarray` has no member `backends`
- xarray/tests/test_backends_api.py:155:36: error[unresolved-attribute] Module `xarray` has no member `backends`
- xarray/tests/test_coding.py:67:18: error[unresolved-attribute] Module `xarray` has no member `conventions`
- xarray/tests/test_coding.py:73:22: error[unresolved-attribute] Module `xarray` has no member `conventions`
- xarray/tests/test_dataset.py:238:31: error[unresolved-attribute] Module `xarray.backends` has no member `common`
- xarray/tests/test_formatting.py:954:14: error[unresolved-attribute] Module `xarray` has no member `core`
+ xarray/tests/test_formatting.py:954:14: error[unresolved-attribute] Module `xarray.core` has no member `formatting`
- xarray/tests/test_formatting.py:1014:13: error[unresolved-attribute] Module `xarray` has no member `coding`
+ xarray/tests/test_formatting.py:1014:13: error[unresolved-attribute] Module `xarray.coding` has no member `cftimeindex`
- Found 1755 diagnostics
+ Found 1732 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/ext/mathjax.py:33:10: error[unresolved-attribute] Module `sphinx` has no member `util`
- sphinx/util/i18n.py:241:25: error[unresolved-attribute] Module `babel` has no member `core`
- Found 514 diagnostics
+ Found 512 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/tests/test_config.py:161:25: error[unresolved-attribute] Module `sklearn.utils` has no member `_array_api`
- Found 2561 diagnostics
+ Found 2562 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/reporting/test_reporting.py:180:24: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- tests/unittests/reporting/test_reporting.py:184:9: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- tests/unittests/reporting/test_reporting.py:191:24: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- tests/unittests/reporting/test_reporting.py:194:9: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- tests/unittests/reporting/test_reporting.py:197:24: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- tests/unittests/reporting/test_reporting.py:200:9: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- tests/unittests/reporting/test_reporting.py:210:33: error[unresolved-attribute] Module `cloudinit.reporting` has no member `handlers`
- Found 1131 diagnostics
+ Found 1124 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/cwlviewer.py:40:26: error[unresolved-attribute] Module `rdflib` has no member `graph`
- cwltool/cwlviewer.py:46:56: error[unresolved-attribute] Module `rdflib` has no member `graph`
- cwltool/cwlviewer.py:175:38: error[unresolved-attribute] Module `rdflib` has no member `term`
+ cwltool/cwlviewer.py:56:40: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[Unknown | str, Unknown | str]`
+ cwltool/cwlviewer.py:124:40: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[Unknown | str, Unknown | str]`
+ cwltool/cwlviewer.py:155:35: error[invalid-argument-type] Argument to bound method `query` is incorrect: Expected `Mapping[str, Identifier] | None`, found `dict[Unknown | str, Unknown | str]`
- tests/test_examples.py:999:14: error[unresolved-attribute] Module `pydot` has no member `core`
- tests/test_examples.py:1050:30: error[unresolved-attribute] Module `pydot` has no member `core`
- Found 139 diagnostics
+ Found 137 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ tests/strategies/test_strategies.py:15:32: warning[possibly-missing-import] Member `pandas_strategies` of module `pandera.strategies` may be missing
- Found 1635 diagnostics
+ Found 1636 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/unittest/patch.py:149:61: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:149:100: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:161:34: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:149:61: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:149:100: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:161:34: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/contrib/internal/unittest/patch.py:192:34: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:207:38: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:211:59: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:217:57: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:223:31: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:286:43: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:192:34: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:207:38: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:211:59: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:217:57: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:223:31: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:286:43: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/contrib/internal/unittest/patch.py:390:23: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:391:22: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:390:23: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:391:22: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/contrib/internal/unittest/patch.py:407:44: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:407:44: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:477:45: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:615:50: error[invalid-argument-type] Argument to function `_update_status_item` is incorrect: Expected `str`, found `str | None`
- ddtrace/contrib/internal/unittest/patch.py:645:43: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:645:43: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/contrib/internal/unittest/patch.py:686:42: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:686:42: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/contrib/internal/unittest/patch.py:703:52: warning[possibly-missing-attribute] Attribute `span_id` may be missing on object of type `Unknown | None`
+ ddtrace/contrib/internal/unittest/patch.py:703:52: warning[possibly-missing-attribute] Attribute `span_id` may be missing on object of type `Span | None`
+ ddtrace/contrib/internal/unittest/patch.py:709:49: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
- ddtrace/contrib/internal/unittest/patch.py:709:49: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Unknown | None`
+ ddtrace/contrib/internal/unittest/patch.py:709:49: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Span | None`
- ddtrace/contrib/internal/unittest/patch.py:729:41: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:729:41: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:747:47: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
- ddtrace/contrib/internal/unittest/patch.py:747:47: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Unknown | None`
+ ddtrace/contrib/internal/unittest/patch.py:747:47: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Span | None`
- ddtrace/contrib/internal/unittest/patch.py:749:50: warning[possibly-missing-attribute] Attribute `span_id` may be missing on object of type `Unknown | None`
+ ddtrace/contrib/internal/unittest/patch.py:749:50: warning[possibly-missing-attribute] Attribute `span_id` may be missing on object of type `Span | None`
+ ddtrace/contrib/internal/unittest/patch.py:754:48: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
- ddtrace/contrib/internal/unittest/patch.py:754:48: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Unknown | None`
+ ddtrace/contrib/internal/unittest/patch.py:754:48: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Span | None`
+ ddtrace/contrib/internal/unittest/patch.py:760:47: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
- ddtrace/contrib/internal/unittest/patch.py:760:47: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Unknown | None`
+ ddtrace/contrib/internal/unittest/patch.py:760:47: warning[possibly-missing-attribute] Attribute `get_tag` may be missing on object of type `Span | None`
- ddtrace/contrib/internal/unittest/patch.py:765:49: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/contrib/internal/unittest/patch.py:765:72: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:765:49: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/contrib/internal/unittest/patch.py:765:72: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/contrib/internal/unittest/patch.py:810:32: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/contrib/internal/unittest/patch.py:783:36: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:784:35: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:785:34: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:790:37: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:797:36: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:798:41: error[invalid-argument-type] Argument to bound method `_set_tag_str` is incorrect: Expected `str`, found `str | None`
+ ddtrace/contrib/internal/unittest/patch.py:810:32: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/internal/ci_visibility/coverage.py:134:36: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/internal/ci_visibility/coverage.py:134:36: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/internal/ci_visibility/utils.py:56:11: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- ddtrace/internal/ci_visibility/utils.py:81:57: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/internal/ci_visibility/utils.py:56:11: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ ddtrace/internal/ci_visibility/utils.py:81:57: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- ddtrace/internal/runtime/runtime_metrics.py:95:22: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ ddtrace/internal/runtime/runtime_metrics.py:95:22: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/ci_visibility/api_client/_util.py:334:55: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/ci_visibility/api_client/_util.py:334:55: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/ci_visibility/test_ci_visibility_check_enabled_features.py:49:55: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/ci_visibility/test_ci_visibility_check_enabled_features.py:49:55: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/contrib/pytest/test_pytest.py:49:19: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/contrib/pytest/test_pytest.py:53:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/contrib/pytest/test_pytest.py:49:19: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/contrib/pytest/test_pytest.py:53:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/contrib/pytest/test_pytest.py:441:27: error[invalid-argument-type] Argument to function `loads` is incorrect: Expected `str | bytes | bytearray`, found `str | None`
+ tests/contrib/pytest/test_pytest.py:661:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["test should fail"]` with `str | None`
+ tests/contrib/pytest/test_pytest.py:924:16: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `str | None`
+ tests/contrib/pytest/test_pytest.py:952:16: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `str | None`
+ tests/contrib/pytest/test_pytest.py:1641:16: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `str | None`
+ tests/contrib/pytest/test_pytest.py:1643:18: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `str | None`
- tests/integration/test_debug.py:162:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/integration/test_debug.py:176:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/integration/test_debug.py:188:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/integration/test_debug.py:199:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/integration/test_debug.py:210:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/integration/test_debug.py:162:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/integration/test_debug.py:176:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/integration/test_debug.py:188:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/integration/test_debug.py:199:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/integration/test_debug.py:210:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/profiling/collector/conftest.py:11:15: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/profiling/collector/conftest.py:11:15: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/profiling_v2/collector/conftest.py:8:12: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/profiling_v2/collector/conftest.py:8:12: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/tracer/test_tracer.py:688:12: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/tracer/test_tracer.py:688:12: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/tracer/test_tracer.py:688:12: warning[possibly-missing-attribute] Attribute `intake_url` may be missing on object of type `Unknown | TraceWriter`
- tests/tracer/test_tracer.py:1978:12: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/tracer/test_tracer.py:1980:9: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/tracer/test_tracer.py:1978:12: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/tracer/test_tracer.py:1980:9: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
- tests/utils.py:1208:13: error[unresolved-attribute] Module `ddtrace` has no member `trace`
- tests/utils.py:1211:53: error[unresolved-attribute] Module `ddtrace` has no member `trace`
+ tests/utils.py:1208:13: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/utils.py:1211:53: warning[possibly-missing-attribute] Member `trace` may be missing on module `ddtrace`
+ tests/utils.py:1215:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `str | None`
- Found 8214 diagnostics
+ Found 8234 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/athena/__init__.py:42:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `athena`
- ibis/backends/athena/__init__.py:287:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/athena/__init__.py:287:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/athena/__init__.py:583:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/athena/__init__.py:583:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/bigquery/__init__.py:171:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `bigquery`
+ ibis/backends/bigquery/__init__.py:844:13: error[invalid-argument-type] Argument to bound method `to_sqlglot` is incorrect: Expected `str | None`, found `str | int | None`
+ ibis/backends/bigquery/tests/system/conftest.py:43:20: error[unresolved-attribute] Module `ibis.backends` has no member `bigquery`
+ ibis/backends/bigquery/tests/system/test_connect.py:82:22: error[unresolved-attribute] Module `ibis.backends` has no member `bigquery`
+ ibis/backends/bigquery/tests/system/test_connect.py:132:22: error[unresolved-attribute] Module `ibis.backends` has no member `bigquery`
+ ibis/backends/bigquery/tests/unit/test_compiler.py:269:23: error[unresolved-attribute] Module `ibis.backends` has no member `bigquery`
- ibis/backends/clickhouse/__init__.py:54:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `clickhouse`
+ ibis/backends/clickhouse/tests/test_client.py:32:28: error[unresolved-attribute] Module `ibis.backends` has no member `clickhouse`
- ibis/backends/databricks/__init__.py:93:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `databricks`
- ibis/backends/databricks/__init__.py:608:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/databricks/__init__.py:608:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/datafusion/__init__.py:91:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `datafusion`
- ibis/backends/druid/__init__.py:38:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `druid`
- ibis/backends/duckdb/__init__.py:93:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `duckdb`
- ibis/backends/duckdb/__init__.py:338:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/duckdb/__init__.py:338:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/duckdb/__init__.py:1723:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/duckdb/__init__.py:1723:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/exasol/__init__.py:42:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `exasol`
- ibis/backends/exasol/__init__.py:274:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/exasol/__init__.py:274:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/flink/__init__.py:64:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `flink`
- ibis/backends/impala/__init__.py:56:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `impala`
+ ibis/backends/impala/tests/conftest.py:148:13: error[unresolved-attribute] Module `ibis.backends` has no member `impala`
- ibis/backends/mssql/__init__.py:93:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `mssql`
+ ibis/backends/mssql/tests/test_client.py:313:17: error[unresolved-attribute] Module `ibis.backends` has no member `mssql`
- ibis/backends/mysql/__init__.py:51:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `mysql`
- ibis/backends/mysql/__init__.py:231:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/mysql/__init__.py:231:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/oracle/__init__.py:91:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `oracle`
- ibis/backends/postgres/__init__.py:60:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `postgres`
- ibis/backends/postgres/__init__.py:444:14: error[unresolved-attribute] Module `ibis.expr.operations` has no member `udf`
- ibis/backends/postgres/__init__.py:497:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/postgres/__init__.py:497:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/pyspark/__init__.py:129:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `pyspark`
- ibis/backends/risingwave/__init__.py:69:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `risingwave`
- ibis/backends/risingwave/__init__.py:309:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/risingwave/__init__.py:309:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/snowflake/__init__.py:166:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `snowflake`
- ibis/backends/snowflake/__init__.py:574:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/snowflake/__init__.py:574:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/snowflake/__init__.py:590:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/snowflake/__init__.py:590:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
+ ibis/backends/snowflake/tests/conftest.py:212:20: error[unresolved-attribute] Module `ibis.backends` has no member `snowflake`
- ibis/backends/sql/compilers/snowflake.py:158:9: error[unresolved-attribute] Module `ibis.expr.operations` has no member `udf`
- ibis/backends/sql/compilers/snowflake.py:168:9: error[unresolved-attribute] Module `ibis.expr.operations` has no member `udf`
- ibis/backends/sqlite/__init__.py:67:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `sqlite`
+ ibis/backends/tests/test_sql.py:224:10: error[unresolved-attribute] Module `ibis.backends` has no member `sql`
- ibis/backends/trino/__init__.py:52:16: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `trino`
- ibis/backends/trino/__init__.py:184:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/trino/__init__.py:184:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
- ibis/backends/trino/__init__.py:345:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/trino/__init__.py:345:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
+ ibis/examples/gen_registry.py:182:14: error[unresolved-attribute] Module `ibis.backends` has no member `duckdb`
- ibis/expr/sql.py:520:33: error[unresolved-attribute] Module `ibis.backends.sql.compilers` has no member `duckdb`
- ibis/tests/benchmarks/test_benchmarks.py:793:15: error[unresolved-attribute] Module `ibis.expr.operations` has no member `core`
+ ibis/tests/expr/test_uuid.py:9:37: error[unresolved-attribute] Module `ibis.expr` has no member `datashape`
- Found 4240 diagnostics
+ Found 4228 diagnostics

jax (https://github.com/google/jax)
- jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1269:3: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1269:3: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1294:11: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1294:11: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1635:7: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1635:7: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1637:10: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/_src/pallas/mosaic/interpret/interpret_pallas_call.py:1637:10: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/_src/pallas/mosaic_gpu/primitives.py:1223:9: error[unresolved-attribute] Module `jax._src.state` has no member `types`
- jax/experimental/_private_mm/mini_dime.py:267:25: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/experimental/_private_mm/mini_dime.py:267:25: error[unresolved-attribute] Module `jax._src` has no member `lib`
- jax/experimental/colocated_python/func.py:283:2: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/experimental/colocated_python/func.py:283:2: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/experimental/colocated_python/obj.py:62:2: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/experimental/colocated_python/obj.py:62:2: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/experimental/colocated_python/serialization.py:107:2: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/experimental/colocated_python/serialization.py:107:2: error[unresolved-attribute] Module `jax._src` has no member `util`
- jax/experimental/slab/djax.py:36:10: error[unresolved-attribute] Module `jax` has no member `_src`
+ jax/experimental/slab/djax.py:36:10: error[unresolved-attribute] Module `jax._src` has no member `config`
- Found 2565 diagnostics
+ Found 2564 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/lib/email_mirror_server.py:38:29: error[unresolved-attribute] Module `email` has no member `message`
- zerver/lib/email_mirror_server.py:73:26: error[unresolved-attribute] Module `email` has no member `message`
- zerver/lib/email_mirror_server.py:120:39: error[unresolved-attribute] Module `email` has no member `message`
- Found 3318 diagnostics
+ Found 3315 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/_testing/__init__.py:507:25: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/_testing/__init__.py:507:25: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/_testing/__init__.py:509:25: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/_testing/__init__.py:509:25: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/api/test_api.py:456:13: error[unresolved-attribute] Module `pandas` has no member `util`
- pandas/tests/api/test_api.py:486:13: error[unresolved-attribute] Module `pandas` has no member `util`
- pandas/tests/apply/test_frame_apply_relabeling.py:103:14: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/apply/test_frame_apply_relabeling.py:103:14: error[unresolved-attribute] Module `pandas.core` has no member `apply`
- pandas/tests/arrays/boolean/test_function.py:117:14: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/arrays/boolean/test_function.py:117:14: error[unresolved-attribute] Module `pandas.core` has no member `algorithms`
- pandas/tests/arrays/sparse/test_accessor.py:254:38: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/arrays/sparse/test_accessor.py:254:38: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/arrays/sparse/test_arithmetics.py:390:20: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/arrays/sparse/test_arithmetics.py:390:20: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/arrays/test_datetimes.py:342:32: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/arrays/test_datetimes.py:342:32: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/base/test_constructors.py:171:15: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/base/test_constructors.py:171:15: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/base/test_conversion.py:208:13: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/base/test_conversion.py:208:13: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- pandas/tests/base/test_conversion.py:302:13: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/base/test_conversion.py:302:13: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/extension/base/interface.py:64:25: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/extension/base/interface.py:64:25: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/extension/base/interface.py:65:25: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/extension/base/interface.py:65:25: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/extension/base/io.py:18:37: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/extension/base/io.py:18:37: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- pandas/tests/extension/base/methods.py:521:13: error[unresolved-attribute] Module `pandas` has no member `util`
- pandas/tests/extension/base/methods.py:522:13: error[unresolved-attribute] Module `pandas` has no member `util`
- pandas/tests/extension/test_arrow.py:1456:12: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/extension/test_arrow.py:1456:12: error[unresolved-attribute] Module `pandas.core` has no member `common`
- pandas/tests/extension/test_numpy.py:73:19: error[unresolved-attribute] Module `pandas._testing` has no member `asserters`
- pandas/tests/frame/indexing/test_indexing.py:1244:21: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/frame/indexing/test_indexing.py:1244:21: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/frame/methods/test_astype.py:864:26: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/frame/methods/test_astype.py:864:26: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/frame/methods/test_to_records.py:67:30: error[unresolved-attribute] Module `email` has no member `message`
- pandas/tests/groupby/methods/test_quantile.py:459:23: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/groupby/methods/test_quantile.py:459:23: error[unresolved-attribute] Module `pandas.core` has no member `indexes`
- pandas/tests/plotting/test_backend.py:39:9: error[unresolved-attribute] Module `pandas.plotting` has no member `_core`
- pandas/tests/plotting/test_backend.py:63:12: error[unresolved-attribute] Module `pandas.plotting` has no member `_core`
- pandas/tests/plotting/test_backend.py:66:16: error[unresolved-attribute] Module `pandas.plotting` has no member `_core`
- pandas/tests/plotting/test_backend.py:90:9: error[unresolved-attribute] Module `pandas.plotting` has no member `_core`
- pandas/tests/plotting/test_boxplot_method.py:101:27: error[unresolved-attribute] Module `pandas.plotting` has no member `_core`
- pandas/tests/plotting/test_misc.py:57:12: error[unresolved-attribute] Module `pandas.plotting` has no member `_core`
+ pandas/tests/plotting/test_misc.py:61:31: error[invalid-argument-type] Argument to function `_get_call_args` is incorrect: Expected `Series | DataFrame`, found `list[Unknown]`
- pandas/tests/reshape/merge/test_merge.py:760:16: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/reshape/merge/test_merge.py:760:16: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/series/methods/test_isin.py:132:15: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/series/methods/test_isin.py:132:15: error[unresolved-attribute] Module `pandas.core` has no member `algorithms`
- pandas/tests/series/methods/test_isin.py:148:15: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/series/methods/test_isin.py:148:15: error[unresolved-attribute] Module `pandas.core` has no member `algorithms`
- pandas/tests/series/methods/test_isin.py:168:15: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/series/methods/test_isin.py:168:15: error[unresolved-attribute] Module `pandas.core` has no member `algorithms`
- pandas/tests/series/test_ufunc.py:276:31: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandas/tests/series/test_ufunc.py:276:31: error[unresolved-attribute] Module `pandas.core` has no member `arrays`
- pandas/tests/test_common.py:142:11: error[unresolved-attribute] Module `pandas.core.ops` has no member `common`
- pandas/tests/util/test_hashing.py:416:14: error[unresolved-attribute] Module `pandas` has no member `util`
- Found 3371 diagnostics
+ Found 3358 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/testing/runtests.py:806:5: error[unresolved-attribute] Module `sympy.external` has no member `importtools`
- sympy/testing/runtests.py:807:5: error[unresolved-attribute] Module `sympy.external` has no member `importtools`
+ sympy/testing/runtests.py:806:5: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `WARN_OLD_VERSION` of type `None | Literal[True]`
+ sympy/testing/runtests.py:807:5: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `WARN_NOT_INSTALLED` of type `None | Literal[True]`


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~4MB
+     struct fields = ~3MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~19MB
+     struct fields = ~18MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~42MB
+     struct fields = ~40MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-11 01:17_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 12 | 175 | 48 |
| `possibly-missing-attribute` | 44 | 0 | 6 |
| `invalid-argument-type` | 19 | 0 | 13 |
| `invalid-assignment` | 2 | 0 | 0 |
| `possibly-missing-import` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **79** | **175** | **67** |

**[Full report with detailed diff](https://gankra-abs-allowed-imp.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-abs-allowed-imp.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-11 01:27_

Some of these results are perplexing... some completely randomly added missing import errors.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1469 on 2025-11-11 12:21_

Nit:

```suggestion
                    && self.current_scope().is_global()
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:5920 on 2025-11-11 12:24_

Nit: I had to lookup the definition of `from_identifier_parts` to understanding what we're doing here. Should we expose a `ModuleName::package_for(self.db(), self.file())` that hides the `tail` and `level` stuff (which I find more confusing here than useful)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:5953 on 2025-11-11 12:26_

Nit. To reduce the duplication between the `else` branches
```suggestion
        let Ok(name) = ModuleName::from_identifier_parts(
            self.db(),
            self.file(),
            import_from.module.as_deref(),
            import_from.level,
        ).and_then(|submodule_name| submodule_name.relative_to(&thispackage_name))
         .and_then(|relative_submodule_name| relative_submodule_name.components().next()) else {
            self.add_binding(import_from.into(), definition, |_, _| Type::unknown());
            return;
        };
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1478 on 2025-11-11 12:28_

I wonder if you see the new failing imports because this operation can fail if the path to the next search root contains any non-valid-identifier parts (e.g. looking up `core-db/root` will fail if the only search path is at `/`)

---

_@MichaReiser reviewed on 2025-11-11 12:28_

---

_Comment by @AlexWaygood on 2025-11-11 12:49_

> Some of these results are perplexing... some completely randomly added missing import errors.

Which diagnostics are you talking about specifically here? I only see one added `possibly-missing-import` diagnostic in the ecosystem-analyzer summary comment, and no new `unresolved-import` diagnostics

---

_Comment by @MichaReiser on 2025-11-11 12:50_

> Some of these results are perplexing... some completely randomly added missing import errors.

This one might actually be correct because it's a re-export and the import is within a `try...except`

---

_Comment by @Gankra on 2025-11-11 12:55_

<img width="807" height="364" alt="Screenshot 2025-11-11 at 7 54 44 AM" src="https://github.com/user-attachments/assets/c65528ba-0174-4b00-8392-83819b650f8a" />

stuff like this -- maybe this is just telling me that we had an Unknown here before and now we don't?

---

_Comment by @AlexWaygood on 2025-11-11 12:59_

> maybe this is just telling me that we had an Unknown here before and now we don't?

Yeah that seems quite likely to me! Would be good to spot-check one or two and spelunk through them to confirm, though 👍

---

_Comment by @Gankra on 2025-11-11 15:32_

Ok yeah the ecosystem results look reasonable, great. Just need to cleanup.

---

_Comment by @Gankra on 2025-11-11 16:24_

Cleaned up impl is up.

---

_@Gankra reviewed on 2025-11-11 16:24_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/infer/builder.rs`:5953 on 2025-11-11 16:24_

I was gonna complain that I need some of these temporaries in scope but you actually perfectly identified that and only factored out the part that doesn't need it, incredible.

---

_@MichaReiser approved on 2025-11-11 17:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_name.rs`:321 on 2025-11-11 18:03_

it looks like this will always take the `if let Some(level) = NonZeroU32::new(level)` branch in `ModuleName::from_identifier_parts()`, because `NonZeroU32::new(1)` will always return `Some()`. So can't this be done with less indirection as

```suggestion
        relative_module_name(db, importing_file, None, NonZeroU32::new(1).unwrap())
```

?

---

_Merged by @Gankra on 2025-11-11 18:04_

---

_Closed by @Gankra on 2025-11-11 18:04_

---

_Branch deleted on 2025-11-11 18:04_

---

_@AlexWaygood approved on 2025-11-11 18:05_

Thank you!

---

_@Gankra reviewed on 2025-11-11 18:36_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_name.rs`:321 on 2025-11-11 18:36_

Ah true, good catch!

---
