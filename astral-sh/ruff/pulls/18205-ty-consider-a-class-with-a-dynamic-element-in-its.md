```yaml
number: 18205
title: "[ty]: Consider a class with a dynamic element in its MRO assignable to any subtype of `type`"
type: pull_request
state: merged
author: felixscherz
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/check-mro-in-is_assignable_to
created_at: 2025-05-19T18:48:16Z
updated_at: 2025-05-19T19:30:31Z
url: https://github.com/astral-sh/ruff/pull/18205
synced_at: 2026-01-10T18:51:02Z
```

# [ty]: Consider a class with a dynamic element in its MRO assignable to any subtype of `type`

---

_Pull request opened by @felixscherz on 2025-05-19 18:48_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Hi, this is in regards to https://github.com/astral-sh/ty/issues/447.

## Summary

Now checks the MRO for `Unknown` when checking the assignability between `Type::ClassLiteral` and `Type::SubclassOf` since `Unknown` is assignable to any type.
There are probably more cases where this check would be interesting.

I'm not entirely sure how to handle the `Specialization` argument to `ClassLiteral::iter_mro` and whether it is ok to pass `None` here.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added markdown tests.
<!-- How was it tested? -->


---

_Review requested from @carljm by @felixscherz on 2025-05-19 18:48_

---

_Review requested from @AlexWaygood by @felixscherz on 2025-05-19 18:48_

---

_Review requested from @sharkdp by @felixscherz on 2025-05-19 18:48_

---

_Review requested from @dcreager by @felixscherz on 2025-05-19 18:48_

---

_Comment by @github-actions[bot] on 2025-05-19 18:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- error[invalid-exception-caught] aioredis/client.py:1105:16: Cannot catch object of type `<class 'TimeoutError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] aioredis/client.py:4047:16: Cannot catch object of type `<class 'TimeoutError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] aioredis/client.py:4452:16: Cannot catch object of type `<class 'TimeoutError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] aioredis/client.py:4648:16: Cannot catch object of type `<class 'TimeoutError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] aioredis/connection.py:826:20: Cannot catch object of type `<class 'TimeoutError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 33 diagnostics
+ Found 28 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[no-matching-overload] comtypes/test/test_comserver.py:177:16: No overload of function `GetActiveObject` matches arguments
- error[no-matching-overload] comtypes/test/test_comserver.py:206:16: No overload of function `CreateObject` matches arguments
- Found 601 diagnostics
+ Found 599 diagnostics

