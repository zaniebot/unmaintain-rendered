```yaml
number: 19625
title: "[ty] Find legacy typevars using a `TypeVisitor`"
type: pull_request
state: closed
author: dcreager
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: dcreager/visitor-recursion
created_at: 2025-07-29T21:17:47Z
updated_at: 2025-08-29T00:05:29Z
url: https://github.com/astral-sh/ruff/pull/19625
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Find legacy typevars using a `TypeVisitor`

---

_Pull request opened by @dcreager on 2025-07-29 21:17_

Now that we have a nice mechanism for visiting a type, we can replace the `find_legacy_typevars` method with a `TypeVisitor`. To make this happen, this PR also adds the following:

- Type visitors can now return an `AbortTypeVisitor` result to end the visitor early.
- The logic for avoiding infinite recursion has been factored out into a more reusable helper method.
- We implement `TypeVisitor` for closures that have the right signature, meaning that we usually won't need to create a named type for the visitor.

---

_Label `internal` added by @dcreager on 2025-07-29 21:17_

---

_Review requested from @carljm by @dcreager on 2025-07-29 21:17_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-29 21:17_

---

_Review requested from @sharkdp by @dcreager on 2025-07-29 21:17_

---

_Label `ty` added by @dcreager on 2025-07-29 21:17_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/visitor.rs`:349 on 2025-07-29 21:19_

Here is what it looks like to use the new recursion guard helper with a closure that automatically implements the `TypeVisitor` trait.

---

_@dcreager reviewed on 2025-07-29 21:19_

---

