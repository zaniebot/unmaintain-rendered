---
number: 13963
title: "`uv pip check` for extras/optional dependency groups?"
type: issue
state: open
author: bollwyvl
labels:
  - enhancement
assignees: []
created_at: 2025-06-11T12:32:22Z
updated_at: 2025-06-12T14:43:27Z
url: https://github.com/astral-sh/uv/issues/13963
synced_at: 2026-01-10T01:25:40Z
---

# `uv pip check` for extras/optional dependency groups?

---

_Issue opened by @bollwyvl on 2025-06-11 12:32_

### Summary

Thanks for `uv`!

It would be lovely if users of `uv pip check` had a way to trigger validation optional dependencies, recursively, for a given set of specifiers.

`optional-dependencies`/`[extra]` are a blessing and curse for users. While these help projects keep their dependency hedges trimmed, once these are used recursively (e.g. an optional dependency of an optional dependency, maybe with some platform/tag selectors stirred in), it can become rather difficult to determine whether the state of an environment correctly fulfills all of them. 

`dependency-groups`/`include-group` (per PEP-735) will make things _more_ complicated for users, as their dependencies start using this syntax, and more terrifyingly, _combining_ it with `optional-dependencies`/`[extra]`.

My specific use case is testing repackaged python packages during a build for `conda-forge`, which presently has no concept of optional dependencies, much less python's specific conventions, so we're stuck with either "kitchen sinking," all `[extras]` or inventing virtual packages. In either case, the `package`-with-`[extras]`-under-test should have been installed, but likely _won't_ appear as the dependency of another package, so would evade detection with the current `uv pip check`, or `pip check`.

### Example

For `[ex,tras]`, likely just one argument would be needed, notionally which presumably could be either be given multiple times or further comma-separated. 

```bash
~$ uv pip check --help
Verify installed packages have compatible dependencies

Usage: uv pip check [OPTIONS]

Options:
      --check-with-extra <PACKAGE<[EXTRA<,...>]>,...>  Check given packages with (optional) optional [extra] dependencies
      --system                                         Check packages in the system Python environment [env: UV_SYSTEM_PYTHON=]
```

Not sure what to do about PEP-735, as it appears that will not have a pithy `requirements.txt`-compatible representation, but perhaps `--check-dep-groups` could be entirely separate, but reuse the square bracket convention.

Running against a (potentially interactively edited) environment:

```bash
~$ uv pip check --check-optional fastapi[all] --check-optional strawberry-graphql[fastapi,pydantic]
Using Python 3.xx.xx environment at: ~/projects/my-project/.venv
Checked xxx packages in xxxms

uvicorn-standard[standard] 0.34.3 requires httptools >=0.6.2 but httptools 0.6.1 is installed
```

Thanks again!

---

_Label `enhancement` added by @bollwyvl on 2025-06-11 12:32_

---

_Comment by @zanieb on 2025-06-11 20:09_

I think the solution here is probably to record requested extras during installation so `pip check` can read it, rather than adding CLI flags.

I'm not quite sure what you expect for dependency groups? That's not an installable unit in the same way as `foo[bar]`, so there's no environment "coherence" to check for?

---

_Comment by @notatallshaw on 2025-06-11 20:51_

I expect trying to record extras in installation metadata and trying to rely on it is going to have edge cases that don't work.

What if extras was requested but wasn't met? What if extras was requested by user on one install but then not requested on the next install by a transative dependency?

pip check is designed to validate if a users environment is consistent, not that it fufils certain requirements.

If you want to check a users environment is consistent and fufils certain requirements maybe it makes sense for something like `pip check -r requirements.txt`?

---

_Comment by @zanieb on 2025-06-11 22:56_

> What if extras was requested but wasn't met? 

What does this mean? Wouldn't the install fail?

> What if extras was requested by user on one install but then not requested on the next install by a transitive dependency?

That's a good question, I'm not sure what the best option is there. It certainly gets complicated.

> pip check is designed to validate if a users environment is consistent, not that it fufils certain requirements.

Yeah I agree with this — this is the crux of my confusion with the `dependency-groups` part of the request in particular.

---

_Comment by @bollwyvl on 2025-06-11 23:02_

> confusion with the `dependency-groups`

Indeed! I just don't know _how_ it will play out, once multiple tools are in the field, as the spec is [pretty vague](https://peps.python.org/pep-0735/#overlapping-install-ux-with-extras).

>> **Overlapping Install UX with Extras** 
>>
>> Tools MAY choose to provide the same interfaces for installing Dependency Groups as they do for installing extras.



---

_Comment by @bollwyvl on 2025-06-11 23:03_

> pip check -r requirements.txt

Woooee! Might as well throw `--constraints` on the barby, too!

---

_Comment by @notatallshaw on 2025-06-12 13:54_

Yes, you would need all the flags that could affect what packages can be selected. Thinking about it this can already by acheived in a few steps with `uv pip freeze` and `uv pip compile`.

1. Output current environment with `uv pip freeze` say to `current_environment.txt`
2. Use `uv pip compile` command with requirements and constraints specifying all the extras and dependency groups you want, where:
 a.  `current_environment.txt` is set as the output file
 b. Do not pass an `--upgrade` command or anything that eagerly updates anything
 c. Do not pass `--universal` or anything that would select for a different platform or Python version
3. Validate `current_environment.txt` has not changed



---
