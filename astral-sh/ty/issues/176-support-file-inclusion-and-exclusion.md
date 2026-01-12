```yaml
number: 176
title: Support file inclusion and exclusion
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - configuration
assignees: []
created_at: 2025-03-14T10:04:09Z
updated_at: 2025-06-12T17:07:33Z
url: https://github.com/astral-sh/ty/issues/176
synced_at: 2026-01-12T15:54:22Z
```

# Support file inclusion and exclusion

---

_@MichaReiser_

* Add the `src.include` and `src.exclude` options (design TBD)
* Add a `--include` and `--exclude` CLI argument (design TBD)
* Respect the include/exclude during project file discovery

---

_Renamed from "[red-knot]: Support file inclusion and exclusion" to "Support file inclusion and exclusion" by @MichaReiser on 2025-05-07 15:57_

---

_Label `cli` added by @AlexWaygood on 2025-05-11 07:27_

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 07:27_

---

_Comment by @tonka3000 on 2025-05-12 18:11_

Would be happy to see this feature.
I have vendored python code which I wanna exclude from the check because it is "out of my control". 

---

_Comment by @scosman on 2025-05-17 15:19_

Yes please! 

Some use cases I have for excludes that might help design (I use these with pyright):

-  `**/test_*.py` - no type checking in test files
- No type checking in non-source directories (for performance, which matters less for ty): `app/desktop/build/**`, `app/web_ui/**`, `**/.venv`

