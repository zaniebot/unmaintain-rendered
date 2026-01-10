```yaml
number: 14930
title: "Rename `custom-typeshed-dir`, `target-version` and `current-directory` CLI options"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/rename-some-cli-options
created_at: 2024-12-12T10:45:30Z
updated_at: 2024-12-13T08:24:07Z
url: https://github.com/astral-sh/ruff/pull/14930
synced_at: 2026-01-10T20:42:27Z
```

# Rename `custom-typeshed-dir`, `target-version` and `current-directory` CLI options

---

_Pull request opened by @MichaReiser on 2024-12-12 10:45_

## Summary

This PR renames the `--custom-typeshed-dir`, `target-version`, and `--current-directory` cli options to `--typeshed`, `--python-version`, and `--project` as discussed in the CLI proposal document. 
I added aliases for `--target-version` (for Ruff compat) and `--custom-typeshed-dir` (for Alex)

## Test Plan

Long help

```
An extremely fast Python type checker.

Usage: red_knot [OPTIONS] [COMMAND]

Commands:
  server  Start the language server
  help    Print this message or the help of the given subcommand(s)

Options:
      --project <PROJECT>
          Run the command within the given project directory.
          
          All `pyproject.toml` files will be discovered by walking up the directory tree from the project root, as will the project's virtual environment (`.venv`).
          
          Other command-line arguments (such as relative paths) will be resolved relative to the current working directory."#,

      --venv-path <PATH>
          Path to the virtual environment the project uses.
          
          If provided, red-knot will use the `site-packages` directory of this virtual environment to resolve type information for the project's third-party dependencies.

      --typeshed-path <PATH>
          Custom directory to use for stdlib typeshed stubs

      --extra-search-path <PATH>
          Additional path to use as a module-resolution source (can be passed multiple times)

      --python-version <VERSION>
          Python version to assume when resolving types
          
          [possible values: 3.7, 3.8, 3.9, 3.10, 3.11, 3.12, 3.13]

  -v, --verbose...
          Use verbose output (or `-vv` and `-vvv` for more verbose output)

  -W, --watch
          Run in watch mode by re-running whenever files change

  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```

Short help 

```
An extremely fast Python type checker.

Usage: red_knot [OPTIONS] [COMMAND]

Commands:
  server  Start the language server
  help    Print this message or the help of the given subcommand(s)

Options:
      --project <PROJECT>         Run the command within the given project directory
      --venv-path <PATH>          Path to the virtual environment the project uses
      --typeshed-path <PATH>      Custom directory to use for stdlib typeshed stubs
      --extra-search-path <PATH>  Additional path to use as a module-resolution source (can be passed multiple times)
      --python-version <VERSION>  Python version to assume when resolving types [possible values: 3.7, 3.8, 3.9, 3.10, 3.11, 3.12, 3.13]
  -v, --verbose...                Use verbose output (or `-vv` and `-vvv` for more verbose output)
  -W, --watch                     Run in watch mode by re-running whenever files change
  -h, --help                      Print help (see more with '--help')
  -V, --version                   Print version

```


---

_Review requested from @carljm by @MichaReiser on 2024-12-12 10:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-12 10:45_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-12 10:45_

---

_Label `red-knot` added by @MichaReiser on 2024-12-12 10:46_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/python_version.rs`:32 on 2024-12-12 10:49_

should these all now be dotted?

```suggestion
            Self::Py37 => "3.7",
            Self::Py38 => "3.8",
            Self::Py39 => "3.9",
            Self::Py310 => "3.10",
            Self::Py311 => "3.11",
            Self::Py312 => "3.12",
            Self::Py313 => "3.13",
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:54 on 2024-12-12 10:50_

...maybe `typeshed_dir` rather than `typeshed_path`, since it has to be a directory? But I don't care too deeply, you can disregard this if you diagree :-)

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:39 on 2024-12-12 10:51_

```suggestion
    /// All `pyproject.toml` files will be discovered by walking up the directory tree from the project root,
    /// as will the project's virtual environment (`.venv`) unless the `venv-path` option is set.
```

---

_Comment by @github-actions[bot] on 2024-12-12 10:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-12-12 10:54_

I still sorta think `target-version` more clearly communicates that it should be the Python version your code is _targeting_, which could be very different to the Python version you're using for local development. But hopefully we can address this with excellent documentation.

---

_@MichaReiser reviewed on 2024-12-12 12:04_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:54 on 2024-12-12 12:04_

I'm not very opinionated on this. I'd prefer using `typeshed_directory` but that's somewhat long. It would also require renaming `extra_search_path` to `extra_search_directory` which I think we should not, because it then no longer maps to `sys.path` 

That's why I'm leaning towards keeping it as is or even just remove the `path` entirely and just call it `typeshed`

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:54 on 2024-12-12 12:06_

