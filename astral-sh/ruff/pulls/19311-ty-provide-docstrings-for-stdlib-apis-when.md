```yaml
number: 19311
title: "[ty] Provide docstrings for stdlib APIs when hovering over them in an IDE"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/typeshed-docstrings
created_at: 2025-07-13T21:00:18Z
updated_at: 2025-07-14T16:23:38Z
url: https://github.com/astral-sh/ruff/pull/19311
synced_at: 2026-01-12T15:56:36Z
```

# [ty] Provide docstrings for stdlib APIs when hovering over them in an IDE

---

_@AlexWaygood_

## Summary

I made a codemod that will auto-add docstrings to stub files by dynamically inspecting the value of the docstrings at runtime. This PR adds a step to our typeshed-sync workflow that applies the codemod, so that we always have docstrings for the stdlib checked into our vendored stubs for the standard library. This will allow us to display the docstrings when users hover over stdlib symbols in their IDE.

The source code for the codemod is [here](https://github.com/AlexWaygood/docstring-adder). The changes the codemod makes can be viewed [here](https://github.com/python/typeshed/compare/main...AlexWaygood:typeshed:docstrings?expand=1).

~~The only issue I _know_ of is that if you have version-dependent method definitions, e.g.~~

```py
class Foo:
    if sys.version_info >= (3, 13):
        def method(self): ...
    else:
        def method(self, arg): ...
```

~~then the codemod will only add a docstring to the definition in the first `sys.version_info` branch, not the second. This issue only exists for nested scopes, however; version-dependent class or function definitions in the global scope should have docstrings added to all definitions without issue.~~

^EDIT: I fixed this issue.

Codemodding docstrings into the stubs increases the size of the vendored-typeshed zipfile that we include as part of the ty binary. Locally, a release build of the ty binary increases in size from 38.7MB to 39.9MB. I think that's probably worth it, given that there's no other way to provide docstrings for C-extension modules in the stdlib. Even modules that are nominally written in Python, such as the `typing` module, often have several classes in them that are actually written in C (`typing.TypeVar`, for example); it would be impossible for ty to obtain docstrings for these classes by inspecting the runtime source code of the stdlib, so codemodding the docstrings into the stub seems to be a more resilient strategy here.

Codemodding docstrings into the stubs at typeshed-sync time is preferable to attempting to maintain these docstrings upstream in typeshed, because docstrings are constantly changing upstream in CPython, and it would be extremely difficult to keep the copies of these docstrings in typeshed up to date. An automated codemod solves this issue.

## Test Plan

- This has been mostly tested by eyeballing the [output](https://github.com/python/typeshed/compare/main...AlexWaygood:typeshed:docstrings?expand=1) of the codemod, and checking that there wasn't anything unexpectedly present or unexpectedly missing from the output.
- The script also has an assertion built into it which checks that every function name and class name in the AST appears the same number of times before and after the codemod
- Mypy is run on the script in CI over at https://github.com/AlexWaygood/docstring-adder/blob/main/.github/workflows/check.yaml
- There's a CI check in https://github.com/AlexWaygood/docstring-adder/blob/main/.github/workflows/check.yaml that ensures that the script never adds a docstring to a class or function that already has a docstring

---

_Review requested from @carljm by @AlexWaygood on 2025-07-13 21:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-13 21:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-13 21:00_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-07-13 21:00_

---

_Label `internal` added by @AlexWaygood on 2025-07-13 21:00_

---

_Label `ty` added by @AlexWaygood on 2025-07-13 21:00_

---

_Review comment by @dhruvmanila on `.github/workflows/sync_typeshed.yaml`:51 on 2025-07-14 06:10_

Is the order of running these dependent on the Python version? i.e., is it required that we run it from latest to oldest Python version? If so, let's document that as a comment.

---

_Review comment by @dhruvmanila on `.github/workflows/sync_typeshed.yaml`:71 on 2025-07-14 06:10_

Use `ruff format` ;)

---

_@dhruvmanila approved on 2025-07-14 06:15_

Thanks! This is great

Is there a reason that this is in a separate repository? As it's a script, can it be added / moved to the Ruff repository mainly so that it's easier to maintain?

---

_Comment by @dhruvmanila on 2025-07-14 06:17_

Do we know if the script (or something else) that Pylance is using to fetch these docstrings is open-sourced and available? If so, can it be used by us?

---

_Comment by @MichaReiser on 2025-07-14 06:56_

> Locally, a release build of the ty binary increases in size from 38.7MB to 39.9MB.

The published ty artifact (built with uv build in the ty repository) increases from 3187088 (3.18MB) to 3218280 (3.21MB). I think that's neglectable.

---

_@MichaReiser reviewed on 2025-07-14 06:57_

---

_Review comment by @MichaReiser on `.github/workflows/sync_typeshed.yaml`:51 on 2025-07-14 06:57_

Does typeshed not support Python 3.8. If that's the case, it probably doesn't make sense for ty to still support Python 3.8 😆 

---

_@MichaReiser reviewed on 2025-07-14 06:58_

---

_Review comment by @MichaReiser on `.github/workflows/sync_typeshed.yaml`:45 on 2025-07-14 06:58_

I think we should create an issue for this instead of this ominous TODO comment where it isn't directly clear what hte issue is with platform specific APIs

---

_Comment by @MichaReiser on 2025-07-14 07:01_

It's good to know that our parser is fast enough that the size increase in code to parse doesn't impact runtime performance in a meaningful way

Do you know why this isn't something that has been done before? 

---

_@MichaReiser approved on 2025-07-14 07:01_

---

_@AlexWaygood reviewed on 2025-07-14 07:38_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:51 on 2025-07-14 07:38_

That's correct, it only supports 3.9+ these days 

---

_@AlexWaygood reviewed on 2025-07-14 12:12_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:51 on 2025-07-14 12:12_

Yes: the codemod will only add docstrings to functions/classes that do not already have docstrings. We run with Python 3.14 before running with any other Python version so that we get the Python 3.14 version of the docstring for a definition that exists on all Python versions: if we ran with Python 3.9 first, then the later runs with Python 3.10+ would not modify the docstring that had already been added using the old version of Python.

I'll add a comment!

---

_@AlexWaygood reviewed on 2025-07-14 12:14_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:71 on 2025-07-14 12:14_

Hehe. Here I'm just invoking typeshed's pre-commit file to make sure the changes we've codemodded in are formatted roughly the same way as the rest of typeshed's code. Typeshed still uses black; ruff-format isn't listed in their pre-commit file. I _could_ invoke ruff-format on the typeshed stdlib directory, but (1) that might reformat other parts of the stdlib (not just the bits we've codemodded), and (2) Ruff's formatter still moves `type: ignore` comments around more than Black, which could mean that typeshed's tests would no longer pass on the reformatted code.

