```yaml
number: 18550
title: "[ty] Infer the Python version from `--python=<system installation>` on Unix"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/system-python-version
created_at: 2025-06-08T13:29:27Z
updated_at: 2025-06-12T06:08:19Z
url: https://github.com/astral-sh/ruff/pull/18550
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Infer the Python version from `--python=<system installation>` on Unix

---

_Pull request opened by @AlexWaygood on 2025-06-08 13:29_

## Summary

If a user passes `--python=<virtual_environment>` and doesn't specify a Python version on the CLI or in a configuration file, we currently use the metadata in that virtual environment's `pyvenv.cfg` file to infer the Python version we should use for resolving modules and types. We currently do not attempt to infer the Python version if a user passes `--python=<system_installation>`, but it's easy enough to do that as well, providing the user is not on a Windows machine. This PR implements that.

## Test Plan

CLI integration tests, and manual testing.

Screenshot of what this looks like on the CLI:

![image](https://github.com/user-attachments/assets/3f15c5b7-f6d9-44cc-aae1-c0947c5f22d4)


---

_Label `ty` added by @AlexWaygood on 2025-06-08 13:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-08 13:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-08 13:29_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-08 13:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-08 13:29_

---

_Review requested from @zanieb by @AlexWaygood on 2025-06-08 13:31_

---

_Review request for @dcreager removed by @AlexWaygood on 2025-06-08 13:31_

---

_Review request for @carljm removed by @AlexWaygood on 2025-06-08 13:31_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-06-08 13:31_

---

_Comment by @github-actions[bot] on 2025-06-08 13:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-08 13:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2025-06-08 13:56_

What makes it hard if the user is on a windows machine? I'd prefer if the CLI has consistent behavior across platforms.

---

_Comment by @AlexWaygood on 2025-06-08 14:04_

> What makes it hard if the user is on a windows machine?

Apologies — I left several comments about this in the source code but forgot to mention it in my PR description.

On Unix, the `site-packages` directory is always located at `<sys.prefix>/lib/pythonX.Y/site-packages` (with minor variations if it's a PyPy installation or a free-threaded installation), so we can just look at the parent directory of the resolved `site-packages` directory to get the information we need. On Windows, the `site-packages` directory is always located at `<sys.prefix>/Lib/site-packages`, so we can't infer the information we need from the directory structure of the installation.

> I'd prefer if the CLI has consistent behavior across platforms.

Yeah, I also wasn't totally sure if this was worth the inconsistency. I think being able to infer the Python version correctly from the minimum number of config options is very high-value, though, and chatting with @zanieb at PyCon they also thought that this was probably worth it

---

_Comment by @zanieb on 2025-06-09 14:59_

Yeah on Windows you'd need to query the interpreter to infer the version. Or.. maybe you could get away with reading the registry and matching paths (but that seems more complicated than it's worth here!)

---

_Comment by @AlexWaygood on 2025-06-09 15:05_

> Or.. maybe you could get away with reading the registry and matching paths (but that seems more complicated than it's worth here!)

yeah I definitely don't want to do that 😆

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:131 on 2025-06-09 16:11_

Nit: Not sure if it's worth it because it's so simple but this is somewhat duplicated now with the serde deserialization logic below.

---

_Review comment by @MichaReiser on `crates/ty/src/args.rs`:92 on 2025-06-09 16:14_

We should also update the `EnvironmentOptions::python_version` documentation. 

The sentence here is somewhat complicated and I don't really feel wiser after reading other than: Okay, ty does some magic but I don't have good suggestions on how to make it clearer (other than trying to split the sentence into multiple sentences to avoid the many and/or)

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/python_environment.rs`:193 on 2025-06-09 16:15_

