```yaml
number: 15683
title: "[red-knot] Support custom typeshed Markdown tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-custom-typeshed
created_at: 2025-01-23T08:48:24Z
updated_at: 2025-01-23T18:01:59Z
url: https://github.com/astral-sh/ruff/pull/15683
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Support custom typeshed Markdown tests

---

_@sharkdp_

## Summary

- Add feature to specify a custom typeshed from within Markdown-based tests
- Port "builtins" unit tests from `infer.rs` to Markdown tests, part of #13696

## Test Plan

- Tests for the custom typeshed feature
- New Markdown tests for deleted Rust unit tests

---

_Label `red-knot` added by @sharkdp on 2025-01-23 08:48_

---

_Review requested from @carljm by @sharkdp on 2025-01-23 08:48_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-23 08:48_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-23 08:48_

---

_@sharkdp reviewed on 2025-01-23 08:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/module_resolver/testing.rs`:76 on 2025-01-23 08:48_

Drive-by fix. Unrelated to this PR.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:154 on 2025-01-23 08:54_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:178 on 2025-01-23 08:55_

Nit: Should we just call `Program::update_from_settings`?

It has the advantage that it avoids setting values (and, therefore, bumping the salsa revision) if the values are unchanged.

---

_@MichaReiser approved on 2025-01-23 08:56_

Sweet

---

_@sharkdp reviewed on 2025-01-23 08:59_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/lib.rs`:178 on 2025-01-23 08:59_

... and looks better. Thanks.

---

_Comment by @github-actions[bot] on 2025-01-23 09:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mdtest_custom_typeshed.md`:82 on 2025-01-23 10:02_

heh, that's annoying 😆 but fine for now, I think, since I imagine making `reveal_type` work even if it's not defined in a custom typeshed would require much more invasive special casing for the function.

Mypy's approach might be interesting to you though -- it _by default_ uses a custom typeshed for its tests, which you can find in https://github.com/python/mypy/tree/master/test-data/unit/lib-stub. The reason for this is largely to improve the runtime of the tests -- the default custom typeshed is very simple, and mypy's tests would be much slower if they all used the much-more-complex "real" typeshed in every test. Mypy's approach has _many_ downsides: there have often been PRs that appear to fix bugs, and pass all tests, but don't actually fix the bug when mypy runs on real user code, because of the difference between mypy's mock typeshed and the real typeshed mypy uses when checking user code. But it also has upsides apart from performance:
- Mypy's tests are better isolated from random typeshed changes that cause tests to fail
- It's very easy to override a single file or two from mypy's custom typeshed in a test case, without having to provide definitions for `reveal_type` in every test case.

Overall I'm definitely not advocating for mypy's approach; I think the costs outweigh the benefits given how much faster our tests are than mypy's. But it is an _interesting_ approach.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/db.rs`:139 on 2025-01-23 10:10_

