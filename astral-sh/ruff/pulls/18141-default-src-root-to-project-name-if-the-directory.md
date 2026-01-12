```yaml
number: 18141
title: "Default `src.root` to `['.', '<project_name>']` if the directory exists"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/project-name
created_at: 2025-05-16T17:56:04Z
updated_at: 2025-05-19T16:11:29Z
url: https://github.com/astral-sh/ruff/pull/18141
synced_at: 2026-01-12T15:56:13Z
```

# Default `src.root` to `['.', '<project_name>']` if the directory exists

---

_@MichaReiser_

## Summary

I noticed in #18137 that we fail to resolve all imports for some ecosystem repositories because they don't use the "traditional" src or flat layouts. Instead, they use a `src` layout but they use the project name instead of `src` for the "src folder name". 

An example is `psycopg` where all source files are in `psycopg/psycopg/_adapters_map.py:279:14`

This PR extends our heuristics for `src.root` to recognize if a project has a `<project>/<project>` folder (and no `src` folder) and, if so, includes it in the `src.root`. 

The motivation for this change is to support more projects with zero configuration. 

I don't have a good sense of how common this project layout is, but it seems worth supporting, given that multiple ecosystem projects use it. This also makes our mypy primer results significantely more useful :)

## Test plan

See mypy primer results, added unit tests.

---

_Label `configuration` added by @MichaReiser on 2025-05-16 17:57_

---

_Label `ty` added by @MichaReiser on 2025-05-16 17:57_

---

_Comment by @MichaReiser on 2025-05-16 18:16_

and mypy primer hangs here too :( I wonder if it is just because of the too large diff. Any suggestions on how I could try this out?

---

_Comment by @github-actions[bot] on 2025-05-17 15:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-05-17 16:15_

---

_Review requested from @carljm by @MichaReiser on 2025-05-17 16:15_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-17 16:15_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-17 16:15_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-17 16:15_

---

_Comment by @MichaReiser on 2025-05-17 16:28_

> - Found 797 diagnostics
> + Found 646 diagnostics

There are no other changes to mypy primer, so maybe it's not worth changing the default just for one project. I'd be interested to hear from people with more knowledge of the Python ecosystem to hear how common this project structure is.


---

_Comment by @MichaReiser on 2025-05-19 09:25_

I think we should fix this for this specific mypy primer project for now. We can consider changing our heuristics if this comes up more frequently.

---

_Closed by @MichaReiser on 2025-05-19 09:25_

---

_Comment by @AlexWaygood on 2025-05-19 15:27_

This proposed change makes a lot of sense to me. The `src/` convention is encouraged by most common package managers, but it's a fairly recent convention. Until relatively recently, it was very common to have the directory structure that this PR adds support for, and it's still not uncommon for older projects.

I think we should also add `tests/` to the `src.root` default, as discussed in https://github.com/astral-sh/ty/issues/414#issuecomment-2886755988

---

_Comment by @carljm on 2025-05-19 15:35_

I also wouldn't have any objection to resurrecting and landing this PR; it doesn't seem to have any downside.

I'm not sure it's necessary that we check that `<project-name>/<project-name>` exists, as this PR currently does? That is, it would be fine to do this simply if `<project-name>/` exists.



---

_Reopened by @MichaReiser on 2025-05-19 15:36_

---

_Comment by @carljm on 2025-05-19 15:38_

> I'm not sure it's necessary that we check that `<project-name>/<project-name>` exists, as this PR currently does? That is, it would be fine to do this simply if `<project-name>/` exists.

Never mind this, Alex pointed out that if just `<project-name>/` exists, it might be a flat layout, and we probably shouldn't add the extra root in that case.

---

_Comment by @MichaReiser on 2025-05-19 15:47_

> I think we should also add tests/ to the src.root default, as discussed in https://github.com/astral-sh/ty/issues/414#issuecomment-2886755988

I'm up for this but I suggest doing this in a separate PR. While related, it needs to apply to all layouts.

---

_Comment by @AlexWaygood on 2025-05-19 15:50_

sounds good

---

_@carljm approved on 2025-05-19 15:58_

---

_Merged by @MichaReiser on 2025-05-19 16:11_

---

_Closed by @MichaReiser on 2025-05-19 16:11_

---

_Branch deleted on 2025-05-19 16:11_

---
