```yaml
number: 18267
title: "[ty] Resolving Python path using `CONDA_PREFIX` variable to support Conda and Pixi"
type: pull_request
state: merged
author: lipefree
labels:
  - ty
assignees: []
merged: true
base: main
head: conda-prefix-var
created_at: 2025-05-23T02:05:48Z
updated_at: 2025-05-23T18:09:20Z
url: https://github.com/astral-sh/ruff/pull/18267
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Resolving Python path using `CONDA_PREFIX` variable to support Conda and Pixi

---

_@lipefree_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement what was discussed in https://github.com/astral-sh/ty/issues/265. Pixi and Conda set the `CONDA_PREFIX` environment variable and not `VIRTUAL_ENV`. With this PR, if no `--python flag` has been passed and `VIRTUAL_ENV` has not been set then we check `CONDA_PREFIX` and resolve the path. 

I am still not sure about how to implement: `CONDA_PREFIX` is checked only if there's no `.venv` directory in the project root. 

My guess is it should be something like this but the `.unwrap_or_else()` is still confusing to me and I can't figure out how to check for `CONDA_PREFIX` after the `.unwrap_or_else()` .
```rust
SearchPathSettings {
            extra_paths: extra_paths
                .unwrap_or_default()
                .into_iter()
                .map(|path| path.absolute(project_root, system))
                .collect(),
            src_roots,
            custom_typeshed: typeshed.map(|path| path.absolute(project_root, system)),
            python_path: python
                .map(|python_path| {
                    PythonPath::from_cli_flag(python_path.absolute(project_root, system))
                })
                .or_else(|| {
                    std::env::var("VIRTUAL_ENV")
                        .ok()
                        .map(PythonPath::from_virtual_env_var)
                })
                .unwrap_or_else(|| PythonPath::Discover(project_root.to_path_buf())
                .or_else(|| {
                                    std::env::var("CONDA_PREFIX")
                                        .ok()
                                        .map(PythonPath::from_conda_prefix_var)
                                })),
        }