_Comment by @github-actions[bot] on 2025-07-29 21:20_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-29 21:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ tests/type_tests.py:52:37: error[missing-argument] No arguments provided for required parameters `_YieldT_co`, `_T_co` of class `_GeneratorContextManager`
- Found 193 diagnostics
+ Found 194 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tools/ci/copy_to_binary.py:37:12: error[no-matching-overload] No overload of function `splitext` matches arguments
- Found 656 diagnostics
+ Found 657 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/porcelain.py:2300:29: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Literal[".git"]`
+ dulwich/porcelain.py:2305:30: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Literal[".git"]`
+ dulwich/refs.py:678:29: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ dulwich/refs.py:679:25: error[no-matching-overload] No overload of bound method `strip` matches arguments
+ dulwich/refs.py:700:29: error[no-matching-overload] No overload of bound method `replace` matches arguments
- Found 163 diagnostics
+ Found 168 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_path.py:124:12: warning[possibly-unbound-attribute] Attribute `__name__` on type `Unknown | ((...) -> Awaitable[Unknown])` is possibly unbound
+ src/trio/_tests/test_path.py:124:12: warning[possibly-unbound-attribute] Attribute `__name__` on type `Unknown | ((...) -> Awaitable[PathT])` is possibly unbound
- src/trio/_tests/test_path.py:125:12: warning[possibly-unbound-attribute] Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[Unknown])` is possibly unbound
+ src/trio/_tests/test_path.py:125:12: warning[possibly-unbound-attribute] Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[PathT])` is possibly unbound
- src/trio/_tests/test_path.py:128:12: warning[possibly-unbound-attribute] Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[Unknown])` is possibly unbound
+ src/trio/_tests/test_path.py:128:12: warning[possibly-unbound-attribute] Attribute `__qualname__` on type `Unknown | ((...) -> Awaitable[PathT])` is possibly unbound
+ src/trio/_tests/test_timeouts.py:279:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/test_timeouts.py:281:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 738 diagnostics
+ Found 740 diagnostics

rich (https://github.com/Textualize/rich)
- examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def beat(length: int = Literal[1]) -> None`
+ examples/table_movie.py:62:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[_T_co]`, found `def beat(length: int = Literal[1]) -> None`
+ rich/_wrap.py:64:53: error[invalid-argument-type] Argument to function `cell_len` is incorrect: Expected `str`, found `T`
+ rich/_wrap.py:66:42: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `T`
+ rich/box.py:157:20: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | Literal[" "]` and `T`
+ rich/live_render.py:104:24: error[not-iterable] Object of type `T` is not iterable
+ rich/markdown.py:379:24: error[not-iterable] Object of type `T` is not iterable
+ rich/markdown.py:395:24: error[not-iterable] Object of type `T` is not iterable
+ rich/pretty.py:538:17: error[invalid-argument-type] Argument is incorrect: Expected `Node | None`, found `T`
+ rich/pretty.py:697:42: error[not-iterable] Object of type `T` is not iterable
+ rich/pretty.py:749:63: error[not-iterable] Object of type `T` is not iterable
+ rich/pretty.py:788:57: error[unresolved-attribute] Type `T` has no attribute `name`
+ rich/pretty.py:789:43: error[unresolved-attribute] Type `T` has no attribute `name`
+ rich/pretty.py:812:43: error[not-iterable] Object of type `T` is not iterable
+ rich/screen.py:52:24: error[not-iterable] Object of type `T` is not iterable
+ rich/syntax.py:780:32: error[not-iterable] Object of type `T` is not iterable
+ rich/table.py:685:53: error[not-iterable] Object of type `T` is not iterable
+ rich/table.py:890:36: error[non-subscriptable] Cannot subscript object of type `T` with no `__getitem__` method
+ rich/tree.py:157:32: error[not-iterable] Object of type `T` is not iterable
- tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:8:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:9:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:10:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:11:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:17:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:18:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:19:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, Unknown]]`
+ tests/test_tools.py:20:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, T]]`
- tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:26:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, T]]`
- tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:27:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, T]]`
- tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:28:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, T]]`
- tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, Unknown]]`
+ tests/test_tools.py:29:17: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[tuple[bool, bool, T]]`
+ tools/make_terminal_widths.py:29:33: error[not-iterable] Object of type `ProgressType` is not iterable
+ tools/make_terminal_widths.py:65:25: error[invalid-argument-type] Argument to function `chr` is incorrect: Expected `SupportsIndex`, found `Unknown | ProgressType`
- Found 312 diagnostics
+ Found 331 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/types/enum.py:124:22: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `value`
+ strawberry/types/enum.py:125:21: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `name`
- Found 368 diagnostics
+ Found 370 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ pymongo/asynchronous/topology.py:631:21: error[unresolved-attribute] Type `_T` has no attribute `address`
+ pymongo/synchronous/topology.py:629:21: error[unresolved-attribute] Type `_T` has no attribute `address`
- Found 412 diagnostics
+ Found 414 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/core.py:2103:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
+ discord/ext/commands/core.py:2103:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[ContextT]`
- discord/ext/commands/core.py:2136:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
+ discord/ext/commands/core.py:2136:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[ContextT]`
- discord/ext/commands/core.py:2165:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
+ discord/ext/commands/core.py:2165:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[ContextT]`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/test_utils.py:147:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def tmp_digits_dataset(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = Literal[False], seed_train: int = Literal[13], seed_dev: int = Literal[13], source_text_prefix_token: str = Literal[""], with_n_source_factors: int = Literal[0], with_n_target_factors: int = Literal[0]) -> dict[str, Any]`
+ sockeye/test_utils.py:147:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[_T_co]`, found `def tmp_digits_dataset(prefix: str, train_line_count: int, train_line_count_empty: int, train_max_length: int, dev_line_count: int, dev_max_length: int, test_line_count: int, test_line_count_empty: int, test_max_length: int, sort_target: bool = Literal[False], seed_train: int = Literal[13], seed_dev: int = Literal[13], source_text_prefix_token: str = Literal[""], with_n_source_factors: int = Literal[0], with_n_target_factors: int = Literal[0]) -> dict[str, Any]`

