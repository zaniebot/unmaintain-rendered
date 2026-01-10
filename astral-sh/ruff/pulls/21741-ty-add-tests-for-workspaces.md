```yaml
number: 21741
title: "[ty] add tests for workspaces"
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: gankra/add-tests
created_at: 2025-12-01T21:07:32Z
updated_at: 2025-12-03T14:48:11Z
url: https://github.com/astral-sh/ruff/pull/21741
synced_at: 2026-01-10T16:48:02Z
```

# [ty] add tests for workspaces

---

_Pull request opened by @Gankra on 2025-12-01 21:07_

Here are a bunch of (variously failing and passing) mdtests that reflect the kinds of issues people encounter when running ty over an entire workspace without sufficient hand-holding (especially because in the IDE it is unclear *how* to provide that hand-holding).

---

_Review requested from @carljm by @Gankra on 2025-12-01 21:07_

---

_Review requested from @AlexWaygood by @Gankra on 2025-12-01 21:07_

---

_Review requested from @sharkdp by @Gankra on 2025-12-01 21:07_

---

_Review requested from @dcreager by @Gankra on 2025-12-01 21:07_

---

_Label `internal` added by @Gankra on 2025-12-01 21:07_

---

_Review requested from @MichaReiser by @Gankra on 2025-12-01 21:07_

---

_Label `ty` added by @Gankra on 2025-12-01 21:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 21:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 21:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5814 diagnostics
+ Found 5815 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:1039:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2040 diagnostics
+ Found 2038 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-12-01 21:30_

Note that some real-world examples are combinations of these. For instance "Tests Directory With Overlapping Names" actually works perfectly, but it fails in pyx because it's actually technically hitting "Current Directory Is Invalid Module Name" (because we try to resolve relative imports like `.setup` to `aproj.tests.setup`, which in pyx's case ends up being `a-proj.tests.setup`, which is rejected).

Some approaches to workspaces may dodge the invalid-module-name situation, some may not. I tried to get a good coverage of interesting cases that different solutions would show differences on.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-01 22:09_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:13 on 2025-12-02 08:11_

Yes, the way I think about it is that you can run a python script with an (mostly?) arbitrary name but whether you can import it depends on the segments relative to the search roots.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:17 on 2025-12-02 08:11_

okay, I didn't know that

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:142 on 2025-12-02 08:14_

Long term, I think we want a different solution to correctly support cases where the projects have different configurations (e.g. enabled / disabled some rules). But that shouldn't prevent us from doing better when there's no member-level configuration.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:172 on 2025-12-02 08:16_

Those tests work because we also apply our default `environment.root` paths in mdtests?

---

_@MichaReiser approved on 2025-12-02 08:17_

---

_Review request for @sharkdp removed by @sharkdp on 2025-12-02 08:19_

---

_@Gankra reviewed on 2025-12-02 11:42_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:172 on 2025-12-02 11:42_

I believe so

---

_Merged by @Gankra on 2025-12-02 11:43_

---

_Closed by @Gankra on 2025-12-02 11:43_

---

_Branch deleted on 2025-12-02 11:43_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:32 on 2025-12-02 17:08_

Both of these imports fail at runtime with `ImportError: attempted relative import with no known parent package` if you execute `python my-main.py`.

This is true (both at runtime and in ty today) even if we rename `my-main.py` to just `main.py` -- that is, it's unrelated to the use of an invalid module name. It's because you cannot relative-import top-level modules.

The only way these two imports can work is if all of these modules are imported as part of the same top-level parent package (that is, the directory containing these files is a Python package, and its parent directory is on the search path). (With the name `my-main.py`, the only way _that_ can happen is via `importlib.import_module`.)

In other words, as this test is currently written, I think the above TODO is wrong, and these imports should error. If we add an outer containing directory and place it on the search path, then I think these imports should work (and already would with a valid module name). With that change, I think the TODO to make them also work with an invalid module name would be correct.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:33 on 2025-12-02 17:53_

The only scenario in which this import can work, and the two imports above also work, is overlapping search paths. The two relative imports can only work if this is all part of a containing package (as described above); this import can only work if these are all top-level modules. Overlapping search paths is the only way those two things can both be true.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:144 on 2025-12-02 17:59_

Nit: "eachother" is not a word, it's two words. (I assumed this was a typo but then saw multiple other occurrences below. Not worth fixing alone but if we're changing this file anyway we may as well.)

