```yaml
number: 18587
title: "[ty] Add decorator check for implicit attribute assignments"
type: pull_request
state: merged
author: med1844
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: check_assignments_in_classmethods
created_at: 2025-06-09T02:54:18Z
updated_at: 2025-06-26T03:52:31Z
url: https://github.com/astral-sh/ruff/pull/18587
synced_at: 2026-01-12T15:56:22Z
```

# [ty] Add decorator check for implicit attribute assignments

---

_@med1844_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Previously, the checks for implicit attribute assignments didn't properly account for method decorators. This PR fixes that by:

- Adding a decorator check in `implicit_instance_attribute`. This allows it to filter out methods with mismatching decorators when analyzing attribute assignments.
- Adding attribute search for implicit class attributes: if an attribute can't be found directly in the class body, the `ClassLiteral::own_class_member` function will now search in classmethods.
- Adding `staticmethod`: it has been added into `KnownClass` and together with the new decorator check, it will no longer expose attributes when the assignment target name is the same as the first method name.

If accepted, it should fix https://github.com/astral-sh/ty/issues/205 and https://github.com/astral-sh/ty/issues/207.

## Test Plan

<!-- How was it tested? -->

This is tested with existing mdtest suites and is able to get most of the TODO marks for implicit assignments in classmethods and staticmethods removed.

However, there's one specific test case I failed to figure out how to correctly resolve:

https://github.com/med1844/ruff/blob/b279508bdc63c1ed6fc4ccf9d43d3719fe7a202b/crates/ty_python_semantic/resources/mdtest/attributes.md?plain=1#L754-L755

