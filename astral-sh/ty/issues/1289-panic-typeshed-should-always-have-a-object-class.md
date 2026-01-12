```yaml
number: 1289
title: "[panic] Typeshed should always have a `object` class in `builtins.pyi`"
type: issue
state: closed
author: copdips
labels:
  - fatal
assignees: []
created_at: 2025-09-30T22:04:17Z
updated_at: 2025-10-06T15:45:32Z
url: https://github.com/astral-sh/ty/issues/1289
synced_at: 2026-01-12T15:54:24Z
```

# [panic] Typeshed should always have a `object` class in `builtins.pyi`

---

_@copdips_

### Summary

`0.0.1-alpha.20` is ok, but `0.0.1-alpha.21` is KO.

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/instance.rs:188:18 when checking `/home/xiang/git/myrepo/backend/rename_invoice_title_to_template_name_migration.py`: `Typeshed should always have a `object` class in `builtins.pyi``
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.21
info: Args: ["ty", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: Type < 'db >::member_lookup_with_policy_(Id(780e))
             at crates/ty_python_semantic/src/types.rs:768
             cycle heads: place_by_id(Id(8801)) -> IterationCount(0)
   1: inner(Id(11400))
             at crates/ty_python_semantic/src/types/instance.rs:596
   2: infer_expression_types_impl(Id(450a))
             at crates/ty_python_semantic/src/types/infer.rs:192
             cycle heads: Type < 'db >::member_lookup_with_policy_(Id(7801)) -> IterationCount(0), infer_definition_types(Id(3c75)) -> IterationCount(0), infer_definition_types(Id(3cc2)) -> IterationCount(0)
   3: infer_definition_types(Id(aca5))
             at crates/ty_python_semantic/src/types/infer.rs:97
   4: place_by_id(Id(8802))
             at crates/ty_python_semantic/src/place.rs:706
             cycle heads: Type < 'db >::member_lookup_with_policy_(Id(7801)) -> IterationCount(0), infer_definition_types(Id(3cc2)) -> IterationCount(0), infer_definition_types(Id(3c75)) -> IterationCount(0), place_by_id(Id(8801)) -> IterationCount(0)
   5: Type < 'db >::member_lookup_with_policy_(Id(7801))
             at crates/ty_python_semantic/src/types.rs:768
   6: infer_definition_types(Id(3c75))
             at crates/ty_python_semantic/src/types/infer.rs:97
   7: infer_definition_types(Id(3cc2))
             at crates/ty_python_semantic/src/types/infer.rs:97
   8: place_by_id(Id(8801))
             at crates/ty_python_semantic/src/place.rs:706
   9: ClassLiteral < 'db >::try_mro_(Id(9000))
             at crates/ty_python_semantic/src/types/class.rs:1386
  10: ClassLiteral < 'db >::is_typed_dict_(Id(8c00))
             at crates/ty_python_semantic/src/types/class.rs:1386
  11: infer_definition_types(Id(3c23))
             at crates/ty_python_semantic/src/types/infer.rs:97
  12: place_by_id(Id(8800))
             at crates/ty_python_semantic/src/place.rs:706
  13: Type < 'db >::member_lookup_with_policy_(Id(7800))
             at crates/ty_python_semantic/src/types.rs:768
  14: infer_definition_types(Id(98c6))
             at crates/ty_python_semantic/src/types/infer.rs:97
  15: infer_scope_types(Id(941f))
             at crates/ty_python_semantic/src/types/infer.rs:68
  16: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522
```

### Version

_No response_

---

_Comment by @MichaReiser on 2025-10-01 06:11_

Thanks for opening this issue. I'll need a few more details to help you.

Can you tell us more about your setup? Are you using a custom typeshed? What changes did you make to it? What's your configuration? How do you run ty?

---

_Label `needs-info` added by @MichaReiser on 2025-10-01 06:11_

---

_Label `fatal` added by @AlexWaygood on 2025-10-01 06:47_

---

_Comment by @carljm on 2025-10-01 15:57_

Regardless of what is causing this for the OP, we need some way to error for user-exposable invariant violations like this that doesn't emit the "This is a bug in ty, please file an issue" message.

---

_Comment by @dcreager on 2025-10-02 01:20_

Echoing above, it would be great to have a minimal reproduction for this. That error message usually means that we've encountered a cycle processing the definition of `object` itself (hence Micha's question about whether you're using a custom typeshed).

---

_Comment by @carljm on 2025-10-02 01:25_

It could also mean (more simply but perhaps less likely?), that a custom typeshed lacking `object` is in use :)

---

_Comment by @copdips on 2025-10-02 23:33_

Hello,

Sorry for the late reply. By commenting out sections of my script step by step, I finally managed to reproduce the issue with just a single line in my script:

```python
from __future__ import annotations
```

In my repo root, I have the following structure: f`rontend`, `backend`, and `.venv `folders. Inside `backend/pyproject.toml`, I have:

```toml
# file backend/pyproject.toml,
[project]
name = "backend"
version = "0.1.0"
requires-python = ">=3.13"

[tool.ty.environment]
root = ["."]
extra-paths = ["../.venv/lib/python3.13/site-packages"]
```

Running `ty check` from the `backend` folder, with `ty==0.0.1a21`, I encounter the error mentioned above.

I can work around the error in three ways:

- by downgrading to `ty==0.0.1a20`
- by running `ty` from the repo root instead of `backend` folder
- by removing the line `extra-paths = ["../.venv/lib/python3.13/site-packages"]` from `backend/pyproject.toml` file

PS: All tests are executed with the `.venv` environment activated.

---

_Comment by @MichaReiser on 2025-10-03 06:21_

Hmm, that's odd. Certainly not what I expected. Can you run ty in verbose mode (`ty check -v`) and share the initial logs where it lists the discovered search paths (and version, etc).

What might also be useful is if you could share `ls -al ../.venv/lib/python3.13/site-packages`. I wonder what's in that directory that breaks our standard library definitions

---

_Comment by @AlexWaygood on 2025-10-03 09:19_

The contents of `extra-paths` _shouldn't_ matter here, since `builtins` is one of the stdlib modules we (correctly) view as non-shadowable, and we skip all non-stdlib search paths when resolving a non-shadowable module name: https://github.com/astral-sh/ruff/blob/f9688bd05c7083374696e95413abe6e7e275f270/crates/ty_python_semantic/src/module_resolver/resolver.rs#L689-L696

That said, I would also be interested in seeing the contents of the `../.venv/lib/python3.13/site-packages` directory here. And FWIW @copdips, using `extra-paths` to point to a virtual environment isn't _really_ the intended use case for that setting -- you should really specify your virtual environment using the [`python`](https://docs.astral.sh/ty/reference/configuration/#python) setting. For most modules (but (theoretically, at least) not `builtins`), search paths in `extra-paths` take precedence over even ty's vendored stdlib stubs when resolving modules. But at runtime, modules installed into `site-packages` have lower precedence than the stdlib when it comes to module resolution, so those aren't the semantics you'd usually want for third-party packages installed into a virtual environment. If you use the `python` setting to specify a virtual environment rather than `extra-paths`, you should get more intuitive behaviour from ty's module resolution overall.

---

_Comment by @copdips on 2025-10-03 21:46_

Hello,

I did some debugging, it seems that the newest `typing_extensions==4.15.0` conflicts with the newest `ty==0.0.1a21`. Downgrading one of them fixed the issue, or I can use other 2 workarounds I explained earlier.

`typing_extensions==4.15.0rc` has the same issue,  `typing_extensions==4.14.1` is ok.

```bash
23:42 $ uv pip list
Using Python 3.13.0 environment at: /home/xiang/git/myrepo/.venv
Package           Version
----------------- --------
ty                0.0.1a21
typing-extensions 4.15.0
```

</p>
</details> 

<details><summary>ty check -vv test_ty.py output:</summary>
<p>

```console
22:58 $ ty check -vv test_ty.py 
2025-10-03 22:58:46.405792903 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
2025-10-03 22:58:46.42123177 DEBUG Version: 0.0.1-alpha.21
2025-10-03 22:58:46.421265495 DEBUG Architecture: x86_64, OS: linux, case-sensitive: case-sensitive
2025-10-03 22:58:46.421370665 DEBUG Searching for a project in '/home/xiang/git/merepo/backend'
2025-10-03 22:58:46.421521112 DEBUG Resolving requires-python constraint: `>=3.13`
2025-10-03 22:58:46.421633781 DEBUG Resolved requires-python constraint to: 3.13
2025-10-03 22:58:46.421656537 DEBUG Found project at '/home/xiang/git/merepo/backend'
2025-10-03 22:58:46.421725159 DEBUG Searching for a user-level configuration at `/home/xiang/.config/ty/ty.toml`
2025-10-03 22:58:46.422581517 INFO Defaulting to python-platform `linux`
2025-10-03 22:58:46.42302159 DEBUG Resolving `VIRTUAL_ENV` environment variable: /home/xiang/git/merepo/.venv
2025-10-03 22:58:46.423071179 DEBUG Attempting to parse virtual environment metadata at '/home/xiang/git/merepo/.venv/pyvenv.cfg'
2025-10-03 22:58:46.423128652 DEBUG Searching for site-packages directory in sys.prefix /home/xiang/git/merepo/.venv
2025-10-03 22:58:46.423173712 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/home/xiang/git/merepo/.venv/lib/python3.13/site-packages", "/home/xiang/git/merepo/.venv/lib64/python3.13/site-packages"})
2025-10-03 22:58:46.4235659 DEBUG Searching for real stdlib directory in sys.prefix /usr
2025-10-03 22:58:46.423651137 DEBUG Resolved real stdlib path for this virtual environment is: /usr/lib/python3.12
2025-10-03 22:58:46.423687143 DEBUG Adding extra search-path `/home/xiang/git/merepo/.venv/lib/python3.13/site-packages`
2025-10-03 22:58:46.423711667 DEBUG Adding first-party search path `/home/xiang/git/merepo/backend`
2025-10-03 22:58:46.423737956 DEBUG Using vendored stdlib
2025-10-03 22:58:46.42413483 DEBUG Adding site-packages search path `/home/xiang/git/merepo/.venv/lib/python3.13/site-packages`
2025-10-03 22:58:46.424184791 DEBUG Adding site-packages search path `/home/xiang/git/merepo/.venv/lib64/python3.13/site-packages`
2025-10-03 22:58:46.424224366 INFO Python version: Python 3.13, platform: linux
2025-10-03 22:58:46.424812197 DEBUG Setting included paths: 1
2025-10-03 22:58:46.424940638 DEBUG Reloading files for project `backend`
2025-10-03 22:58:46.425064603 DEBUG Starting main loop
2025-10-03 22:58:46.425104271 DEBUG Waiting for next main loop message.
2025-10-03 22:58:46.425735338 DEBUG Checking all files in project 'backend'
2025-10-03 22:58:46.427934109 INFO Indexed 1 file(s) in 0.002s
2025-10-03 22:58:46.428126785 DEBUG Checking file '/home/xiang/git/merepo/backend/test_ty.py'
2025-10-03 22:58:46.467839896 INFO Error looking up `builtins.object` in typeshed: expected to find a class definition on Python 3.13, but found a symbol of type `Never` instead. Falling back to `Unknown` for the symbol instead.
2025-10-03 22:58:46.468618179 DEBUG Checking all files took 0.041s
error[panic]: Panicked at crates/ty_python_semantic/src/types/instance.rs:188:18 when checking `/home/xiang/git/merepo/backend/test_ty.py`: `Typeshed should always have a `object` class in `builtins.pyi``
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.21
info: Args: ["ty", "check", "-vv", "test_ty.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: Type < 'db >::member_lookup_with_policy_(Id(2c0e))
             at crates/ty_python_semantic/src/types.rs:768
             cycle heads: place_by_id(Id(3401)) -> IterationCount(0)
   1: inner(Id(ac00))
             at crates/ty_python_semantic/src/types/instance.rs:596
   2: infer_expression_types_impl(Id(30e0))
             at crates/ty_python_semantic/src/types/infer.rs:192
             cycle heads: Type < 'db >::member_lookup_with_policy_(Id(2c01)) -> IterationCount(0), infer_definition_types(Id(1469)) -> IterationCount(0), infer_definition_types(Id(14b6)) -> IterationCount(0)
   3: infer_definition_types(Id(4899))
             at crates/ty_python_semantic/src/types/infer.rs:97
   4: place_by_id(Id(3402))
             at crates/ty_python_semantic/src/place.rs:706
             cycle heads: Type < 'db >::member_lookup_with_policy_(Id(2c01)) -> IterationCount(0), infer_definition_types(Id(14b6)) -> IterationCount(0), infer_definition_types(Id(1469)) -> IterationCount(0), place_by_id(Id(3401)) -> IterationCount(0)
   5: Type < 'db >::member_lookup_with_policy_(Id(2c01))
             at crates/ty_python_semantic/src/types.rs:768
   6: infer_definition_types(Id(1469))
             at crates/ty_python_semantic/src/types/infer.rs:97
   7: infer_definition_types(Id(14b6))
             at crates/ty_python_semantic/src/types/infer.rs:97
   8: place_by_id(Id(3401))
             at crates/ty_python_semantic/src/place.rs:706
   9: ClassLiteral < 'db >::try_mro_(Id(3c00))
             at crates/ty_python_semantic/src/types/class.rs:1386
  10: ClassLiteral < 'db >::is_typed_dict_(Id(3800))
             at crates/ty_python_semantic/src/types/class.rs:1386
  11: infer_definition_types(Id(1417))
             at crates/ty_python_semantic/src/types/infer.rs:97
  12: place_by_id(Id(3400))
             at crates/ty_python_semantic/src/place.rs:706
  13: Type < 'db >::member_lookup_with_policy_(Id(2c00))
             at crates/ty_python_semantic/src/types.rs:768
  14: infer_definition_types(Id(1400))
             at crates/ty_python_semantic/src/types/infer.rs:97
  15: infer_scope_types(Id(1000))
             at crates/ty_python_semantic/src/types/infer.rs:68
  16: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
2025-10-03 22:58:46.468995557 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
2025-10-03 22:58:46.46900536 DEBUG Exiting main loop
```

</p>
</details> 

---

_Comment by @copdips on 2025-10-03 22:04_

I just created a new repo to reproduce this issue:

https://github.com/copdips/ty-issues-1289

---

_Comment by @copdips on 2025-10-04 13:36_

> you should really specify your virtual environment using the [python](https://docs.astral.sh/ty/reference/configuration/#python) setting

yes, this one fixed the issue too.

---

_Comment by @AlexWaygood on 2025-10-04 13:48_

This means that the issue is likely that we're resolving `typing_extensions` to the `typing_extensions.py` file in your virtual environment rather than the `typing_extensions.pyi` file in our vendored typeshed stubs for the stdlib. The reason why we're giving the file in your virtual environment priority over the file in our vendored typeshed stubs is because you've specified the virtual environment using the `extra-paths` setting rather than the `python` setting. The effect of us resolving the `typing_extensions` module to the wrong file is then that we end up with cycles when trying to resolve the class definition for `object` in typeshed's `builtins.pyi`, because the `builtins` module imports `typing_extensions` and depends on many definitions in that module. 

---

_Label `needs-info` removed by @AlexWaygood on 2025-10-04 13:49_

---

_Comment by @sharkdp on 2025-10-06 08:00_

Can we detect this misuse of `environment.extra-paths` somehow (by detecting that it points to a virtual environment), and show a helpful error message (that refers to `environment.python`)? Or are there valid use cases where I'd point `extra-paths` to a virtual env.? Or would the detection itself be prone to false positives?

---

_Comment by @sharkdp on 2025-10-06 08:02_

[Our documentation](https://docs.astral.sh/ty/reference/configuration/#environment) also lists `extra-paths` first (before `python`). I realize that it's just alphabetical order, but maybe we could still move `extra-paths` further down somehow? Instead (or in addition), we could also mention the `python` setting in the `extra-paths` description.

---

_Comment by @AlexWaygood on 2025-10-06 11:26_

I've put up three PRs that should help with this issue: one PR to fix the immediate crash, and two to help guide users towards using the `python` option and away from using the `extra-paths` option:
- https://github.com/astral-sh/ruff/pull/20715
- https://github.com/astral-sh/ruff/pull/20717
- https://github.com/astral-sh/ruff/pull/20718

One last thing that we could consider is to rename the `extra-paths` option to something like `non-environment-paths`, to make it less "inviting", since this isn't the first time I've seen users making use of this option when they shouldn't really be doing so. I think this is also worth considering (though unfortunately `non-environment-paths` still comes before `python` in the alphabet 😄). I'll hold off on proposing this until my already-filed PRs are reviewed, however, since it's a more invasive change.

---

_Comment by @AlexWaygood on 2025-10-06 15:40_

It was decided not to rename the `extra-paths` option for now, so I'll close this. Thanks for the report @copdips, and thanks especially for creating the repo to easily reproduce the issue!

---

_Closed by @AlexWaygood on 2025-10-06 15:40_

---

_Comment by @carljm on 2025-10-06 15:45_

> Regardless of what is causing this for the OP, we need some way to error for user-exposable invariant violations like this that doesn't emit the "This is a bug in ty, please file an issue" message.

Created #1313 to track this.

---