```suggestion
It's common for a monorepo to define many separate projects that may or may not depend on each other
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:173 on 2025-12-02 18:25_

The use of `extra-paths` here is significantly different from an installed editable, in that `extra-paths` are higher priority than `environment.roots`, while editable installs are lower priority. This seems like it could make a noticeable difference in the behavior of these tests, particularly those that test the interaction between same-named "outer" and "inner" packages.

It would be a bit more boilerplate, but I think these tests could better reflect a realistic scenario if they instead used `python =` config and then created `.pth` files in the fake site-packages that pointed to `aproj/src`, `bproj/src` -- that would actually get those paths added as editable installs. `import/site_packages_discovery.md` demonstrates this, minus the `.pth` file part.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:259 on 2025-12-02 18:32_

So I guess what happens here is we decide the package we should import is `a.tests.setup`, but then (because `./a/src/` is a higher priority search path than `.`) we go looking for `a/src/a/tests/setup.py` and don't find it.

This behavior seems correct (i.e. matches runtime)? Except that as discussed above, a more realistic test would not use `extra-paths`, which would make `.` higher priority than `./a/src/` and thus totally change the behavior of this test.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:328 on 2025-12-02 18:34_

This all seems totally redundant? It's testing the exact same thing as the `a` part above, in exactly the same way, and with no interaction between the two parts. Just delete it?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:424 on 2025-12-02 18:35_

Same comment as above: the existence of `b` seems irrelevant to the test, we're just doing exactly the same test twice, once with the name `a` and once with the name `b`

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:443 on 2025-12-02 18:37_

I don't understand why either of these imports should work. Nothing places `a/` on the search path, so `main` is not a top-level module, and these aren't relative imports.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:333 on 2025-12-02 18:40_

I don't think it can make a difference to this test. In theory testing various cases makes sense, but there are an infinite number of possible scenarios to test, and I'm not sure it scales to sort of randomly test little tweaks "in case", even if we haven't ever seen evidence (i.e. a failure in real code) that it makes a difference. So I'm kind of not convinced we should have both this test and the previous one, unless there's an observed behavior difference.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:511 on 2025-12-02 18:40_

Same comment as above.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:525 on 2025-12-02 18:40_

Same as above -- it seems entirely correct to me for these imports not to work.

---

_Review comment by @carljm on `my-script.py`:1 on 2025-12-02 18:43_

I don't think we want this as a new top-level file in the ruff repo 😆 

---

_@carljm reviewed on 2025-12-02 18:43_

Thanks for adding these tests!

---

_Comment by @carljm on 2025-12-02 19:18_

I think another test we should add here is where we have overlapping search paths, where the outer path (say `.`) is a higher-priority search path than the inner path (say `./src`). If a file inside `./src` (say `./src/foo/bar.py`) does `from . import baz`, we currently reverse-resolve the module name of `./src/foo/bar.py` as `src.foo.bar` (rather than `foo.bar`) and then import `./src/foo/baz.py` as the module name `src.foo.baz`.

I think it would be better if we always preferred the inner, or more precise, matching search path instead. So `./src/foo/bar.py` would then reverse-resolve to the module name `foo.bar` (instead of `src.foo.bar`), even though `.` comes before `./src` in the search paths. And then our relative import would import `foo.baz` instead of `src.foo.baz`.

Cc @AlexWaygood since this is related to what we were discussing on https://github.com/astral-sh/ty/issues/1682

---

_@AlexWaygood reviewed on 2025-12-02 19:20_

---

_Review comment by @AlexWaygood on `my-script.py`:1 on 2025-12-02 19:20_

beat you to it ;) https://github.com/astral-sh/ruff/pull/21751

---

_@carljm reviewed on 2025-12-02 19:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:443 on 2025-12-02 19:38_

Ok, I guess we want these to work via https://github.com/astral-sh/ty/issues/1539 "desperation mode", where effectively we want to treat `a/` locally as an undeclared import root.

---

_@carljm reviewed on 2025-12-02 19:40_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:525 on 2025-12-02 19:40_

Also here we want these to work via https://github.com/astral-sh/ty/issues/1539

---

_Comment by @carljm on 2025-12-02 19:44_

Created https://github.com/astral-sh/ty/issues/1730 to track the suggested new case separately.

---

_@Gankra reviewed on 2025-12-03 14:34_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:32 on 2025-12-03 14:34_

Great callout --  I've put everything but `mod3` under a `tests/` directory and renamed `my-main.py` to `my-mod.py`, and made a clarifying comment that in this case we're assuming `importlib` shenanigans.

(We in fact have had one user complain that they had a random `def.py` that they just `importlib.import_module` but we wouldn't resolve relative imports from it)

---

_@Gankra reviewed on 2025-12-03 14:35_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:33 on 2025-12-03 14:35_

(Fix mentioned in previous comment also handles this)

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:144 on 2025-12-03 14:36_

~~it's Canadian English~~

---

_@Gankra reviewed on 2025-12-03 14:36_

---

_@Gankra reviewed on 2025-12-03 14:40_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:173 on 2025-12-03 14:40_

Also a great point, I misremembered the order of extra and first-party.

I think per discussion of "we should be auto-sorting nested search-paths" I probably want to have at least one test that uses editables and one that uses extra-paths (I imagine some users will find extra-paths, do this kind of thing manually, and it will behoove us to untangle it).

---

_@Gankra reviewed on 2025-12-03 14:44_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:328 on 2025-12-03 14:44_

Having these duplicates is important, as discussed in the opening to the "Multiple Projects" section:

> Often the fact that there is both an `a` and `b` project seemingly won't matter, but many possible
solutions will misbehave under these conditions, as e.g. if both define a `main.py` and test code
has `import main`, we need to resolve each project's main as appropriate.

For instance without `b` also existing it's easy to convince yourself that, instead of doing desperate resolution, you could globally add every `pyproject.toml` as a search-path. However this would result in `a/main.py` and `b/main.py` both being the absolute module `main`.

I agree it's tedious boilerplate but I think it's really important for convincing ourselves the approach actually works in real monorepos with repetitive per-project structures.

---

_@Gankra reviewed on 2025-12-03 14:48_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/workspaces.md`:333 on 2025-12-03 14:48_

This particular case was because I was perplexed by seeing this random variation in pyx's codebase and got a bit paranoid about whether it mattered if `tests` was a namespace package or a normal package. I think the `__init__.py` version is "strictly harder" for us to handle, so I should probably only have that one.

---