```

Closes https://github.com/astral-sh/ty/issues/265

## Test Plan

I reproduced the error from #265, and now there is no more `lint:unresolved-import` error. I am still not familar with the markdown tests but I will do more commits to provide tests. 


This is my first opensource contribution and I am still a student with 0 experience in Rust, please give me constructive feedbacks! I will try to learn as quickly as I can!


---

_Review requested from @carljm by @lipefree on 2025-05-23 02:05_

---

_Review requested from @AlexWaygood by @lipefree on 2025-05-23 02:05_

---

_Review requested from @sharkdp by @lipefree on 2025-05-23 02:05_

---

_Review requested from @dcreager by @lipefree on 2025-05-23 02:05_

---

_Review requested from @MichaReiser by @lipefree on 2025-05-23 02:05_

---

_Label `ty` added by @AlexWaygood on 2025-05-23 02:07_

---

_Comment by @github-actions[bot] on 2025-05-23 02:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[Ty] Resolving Python path using `CONDA_PREFIX` variable to support Conda and Pixi" to "[ty] Resolving Python path using `CONDA_PREFIX` variable to support Conda and Pixi" by @lipefree on 2025-05-23 02:13_

---

_Review request for @sharkdp removed by @sharkdp on 2025-05-23 07:28_

---

_@lipefree reviewed on 2025-05-23 13:22_

---

_Review comment by @lipefree on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 13:22_

If someone can help me out since I am new to Rust, this is the solution I came up with to deal with the compiler errors. Is it fine to wrap `PythonPath::Discover(project_root.to_path_buf())` in `Some(.)` and is it fine to simply unwrap `std::env::var("CONDA_PREFIX").ok().map(PythonPath::from_conda_prefix_var)` ?

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:00_

This may not work as intended (unless I'm misreading the code here).

The `.unwrap_or_else` will only look at the `CONDA_PREFIX` if the `or_else` on line 177 returned `None`. However, this can never be the case, because the `or_else` on line 177 always.

Given that `CONDA_PREFIX` is more explicit than an existing `.venv` directory, I think I'm leaning towards changing the order to

* `VIRTUAL_ENV` if it is set
* `CONDA_PREFIX`
* Fallback to inspecting the project directory structure

That should also simplify your live because all you need to do is add an `or_else` similar to the `VIRTUAL_ENV` branch before the `unwrap_or_else`

---

_@MichaReiser approved on 2025-05-23 14:01_

Thanks for working on this. I left an inline comment. I hope the rest of the team agrees with my proposed change but that should simplify your life 

---

_Review requested from @MichaReiser by @MichaReiser on 2025-05-23 14:01_

---

_@AlexWaygood reviewed on 2025-05-23 14:06_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:06_

> Given that `CONDA_PREFIX` is more explicit than an existing `.venv` directory

What worries me here is that it's possible (though not necessarily recommended) to create a virtual environment inside a conda environment. If the user has done that, it might be pretty surprising if we require them to explicitly activate the virtual environment in order for us to recognise it (it goes against the general "Astral philosophy" of not requiring users to imperatively activate their virtual environments).

I say "activate a virtual environment" because I think almost no one sets the `VIRTUAL_ENV` environment variable manually: it's usually set as part of the virtual-environment activation (`source .venv/bin/activate` in the user's shell)

---

_@MichaReiser reviewed on 2025-05-23 14:09_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:09_

Fair enough. Implementing this with this specific discovery ordering would be much more complicated because it can't be implemented here. Instead, it would need to happen inside the search path setting resolution

---

_@MichaReiser reviewed on 2025-05-23 14:09_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:09_

> (it goes against the general "Astral philosophy" of not requiring users to imperatively activate their virtual environments).

Fair, but it would be activate if we have a tigher uv integration

---

_@AlexWaygood reviewed on 2025-05-23 14:11_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:11_

I doubt people who use conda environments are going to want to run ty via uv — pixi or conda are the project managers you probably want to use if you're using conda environments

---

_@MichaReiser reviewed on 2025-05-23 14:14_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:14_

Now I'm confused. if you use conda, then changing the order wouldn't matter? And I think it's fine to ask for explicit configuration if you have a project where you have an activated CONDA env and a nested venv. It's just the best information that the user has given us and a environment variable is, IMO, always more explicit than using some heuristic.

---

_@AlexWaygood reviewed on 2025-05-23 14:20_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:20_

OK, yes, that's fair enough. If the user is using a non-conda virtual environment inside their conda environment, I guess in 2025 they might be using a non-conda project manager to run commands in their project and relying on that non-conda project manager to keep that virtual environment activated for the duration of those commands. I guess I'm wrapping my head round this paradigm too — I haven't used conda since before uv was a thing 😆

In that case, I'm OK with what you suggest: preferring an explicitly activated conda environment over a virtual environment unless the virtual environment has also been explicitly activated (either by the user or by a non-conda project manager such as uv/hatch/poetry/pdm).

We should make sure we write up the rationale for this in a comment, though!

---

_@MichaReiser reviewed on 2025-05-23 14:24_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:24_

I'm also very open to changing this if it proves problematic. The benefit of still being in preview :)

---

_@lipefree reviewed on 2025-05-23 14:51_

---

_Review comment by @lipefree on `crates/ty_project/src/metadata/options.rs`:177 on 2025-05-23 14:51_

"The .unwrap_or_else will only look at the CONDA_PREFIX if the or_else on line 177 returned None. However, this can never be the case, because the or_else on line 177 always." so from what I understand, you are saying that even if no `.venv` is found, we will never reach line `unwrap_or_else` in line 178. From my testing, if no `.venv` directory is found then ty will set the python path using `CONDA_PREFIX` which means that `unwrap_or_else` in line 178 is used (It's my first time writing Rust so please correct me). 

Also from what I understand from your conversations is that I should revert the changes of my last commit and have the following check order : 

   - `VIRTUAL_ENV` if it is set
   -  `CONDA_PREFIX` if it is set
   - Fallback to inspecting the project directory structure 

Thank you for reviewing my PR!

---

_Comment by @lipefree on 2025-05-23 14:57_

I was looking for a way to write tests by setting/unsetting the `VIRTUAL_ENV` and `CONDA_PREFIX` environment variables and having a `.venv` directory. But looking at (writing tests guide)[https://github.com/astral-sh/ruff/tree/main/crates/ty_test], I was not able to understand if the current TOML-based configuration format support my testing case.

---

_Comment by @MichaReiser on 2025-05-23 14:58_

our markdown tests don't support setting environment variables. The best you can try is to write a CLI test in https://github.com/astral-sh/ruff/blob/d098118e37be9437345d527c29a5aeea91b676a6/crates/ty/tests/cli.rs#L11

---

_Comment by @MichaReiser on 2025-05-23 15:24_

I'll convert this to draft. @lipefree you can convert it back to "ready for review" once your PR is ready for another round. This makes it easier for me to know when I should take a look at your PR

---

_Converted to draft by @MichaReiser on 2025-05-23 15:24_

---

_Comment by @lipefree on 2025-05-23 16:59_

> our markdown tests don't support setting environment variables. The best you can try is to write a CLI test in
> 
> https://github.com/astral-sh/ruff/blob/d098118e37be9437345d527c29a5aeea91b676a6/crates/ty/tests/cli.rs#L11

I tried my best for the CLI test. I don't know if it is enough for you. 

---

_Marked ready for review by @lipefree on 2025-05-23 17:00_

---

_@MichaReiser approved on 2025-05-23 17:26_

This is great and nice write up in the test!

---

_Comment by @MichaReiser on 2025-05-23 17:31_

Oh no, the tests don't work on Windows because the venv path is slightly different. Do you want to take a look?

---

_Comment by @lipefree on 2025-05-23 17:37_

> Oh no, the tests don't work on Windows because the venv path is slightly different. Do you want to take a look?

I don't have a Windows machine so it is hard for me to look at how it works. I guess I just need to figure out how Conda environments are structured on Windows but then I don't know how to have OS dependent tests run on my mac (apart from letting the CI do it for me which is quite slow).

---

_Comment by @MichaReiser on 2025-05-23 17:40_

I also don't have a windows machine ;(

Here's an example on how you can have custom logic based on whether you're on windows. 

https://github.com/astral-sh/ruff/blob/d098118e37be9437345d527c29a5aeea91b676a6/crates/ty/tests/cli.rs#L957-L961



---

_@AlexWaygood reviewed on 2025-05-23 17:42_

---

_Review comment by @AlexWaygood on `crates/ty/tests/cli.rs`:1503 on 2025-05-23 17:42_

on Windows the path needs to be `conda-env/Lib/site-packages/package1/__init__.py` rather than `conda-env/lib/python3.13/site-packages/package1/__init__.py`


---

_@lipefree reviewed on 2025-05-23 17:47_

---

_Review comment by @lipefree on `crates/ty/tests/cli.rs`:1503 on 2025-05-23 17:47_

Does "conda-env/Lib/site-packages/package1/init.py" works instead of  "conda-env/Lib/site-packages/package1/__init__.py" ? I am just trusting my copy/paste skill at this point since I can't test locally.

---

_@lipefree reviewed on 2025-05-23 17:48_

---

_Review comment by @lipefree on `crates/ty/tests/cli.rs`:1503 on 2025-05-23 17:48_

Ok it's just markdown doing some formatting. I got it.

---

_@AlexWaygood reviewed on 2025-05-23 17:49_

---

_Review comment by @AlexWaygood on `crates/ty/tests/cli.rs`:1503 on 2025-05-23 17:49_

I mistyped my message originally -- sorry! If you refresh your browser, the edited version of my message should be displayed :-)

It does indeed need to be `__init__.py` rather than `init.py`

---

_@AlexWaygood reviewed on 2025-05-23 17:49_

---

_Review comment by @AlexWaygood on `crates/ty/tests/cli.rs`:1503 on 2025-05-23 17:49_

> Ok it's just markdown doing some formatting. I got it.

yeah, exactly 😄

---

_@lipefree reviewed on 2025-05-23 17:53_

---

_Review comment by @lipefree on `crates/ty/tests/cli.rs`:1503 on 2025-05-23 17:53_

I hope it will pass now and that I did not do any typo !

---

_Comment by @MichaReiser on 2025-05-23 17:59_

Perfect! ty

---

_Merged by @MichaReiser on 2025-05-23 18:00_

---

_Closed by @MichaReiser on 2025-05-23 18:00_

---

_Comment by @AlexWaygood on 2025-05-23 18:05_

@lipefree congrats on your first open-source contribution!! This is a fantastic feature for us to have 😃

---

_Comment by @lipefree on 2025-05-23 18:09_

Thank you for your help @MichaReiser @AlexWaygood on my first contribution !

---