jinja (https://github.com/pallets/jinja)
+ src/jinja2/compiler.py:849:16: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/compiler.py:850:36: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/compiler.py:851:25: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/compiler.py:855:16: error[unresolved-attribute] Type `_NodeBound` has no attribute `importname`
+ src/jinja2/compiler.py:856:23: error[unresolved-attribute] Type `_NodeBound` has no attribute `importname`
+ src/jinja2/compiler.py:1188:20: error[unresolved-attribute] Type `_NodeBound` has no attribute `scoped`
+ src/jinja2/compiler.py:1239:16: error[unresolved-attribute] Type `_NodeBound` has no attribute `ctx`
+ src/jinja2/compiler.py:1239:40: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/compiler.py:1586:16: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/compiler.py:1591:27: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/compiler.py:1592:37: error[unresolved-attribute] Type `_NodeBound` has no attribute `name`
+ src/jinja2/ext.py:678:28: error[unresolved-attribute] Type `_NodeBound` has no attribute `node`
+ src/jinja2/ext.py:679:16: error[unresolved-attribute] Type `_NodeBound` has no attribute `node`
+ src/jinja2/ext.py:685:20: error[unresolved-attribute] Type `_NodeBound` has no attribute `args`
+ src/jinja2/ext.py:691:18: error[unresolved-attribute] Type `_NodeBound` has no attribute `kwargs`
+ src/jinja2/ext.py:693:12: error[unresolved-attribute] Type `_NodeBound` has no attribute `dyn_args`
+ src/jinja2/ext.py:695:12: error[unresolved-attribute] Type `_NodeBound` has no attribute `dyn_kwargs`
+ src/jinja2/ext.py:709:28: error[unresolved-attribute] Type `_NodeBound` has no attribute `node`
- src/jinja2/meta.py:80:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 191 diagnostics
+ Found 208 diagnostics

nox (https://github.com/wntrblm/nox)
+ nox/tasks.py:444:35: error[invalid-argument-type] Argument to function `all` is incorrect: Argument type `Iterable[object]` does not satisfy upper bound of type variable `Sequence_Results_T`
- Found 23 diagnostics
+ Found 24 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/url_helper.py:696:28: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | None, UrlResponse | None]`, found `tuple[@Todo(dict comprehension type), _T & ~AlwaysFalsy]`
+ cloudinit/url_helper.py:723:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str | None, UrlResponse | None]`, found `tuple[None | @Todo(dict comprehension type), None | _T]`
- Found 599 diagnostics
+ Found 601 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/cli/ext/options.py:53:32: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `name`
- Found 289 diagnostics
+ Found 290 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/structure/files.py:553:9: error[no-matching-overload] No overload of bound method `sort` matches arguments
- Found 183 diagnostics
+ Found 184 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/files.py:24:40: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `AnyStr`
+ isort/files.py:35:44: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `AnyStr`
- Found 37 diagnostics
+ Found 39 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/listing.py:300:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `LocalPath` and `AnyStr`
+ boostedblob/listing.py:300:29: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `AnyStr`
- Found 26 diagnostics
+ Found 28 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ mkosi/config.py:784:30: error[no-matching-overload] No overload of function `fspath` matches arguments
- Found 86 diagnostics
+ Found 87 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/_reloader.py:94:20: error[no-matching-overload] No overload of function `basename` matches arguments
+ src/werkzeug/_reloader.py:108:45: error[no-matching-overload] No overload of function `dirname` matches arguments
- src/werkzeug/datastructures/file_storage.py:133:38: error[invalid-argument-type] Argument to function `copyfileobj` is incorrect: Expected `SupportsWrite[Unknown]`, found `IO[bytes] | (Unknown & ~str) | str | PathLike[str]`
+ src/werkzeug/datastructures/file_storage.py:133:38: error[invalid-argument-type] Argument to function `copyfileobj` is incorrect: Expected `SupportsWrite[AnyStr]`, found `IO[bytes] | (Unknown & ~str) | str | PathLike[str]`
+ src/werkzeug/wrappers/request.py:310:13: error[unresolved-attribute] Type `V` has no attribute `close`
- Found 366 diagnostics
+ Found 369 diagnostics

vision (https://github.com/pytorch/vision)
+ test/test_extended_models.py:291:29: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:294:28: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:295:31: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:298:34: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:302:45: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:305:33: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:307:24: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:312:24: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:319:20: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:331:42: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:334:16: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `name`
+ test/test_extended_models.py:337:35: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `meta`
+ test/test_extended_models.py:395:22: error[unresolved-attribute] Type `_EnumMemberT` has no attribute `transforms`
- Found 1483 diagnostics
+ Found 1496 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/apply_external_resources.py:22:25: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Unknown | Literal[".applied"]`
+ paasta_tools/autoscaling/max_all_k8s_services.py:30:29: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_max_instances`
+ paasta_tools/autoscaling/max_all_k8s_services.py:32:41: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `format_kubernetes_app`
+ paasta_tools/check_autoscaler_max_instances.py:63:20: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_autoscaling_metric_spec`
+ paasta_tools/check_autoscaler_max_instances.py:80:17: error[invalid-argument-type] Argument to function `autoscaling_status` is incorrect: Expected `LongRunningServiceConfig`, found `InstanceConfig_T`
+ paasta_tools/check_autoscaler_max_instances.py:85:28: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_sanitised_deployment_name`
+ paasta_tools/check_autoscaler_max_instances.py:103:44: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_autoscaling_params`
+ paasta_tools/cli/cmds/status.py:2315:39: error[not-iterable] Object of type `_T` is not iterable
+ paasta_tools/contrib/timeouts_metrics_prom.py:62:49: error[invalid-argument-type] Argument to function `read_and_write_timeouts_metrics` is incorrect: Expected `str`, found `AnyStr`
+ paasta_tools/contrib/timeouts_metrics_prom.py:62:55: error[invalid-argument-type] Argument to function `read_and_write_timeouts_metrics` is incorrect: Expected `str`, found `str | bytes`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:245:21: error[invalid-argument-type] Argument to function `get_secrets_used_by_instance` is incorrect: Expected `KubernetesDeploymentConfig`, found `InstanceConfig_T`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:502:37: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_datastore_credentials`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:547:32: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_datastore_credentials_signature_name`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:548:29: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_datastore_credentials_secret_name`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:580:27: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_crypto_keys_from_config`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:599:63: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_sanitised_deployment_name`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:612:32: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_crypto_secret_signature_name`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:615:29: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_crypto_secret_name`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:665:32: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_boto_secret_signature_name`
+ paasta_tools/kubernetes/bin/paasta_secrets_sync.py:666:29: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_boto_secret_name`
+ paasta_tools/long_running_service_tools.py:676:40: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_registrations`
+ paasta_tools/long_running_service_tools.py:677:31: error[unresolved-attribute] Type `InstanceConfig_T` has no attribute `get_instances`
+ paasta_tools/setup_prometheus_adapter_config.py:859:25: error[invalid-argument-type] Argument to function `get_rules_for_service_instance` is incorrect: Expected `KubernetesDeploymentConfig`, found `InstanceConfig_T`
- Found 881 diagnostics
+ Found 904 diagnostics

colour (https://github.com/colour-science/colour)
+ colour/utilities/network.py:2270:30: error[not-iterable] Object of type `_T` is not iterable
+ colour/utilities/network.py:2441:30: error[not-iterable] Object of type `_T` is not iterable
- Found 478 diagnostics
+ Found 480 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ django-stubs/db/models/fields/related.pyi:245:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
- Found 431 diagnostics
+ Found 432 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/application/handlers/code.py:193:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def _monkeypatch_io(loggers: dict[str, (...) -> None]) -> dict[str, Any]`
+ src/bokeh/application/handlers/code.py:193:1: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[_T_co]`, found `def _monkeypatch_io(loggers: dict[str, (...) -> None]) -> dict[str, Any]`

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/web_app.py:55:18: error[missing-argument] No argument provided for required parameter `_YieldT_co` of class `Signal`
+ aiohttp/web_app.py:56:26: error[missing-argument] No argument provided for required parameter `_YieldT_co` of class `Signal`
- Found 168 diagnostics
+ Found 170 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/utils/universal.py:1719:39: error[invalid-argument-type] Argument is incorrect: Expected `_T`, found `_T`
+ mesonbuild/utils/universal.py:1719:67: error[invalid-argument-type] Argument is incorrect: Expected `_T`, found `_T`
- unittests/internaltests.py:1596:9: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def mock_trial(value: str) -> Iterable[None]`
+ unittests/internaltests.py:1596:9: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[_T_co]`, found `def mock_trial(value: str) -> Iterable[None]`
- unittests/internaltests.py:1660:9: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[Unknown]`, found `def mock_trial(value: str) -> Iterable[None]`
+ unittests/internaltests.py:1660:9: error[invalid-argument-type] Argument to function `contextmanager` is incorrect: Expected `(...) -> Iterator[_T_co]`, found `def mock_trial(value: str) -> Iterable[None]`
+ unittests/pythontests.py:72:85: error[no-matching-overload] No overload of function `splitext` matches arguments
- Found 773 diagnostics
+ Found 776 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/builders/__init__.py:217:23: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:220:27: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:222:33: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:226:40: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:227:63: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:244:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:246:29: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/__init__.py:270:13: error[unresolved-attribute] Type `T` has no attribute `write_mo`
+ sphinx/builders/__init__.py:784:32: error[invalid-argument-type] Argument to function `_write_docname` is incorrect: Expected `str`, found `T`
+ sphinx/builders/_epub_base.py:287:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `N`, in comparing `Literal["refuri"]` with `N`
+ sphinx/builders/_epub_base.py:288:42: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/_epub_base.py:290:21: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/_epub_base.py:291:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `N`, in comparing `Literal["refid"]` with `N`
+ sphinx/builders/_epub_base.py:292:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/_epub_base.py:292:60: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/_epub_base.py:347:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `N`
+ sphinx/builders/gettext.py:280:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `T`
+ sphinx/builders/gettext.py:308:36: error[not-iterable] Object of type `T` is not iterable
+ sphinx/builders/html/__init__.py:972:29: error[unsupported-operator] Operator `in` is not supported for types `str` and `N`, in comparing `Literal["scale", "width", "height"]` with `N`
+ sphinx/builders/html/__init__.py:979:40: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/html/__init__.py:983:23: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/html/__init__.py:989:17: error[unresolved-attribute] Type `N` has no attribute `replace_self`
+ sphinx/builders/latex/__init__.py:403:23: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/latex/__init__.py:404:24: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/latex/__init__.py:414:13: error[unresolved-attribute] Type `N` has no attribute `replace_self`
+ sphinx/builders/manpage.py:98:17: error[unresolved-attribute] Type `N` has no attribute `replace_self`
+ sphinx/builders/manpage.py:98:42: error[unresolved-attribute] Type `N` has no attribute `children`
+ sphinx/builders/singlehtml.py:65:16: error[unsupported-operator] Operator `not in` is not supported for types `str` and `N`, in comparing `Literal["refuri"]` with `N`
+ sphinx/builders/singlehtml.py:67:22: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/singlehtml.py:74:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/texinfo.py:164:23: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/texinfo.py:165:24: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/builders/texinfo.py:175:13: error[unresolved-attribute] Type `N` has no attribute `replace_self`
+ sphinx/builders/xml.py:67:31: error[unresolved-attribute] Type `N` has no attribute `attributes`
+ sphinx/builders/xml.py:69:21: error[unresolved-attribute] Type `N` has no attribute `attributes`
+ sphinx/builders/xml.py:70:25: error[unresolved-attribute] Type `N` has no attribute `attributes`
+ sphinx/directives/__init__.py:311:32: error[unresolved-attribute] Type `N` has no attribute `get`
+ sphinx/environment/__init__.py:696:17: error[invalid-argument-type] Argument to function `_resolve_toctree` is incorrect: Expected `toctree`, found `N | Unknown`
+ sphinx/environment/__init__.py:704:17: warning[possibly-unbound-attribute] Attribute `replace_self` on type `N | Unknown` is possibly unbound
+ sphinx/environment/adapters/toctree.py:66:9: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/adapters/toctree.py:66:26: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/adapters/toctree.py:217:25: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/adapters/toctree.py:218:57: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/adapters/toctree.py:219:13: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/adapters/toctree.py:219:43: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:58:13: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:59:22: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:71:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:86:32: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:87:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:88:35: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:89:20: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:90:21: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:155:26: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:157:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/asset.py:172:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/title.py:52:13: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/environment/collectors/toctree.py:279:25: error[unresolved-attribute] Type `N` has no attribute `get`
+ sphinx/environment/collectors/toctree.py:283:35: error[invalid-argument-type] Argument to function `_walk_toctree` is incorrect: Expected `toctree`, found `N`
+ sphinx/ext/autosectionlabel.py:41:19: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/ext/autosectionlabel.py:43:37: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
- sphinx/ext/doctest.py:446:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/ext/doctest.py:452:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/ext/doctest.py:458:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/ext/doctest.py:461:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sphinx/ext/doctest.py:463:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sphinx/ext/todo.py:94:49: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/ext/todo.py:185:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `N`, in comparing `Literal["refdoc"]` with `N`
+ sphinx/ext/todo.py:186:17: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/ext/viewcode.py:293:27: error[not-iterable] Object of type `T` is not iterable
+ sphinx/search/__init__.py:533:70: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/util/nodes.py:417:9: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/util/nodes.py:531:28: error[unsupported-operator] Operator `not in` is not supported for types `str` and `N`, in comparing `Literal["docname"]` with `N`
+ sphinx/util/nodes.py:532:29: error[non-subscriptable] Cannot subscript object of type `N` with no `__getitem__` method
+ sphinx/util/nodes.py:721:37: error[invalid-argument-type] Argument to function `_only_node_keep_children` is incorrect: Expected `only`, found `N`
+ sphinx/util/nodes.py:722:13: error[unresolved-attribute] Type `N` has no attribute `replace_self`
+ sphinx/util/nodes.py:722:31: error[unresolved-attribute] Type `N` has no attribute `children`
+ sphinx/util/nodes.py:728:13: error[unresolved-attribute] Type `N` has no attribute `replace_self`
- sphinx/versioning.py:44:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 532 diagnostics
+ Found 599 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/contentviews/_view_xml_html.py:219:34: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:219:41: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:219:55: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:219:62: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:226:30: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:226:37: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:226:51: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:226:58: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:236:26: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:236:33: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:236:47: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
+ mitmproxy/contentviews/_view_xml_html.py:236:54: error[invalid-argument-type] Argument to function `is_inline` is incorrect: Expected `Token | None`, found `T | None`
- Found 1814 diagnostics
+ Found 1826 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/docker/docker_image.py:74:17: error[missing-argument] No argument provided for required parameter `context` of function `build_image`
- src/prefect/docker/docker_image.py:77:13: error[missing-argument] No argument provided for required parameter `context` of function `build_image`
- src/prefect/task_engine.py:1710:16: error[invalid-return-type] Return type does not match returned value: expected `R | State[Any] | None | Coroutine[Any, Any, R | State[Any] | None]`, found `Generator[Unknown, None, None]`
+ src/prefect/task_engine.py:1710:16: error[invalid-return-type] Return type does not match returned value: expected `R | State[Any] | None | Coroutine[Any, Any, R | State[Any] | None]`, found `Generator[R, None, None]`
- src/prefect/utilities/asyncutils.py:376:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `Overload[(value: None = None, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[None, None], (value: R, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[R, None]]`
+ src/prefect/utilities/asyncutils.py:376:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[_T_co]`, found `Overload[(value: None = None, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[None, None], (value: R, *args: Any, **kwargs: Any) -> AbstractAsyncContextManager[R, None]]`
- Found 3712 diagnostics
+ Found 3710 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/appsec/_iast/_taint_tracking/_taint_objects.py:78:35: error[invalid-argument-type] Argument to function `copy_ranges_to_string` is incorrect: Expected `str`, found `_T`
+ ddtrace/vendor/psutil/_pslinux.py:1109:24: error[no-matching-overload] No overload of function `basename` matches arguments
- tests/internal/test_wrapping.py:231:10: error[unresolved-attribute] Type `(...) -> _AsyncGeneratorContextManager[Unknown, None]` has no attribute `__wrapped__`
+ tests/internal/test_wrapping.py:231:10: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__wrapped__`
- Found 6513 diagnostics
+ Found 6515 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/lib/upload/local.py:133:36: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Literal["thumbnail"]`
+ zerver/tests/test_auth_backends.py:8035:26: error[invalid-argument-type] Argument to bound method `set` is incorrect: Argument type `Unknown | int` does not satisfy upper bound of type variable `Self`
- Found 7418 diagnostics
+ Found 7420 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/db/dbhandler.py:3234:25: error[no-matching-overload] No overload of bound method `search` matches arguments
+ rotkehlchen/tests/test_no_missing_init.py:11:25: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Literal["__pycache__"]`
- Found 1562 diagnostics
+ Found 1564 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
+ misc/python/materialize/mzbuild.py:1351:29: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Literal["python"]`
+ misc/python/materialize/version_list.py:517:41: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `str | IOBase`, found `Self`
- Found 3244 diagnostics
+ Found 3246 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/core/tests/test_args.py:53:25: error[unsupported-operator] Operator `+` is unsupported between objects of type `str | bytes` and `str | bytes`
+ sympy/polys/domains/algebraicfield.py:21:49: error[missing-argument] No argument provided for required parameter `Er` of class `SimpleDomain`
+ sympy/polys/domains/field.py:11:13: error[missing-argument] No argument provided for required parameter `Er` of class `Ring`
+ sympy/polys/domains/integerring.py:21:19: error[missing-argument] No argument provided for required parameter `Er` of class `Ring`
+ sympy/polys/domains/polynomialring.py:23:22: error[missing-argument] No argument provided for required parameter `Er` of class `Ring`
- sympy/polys/domains/tests/test_domains.py:818:43: error[not-iterable] Object of type `Unknown | MPZ` may not be iterable
- sympy/polys/matrices/tests/test_ddm.py:134:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | MPZ` and `DDM`
- sympy/polys/matrices/tests/test_ddm.py:148:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | MPZ` and `DDM`
- sympy/polys/numberfields/minpoly.py:62:46: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `object`
- sympy/polys/numberfields/minpoly.py:63:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `object`
- sympy/polys/numberfields/minpoly.py:698:39: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `object`, in comparing `Unknown | Dummy` with `object`
+ sympy/polys/polyclasses.py:183:62: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:183:79: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
- sympy/polys/polyclasses.py:1364:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ sympy/polys/polyclasses.py:1363:63: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1379:37: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1407:28: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
+ sympy/polys/polyclasses.py:1419:41: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1443:36: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1455:67: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1461:41: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1467:38: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1471:38: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1952:29: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1960:41: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
+ sympy/polys/polyclasses.py:1960:57: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:1999:36: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
+ sympy/polys/polyclasses.py:2017:67: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
+ sympy/polys/polyclasses.py:2022:41: error[missing-argument] No argument provided for required parameter `Er` of class `DMP_Python`
+ sympy/polys/polyclasses.py:2027:38: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
+ sympy/polys/polyclasses.py:2032:38: error[missing-argument] No argument provided for required parameter `Er` of class `DUP_Flint`
- Found 12934 diagnostics
+ Found 12951 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/signal/tests/test_signaltools.py:1343:40: error[not-iterable] Object of type `_T` is not iterable
+ scipy/special/utils/makenpz.py:59:23: error[unsupported-operator] Operator `+` is unsupported between objects of type `str | bytes` and `str | bytes`
+ tools/check_python_h_first.py:169:33: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `Literal["build", ".git", "boost"]`
+ tools/check_python_h_first.py:173:29: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AnyStr`, found `str`
+ tools/check_python_h_first.py:178:20: error[no-matching-overload] No overload of function `splitext` matches arguments
+ tools/check_test_name.py:171:30: error[invalid-argument-type] Argument to function `main` is incorrect: Expected `str`, found `Self`
- Found 6784 diagnostics
+ Found 6790 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~73MB
+     memo metadata = ~76MB

```
</details>


---

_Converted to draft by @dcreager on 2025-07-29 21:28_

---

_Comment by @codspeed-hq[bot] on 2025-07-29 21:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fvisitor-recursion?runnerMode=Instrumentation)

### Merging #19625 will **degrade performances by 35.67%**

<sub>Comparing <code>dcreager/visitor-recursion</code> (11ee8fb) with <code>main</code> (04a8f64)</sub>



### Summary

`❌ 1` regressions  
`✅ 40` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fvisitor-recursion?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` DateType `` | 230.1 ms | 357.7 ms | -35.67% |


---

_Comment by @codspeed-hq[bot] on 2025-07-29 21:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fvisitor-recursion?runnerMode=WallTime)

### Merging #19625 will **degrade performances by 4.39%**

<sub>Comparing <code>dcreager/visitor-recursion</code> (11ee8fb) with <code>main</code> (04a8f64)</sub>



### Summary

`❌ 1` regressions  
`✅ 6` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fvisitor-recursion?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` medium[colour-science] `` | 7.5 s | 7.8 s | -4.39% |


---

_Comment by @dcreager on 2025-07-29 21:29_

This should be a no-op; moving to draft while I investigate the ecosystem changes

---

_Comment by @dcreager on 2025-08-28 23:57_

Superseded by https://github.com/astral-sh/ruff/pull/20124

---

_Closed by @dcreager on 2025-08-28 23:57_

---

_Branch deleted on 2025-08-28 23:58_

---

_Comment by @carljm on 2025-08-29 00:05_

I forgot about this PR -- it does some nice things that my PR didn't, that would be great to revisit someday. But apparently it also changed semantics, which mine didn't 😆 

---
