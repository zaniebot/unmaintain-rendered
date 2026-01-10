```yaml
number: 18127
title: "[ty] NamedTuple 'fallback' attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/namedtuple-fallback-attributes
created_at: 2025-05-16T06:36:10Z
updated_at: 2025-05-16T12:05:14Z
url: https://github.com/astral-sh/ruff/pull/18127
synced_at: 2026-01-10T18:51:01Z
```

# [ty] NamedTuple 'fallback' attributes

---

_Pull request opened by @sharkdp on 2025-05-16 06:36_

## Summary

Add various attributes to `NamedTuple` classes/instances that are available at runtime.

closes https://github.com/astral-sh/ty/issues/417

## Test Plan

New Markdown tests


---

_Review requested from @carljm by @sharkdp on 2025-05-16 06:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-16 06:36_

---

_Review requested from @dcreager by @sharkdp on 2025-05-16 06:36_

---

_Label `ty` added by @sharkdp on 2025-05-16 06:36_

---

_Comment by @github-actions[bot] on 2025-05-16 06:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
graphql-core (https://github.com/graphql-python/graphql-core)
- error[unresolved-attribute] src/graphql/language/source.py:44:31: Type `<class 'SourceLocation'>` has no attribute `_make`
- Found 412 diagnostics
+ Found 411 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[unresolved-attribute] src/urllib3/poolmanager.py:139:18: Type `type[PoolKey]` has no attribute `_fields`
- Found 467 diagnostics
+ Found 466 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[unresolved-attribute] src/poetry/vcs/git/backend.py:547:11: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] tests/integration/test_utils_vcs_git.py:422:33: Type `ParseResult` has no attribute `_replace`
- Found 1029 diagnostics
+ Found 1027 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[unresolved-attribute] cki_lib/inttests/cluster.py:91:26: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] cki_lib/inttests/remote_responses.py:172:15: Type `SplitResult` has no attribute `_replace`
- Found 185 diagnostics
+ Found 183 diagnostics

paasta (https://github.com/yelp/paasta)
- error[unresolved-attribute] paasta_tools/contrib/get_running_task_allocation.py:338:21: Type `TaskAllocationInfo` has no attribute `_asdict`
- error[unresolved-attribute] paasta_tools/mesos/master.py:102:20: Type `ParseResult` has no attribute `_replace`
- Found 976 diagnostics
+ Found 974 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- error[unresolved-attribute] mkdocs/config/config_options.py:523:30: Type `SplitResult` has no attribute `_replace`
- Found 347 diagnostics
+ Found 346 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- error[unresolved-attribute] github/Requester.py:415:54: Type `ParseResult` has no attribute `_replace`
- Found 328 diagnostics
+ Found 327 diagnostics

mypy (https://github.com/python/mypy)
- error[unresolved-attribute] mypy/build.py:1443:24: Type `CacheMeta` has no attribute `_replace`
- error[unresolved-attribute] mypy/build.py:1466:20: Type `CacheMeta` has no attribute `_replace`
- Found 3338 diagnostics
+ Found 3336 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[unresolved-attribute] testing/test_capture.py:971:12: Type `CaptureResult[Unknown]` has no attribute `_replace`
- Found 931 diagnostics
+ Found 930 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[unresolved-attribute] examples/contrib/xss_scanner.py:140:11: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] examples/contrib/xss_scanner.py:184:15: Type `ParseResult` has no attribute `_replace`
- Found 2163 diagnostics
+ Found 2161 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- warning[possibly-unbound-attribute] pymongo/server_description.py:132:16: Attribute `_fields` on type `Unknown | _ServerType` is possibly unbound
- warning[possibly-unbound-attribute] pymongo/topology_description.py:214:16: Attribute `_fields` on type `Unknown | _TopologyType` is possibly unbound
- Found 557 diagnostics
+ Found 555 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-attribute] scrapy/commands/genspider.py:43:18: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] scrapy/http/request/form.py:59:32: Type `SplitResult` has no attribute `_replace`
- Found 1422 diagnostics
+ Found 1420 diagnostics

optuna (https://github.com/optuna/optuna)
- error[unresolved-attribute] tests/visualization_tests/test_pareto_front.py:391:16: Type `_ParetoFrontInfo` has no attribute `_replace`
- warning[possibly-unbound-attribute] tests/visualization_tests/test_pareto_front.py:393:16: Attribute `_replace` on type `_ParetoFrontInfo | Unknown` is possibly unbound
- Found 1185 diagnostics
+ Found 1183 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[unresolved-attribute] mesonbuild/cargo/interpreter.py:868:47: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] mesonbuild/wrap/wrap.py:95:17: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] mesonbuild/wrap/wrap.py:708:57: Type `ParseResult` has no attribute `_replace`
- Found 1452 diagnostics
+ Found 1449 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[unresolved-attribute] cloudinit/distros/__init__.py:1639:36: Type `SplitResult` has no attribute `_replace`
- Found 744 diagnostics
+ Found 743 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[unresolved-attribute] aiohttp/connector.py:1399:19: Type `ConnectionKey` has no attribute `_replace`
- Found 174 diagnostics
+ Found 173 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- error[unresolved-attribute] static_frame/core/www.py:118:17: Type `ParseResult` has no attribute `_replace`
- Found 2770 diagnostics
+ Found 2769 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- error[unresolved-attribute] openlibrary/coverstore/utils.py:91:23: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] openlibrary/plugins/upstream/utils.py:1513:26: Type `ParseResult` has no attribute `_replace`
- Found 763 diagnostics
+ Found 761 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[unresolved-attribute] sphinx/builders/linkcheck.py:747:31: Type `ParseResult` has no attribute `_replace`
- Found 676 diagnostics
+ Found 675 diagnostics

