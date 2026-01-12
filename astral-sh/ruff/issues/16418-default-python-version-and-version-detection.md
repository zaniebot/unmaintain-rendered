```yaml
number: 16418
title: Default Python version and version detection
type: issue
state: closed
author: ntBre
labels:
  - configuration
assignees: []
created_at: 2025-02-27T16:30:10Z
updated_at: 2025-05-09T19:22:51Z
url: https://github.com/astral-sh/ruff/issues/16418
synced_at: 2026-01-12T15:54:55Z
```

# Default Python version and version detection

---

_@ntBre_

As brought up in https://github.com/astral-sh/ruff/issues/16417, the default Python version of 3.9 in ruff can be quite confusing, especially with the new version-related syntax errors. It would be nice if we could improve the no-config experience here. 

A few possibilities for addressing this could be:
* change the default Python version to latest (3.13 currently)
* introduce a distinct `undefined` version value to suppress version-related errors and rules
* try to detect the environment Python version

The first of these would probably be easiest to implement but would make some rules (like the [`UP`](https://docs.astral.sh/ruff/rules/#pyupgrade-up) rules) ~~useless~~ apply incorrectly without setting a lower target version. The second option would likely have the same effect (`target_version < 3.10` just becomes `target_version.is_some_and(< 3.10)` for example).

---

_Label `configuration` added by @ntBre on 2025-02-27 16:30_

---

_Comment by @AlexWaygood on 2025-02-27 16:40_

> The first of these would probably be easiest to implement but would make some rules (like the [`UP`](https://docs.astral.sh/ruff/rules/#pyupgrade-up) rules) useless without setting a lower target version.

Rather than making the `UP` rules useless, this would make the `UP` rules break a lot of code unintentionally! They might start upgrading Python files to use newer syntax that doesn't work on older Python versions, even though the library or application is meant to still work on those older Python versions.

---

_Comment by @ntBre on 2025-02-27 16:45_

Oh of course, thanks! I've been using so many `target_version < ...` checks for syntax errors that I forgot most rules bail out in those cases.

---

_Comment by @AlexWaygood on 2025-02-27 18:32_

> The second option would likely have the same effect

So here's some different effects the second option might have:
-  We could just disapply all pyupgrade rules (and rules like them) if no target version had been explicitly set in `ruff.toml` or `pyproject.toml`. If we see that no target version has been explicitly set, arguably we can't say for sure that upgrading to use new syntax won't break the user's code, so no pyupgrade rule would be safe.
- We could not emit any syntax error rules that only occur on some Python versions
- We could apply a "generous" interpretation of which modules are standard-library. We get quite a lot of issues like https://github.com/astral-sh/ruff/issues/16405, where users are confused that `tomllib`, for example, is not considered standard-library unless they explicitly set `requires-python = ">= 3.11"` (or similar with `tool.ruff.target-version`). If we're sorting imports and we see that no target version has been set explicitly, we could consider a module standard-library if it's a standard-library module on _any_ Python version rather than checking if it's a standard-library module on _some specific_ Python version.
- Similarly, we've had reports of user confusion before when a symbol is considered undefined because it's a builtin symbol added in a new version of Python, but they haven't set the target version explicitly. We could do the same thing there -- be generous and assume a symbol is a bound builtin symbol if it's a builtin on _any_ Python version, when the target version is not set.

---

_Comment by @ntBre on 2025-02-27 23:01_

Ah those are good points, and I think they make this sound like the best option to me, at least of the ones listed in the issue.

Are there any cases where "any Python version" is different from "latest Python version" in the "generous" interpretation? I guess that would be the case if symbols were removed from the standard library. It would be nice if using the latest version for these checks would cover everything, like with the syntax errors, but that's just an implementation detail really.

---

_Comment by @AlexWaygood on 2025-02-27 23:04_

> Are there any cases where "any Python version" is different from "latest Python version" in the "generous" interpretation?

Yes -- an example might be that a bunch of modules were removed in Python 3.13 as part of [PEP 594](https://peps.python.org/pep-0594/). Under the "generous" interpretation, we would still sort these as if they were stdlib modules when no Python version was specified, since they are stdlib modules on _some_ Python versions.

---

_Comment by @dhruvmanila on 2025-02-28 06:13_

I think (3) is also a very compelling option because that would move us closer towards the "better default behavior" goal. 

At least in an editor context, especially with the VS Code extension, we get that for free (at least for now, see https://github.com/astral-sh/ruff-vscode/issues/479#issuecomment-2688555550). We would still have to make a decision or highlight it to the user when there's a mismatch between the selected Python version in the editor v/s the one in `pyproject.toml` (`project.requires-python`) or the [`tool.ruff.target-version`](https://docs.astral.sh/ruff/settings/#target-version).

On the command-line, we could default to (2) and a possibility to develop it during a uv integration in the future.

---

_Comment by @MichaReiser on 2025-02-28 07:13_

I'm not convinced that 3 is the right solution here for two reasons:

* We still need 1. or 2. in cases where Python isn't installed. That's unlikely but can be the case for CI jobs that only run `ruff format` and `ruff lint` (they don't need python!)
* Your Ruff configuration might work fine on one machine and fail on another. For example, you setup Ruff in your project and push to CI where checks suddenly start failing (or formatting changes) because a different or no Python version is installed. 

I think my preference is a combination of 1 and 2:

Using `Unknown` for the python version gives each tool -- each rule even -- the possibility to decide what the least error prone version is: syntax errors: assume latest, formatter: assume a reasonably old version but not ancient version, builtins lint rule: assume latest (because it's more likely that new builtins are added then removed), py upgrade: skip? 



---

_Comment by @dhruvmanila on 2025-02-28 07:24_

My main motivation for (3) at least in an editor context is from the screenshot in https://github.com/astral-sh/ruff-vscode/issues/709 and I don't consider it as a priority right now but something to consider for the future. I think it's a common scenario that a user would open an editor with a Python file and start writing code. They might use an older Python version or the editor selected an older Python version by default which might result in Ruff not highlighting this because it assumed the latest version. For an editor context, it might be useful to consider this as the last resort before using `Unknown`.

---

_Comment by @MichaReiser on 2025-02-28 07:27_

I'm okay to change how we initialize the python version in the editor. I do think we should make use of the information that the editor provides us or implement 3 ourselves. I think that's also something that has come up in the red knot proposal. 

---

_Comment by @Kristinita on 2025-04-19 12:48_

### 1. Summary

I use latest Python versions. I don’t want to get warnings and errors specific for old Python versions, and I want to get warnings and errors from tools like Pyupgrade about outdated syntax.

It would be nice if would be possible not change the value of the variable `target-version` manually every time when a new Python version be released.

### 2. Examples of the desired behavior

1. `target-version = "current"` — Ruff will detect the Python version of the current environment and set the value of the `target-version` based on it [**like Pylint use by default the value `sys.version_info[:2]` for its option `--py-version`**](https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#py-version).
1. `target-version = "latest"` — Ruff will use for the value of the option `target-version` the number of the latest Python version. I.e., at the moment of writing this comment `target-version = "latest"` is the equivalent of `target-version = "py313"`.

Thanks.


---

_Comment by @danra on 2025-04-21 22:37_

> A few possibilities for addressing this could be:

>    change the default Python version to latest (3.13 currently)

Doing so with no transition period would likely cause friction in many codebases after upgrading `ruff` due to new rules kicking-in which were previously disabled, e.g., [B905](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/) (I've only just become aware of the existence of such rules myself)

---

_Comment by @MichaReiser on 2025-05-09 06:04_

@ntBre I think we can close this now

---

_Closed by @ntBre on 2025-05-09 18:05_

---

_Comment by @Kristinita on 2025-05-09 18:55_

**Type: Question** :question:

@ntBre, @Glyphack, @MichaReiser, what needs to do users that want to have something like `target-version = "latest"` or `target-version = "current"`? I don’t see changes in the [**documentation of the option `target-version`**](https://docs.astral.sh/ruff/settings/#target-version).

Thanks.


---

_Comment by @ntBre on 2025-05-09 19:22_

You can open a separate feature request, but I don't think we're planning to support this kind of thing in the near future.

---