hmm, it is still true that we will still _have_ a typeshed directory to use even if this field is `None` though, right? (We'll just use the vendored typeshed directory.) In that sense, even if we're renaming the CLI option to `--typeshed` rather than `--custom-typeshed`, I still think `custom_typeshed` might be a better variable name for us to use internally for fields like this.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mdtest_custom_typeshed.md`:1 on 2025-01-23 10:12_

it took me a second to clock that this was actually an integration test for the feature, as well as documentation 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:12 on 2025-01-23 10:14_

If you like, you could link to https://github.com/python/typeshed/blob/c546278aae47de0b2b664973da4edb613400f6ce/stdlib/VERSIONS#L1-L18 here, which explains the format of the `VERSIONS` file. And our parser for the `VERSIONS` file is at https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/module_resolver/typeshed.rs

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:258 on 2025-01-23 10:14_

maybe link to the test?

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:151 on 2025-01-23 10:20_

nit: strictly speaking we're constructing a module _name_ from the module path

```suggestion
                    let module_name = path
```

The logic here LGTM though, assuming all paths in `typeshed_files` are already relative to `/typeshed/stdlib`.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:156 on 2025-01-23 10:22_

should this be something like this?

```suggestion
                    let default_version = PythonVersion::default();
                    let _ = writeln!(content, "{module_path}: {default_version}-");
```

though that might make upgrading our default Python version harder, I suppose? Not sure

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:136 on 2025-01-23 10:23_

it might again be good to link to https://github.com/python/typeshed/blob/c546278aae47de0b2b664973da4edb613400f6ce/stdlib/VERSIONS#L1-L18 here, as many readers of this might not be familiar with the `VERSIONS` file

---

_@AlexWaygood approved on 2025-01-23 10:23_

Brilliant!

---

_Comment by @AlexWaygood on 2025-01-23 10:26_

I guess this feature again suffers from the problem that, when rendered, you don't see the filenames at all :/

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/d443ca8e-5354-4f55-ada5-8425bd36bafc)

</details>

But, not a problem new to this PR, and this PR obviously doesn't have to fix it!

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/db.rs`:139 on 2025-01-23 10:45_

The field is called `typeshed` (with the same type) in `program::SearchPathSettings`, which is used in lots of places. I chose a consistent naming here. I renamed both fields now.

---

_@sharkdp reviewed on 2025-01-23 10:45_

---

_@sharkdp reviewed on 2025-01-23 10:53_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/lib.rs`:156 on 2025-01-23 10:53_

Also not sure. I feel like what we have is less surprising? And it follows this (now removed) comment from the "planned features" section:

> If no such file is created explicitly, one should be created implicitly including entries enabling all specified `<typeshed-root>/stdlib` files **for all supported Python versions**.

---

_@AlexWaygood reviewed on 2025-01-23 10:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:156 on 2025-01-23 10:57_

hmm, yeah. So I guess if we wanted to make it dynamic, it should be something like `PythonVersion::minimum_supported()` rather than `PythonVersion::default()`. But we don't have that method, and I don't think it's worth adding just for this. So let's go with what you have now.

---

_@AlexWaygood reviewed on 2025-01-23 11:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/db.rs`:139 on 2025-01-23 11:32_

It looks like many of these fields have now been renamed from `typeshed` to `custom_typeshed`, but still not this specific one 😆

---

_Merged by @sharkdp on 2025-01-23 11:36_

---

_Closed by @sharkdp on 2025-01-23 11:36_

---

_Branch deleted on 2025-01-23 11:36_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/mdtest_custom_typeshed.md`:82 on 2025-01-23 14:22_

> Mypy's tests are better isolated from random typeshed changes that cause tests to fail

To clarify my understanding:  This part we're fairly well insulated from, since we're vendoring specific SHAs of typeshed.  We'll get test failures when we update our vendored copy, if there have been incompatible changes — but that's a _good_ thing, since those are real things we have to fix, and CI forces us to fix them before we can commit the typeshed update.  Do I have all of that right?

> It's very easy to override a single file or two from mypy's custom typeshed in a test case, without having to provide definitions for `reveal_type` in every test case.

Does mypy's ability to override individual files depend on it using a custom typeshed by default?  i.e. I could imagine an additional `environment.typeshed_override = true` mdtest config that lets us override individual files but retain the rest of our vendored real typeshed.

---

_@dcreager reviewed on 2025-01-23 14:27_

---

_@AlexWaygood reviewed on 2025-01-23 15:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mdtest_custom_typeshed.md`:82 on 2025-01-23 15:11_

> > Mypy's tests are better isolated from random typeshed changes that cause tests to fail
> 
> To clarify my understanding: This part we're fairly well insulated from, since we're vendoring specific SHAs of typeshed. We'll get test failures when we update our vendored copy, if there have been incompatible changes — but that's a _good_ thing, since those are real things we have to fix, and CI forces us to fix them before we can commit the typeshed update. Do I have all of that right?

Yes, that's all correct! Our tests are more vulnerable to breaking whenever we do a typeshed sync, and already have broken several times when we've tried to sync typeshed. So mypy's tests are closer to being unit tests, in that they control the input to their tests totally; ours are more like integration tests. But as you say, this makes our tests more representative of how we'll actually type-check our users' code, so it's a good thing!

> Does mypy's ability to override individual files depend on it using a custom typeshed by default? i.e. I could imagine an additional `environment.typeshed_override = true` mdtest config that lets us override individual files but retain the rest of our vendored real typeshed.

That's true, I suppose that could work!

---

_@sharkdp reviewed on 2025-01-23 16:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/db.rs`:139 on 2025-01-23 16:47_

Oops, I'll take a look. Thanks.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/builtins.md`:41 on 2025-01-23 17:01_

I wonder if mdtest should just automatically include this in any custom typeshed (if you don't specify your own `typing_extensions`), so we don't have to specify it in each test?

But I guess there's value in simple, explicit behavior; we can wait and see how painful this is; maybe not too bad if we don't have too many tests using custom typeshed.

---

_@carljm reviewed on 2025-01-23 17:06_

This is awesome!

---

_@AlexWaygood reviewed on 2025-01-23 17:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/builtins.md`:41 on 2025-01-23 17:07_

(I think ideally we would have at least a _few_ more tests using custom typesheds, so I wouldn't want to make it _too_ painful to write these tests)

---

_@AlexWaygood reviewed on 2025-01-23 17:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/builtins.md`:41 on 2025-01-23 17:08_

Anyway, we already discussed this at https://github.com/astral-sh/ruff/pull/15683#discussion_r1926701227 ;)

---

_@carljm reviewed on 2025-01-23 18:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/builtins.md`:41 on 2025-01-23 18:00_

I saw that convo but I didn't see any specific proposal there to do something to make this easier. I guess we could do the override-files-instead-of-replace-entirely thing, but I kind of like the idea that we can write tests that more fully control typeshed contents; I think replacing just one file without causing lots of cascading import errors would be hard (though I guess maybe import errors are fine, we won't surface them in typeshed.)

---

_@MichaReiser reviewed on 2025-01-23 18:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/import/builtins.md`:41 on 2025-01-23 18:01_

Replacing files is probably also more expensive to implement because it requires reading and copying all vendored files to the memory file system (not hugely expensive but not free). 

I do like the idea of an extra `environment` option that e.g. includes whatever's necessary for `reveal_type` 

---