I tried to add `instance_member().is_unbound()` check in this [else branch](https://github.com/med1844/ruff/blob/b279508bdc63c1ed6fc4ccf9d43d3719fe7a202b/crates/ty_python_semantic/src/types/infer.rs#L3299-L3301) but it causes tests with class attributes defined in class body to fail. While it's possible to implicitly add `ClassVar` to qualifiers to make this assignment fail and keep everything else passing, it doesn't feel like the right solution.

Another problem is that this PR also kind of breaks `dependency_implicit_instance_attribute` and `dependency_own_instance_member` in `src/types/infer.rs`. This seems to be caused by the new decorator check, which uses the inferred method function type to get the inferred decorator type. Thus, any change inside function body causes re-inferring method type. As a result, both "no re-infer" assertions in these two tests fails. I'm not 100% sure, but that's my current theory.

Thank you very much for taking time to review 🙏

---

_Review requested from @carljm by @med1844 on 2025-06-09 02:54_

---

_Review requested from @AlexWaygood by @med1844 on 2025-06-09 02:54_

---

_Review requested from @sharkdp by @med1844 on 2025-06-09 02:54_

---

_Review requested from @dcreager by @med1844 on 2025-06-09 02:54_

---

_Renamed from "Add decorator check for implicit attribute assignments" to "[ty] Add decorator check for implicit attribute assignments" by @med1844 on 2025-06-09 02:55_

---

_Comment by @github-actions[bot] on 2025-06-09 02:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- warning[possibly-unbound-attribute] tests/test_text.py:247:9: Attribute `link` on type `str | Style` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_text.py:247:9: Attribute `link` on type `Unknown | str | Style` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_text.py:261:9: Attribute `link` on type `str | Style` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_text.py:261:9: Attribute `link` on type `Unknown | str | Style` is possibly unbound

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ error[invalid-assignment] tests/test_psql.py:40:9: Object of type `Literal[0]` is not assignable to attribute `closed` on type `Unknown | None`
+ error[invalid-assignment] tests/test_psql.py:50:9: Object of type `Literal[2]` is not assignable to attribute `closed` on type `Unknown | None`
+ error[invalid-assignment] tests/test_psql.py:83:9: Object of type `Literal[0]` is not assignable to attribute `closed` on type `Unknown | None`
- Found 166 diagnostics
+ Found 169 diagnostics

colour (https://github.com/colour-science/colour)
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:221:13: Object of type `SpectralShape` is not assignable to attribute `_shape` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:222:13: Object of type `MultiSpectralDistributions` is not assignable to attribute `_cmfs` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:222:24: Object of type `SpectralDistribution` is not assignable to attribute `_sd_D65` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:223:13: Object of type `Unknown` is not assignable to attribute `_XYZ_D65` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:224:13: Object of type `Unknown` is not assignable to attribute `_xy_D65` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:226:13: Object of type `RGB_Colourspace` is not assignable to attribute `_RGB_colourspace` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[invalid-assignment] colour/recovery/tests/test_jakob2019.py:228:13: Object of type `LUT3D_Jakob2019` is not assignable to attribute `_LUT` on type `type[TestLUT3D_Jakob2019] & ~<Protocol with members '_LUT'>`
- error[unresolved-attribute] colour/recovery/tests/test_jakob2019.py:231:16: Attribute `_LUT` can only be accessed on instances, not on the class object `type[TestLUT3D_Jakob2019]` itself.
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:221:13: Attribute `_shape` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:222:13: Attribute `_cmfs` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:222:24: Attribute `_sd_D65` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:223:13: Attribute `_XYZ_D65` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:224:13: Attribute `_xy_D65` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:226:13: Attribute `_RGB_colourspace` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:228:13: Attribute `_LUT` on type `type[TestLUT3D_Jakob2019]` is possibly unbound
+ warning[possibly-unbound-attribute] colour/recovery/tests/test_jakob2019.py:231:16: Attribute `_LUT` on type `type[TestLUT3D_Jakob2019]` is possibly unbound

bandersnatch (https://github.com/pypa/bandersnatch)
+ error[missing-argument] src/bandersnatch_storage_plugins/swift.py:172:16: No argument provided for required parameter `path` of bound method `mkdir`
+ error[missing-argument] src/bandersnatch_storage_plugins/swift.py:178:13: No argument provided for required parameter `path` of bound method `delete_file`
+ error[missing-argument] src/bandersnatch_storage_plugins/swift.py:187:16: No arguments provided for required parameters `source`, `dest` of bound method `copy_file`
+ error[missing-argument] src/bandersnatch_storage_plugins/swift.py:192:13: No argument provided for required parameter `path` of bound method `rmdir`
+ error[missing-argument] src/bandersnatch_storage_plugins/swift.py:199:16: No arguments provided for required parameters `source`, `dest` of bound method `copy_file`
+ error[missing-argument] src/bandersnatch_storage_plugins/swift.py:203:16: No arguments provided for required parameters `source`, `dest` of bound method `copy_file`
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:128:13: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:138:14: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:140:17: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:156:14: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:158:17: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:172:16: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:178:13: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:187:16: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:192:13: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:199:16: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:203:16: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:213:16: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- error[unresolved-attribute] src/bandersnatch_storage_plugins/swift.py:219:16: Attribute `BACKEND` can only be accessed on instances, not on the class object `<class '_SwiftAccessor'>` itself.
- Found 132 diagnostics
+ Found 125 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-return-type] examples/contrib/ntlm_upstream_proxy.py:79:33: Return type does not match returned value: expected `HTTPFlow`, found `(@Todo(Type::Intersection.call()) & HTTPFlow) | None`
+ error[invalid-return-type] examples/contrib/ntlm_upstream_proxy.py:79:33: Return type does not match returned value: expected `HTTPFlow`, found `@Todo(Type::Intersection.call()) | (@Todo(Type::Intersection.call()) & HTTPFlow) | None`

freqtrade (https://github.com/freqtrade/freqtrade)
- error[unresolved-attribute] freqtrade/rpc/api_server/deps.py:19:16: Attribute `_rpc` can only be accessed on instances, not on the class object `<class 'ApiServer'>` itself.
- error[invalid-attribute-access] freqtrade/rpc/api_server/webserver.py:80:13: Cannot assign to instance attribute `_rpc` from the class object `<class 'ApiServer'>`
- error[unresolved-attribute] freqtrade/rpc/api_server/webserver.py:89:13: Attribute `_rpc` can only be accessed on instances, not on the class object `<class 'ApiServer'>` itself.
- Found 445 diagnostics
+ Found 442 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-attribute] tests/test_closespider.py:30:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_closespider.py:40:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_closespider.py:59:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_closespider.py:77:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_closespider.py:87:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_closespider.py:89:36: Type `Spider | None` has no attribute `exception_cls`
- error[unresolved-attribute] tests/test_closespider.py:99:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_closespider.py:109:18: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_contracts.py:535:16: Type `Spider | None` has no attribute `visited`
- error[unresolved-attribute] tests/test_crawl.py:79:20: Type `Spider | None` has no attribute `urls_visited`
- error[unresolved-attribute] tests/test_crawl.py:127:16: Type `Spider | None` has no attribute `t1`
- error[unresolved-attribute] tests/test_crawl.py:128:16: Type `Spider | None` has no attribute `t2`
- error[unresolved-attribute] tests/test_crawl.py:129:16: Type `Spider | None` has no attribute `t2`
- error[unresolved-attribute] tests/test_crawl.py:129:36: Type `Spider | None` has no attribute `t1`
- error[unresolved-attribute] tests/test_crawl.py:135:16: Type `Spider | None` has no attribute `t1`
- error[unresolved-attribute] tests/test_crawl.py:136:16: Type `Spider | None` has no attribute `t2`
- error[unresolved-attribute] tests/test_crawl.py:137:16: Type `Spider | None` has no attribute `t2_err`
- error[unresolved-attribute] tests/test_crawl.py:138:16: Type `Spider | None` has no attribute `t2_err`
- error[unresolved-attribute] tests/test_crawl.py:138:40: Type `Spider | None` has no attribute `t1`
- error[unresolved-attribute] tests/test_crawl.py:143:16: Type `Spider | None` has no attribute `t1`
- error[unresolved-attribute] tests/test_crawl.py:144:16: Type `Spider | None` has no attribute `t2`
- error[unresolved-attribute] tests/test_crawl.py:145:16: Type `Spider | None` has no attribute `t2_err`
- error[unresolved-attribute] tests/test_crawl.py:146:16: Type `Spider | None` has no attribute `t2_err`
- error[unresolved-attribute] tests/test_crawl.py:146:40: Type `Spider | None` has no attribute `t1`
- error[unresolved-attribute] tests/test_crawl.py:243:16: Type `Spider | None` has no attribute `visited`
- error[unresolved-attribute] tests/test_crawl.py:252:16: Type `Spider | None` has no attribute `visited`
- error[unresolved-attribute] tests/test_crawl.py:325:31: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:326:34: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:328:39: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:331:39: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:334:39: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:337:39: Type `Spider | None` has no attribute `meta`
- error[invalid-argument-type] tests/test_crawl.py:347:42: Argument to function `get_engine_status` is incorrect: Expected `ExecutionEngine`, found `Unknown | ExecutionEngine | None`
- warning[possibly-unbound-attribute] tests/test_crawl.py:355:43: Attribute `name` on type `Spider | None` is possibly unbound
- error[invalid-argument-type] tests/test_crawl.py:365:45: Argument to function `format_engine_status` is incorrect: Expected `ExecutionEngine`, found `Unknown | ExecutionEngine | None`
- warning[possibly-unbound-attribute] tests/test_crawl.py:380:43: Attribute `name` on type `Spider | None` is possibly unbound
- error[unresolved-attribute] tests/test_crawl.py:634:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:641:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:654:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:664:22: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:673:22: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:681:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:682:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:683:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:683:56: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:687:17: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:688:15: Type `Spider | None` has no attribute `full_response_length`
- error[unresolved-attribute] tests/test_crawl.py:695:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:696:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:697:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:698:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:699:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:701:34: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:703:17: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:704:15: Type `Spider | None` has no attribute `full_response_length`
- error[unresolved-attribute] tests/test_crawl.py:711:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:712:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:713:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:713:59: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:721:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:722:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:723:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:724:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:725:16: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_crawl.py:727:37: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_downloader_handlers_http_base.py:617:19: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_downloader_handlers_http_base.py:628:19: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_downloader_handlers_http_base.py:630:18: Type `Spider | None` has no attribute `meta`
- warning[possibly-unbound-attribute] tests/test_downloaderslotssettings.py:69:17: Attribute `downloader` on type `ExecutionEngine | None` is possibly unbound
- error[unresolved-attribute] tests/test_downloaderslotssettings.py:70:17: Type `Spider | None` has no attribute `times`
- error[invalid-assignment] tests/test_pqueues.py:101:9: Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
- error[unresolved-attribute] tests/test_proxy_connect.py:116:27: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_attribute_binding.py:74:20: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_attribute_binding.py:83:23: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_attribute_binding.py:100:19: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_attribute_binding.py:133:20: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_attribute_binding.py:166:20: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_attribute_binding.py:189:20: Type `Spider | None` has no attribute `meta`
- error[unresolved-attribute] tests/test_request_cb_kwargs.py:167:20: Type `Spider | None` has no attribute `checks`
- error[unresolved-attribute] tests/test_request_cb_kwargs.py:168:20: Type `Spider | None` has no attribute `checks`
- error[unresolved-attribute] tests/test_request_left.py:41:16: Type `Spider | None` has no attribute `caught_times`
- error[unresolved-attribute] tests/test_request_left.py:47:16: Type `Spider | None` has no attribute `caught_times`
- error[unresolved-attribute] tests/test_request_left.py:53:16: Type `Spider | None` has no attribute `caught_times`
- error[unresolved-attribute] tests/test_request_left.py:59:16: Type `Spider | None` has no attribute `caught_times`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:189:20: Type `Spider | None` has no attribute `skipped`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:189:44: Type `Spider | None` has no attribute `skipped`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:190:16: Type `Spider | None` has no attribute `parsed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:191:16: Type `Spider | None` has no attribute `failed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:204:16: Type `Spider | None` has no attribute `parsed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:205:16: Type `Spider | None` has no attribute `skipped`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:206:16: Type `Spider | None` has no attribute `failed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:219:16: Type `Spider | None` has no attribute `parsed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:220:16: Type `Spider | None` has no attribute `failed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:231:16: Type `Spider | None` has no attribute `parsed`
- error[unresolved-attribute] tests/test_spidermiddleware_httperror.py:232:16: Type `Spider | None` has no attribute `failed`
- Found 1253 diagnostics
+ Found 1158 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ warning[unused-ignore-comment] testing/test_terminal.py:1988:33: Unused blanket `type: ignore` directive
- Found 647 diagnostics
+ Found 648 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- warning[possibly-unbound-attribute] src/prefect/server/orchestration/global_policy.py:460:13: Attribute `state_details` on type `(Unknown & State) | (Unknown & None) | Unknown | State | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/prefect/server/orchestration/global_policy.py:460:13: Attribute `state_details` on type `Unknown | (Unknown & None) | (State & @Todo(specialized non-generic class)) | None` is possibly unbound

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-assignment] tests/ci_visibility/api/fake_runner_efd_all_pass.py:25:5: Object of type `int` is not assignable to attribute `start_ns` on type `Span | None`
+ error[invalid-assignment] tests/ci_visibility/api/fake_runner_efd_all_pass.py:25:5: Object of type `Unknown | int` is not assignable to attribute `start_ns` on type `Span | None`
- error[invalid-assignment] tests/ci_visibility/api/fake_runner_efd_mix_fail.py:25:5: Object of type `int` is not assignable to attribute `start_ns` on type `Span | None`
+ error[invalid-assignment] tests/ci_visibility/api/fake_runner_efd_mix_fail.py:25:5: Object of type `Unknown | int` is not assignable to attribute `start_ns` on type `Span | None`
- error[invalid-assignment] tests/ci_visibility/api/fake_runner_efd_mix_pass.py:25:5: Object of type `int` is not assignable to attribute `start_ns` on type `Span | None`
+ error[invalid-assignment] tests/ci_visibility/api/fake_runner_efd_mix_pass.py:25:5: Object of type `Unknown | int` is not assignable to attribute `start_ns` on type `Span | None`

zulip (https://github.com/zulip/zulip)
+ error[invalid-assignment] zerver/lib/narrow.py:882:17: Object of type `Unknown | str | int | list[int]` is not assignable to `str | int`
+ error[invalid-argument-type] zerver/lib/narrow.py:911:70: Argument to function `get_stream_by_narrow_operand_access_unchecked` is incorrect: Expected `str | int`, found `Unknown | str | int | list[int]`
+ error[invalid-argument-type] zerver/lib/narrow.py:945:69: Argument to function `maybe_rename_general_chat_to_empty_topic` is incorrect: Expected `str`, found `Unknown | str | int | list[int]`
+ error[unsupported-operator] zerver/views/message_fetch.py:209:42: Operator `+` is unsupported between objects of type `Literal["is:"]` and `Unknown | str | int | list[int]`
- Found 7465 diagnostics
+ Found 7469 diagnostics

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-06-09 03:00_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/med1844%3Acheck_assignments_in_classmethods?runnerMode=Instrumentation)

### Merging #18587 will **degrade performances by 6.91%**

<sub>Comparing <code>med1844:check_assignments_in_classmethods</code> (08be875) with <code>main</code> (ca79338)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 36` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_micro[many_string_assignments] `` | 66.5 ms | 71.5 ms | -6.91% |


---

_Comment by @med1844 on 2025-06-09 03:15_

mypy primer looks really bad... converting to draft for now. I think I might have messed up staticmethods...

---

_Converted to draft by @med1844 on 2025-06-09 03:15_

---

_Comment by @med1844 on 2025-06-09 04:36_

Removing the mapping `"staticmethod" => Self::StaticMethod` in `KnownClass::try_from_file_and_name` seems to fix mypy primer, but this will break the known class round trip test. Will look further into this tomorrow.

---

_Label `ty` added by @AlexWaygood on 2025-06-09 10:26_

---

_Comment by @sharkdp on 2025-06-10 11:21_

Thank you for working on this. I'm not doing a review for now since it's still a draft. Let us know when you need help with this. 

---

_Comment by @med1844 on 2025-06-11 04:41_

It took me a good while to manually check why there's so many more diagnostics in `mypy_primer`. For example in [`apprise`](https://github.com/caronc/apprise/tree/master) there's 774 more diagnostics.

I thought it's me that screwed up some `staticmethod` property, but further inspection shows that it's because now `staticmethod` are supported, it actually enables `ty` to check more code, and the diagnostics yielded are actually correct.

As for `apprise`, it turns out it's caused by the combination of checking more code, and low quality `.pyi` stub files. For example, `NotifyBase` defined in [normal python file](https://github.com/caronc/apprise/blob/master/apprise/plugins/base.py#L47) inherits `URLBase`, but in the [stub file](https://github.com/caronc/apprise/blob/master/apprise/plugins/base.pyi#L1) it doesn't! I checked some other diagnostics in `apprise` and they all seem to point to some faulty `.pyi` file. By modifying this `.pyi` to inherit `URLBase` and re-run `ty` again locally, I was able to get rid of 61 diagnostics...

So, the conclusion is that to my best knowledge, the `staticmethod` implementation itself is ok. I'm change this PR to be ready for review.

---

_Marked ready for review by @med1844 on 2025-06-11 04:41_

---

_@MichaReiser reviewed on 2025-06-11 05:42_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-11 05:42_

Can you tell us more what the reason is that these tests are commented out?

---

_@med1844 reviewed on 2025-06-11 15:07_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-11 15:07_

Unfortunately it seems to be related to the new `implicit_instance_attribute` call I added in `ClassLiteral::own_class_member`. Adding that call alone would crash these two tests.

Edit: My previous analysis is not correct

---

_@MichaReiser reviewed on 2025-06-12 06:15_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-12 06:15_

We probably need to fix this. It suggests that we now have queries that introduce cross-module dependencies (where the query needs to re-run not if just one file changes, but if any file changes). 

A good starting point would be to figure out what the query trace/backtrace is when `implicit_instance_attribute` is called. The next step is then to discuss whether we need to introduce a new salsa query boundary or if the call shouldn't be happening at that place to begin with.

---

_Converted to draft by @med1844 on 2025-06-13 00:34_

---

_@med1844 reviewed on 2025-06-13 00:35_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-13 00:35_

Sounds good I will do some salsa crash course and see if I can figure this out. At the mean time I will mark this PR as draft.

---

_@med1844 reviewed on 2025-06-15 18:46_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-15 18:46_

After some deep dive into salsa internals, it seems to be caused by when calling `infer_expression_type(Id(1000))` (i.e. in `"/src/main.py"`) it tries to find `__new__` of class `C` in the class body using the newly added `implicit_instance_attribute` call.

This added `parsed_module(Id(409))` (i.e. `"/src/mod.py"`) alongside some other salsa queries as ingredients for `infer_expression_type(Id(1000))` to derive. This file is modified later, causing re-run and thus test failure.

Guard the newly added `implicit_instance_attribute` call with `if name != "__new__"` would make these two tests pass, though I'm worried that this might not be enough. I will look deeper into this and update the PR once I have a better understanding.

---

_@carljm reviewed on 2025-06-17 02:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 02:02_

The typical solution to these kinds of problems is to ensure that the cross-module information is protected by a Salsa query (that is, we call some Salsa query that ends up querying `parsed_module` for the source module of class C). That means that if the source module for class `C` changes in a way that doesn't impact the result of the intermediate query, other modules' type information won't be invalidated by that change.

---

_@carljm reviewed on 2025-06-17 02:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 02:02_

Thanks for working on this important feature! Let us know if there are other questions we can answer; these Salsa dependency issues are tricky.

---

_@med1844 reviewed on 2025-06-17 05:09_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 05:09_

Thank you very much for the tips, that's god-send for someone drowning in tracing log... though I'm not very sure how that cross-module information is protected in existing code base but I will keep digging into this when free, thank you!

---

_@med1844 reviewed on 2025-06-17 15:03_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 15:03_

I guess that means the newly created salsa query would take `parsed_module("mod.py")` and decide it doesn't change the query result in a meaningful way, so it would fail `shallow_verify_memo` but pass `deep_verify_memo`, and thus the new query as ingredient would not be marked as changed, as result `infer_expression_types` won't re-execute because this ingredient is not changing...? Is this what "new salsa query boundary" mean?

---

_@carljm reviewed on 2025-06-17 15:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 15:43_

Yes, that's an accurate description! Getting even further into the Salsa weeds, the unchanged output of the query in revision N+1 would be "back-dated" to revision N (because although it re-executed in revision N+1, its output did not change between N and N+1). This means that all other queries depending on that output don't need to re-execute in N+1.

---

_@carljm reviewed on 2025-06-17 15:45_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 15:45_

> I'm not very sure how that cross-module information is protected in existing code base

There's no specific single thing that ensures this protection -- we try to catch issues using regression tests like the one that is failing here. But in general we try to ensure that wherever we use `parsed_module` and rely on the AST, it is in a function that can only be called while analyzing that same module, not while analyzing some other module. And if we need such information while analyzing some other module, then we need to add a query to get it.

---

_@carljm reviewed on 2025-06-17 15:46_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-17 15:46_

I haven't looked into the details here, but from the description above, it sounds like we need the implicit-attribute lookup for `__new__` to go through a Salsa query. I'm not sure without looking in more detail whether there is an existing query we could be using here, or we need a new one.

---

_@med1844 reviewed on 2025-06-18 04:44_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-18 04:44_

It confuses me for a good while why all existing `implicit_instance_attribute` calls won't trigger `infer_expression_types` again even though it's called inside of that query. It turns out the upstream call to `Type::member_lookup_with_policy` was being tracked, and thus it's result won't change after the parsed module changed.
The place where I added the new call doesn't have a similar intermediate query, and thus the parsed module becomes a direct dependency of `infer_expression_types` and causing the re-run.
So yes, your analysis is correct, thanks for the guidance! Now the root cause is clear, it should be now possible to completely eliminate the bug.

---

_@med1844 reviewed on 2025-06-18 14:21_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-18 14:21_

It seems that `own_class_member` is also guarded by a salsa query (`Type::class_member_with_policy`), that's why it works for most inputs. The reason why `__new__` fails is because `Type::try_call_constructor` calls `Type::find_name_in_mro_with_policy` directly, which bypasses salsa tracked queries (`class_member_with_policy` and `member_lookup_with_policy`).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-18 18:41_

Thanks for tracking this down! This seems like a pre-existing issue which would have affected us anytime someone instantiated a class from a module other than the one it is defined in.

I think the ideal approach here is that in a separate PR, we add an explicit test for this case (that defining a class in one module and instantiating it in another module should not mean that a trivial change to the first module requires re-inferring types in the second), and we add a new tracked query method `lookup_dunder_new` (which is just a light wrapper around `find_name_in_mro_with_policy`), which should fix it. Then we rebase this PR after landing that separate fix.

Are you interested in taking on this fix?

---

_@carljm reviewed on 2025-06-18 18:41_

---

_@med1844 reviewed on 2025-06-19 04:13_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-19 04:13_

Thank you for help! I'm definitely interested. Though I don't think it's possible to create a test case that would capture the new dependency - it's introduced by the new call after all. There doesn't seem to exist any other downstream calls in `find_name_in_mro_with_policy` that would introduce dependency on `parsed_module`.

The new query sounds good. I'm also experimenting modifying `implicit_instance_attribute` to be tracked.

Regarding new PRs, I think it might also make sense to split out the staticmethod support into another separate PR, so we will have 3 PRs.


---

_@med1844 reviewed on 2025-06-19 23:09_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-19 23:09_

Confirmed that marking `implicit_instance_attribute` with `#[salsa::tracked]` would solve the issue, the code will pass tests without the `if name != "__new__"` guard.

I will work on the staticmethod PR and mark this one as ready to review once done.

---

_@carljm reviewed on 2025-06-20 17:13_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-20 17:13_

> I don't think it's possible to create a test case that would capture the new dependency - it's introduced by the new call after all. There doesn't seem to exist any other downstream calls in `find_name_in_mro_with_policy` that would introduce dependency on `parsed_module`.

Based on your description of the issue (and a brief look at the code), it looked to me like a pre-existing problem on main that could be triggered simply by defining a class in one module and then instantiating it in another module (because the instantiation will call `try_call_constructor`, which will call `find_name_in_mro_with_policy`, all with no query protection.) If this is the case, it should be possible to create a test case (on main) that does this, and fails on main because the using module is wrongly invalidated by an irrelevant change to the defining module? Or am I missing some other factor that requires a change in this PR to trigger the issue?

> I'm also experimenting modifying `implicit_instance_attribute` to be tracked.

I would expect this to fix the problem, and it could be a fine solution. The main issue is that there is a cost to every distinct memoized query-execution. So if memoizing `implicit_instance_attribute` means that in many common call paths we now go through two tracked queries instead of one, and the two memoizations are mostly redundant in terms of the work they avoid, this can harm efficiency. In this case it would be better to add a new query specific to the `__new__` lookup that is currently unguarded, so as to avoid unnecessary extra queries in other paths.

---

_@med1844 reviewed on 2025-06-21 03:52_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-21 03:52_

> it looked to me like a pre-existing problem on main that could be triggered simply by defining a class in one module and then instantiating it in another module

This won't trigger re-run. You can add the following test into `infer.rs` on main:

```rust
    #[test]
    fn dependency_cross_module_class_and_instance() -> anyhow::Result<()> {
        fn x_rhs_expression(db: &TestDb) -> Expression<'_> {
            let file_main = system_path_to_file(db, "/src/main.py").unwrap();
            let ast = parsed_module(db, file_main).load(db);
            // Get the second statement in `main.py` (x = …) and extract the expression
            // node on the right-hand side:
            let x_rhs_node = &ast.syntax().body[1].as_assign_stmt().unwrap().value;

            let index = semantic_index(db, file_main);
            index.expression(x_rhs_node.as_ref())
        }

        let mut db = setup_db();

        db.write_dedented(
            "/src/mod.py",
            r#"
            class C:
                pass
            "#,
        )?;
        db.write_dedented(
            "/src/main.py",
            r#"
            from mod import C
            x = C()
            "#,
        )?;

        let file_main = system_path_to_file(&db, "/src/main.py").unwrap();
        let _attr_ty = global_symbol(&db, file_main, "x").place.expect_type();

        // Add a comment; this should not trigger the type of `x` to be re-inferred
        db.write_dedented(
            "/src/mod.py",
            r#"
            class C:
                # comment
                pass
            "#,
        )?;

        let events = {
            db.clear_salsa_events();
            let _attr_ty = global_symbol(&db, file_main, "x").place.expect_type();
            db.take_salsa_events()
        };

        assert_function_query_was_not_run(
            &db,
            infer_expression_types,
            x_rhs_expression(&db),
            &events,
        );

        Ok(())
    }
```

This test passes. The reason is because for normal class member lookup, the call path looks like

`Type::member_lookup_with_policy/Type::class_member_with_policy (both tracked) -> Type::find_name_in_mro_with_policy -> ClassLiteral::class_member -> ClassLiteral::class_member_inner -> ClassLiteral::class_member_from_mro -> ClassType::own_class_member -> ClassLiteral::own_class_member`.

No other function in this call chain calls `parsed_module`. Therefore it's safe to directly call `find_name_in_mro_with_policy` to query `__new__`. It's the added call inside `own_class_member` that introduces dependency on `parsed_module`.

> avoid unnecessary extra queries in other paths.

Got it! I will try to create a query for `__new__` lookup. Though my previous attempt failed to get it compile. `fn lookup_dunder_new(self, _db: &'db dyn Db) -> PlaceAndQualifiers<'db>` compiles but returning `Option<PlaceAndQualifiers<'db>>` ended up compiler shouting at me "the trait bound `types::Type<'db>: SalsaStructInDb` is not satisfied". But I guess it's solvable, just need more time and effort.

---

_@carljm reviewed on 2025-06-21 20:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-21 20:43_

Ah, makes sense! Sorry I didn't fully grasp that this PR was adding the AST access within the `__new__` lookup.

> the trait bound `types::Type<'db>: SalsaStructInDb` is not satisfied

Ah! I think this is an unfortunate gotcha in Salsa, where the sole argument to a query function must be a Salsa input, interned struct, or tracked struct. If there are multiple arguments, they are implicitly combined into a hidden interned struct, but if there is only a single argument, and it is not a Salsa tracked struct, then you get this error.

In this case, the sole argument is `self`, which is a `Type`, which is not a Salsa struct.

The workaround is ugly, but you can just add a second unused argument of type `()`.

---

_@med1844 reviewed on 2025-06-21 22:26_

---

_Review comment by @med1844 on `crates/ty_python_semantic/src/types/infer.rs`:10356 on 2025-06-21 22:26_

No need to sorry and thank you for the tips! My previous workaround is to simply create `find_name_in_mro_with_policy_tracked` but the unit type with `lookup_dunder_new` seems much better here.

I think it's finally ready to review! Thank you very much again, it's been really helpful.

---

_Marked ready for review by @med1844 on 2025-06-21 22:57_

---

_Comment by @sharkdp on 2025-06-23 18:36_

Thank you very much for working on this! I am planning to review this tomorrow.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-24 08:04_

---

_@sharkdp approved on 2025-06-24 09:31_

Thank you very much — this is great!

In particular, thank you for going through the salsa debugging session and for adding a incremental-computation test.

While looking through the ecosystem changes, I noticed a behavior related to attributes that are implicitly defined in methods with an unknown decorator:
```py
class C:
    @unknown_decorator
    def f(self):
        self.x: int = 1

C.x  # this was previously an error
```
We now consider these attributes to be available on the class itself. I think this is actually a good example of the gradual guarantee: `unknown_decorator` *could*, in principle, be an alias to `builtins.classmethod`. And so it seems good to not show an error here. Because if `unknown_decorator` was annotated appropriately, we would not show an error as well.

I added a test to document this.

---

_Comment by @sharkdp on 2025-06-24 09:35_

> However, there's one specific test case I failed to figure out how to correctly resolve:
> 
> https://github.com/med1844/ruff/blob/b279508bdc63c1ed6fc4ccf9d43d3719fe7a202b/crates/ty_python_semantic/resources/mdtest/attributes.md?plain=1#L754-L755
> 
> I tried to add `instance_member().is_unbound()` check in this [else branch](https://github.com/med1844/ruff/blob/b279508bdc63c1ed6fc4ccf9d43d3719fe7a202b/crates/ty_python_semantic/src/types/infer.rs#L3299-L3301) but it causes tests with class attributes defined in class body to fail. While it's possible to implicitly add `ClassVar` to qualifiers to make this assignment fail and keep everything else passing, it doesn't feel like the right solution.

I think it's fine to keep this for a follow-up changeset. I generally agree with your analysis, but maybe adding an implicit `ClassVar` qualifier to attributes that are defined in `@classmethod`s wouldn't be too bad? This would mean that we handle the `x` attributes on the following two classes equivalently, which seems like a good thing?
```py
class C1:
    x: ClassVar[int]

class C2:
    @classmethod
    def method(cls):
        cls.x: int
```
If this is inconsistent for some reason (maybe when printing the type + qualifiers of `C2.x` somewhere), we could perhaps add a new `ImplicitClassVar` qualifier with similar behavior?

---

_Comment by @sharkdp on 2025-06-24 09:41_

> Merging #18587 will **degrade performances by 6.91%**
> `ty_micro[many_string_assignments]`

I acknowledged this performance regression. There are also a few smaller 1-2% regressions in other benchmarks. On this branch, we need to do a lot more work on *every attribute access* `obj.x`. Since we always look up `x` on the type of `obj` first, and `x` might be implicitly defined in a `@classmethod` on the type of `obj`, we now go through all methods on the type of `obj` twice, instead of just once (looking for implicit *instance* attributes).

The `many_string_assignments` benchmark is affected in particular, presumably because it accesses attributes (e.g. `__add__`) on `str` with a lot of methods.

---

_Merged by @sharkdp on 2025-06-24 09:42_

---

_Closed by @sharkdp on 2025-06-24 09:42_

---

_Comment by @med1844 on 2025-06-24 15:44_

Thank you for the review and the commits! Sorry I was not paying enough attention to code comments.

Regarding the test case:

> but maybe adding an implicit `ClassVar` qualifier to attributes that are defined in `@classmethod`s wouldn't be too bad?

My main concern is that it would be surprising for users to see `C2.x` implicitly become a class variable in the diagnostics without being explicitly marked as such. It could be especially confusing for those not already familiar with `ClassVar`.

> If this is inconsistent for some reason (maybe when printing the type + qualifiers of `C2.x` somewhere), we could perhaps add a new `ImplicitClassVar` qualifier with similar behavior?

I'm not a huge fan of adding something so similar to `ClassVar`, but if there isn't a more elegant approach, this would be the best option. It would also allow us to provide a more user-friendly diagnostic message.

If we decides to go with `ImplicitClassVar`, I will create a follow up PR, which should be relatively small.


---

_Comment by @sharkdp on 2025-06-25 07:07_

> My main concern is that it would be surprising for users to see C2.x implicitly become a class variable in the diagnostics without being explicitly marked as such. It could be especially confusing for those not already familiar with ClassVar.

:+1: 

> if there isn't a more elegant approach

I think we'll eventually want to add a richer return type for methods such as `Type::member_lookup_with_policy` that would not just return type, boundness information, and qualifiers, but also other metadata such as:
* was the attribute found on the meta type or on the instance?
* did we end up calling a descriptor `__get__` method while resolving the attribute access?
* if so, was is a data or a non-data descriptor?
* if we could not resolve the attribute access, what was the reason (not found anywhere, call to `__get__` failed, …)

In this process, we could probably also add metadata for what is needed here.

> I'm not a huge fan of adding something so similar to `ClassVar`

I think we could probably try to build a nice abstraction? `TypeQualifiers` could become a single-field struct that stores the bitflags internally. We could then provide an API that can not be misused: a `is_class_var()` method that would return true for both explicit and "implicit" `ClassVar`s. And a second method for displaying/explaining a type with such a qualifier to the user, that would distinguish between the two flags.

What I'm more concerned about is the fact that type qualifiers have a rather precise meaning, and they are only associated with *declared* / explicitly annotated symbols or attributes. With this change, we would also attach non-trivial type qualifiers to attributes that were defined without an annotation (`cls.x = 1` in a `@classmethod`).

---

_Comment by @med1844 on 2025-06-25 15:26_

I'm not sure if I fully understands your points, here's my understanding:

- We can add `ImplicitClassVar`, and also add more methods into `TypeQualifiers` to provide better abstraction.
- This will be replaced with a separate metadata field in future.

The metadata approach seems to solve the root problem and is extensible, though I guess it needs a little bit more refactoring.

---

_Comment by @sharkdp on 2025-06-25 17:55_

> The metadata approach seems to solve the root problem and is extensible, though I guess it needs a little bit more refactoring.

Yes, exactly. I'm afraid this would be a larger project, but the issue here is not that urgent, I think. So it might make sense to just wait for that metadata refactoring/extension.

---

_Comment by @med1844 on 2025-06-26 03:52_

Sounds good! I will work on other issues while waiting for the refactor.

---