The comment here confused me a bit because I assumed it documents the test (which it obviously doesn't). 

I think it would be easier to understand if we document the test. What is it testing and then explaining on why we only do this for unix

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:163 on 2025-06-09 16:18_

What does `from_metadata` mean here? Is it that the information comes from the `ProjectMetadata`? Or is it something else? If it's something else, then I'd prefer to either use a different name or extend the documentation (which suggests that it is from `pyenv`, in which case I would name it `pyenv_python_version`)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:335 on 2025-06-09 16:19_

Nit: Maybe `to_python_version` to signify that this operation is no longer "free"

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:366 on 2025-06-09 16:21_

This won't strictly be the parent component. E.g if you have `D:\`, then the second last component is the ROOT component (or dir?). 

Should this be using `ancestor` instead of `components`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:204 on 2025-06-09 16:23_

I think I still prefer not to have the values assigned. It's so noisy when making updates ;)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:43 on 2025-06-09 16:24_

Should we pass the `search_paths` to `resolve_python_version` instead to avoid calling `to_python_version` when `python_version_with_source` isn't `None`?

---

_@MichaReiser approved on 2025-06-09 16:24_

Ty

Mainly a few comments around terminology. 

---

_Comment by @AlexWaygood on 2025-06-09 16:51_

Apologies for the _terrible_ image quality of this screenshot (taken on an ancient Windows machine I have hanging around that does not have -- and will never have -- Rust installed on it). But it does look like it is possible to query the metadata of a Python binary on Windows without actually invoking that binary in a subprocess. The "File version" and "Product version" fields here both contain the Python version in them (obtained here by right-clicking on the `.exe` file in Windows Explorer, clicking on "properties", and then nagivating to the "Details" tab).

![FDF1B5F7-4BFE-408C-B0F3-4DF8645C621F_1_201_a](https://github.com/user-attachments/assets/ba81ca57-c18f-4c16-8683-3c6531ed44ff)

The metadata appears consistent regardless of whether the binary was installed using "official" python.org installers or using uv.

From what I can tell, though, querying these `.exe`-file properties from Rust might not be trivial. So I'll definitely pass on it for now.

---

_Comment by @charliermarsh on 2025-06-09 16:55_

Oh that's cool.

---

_Comment by @zanieb on 2025-06-09 16:57_

That is cool indeed. cc @konstin we could use this to filter Python binaries more aggressively on Windows without querying the interpreter!

---

_Comment by @AlexWaygood on 2025-06-09 17:20_

Here's a somewhat higher-quality screenshot since that last one appears to have generated some excitement 😆 
![ABF429DA-81BD-40D2-8B3E-13AFB1AE90B4_1_201_a](https://github.com/user-attachments/assets/4c219c8f-1c9b-4e3d-b946-141b4c09ec48)


---

_@AlexWaygood reviewed on 2025-06-11 10:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:204 on 2025-06-11 10:18_

alright, alright!

---

_@AlexWaygood reviewed on 2025-06-11 11:01_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/python_version.rs`:131 on 2025-06-11 11:01_

This was bothering me too, especially because we also had duplicated logic in this third place:

https://github.com/astral-sh/ruff/blob/3aae1cd59b6490c72af055d28add6cc2f983e545/crates/ty_python_semantic/src/module_resolver/typeshed.rs#L331-L346

I've unified the three implementations.

---

_@AlexWaygood reviewed on 2025-06-11 11:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:366 on 2025-06-11 11:07_

> E.g if you have `D:\`, then the second last component is the ROOT component (or dir?).

I think we can be confident that that won't be the case here. We wouldn't have succeeded in resolving the `site-packages` directory unless that directory was the grandchild of the `sys.prefix` path. Should I add a comment?

> Should this be using `ancestor` instead of `components`?

Hmm, I think that would be more fiddly to deal with? `ancestors` iterates over the ancestor paths (`<path/to/site-packages>.ancestors()` -> `[<path/to/site-packages>, <path/to>, <path>]`), so I'd have to call `.file_name()` on each path yielded by the method to be able to inspect the information I'm interested in

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-11 11:15_

---

_Comment by @MichaReiser on 2025-06-11 11:41_

Is there anything specific you want me to re-review?

---

_Comment by @AlexWaygood on 2025-06-11 11:45_

> Is there anything specific you want me to re-review?

Are you okay with my response in https://github.com/astral-sh/ruff/pull/18550#discussion_r2139856680?

`crates/ruff_python_ast/src/python_version.rs` and `crates/ty_python_semantic/src/module_resolver/typeshed.rs` have also seen significant changes since you last reviewed, as a result of your comment at https://github.com/astral-sh/ruff/pull/18550#discussion_r2136014362!

---

_@MichaReiser reviewed on 2025-06-11 14:06_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/resolver.rs`:366 on 2025-06-11 14:06_

Oh right, you only want components. A short comment might be helpful althought it might also be fine the way it is. Up to you

---

_Merged by @AlexWaygood on 2025-06-11 14:32_

---

_Closed by @AlexWaygood on 2025-06-11 14:32_

---

_Branch deleted on 2025-06-11 14:32_

---

_Comment by @MichaReiser on 2025-06-11 15:14_

Sorry, I was in meetings. Do you still want me to re-review or are you okay, considering that you merged the change 😆 

---

_Comment by @AlexWaygood on 2025-06-11 15:18_

Ohh sorry! Still might be good for you to give the changes in `crates/ruff_python_ast/src/python_version.rs` and `crates/ty_python_semantic/src/module_resolver/typeshed.rs` a quick once-over (unifying our various `PythonVersion` deserialization methods) -- happy to revert/modify any of those changes if you dislike them!

---

_Comment by @MichaReiser on 2025-06-11 16:00_

Okay, I'll do so tomorrow

---