nox (https://github.com/wntrblm/nox)
- error[invalid-argument-type] nox/logger.py:98:24: Argument to function `setLoggerClass` is incorrect: Expected `type[Logger]`, found `<class 'LoggerWithSuccessAndOutput'>`
- Found 38 diagnostics
+ Found 37 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[invalid-exception-caught] cki_lib/yaml.py:222:12: Cannot catch object of type `<class 'ValidationError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 170 diagnostics
+ Found 169 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-return-type] src/jinja2/runtime.py:967:12: Return type does not match returned value: expected `type[Undefined]`, found `<class 'LoggingUndefined'>`
- Found 235 diagnostics
+ Found 234 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] tests/test_spider.py:121:27: Argument to bound method `__init__` is incorrect: Expected `type[Spider]`, found `<class 'TestSpider'>`
- error[invalid-argument-type] tests/test_spider.py:160:31: Argument to function `get_crawler` is incorrect: Expected `type[Spider] | None`, found `<class 'TestSpider'>`
- error[invalid-argument-type] tests/test_spider.py:473:31: Argument to function `get_crawler` is incorrect: Expected `type[Spider] | None`, found `<class 'TestSpider'>`
- error[invalid-argument-type] tests/test_spider.py:800:31: Argument to function `get_crawler` is incorrect: Expected `type[Spider] | None`, found `<class 'TestSpider'>`
- Found 1307 diagnostics
+ Found 1303 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[invalid-argument-type] static_frame/test/property/strategies.py:973:13: Argument to function `get_frame` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- error[invalid-argument-type] static_frame/test/property/test_strategies.py:112:27: Argument to function `get_frame` is incorrect: Expected `type[Index]`, found `<class 'IndexHierarchy'>`
- error[invalid-argument-type] static_frame/test/property/test_strategies.py:112:53: Argument to function `get_frame` is incorrect: Expected `type[Index]`, found `<class 'IndexHierarchy'>`
- error[invalid-argument-type] static_frame/test/unit/test_index_hierarchy.py:51:20: Argument is incorrect: Expected `type[IndexHierarchy]`, found `<class 'IndexHierarchyGO'>`
- error[invalid-argument-type] static_frame/test/unit/test_interface.py:292:69: Argument to bound method `name_obj_iter` is incorrect: Expected `type[ContainerBase]`, found `<class 'Series'> | <class 'Frame'> | <class 'FrameGO'> | <class 'Index'> | <class 'IndexDate'> | <class 'IndexDateGO'> | <class 'IndexHierarchy'> | <class 'Bus'> | <class 'Batch'> | <class 'Yarn'> | <class 'Quilt'>`
- error[invalid-argument-type] static_frame/test/unit/test_interface.py:292:69: Argument to bound method `name_obj_iter` is incorrect: Expected `type[ContainerBase]`, found `<class 'Series'> | <class 'Frame'> | <class 'FrameGO'> | <class 'Index'> | <class 'IndexDate'> | <class 'IndexDateGO'> | <class 'IndexHierarchy'> | <class 'Bus'> | <class 'Batch'> | <class 'Yarn'> | <class 'Quilt'>`
- error[invalid-argument-type] static_frame/test/unit/test_interface.py:292:69: Argument to bound method `name_obj_iter` is incorrect: Expected `type[ContainerBase]`, found `<class 'Series'> | <class 'Frame'> | <class 'FrameGO'> | <class 'Index'> | <class 'IndexDate'> | <class 'IndexDateGO'> | <class 'IndexHierarchy'> | <class 'Bus'> | <class 'Batch'> | <class 'Yarn'> | <class 'Quilt'>`
- error[invalid-argument-type] static_frame/test/unit/test_interface.py:292:69: Argument to bound method `name_obj_iter` is incorrect: Expected `type[ContainerBase]`, found `<class 'Series'> | <class 'Frame'> | <class 'FrameGO'> | <class 'Index'> | <class 'IndexDate'> | <class 'IndexDateGO'> | <class 'IndexHierarchy'> | <class 'Bus'> | <class 'Batch'> | <class 'Yarn'> | <class 'Quilt'>`
- error[invalid-argument-type] static_frame/test/unit/test_store_zip.py:165:35: Argument to function `get_test_framesA` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- error[invalid-argument-type] static_frame/test/unit/test_store_zip.py:165:35: Argument to function `get_test_framesA` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- error[invalid-argument-type] static_frame/test/unit/test_store_zip.py:199:39: Argument to function `get_test_framesA` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- error[invalid-argument-type] static_frame/test/unit/test_store_zip.py:199:39: Argument to function `get_test_framesA` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- error[invalid-argument-type] static_frame/test/unit/test_store_zip.py:199:39: Argument to function `get_test_framesA` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- error[invalid-argument-type] static_frame/test/unit/test_store_zip.py:199:39: Argument to function `get_test_framesA` is incorrect: Expected `type[Frame]`, found `<class 'FrameGO'>`
- Found 2077 diagnostics
+ Found 2063 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[invalid-argument-type] cloudinit/templater.py:155:25: Argument to function `__new__` is incorrect: Expected `type[Undefined]`, found `<class 'UndefinedJinjaVariable'>`
- Found 718 diagnostics
+ Found 717 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[invalid-exception-caught] src/poetry/console/application.py:261:20: Cannot catch object of type `<class 'PoetryRuntimeError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] src/poetry/console/commands/python/install.py:116:16: Cannot catch object of type `<class 'PoetryRuntimeError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] tests/installation/test_chooser.py:53:12: Cannot catch object of type `<class 'PoetryRuntimeError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 1018 diagnostics
+ Found 1015 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[invalid-return-type] sphinx/domains/__init__.py:200:16: Return type does not match returned value: expected `type[Directive] | None`, found `<class 'DirectiveAdapter'>`
- Found 664 diagnostics
+ Found 663 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-exception-caught] zerver/management/commands/sync_ldap_user_data.py:35:20: Cannot catch object of type `<class 'ZulipLDAPError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] zproject/backends.py:1008:16: Cannot catch object of type `<class 'NoMatchingLDAPUserError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[invalid-exception-caught] zproject/backends.py:1339:16: Cannot catch object of type `<class 'NoMatchingLDAPUserError'>` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 6905 diagnostics
+ Found 6902 diagnostics

```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1548 on 2025-05-19 18:54_

I think we should also view classes that inherit from `Any` or `Todo` as assignable to arbitrary `type[]` types

```suggestion
                    .any(|superclass| superclass.is_dynamic()) =>
```

---

_@AlexWaygood reviewed on 2025-05-19 18:54_

Thank you!!

---

_@felixscherz reviewed on 2025-05-19 19:03_

---

_Review comment by @felixscherz on `crates/ty_python_semantic/src/types.rs`:1548 on 2025-05-19 19:03_

I added a `is_dynamic` method to `ClassBase` and replaced this check using it:)

---

_Label `ty` added by @AlexWaygood on 2025-05-19 19:27_

---

_Comment by @AlexWaygood on 2025-05-19 19:27_

> I'm not entirely sure how to handle the `Specialization` argument to `ClassLiteral::iter_mro` and whether it is ok to pass `None` here.

I think this is fine!

---

_Renamed from "feat: check mro for `Unknown` in `is_assignable_to`" to "[ty]: Consider a class with a dynamic element in its MRO assignable to any subtype of `type`" by @AlexWaygood on 2025-05-19 19:28_

---

_@AlexWaygood approved on 2025-05-19 19:28_

Excellent, thank you!

---

_Merged by @AlexWaygood on 2025-05-19 19:30_

---

_Closed by @AlexWaygood on 2025-05-19 19:30_

---