rotki (https://github.com/rotki/rotki)
- error[unresolved-attribute] rotkehlchen/db/settings.py:289:24: Type `<class 'ModifiableDBSettings'>` has no attribute `_fields`
- error[unresolved-attribute] rotkehlchen/db/settings.py:441:21: Type `ModifiableDBSettings` has no attribute `_fields`
- error[unresolved-attribute] rotkehlchen/tests/api/test_settings.py:173:20: Type `<class 'ModifiableDBSettings'>` has no attribute `_fields`
- Found 2107 diagnostics
+ Found 2104 diagnostics

zulip (https://github.com/zulip/zulip)
- error[unresolved-attribute] corporate/views/remote_billing_page.py:510:20: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/lib/markdown/__init__.py:797:20: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/lib/markdown/__init__.py:840:35: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/lib/markdown/__init__.py:2618:30: Type `SplitResult` has no attribute `_replace`
- warning[possibly-unbound-attribute] zerver/lib/markdown/__init__.py:2619:23: Attribute `_replace` on type `SplitResult | Unknown` is possibly unbound
- error[unresolved-attribute] zerver/lib/upload/s3.py:480:20: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/lib/url_encoding.py:111:12: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/tests/test_realm_export.py:89:13: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/tests/test_upload_s3.py:623:26: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/tests/test_upload_s3.py:710:30: Type `SplitResult` has no attribute `_replace`
- error[unresolved-attribute] zerver/views/sentry.py:45:11: Type `SplitResult` has no attribute `_replace`
- Found 4881 diagnostics
+ Found 4870 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[unresolved-attribute] misc/python/materialize/cli/orchestratord.py:207:9: Type `ParseResult` has no attribute `_replace`
- error[unresolved-attribute] misc/python/materialize/cli/orchestratord.py:214:9: Type `ParseResult` has no attribute `_replace`
- Found 3278 diagnostics
+ Found 3276 diagnostics

```
</details>


---

_@sharkdp reviewed on 2025-05-16 06:44_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1292 on 2025-05-16 06:44_

I needed to exclude `__init__` here (for now) because we currently try to call it during `NamedTuple` construction (which is probably wrong?):
```py
class NamedTupleFallback(tuple[Any, ...]):
    # …
    @overload
    def __init__(self, typename: str, fields: Iterable[tuple[str, Any]], /) -> None: ...
    @overload
    @typing_extensions.deprecated(
        "Creating a typing.NamedTuple using keyword arguments is deprecated and support will be removed in Python 3.15"
    )
    def __init__(self, typename: str, fields: None = None, /, **kwargs: Any) -> None: ...
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1295 on 2025-05-16 10:24_

This will panic if a user has a custom typeshed that doesn't include the symbol 

---

_@AlexWaygood reviewed on 2025-05-16 10:24_

---

_@AlexWaygood approved on 2025-05-16 10:25_

Thank you! Looks great other than https://github.com/astral-sh/ruff/pull/18127/files#r2092784838

---

_@carljm reviewed on 2025-05-16 10:52_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:1292 on 2025-05-16 10:52_

Can you say more about why you think it's probably wrong that we try to call `__init__` during `NamedTuple` construction? In general the correct construction behavior is that we call `__new__`, and if that succeeds and returns an instance of the type being constructed, then we also call `__init__` on that instance with the same arguments.

---

_Merged by @sharkdp on 2025-05-16 10:56_

---

_Closed by @sharkdp on 2025-05-16 10:56_

---

_Branch deleted on 2025-05-16 10:56_

---

_@sharkdp reviewed on 2025-05-16 11:05_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1292 on 2025-05-16 11:05_

That was wrong/imprecise. I should have said: it's wrong that we try to call *this specific* `__init__` method.

At runtime, there's no `__init__` method on named tuples:
```pycon
>>> from typing import NamedTuple
>>> class N(NamedTuple):
...     name: str
...     
>>> N.__new__
<function N.__new__ at 0x7d79400b2020>
>>> N.__init__
<slot wrapper '__init__' of 'object' objects>
```

Instead, at runtime, `typing.NamedTuple` is a *function* with the signature of that `NamedTupleFallback.__init__` method. It might help us model a `NamedTuple("N", ["foo", "bar"])` call, but I don't see how it would help to include that as a method on user-generated `NamedTuple`s.

---

_@carljm reviewed on 2025-05-16 12:05_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:1292 on 2025-05-16 12:05_

I see, makes sense! In that case, what you've done here seems pretty reasonable.

---