I'll add a comment explaining this!

---

_Comment by @AlexWaygood on 2025-07-14 12:20_

> Do we know if the script (or something else) that Pylance is using to fetch these docstrings is open-sourced and available? If so, can it be used by us?

I believe it's closed-source. An early open-source prototype is available at https://github.com/gramster/stubsplit, but note that the README for that project says:

> It would be better to rewrite this at some point using libcst

which is basically what I've done ;)

---

_Comment by @AlexWaygood on 2025-07-14 12:23_

> Is there a reason that this is in a separate repository? As it's a script, can it be added / moved to the Ruff repository mainly so that it's easier to maintain?

One reason why I'd be interested in keeping it separate is that I'd like to explore running this script at build time in https://github.com/typeshed-internal/stub_uploader when uploading typeshed's third-party stubs packages to PyPI. This would also be really beneficial to ty users, as well as users of other type checkers.

I'd be interested in moving the repo to the `astral` organisation, though, and giving other people commit access?

---

_@AlexWaygood reviewed on 2025-07-14 12:24_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:45 on 2025-07-14 12:24_

The issue is that we get the runtime docstrings by inspecting the docstrings at runtime, so if an API doesn't exist at runtime (because e.g. it's Windows-specific and we're running on Linux), then we won't add a docstring to it.

