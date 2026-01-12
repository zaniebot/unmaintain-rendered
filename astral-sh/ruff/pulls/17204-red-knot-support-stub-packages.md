```yaml
number: 17204
title: "[red-knot] Support stub packages"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/stub-packages
created_at: 2025-04-04T13:26:55Z
updated_at: 2025-04-07T13:11:54Z
url: https://github.com/astral-sh/ruff/pull/17204
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Support stub packages

---

_@MichaReiser_

## Summary

This PR adds support for stub packages, except for partial stub packages (a stub package is always considered non-partial). 

I read the specification at [typing.python.org/en/latest/spec/distributing.html#stub-only-packages](https://typing.python.org/en/latest/spec/distributing.html#stub-only-packages) but I found it lacking some details, especially on how to handle namespace packages or when the regular and stub packages disagree on whether they're namespace packages. I tried to document my decisions in the mdtests where the specification isn't clear and compared the behavior to Pyright. 

Mypy seems to only support stub packages in the venv folder. At least, it never picked up my stub packages otherwise. I decided not to spend too much time fighting mypyp, which is why I focused the comparison around Pyright 

Closes https://github.com/astral-sh/ruff/issues/16612

## Test plan

Added mdtests

---

_Label `red-knot` added by @MichaReiser on 2025-04-04 13:27_

---

_Comment by @github-actions[bot] on 2025-04-04 13:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scrapy (https://github.com/scrapy/scrapy)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/tests/test_webclient.py:13:8: Cannot resolve import `OpenSSL.SSL`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/scrapy/core/downloader/contextfactory.py:6:6: Cannot resolve import `OpenSSL`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/tests/test_core_downloader.py:9:8: Cannot resolve import `OpenSSL.SSL`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/ssl.py:23:9: Type `X509Name` has no attribute `_name`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/scrapy/core/downloader/tls.py:4:6: Cannot resolve import `OpenSSL`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/tests/mockserver.py:15:6: Cannot resolve import `OpenSSL`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/scrapy/utils/ssl.py:6:8: Cannot resolve import `OpenSSL.SSL`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/scrapy/utils/ssl.py:7:8: Cannot resolve import `OpenSSL.version`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/scrapy/scrapy/utils/ssl.py:12:10: Cannot resolve import `OpenSSL.crypto`
- Found 1509 diagnostics
+ Found 1502 diagnostics

```
</details>


---

_@MichaReiser reviewed on 2025-04-04 16:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:604 on 2025-04-04 16:25_

I moved most of this block into `resolve_module_in_search_path` except the early-returning if package is a regular package (not a namespace package)

---

_Marked ready for review by @MichaReiser on 2025-04-04 16:26_

---

_Review requested from @carljm by @MichaReiser on 2025-04-04 16:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-04 16:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-04 16:26_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-04 16:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:41 on 2025-04-04 17:05_

Would it be slightly clearer to call this `resolved` instead of `module`? On first reading I was confused about `ResolvedModule` type vs `Module` type created below.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:611 on 2025-04-04 17:09_

We can probably skip this for typeshed too

---

_@MichaReiser reviewed on 2025-04-04 17:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:620 on 2025-04-04 17:09_

```suggestion
                    // keep searching the next search path for a stub package with the same name.
```

---

_@carljm approved on 2025-04-04 17:13_

LGTM!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/stub_packages.md`:109 on 2025-04-04 17:56_

```suggestion
Stub packages where one is a namespace package and the other is a regular package. Module resolution
```

---

_@AlexWaygood reviewed on 2025-04-04 17:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/stub_packages.md`:169 on 2025-04-04 17:56_

```suggestion
here is specified, and using the stubs without probing the runtime package first requires slightly
```

---

_@AlexWaygood reviewed on 2025-04-04 17:56_

---

_@AlexWaygood reviewed on 2025-04-04 18:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/stub_packages.md`:110 on 2025-04-04 18:08_

```suggestion
should stop after the first non-namespace stub package. This matches pyright's behavior.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/stub_packages.md`:170 on 2025-04-04 18:29_

this behaviour makes sense to me. I think in general, stubs packages should work exactly the same regardless of whether the runtime package is installed or not, and this behaviour seems consistent with that principle.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/stub_packages.md`:253 on 2025-04-04 18:30_

lol, I have no opinion at all on this 😆 I'd be fine if we didn't support it, but I'm also fine if we do! It seems very unlikely to come up

---

_@AlexWaygood reviewed on 2025-04-04 18:53_

This looks basically good! Two edge cases (which I'm not sure if you handle correctly or not here -- but would be good to test explicitly in any event) relate to the case of a single-file module in `site-packages`. E.g. if `foo.py` is a single-file module at the top-level in `site-packages`, I don't _think_ it's valid to have a `foo-stubs.pyi` file in `site-packages`; `foo-stubs.pyi` should not be treated as the "stubs package" for the runtime `foo` module. (I'd be curious to see what mypy/pyright do there.) But it _is_ definitely valid to have a `foo-stubs/__init__.pyi` file in `site-packages`, and that should be treated as the stub for the runtime `foo` package.

---

_@MichaReiser reviewed on 2025-04-07 11:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/import/stub_packages.md`:253 on 2025-04-07 11:56_

Yeah, not supporting it is harder, that's why I went with supporting it :)

---

_Comment by @MichaReiser on 2025-04-07 12:25_

I added a test for `foo-stubs.pyi`. Pyright ignores those modules too. I didn't test mypy. 

There's already a test covering `foo-stubs/__init__.py`

---

_Merged by @MichaReiser on 2025-04-07 12:40_

---

_Closed by @MichaReiser on 2025-04-07 12:40_

---

_Branch deleted on 2025-04-07 12:40_

---

_Comment by @AlexWaygood on 2025-04-07 13:11_

> I didn't test mypy.

I confirmed that mypy also ignores a `foo-stubs.pyi` file in `site-packages` if you're importing a `foo` module at runtime!

---

_Comment by @AlexWaygood on 2025-04-07 13:11_

Thanks @MichaReiser! :-D

---