Something like this would be nice (from [ruff](https://docs.astral.sh/ruff/settings/#exclude)):

```
[tool.ty]
exclude=["**/test_*.py", "app/desktop/build/**", "app/web_ui/**", "**/.venv"]
```

---

_Comment by @MichaReiser on 2025-05-20 09:42_

For a workaround, see https://github.com/astral-sh/ty/tree/main/docs#excluding-files

---

_Comment by @carljm on 2025-05-20 14:29_

> Following on this, I hate that it check my .venv for some reasons

You mean that ty is checking the code inside your `.venv` directory as if it were first-party code? I can't reproduce this behavior. If you can provide a reproducible set of steps, please open a new issue for that.

---

_Comment by @MichaReiser on 2025-05-20 14:32_

> You mean that ty is checking the code inside your .venv directory as if it were first-party code? I can't reproduce this behavior. If you can provide a reproducible set of steps, please open a new issue for that.

It depends on if your in a git repo and have a `.gitignore` that excludes `.venv` (or an `.ignore` file). 

---

_Comment by @charliermarsh on 2025-05-20 14:33_

(Any `.venv` created by `uv` or `virtualenv` automatically includes an ignore. That's also true of environments created by `python -m venv` in recent versions.)

---

_Assigned to @MichaReiser by @MichaReiser on 2025-06-02 11:42_

---

_Comment by @MichaReiser on 2025-06-02 14:56_

ty’s configuration is designed so that it allows merging configurations from different files or the options specified on the CLI. An ideal design would allow users to:

* override values inherited from a lower-precedence source. E.g. override an `exclude` from a user or project configuration file (by e.g. negating)
* captures the user’s intent as closely as possible `--exclude src/*test.py --include src/abtest.py` should include `src/abtest.py` because it was specified later

Most tools use separate `include` and `exclude` options that take glob patterns. Having two options is easy to understand but it has the downside that it doesn’t work for the cases outlined above:

* Users can’t override an `exclude`. A path that’s excluded once can’t be re-included. 
* The order doesn’t define precedence. `exclude` always wins over `include`. 

That’s why I think we should have a single `src.files` option that takes a list of ignore patterns. 

* Only include src: `src.files = [“src”]`
* Exclude notebooks: `src.files = [“!**/*.ipynb”]`

The only worry with this is that `excludes` are slightly less obvious and new users might not be familiar with the -- somewhat complicated -- gitignore syntax. 

For the CLI, I think dedicated `--include` and `--exclude` options that take glob patterns would be nice. We can accomplish this by mapping them to `src.files` (and preserving order!). I don’t that it would be confusing that they take globs instead of git ignore patterns, given that they are named differently.

In the future, we may want to allow users to restrict which files are formatted or listed. For this, we can add a `format.files` option that applies additional filtering to `src.files`. `format.files` would also accept a git-ignore pattern.

The last design challenge is how to handle default exclusion. Common options are:

1. Default excludes apply unless a user specifies its own excludes. This won’t work with the proposed schema because it doesn’t have an `exclude` field.
1. Allow re-including excludes by using an exact match. E.g. you can include `.venv` by using `src.files = [“.venv”]`. I like that a lot. 
1. Have a `standard-excludes` option that, when turned off, disables all standard excludes. That’s what uv does. It has the downside that it’s an all or nothing. 


I’m leaning towards option 2. The only thing that’s awkward is that e.g., re-including files and folders starting with a `.` would extend the rule to not only apply to files but exact patterns (having a default excluded pattern in include “removes it”)

For the globs, we should use the same glob syntax as supported by uv.

Curious to hear your thoughts. CC: @konstin, since you worked on the build backend file inclusion/exclusion. 


Edit:

**alternatives**:

* Support both `include` and `exclude` options but make them mutually exclusive (Cargo)
* `include` and `exclude` options but without override support (mypy, pyright, pyrefly)
* `include` and `exclude` options that both support negations. This would be the most complicated

**Related ruff issues**

* https://github.com/astral-sh/ruff/issues/8627
* https://github.com/astral-sh/ruff/issues/6262
* https://github.com/astral-sh/ruff/issues/8267



---

_Comment by @konstin on 2025-06-03 09:19_

Re default excludes, what are your default excludes? The uv build backend uses 3 as its default excludes are high-confidence (`"__pycache__", "*.pyc", "*.pyo"`).

I like the concept: Even with the include and exclude options, we can lower everything to a list of `<glob>` and `!<glob>`. It should be possible to build an efficient directory traversal from that.

---

_Comment by @MichaReiser on 2025-06-03 09:35_

> Re default excludes, what are your default excludes? 

I would probably take ruff's excludes as a starting point (minus `.ruff-cache`):

```
[".bzr", ".direnv", ".eggs", ".git", ".git-rewrite", ".hg", ".mypy_cache", ".nox", ".pants.d", ".pytype", ".ruff_cache", ".svn", ".tox", ".venv", "__pypackages__", "_build", "buck-out", "dist", "node_modules", "venv"]
```



---

_Comment by @MichaReiser on 2025-06-03 15:33_

I prototyped what I outlined above using gitignore syntax and I find the outcome confusing. For example, 

```
[tool.ty.src]
files = ["yaml/", "!yaml/main.py", "!.git/"]
```

I'd expect that all files in the `yaml` directory are included with the exception of `main.py` But this isn't the case because `yaml/` matches: 

* only the directory, but not its files
* but it matches all directories called `yaml`, not just the directory with said name at the project root. This is less of a usability issue because it probably does the right thing in most cases. However, it's a performance issue because it roughly matches the glob pattern `**/yaml` which requires ty to traverse all directories, whereas users might expect that ty only traverses the `yaml` directory.

I think we want "regular" glob syntax with the exception that we borrow the `!` to negate patterns (for exclusion). This obviously has the downside that it looks like gitignore syntax when it isn't and that might be confusing for users who are very familiar with gitignore.

---

_Comment by @dhruvmanila on 2025-06-03 16:06_

For another data point, there's a similar convention for `eslint.codeActionsOnSave.rule` (https://github.com/microsoft/vscode-eslint?tab=readme-ov-file#settings-options) that uses `!` for exclusion and I think it uses glob syntax. Additionally, this is also something that I'm interesting in adding support for including / excluding rules for code actions on save (https://github.com/astral-sh/ruff/issues/12709).

---

_Comment by @MichaReiser on 2025-06-03 16:07_

> For another data point, there's a similar convention for eslint.codeActionsOnSave.rule ([microsoft/vscode-eslint#settings-options](https://github.com/microsoft/vscode-eslint?tab=readme-ov-file#settings-options)) that uses ! for exclusion and I think it uses glob syntax. Additionally, this is also something that I'm interesting in adding support for including / excluding rules for code actions on save (https://github.com/astral-sh/ruff/issues/12709).

Yes, I think we'll probably use `!` as a general "negation" pattern in our configuration. At least in all situations where there isn't a more natural way (e.g. a dict where the values of the same keys are overriden) or natural value to do so

---

_Comment by @MichaReiser on 2025-06-05 14:45_

I'm now leaning towards to fields again because I realised that a single field is problematic when it comes to excludes:

* There's a user configuration with `!**/dist`
* There's a project configuration with `src`

Matching `src/dist` would now be included because the project configuration has a higher precedence. This might be what the user intended but it's more likely that it isn't. The same problem applies to default exclusions where default exclusions would have to be tested last, preventing a user from overriding any default exclusion (e.g. a user includes `build` which is a default exclusion, any path in `build` would match, overriding the default exclusion)

---

_Closed by @MichaReiser on 2025-06-12 17:07_

---