This is probably solvable, but it might require complicating the GitHub workflow a bit. I'll add a comment.

---

_Comment by @MichaReiser on 2025-07-14 12:25_

> I'd be interested in moving the repo to the astral organisation, though, and giving other people commit access?

I think that would be great. You should have the necessary permissions to create a new repository

---

_Comment by @AlexWaygood on 2025-07-14 12:28_

> It's good to know that our parser is fast enough that the size increase in code to parse doesn't impact runtime performance in a meaningful way

I'm not sure we actually know that from this PR, because this PR itself doesn't actually add docstrings (it just adds the workflow that means they'll be auto-added in the next typeshed sync). I was going to trigger the workflow manually immediately after landing this, but I can open a draft PR now with all the docstrings added to check performance doesn't degrade on Codspeed.

---

_Comment by @MichaReiser on 2025-07-14 12:30_

> I'm not sure we actually know that from this PR, because this PR itself doesn't actually add docstrings (it just adds the workflow that means they'll be auto-added in the next typeshed sync). I was going to trigger the workflow manually immediately after landing this, but I can open a draft PR now with all the docstrings added to check performance doesn't degrade on Codspeed.

Whooops. I didn't realize this. That also means that my binary size measurement is off because what I did is checkout this PR. Can you measure the binary size increase of the released ty artifact (use `uv build` in the ty repository).

---

_Comment by @AlexWaygood on 2025-07-14 12:39_

Screenshot of signature help in the playground for a builtin function when the stubs have docstrings:

<img width="1232" height="618" alt="image" src="https://github.com/user-attachments/assets/9de6c6d7-9994-4ad4-9f8d-09e5b883bf66" />


---

_Comment by @AlexWaygood on 2025-07-14 12:51_

> Can you measure the binary size increase of the released ty artifact (use `uv build` in the ty repository).

For the latest release of ty, I have these numbers:
- Wheel: 6,700,111 bytes (6.7 MB on disk)
- Sdist: 3,186,425 bytes (3.2 MB on disk)

Updating the submodule to https://github.com/astral-sh/ruff/pull/19327, I have:
- Wheel: 7,453,238 bytes (7.5 MB on disk)
- Sdist: 3,848,622 bytes (3.9 MB on disk)

---

_Comment by @AlexWaygood on 2025-07-14 12:55_

The [Codspeed report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fadd-docstrings) on https://github.com/astral-sh/ruff/pull/19327 reports some regressions, but these appear most pronounced on the microbenchmarks (which makes sense, as parsing the vendored typeshed stubs takes up a much higher percentage of the total execution time for smaller projects). There are regressions of up to 4% on the microbenchmarks, a 2% regression on the cold tomllib benchmark, and regressions of 1% or lower for all other benchmarks.

---

_Comment by @MichaReiser on 2025-07-14 13:02_

Thanks @AlexWaygood for getting all those numbers. The binary size increase makes way more sense than the numbers I shared.

I think those regressions are fine, considering the value they provide in an IDE context and in-stubs documentation has much better ergonomics when reading a builtin-stub file in the IDE over an external JSON file. I also don't think that it justifies shipping typeshed twice. 

The long-term solution here is to pre-process typeshed so that we don't need to parse the files in the first place.

---

_Comment by @AlexWaygood on 2025-07-14 13:08_

> > I'd be interested in moving the repo to the astral organisation, though, and giving other people commit access?
> 
> I think that would be great. You should have the necessary permissions to create a new repository

Okay, the repo is now at https://github.com/astral-sh/docstring-adder !

---

_Merged by @AlexWaygood on 2025-07-14 16:00_

---

_Closed by @AlexWaygood on 2025-07-14 16:00_

---

_Branch deleted on 2025-07-14 16:00_

---

_Comment by @AlexWaygood on 2025-07-14 16:23_

I manually triggered the workflow, and it created https://github.com/astral-sh/ruff/pull/19334 -- everything seems to be working as expected 🥳

---