`--typeshed` seems quite nice to me. I agree it's good to keep it concise

---

_@AlexWaygood reviewed on 2024-12-12 12:06_

---

_@MichaReiser reviewed on 2024-12-12 12:08_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:54 on 2024-12-12 12:08_

Okay, I'll then go with typeshed 

---

_Review comment by @AlexWaygood on `crates/red_knot/src/main.rs`:42 on 2024-12-12 12:37_

```suggestion
    /// Other command-line arguments (such as relative paths) will be resolved relative to the current working directory.
```

---

_@AlexWaygood reviewed on 2024-12-12 12:40_

---

_@sharkdp approved on 2024-12-12 14:23_

Thanks. Looking forward to not having to type `--current-directory` anymore.

---

_Review comment by @carljm on `crates/red_knot/src/main.rs`:39 on 2024-12-12 17:09_

I find this sentence a bit confusing, because I thought the "project root" is defined by where we find a `pyproject.toml` with the expected config? It might make more sense to phrase this as something like "The project root will be found relative to this directory" or similar?

---

_@carljm approved on 2024-12-12 17:10_

LGTM!

---

_Comment by @carljm on 2024-12-12 17:12_

> I still sorta think `target-version` more clearly communicates that it should be the Python version your code is _targeting_, which could be very different to the Python version you're using for local development.

I don't really understand what the potential confusion would be here. If you are telling a type checker your Python version, what other meaning could there be other than "please typecheck this code for this Python version"? I guess I think it's totally fine if in simple cases people conflate "Python version I'm using for local development" with "Python version this code is targeting" -- wanting to separate those is a slightly more advanced use case, and I just don't see any likelihood that anyone who needs to separate those two will be confused about what `--python-version` means.

It has the advantage over `--target-version` that it's clear about "version of what" -- it's not a red-knot version, for example.

---

_Comment by @AlexWaygood on 2024-12-12 17:29_

I obviously don't feel strongly, or I wouldn't have approved!

> I don't really understand what the potential confusion would be here. If you are telling a type checker your Python version, what other meaning could there be other than "please typecheck this code for this Python version"?

I think the potential confusion is that users might not understand why we need this setting at all, given that we already have other settings with which users can tell us the path to the Python executable they're using. If we always used the local-development Python version, we could just derive this information from that setting. But the setting doesn't really mean that at all -- it means "Please specify the minimum Python version you expect this app/library to support (not necessarily the version you're running now), so that we can emit errors if you use features or syntax that are newer than that".

> I guess I think it's totally fine if in simple cases people conflate "Python version I'm using for local development" with "Python version this code is targeting" -- wanting to separate those is a slightly more advanced use case, and I just don't see any likelihood that anyone who needs to separate those two will be confused about what `--python-version` means.

It doesn't seem that advanced to me. I think it's pretty common that people use a version of Python for local development that's pretty new, but they nonetheless want it to be possible for other users to run their code on older versions of Python as well.

> It has the advantage over `--target-version` that it's clear about "version of what" -- it's not a red-knot version, for example.

I can't recall anybody ever being confused about this on the Ruff issue tracker (Ruff uses `--target-version`)

---

_Comment by @carljm on 2024-12-12 17:44_

I don't feel strongly either! Just trying to understand what I'm missing about the arguments either way.

> users might not understand why we need this setting at all, given that we already have other settings with which users can tell us the path to the Python executable they're using.

One answer to this is simply that we want to avoid _running_ a Python executable as part of red-knot settings derivation, and there's no reliable way to tell the Python version of a Python executable without running it. If there were such a way, I would be in favor of automatically deriving the Python version from the executable we are pointed towards, if a version isn't explicitly given.

> I can't recall anybody ever being confused about this on the Ruff issue tracker (Ruff uses `--target-version`)

Fair enough, that's certainly relevant data!

---

_Comment by @MichaReiser on 2024-12-13 08:14_

> It doesn't seem that advanced to me. I think it's pretty common that people use a version of Python for local development that's pretty new, but they nonetheless want it to be possible for other users to run their code on older versions of Python as well.

I expect most users will use `requires-python` in their `pyproject.toml. The main use case that I see for `-python-version`, and why it's important as a CLI flag, is to check a library for newer Python versions than their minimal supported Python version. 

I also think that the setting is clearer because it is in `environment.python-version`. The main motivation for renaming is to be consistent with uv.

However, I agree with you that we could also just derive the value from `--venv-path` (to be renamed to `--python`). 

---

_@MichaReiser reviewed on 2024-12-13 08:14_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:39 on 2024-12-13 08:14_

Fair, I copied this from uv 🤭 

---

_Merged by @MichaReiser on 2024-12-13 08:21_

---

_Closed by @MichaReiser on 2024-12-13 08:21_

---

_Branch deleted on 2024-12-13 08:21_

---
