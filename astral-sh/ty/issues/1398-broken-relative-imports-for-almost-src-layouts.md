```yaml
number: 1398
title: "Broken relative imports for almost-`src` layouts"
type: issue
state: closed
author: Salamandar
labels:
  - question
  - imports
assignees: []
created_at: 2025-10-19T18:54:38Z
updated_at: 2025-10-26T08:25:08Z
url: https://github.com/astral-sh/ty/issues/1398
synced_at: 2026-01-12T15:54:25Z
```

# Broken relative imports for almost-`src` layouts

---

_@Salamandar_

### Summary

Here's the project topology:
```
myproject/
├─ src/
│  ├─ __init__.py
│  ├─ utils.py
│  └─ something.py
├─ tests/
└─ pyproject.toml
```

with `something.py` importing like `from .utils import utilityfunc`.

`ty` doesn't like that at all. debugging gives:

```
2025-10-19T18:52:58.723460Z DEBUG ty_python_semantic::types::infer::builder: Relative module resolution `.utils` failed: too many leading dots
    at crates/ty_python_semantic/src/types/infer/builder.rs:5010 on ThreadId(2)
    in ty_project::check_file with file=file(Id(c00))
    in ty_project::Project::check

```

So i guess `ty` finds the src directory and thinks it's gone one time up too many.

Is there any way to support this project layout ?

(of course creating a subdir src/myproject or renaming src to myproject "fixes" the issue)

### Version

ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)

---

_Comment by @AlexWaygood on 2025-10-19 19:04_

The issue here is that ty has spotted you have an `src` directory and assumed that you're therefore using the standard `src` layout. As a result, it's added `./src` as an import search path as well as the project root `.`, and `./src` takes precedence over `.` when resolving the import in `./src/something.py` because `./src/something.py` is relative to both paths, but `./src` is a subdirectory of `.`.

You can change ty's behaviour here by specifying `tool.ty.environment.root = ["."]` in your pyproject.toml file (https://docs.astral.sh/ty/reference/configuration/#root).

However, I would advise against this layout! This isn't really an "almost-`src` layout" -- it's the "flat layout", but your package happens to have the name `src`. That means that an `src` package will be directly importable from the root of your project, which seems like it's probably not what you'd want. I'm curious what build backend your using; I'd imagine this would confuse packaging tools as well as ty :-)

---

_Closed by @AlexWaygood on 2025-10-19 19:04_

---

_Label `question` added by @AlexWaygood on 2025-10-19 19:04_

---

_Label `imports` added by @AlexWaygood on 2025-10-19 19:04_

---

_Comment by @Salamandar on 2025-10-25 12:16_

> You can change ty's behaviour here by specifying tool.ty.environment.root = ["."] in your pyproject.toml file ([docs.astral.sh/ty/reference/configuration#root](https://docs.astral.sh/ty/reference/configuration/#root)).

Thanks ! That actually does the trick.

> However, I would advise against this layout
> That means that an src package will be directly importable from the root of your project

Yeah, I get what you mean. But actually this is a project with a long history, that is *always* installed as a deb package, so this is not an issue we can encounter (except when trying to run linters on the tests directory…)

> I'm curious what build backend your using

dh-build, and it looks like it doesn't have any issues with this layout. I think we manually describe which files goes where in the final package, that's probably why ;)

> I'd imagine this would confuse packaging tools as well as ty :-)

Well i've been using pyright, mypy and other linting / type checking tools and they don't have any issue with the current layout of the project. Maybe that's something they detect and adapt to ?

---

_Comment by @MichaReiser on 2025-10-26 08:25_

Some other type checker implement a fallback where they do a relative search from the importing module. I think we'll implement this behavior too, for better out-of-the-box IDE experience but there are a few drawbacks and we have to think how we can inform users that the fallback behavior is applied

---
