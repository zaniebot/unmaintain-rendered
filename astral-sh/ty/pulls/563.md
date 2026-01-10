```yaml
number: 563
title: "Update `docs/README.md`'s table of contents automatically"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - documentation
assignees: []
base: main
head: docs-readme-toc
created_at: 2025-06-02T09:02:52Z
updated_at: 2025-06-03T09:43:07Z
url: https://github.com/astral-sh/ty/pull/563
synced_at: 2026-01-10T02:34:10Z
```

# Update `docs/README.md`'s table of contents automatically

---

_Pull request opened by @InSyncWithFoo on 2025-06-02 09:02_

## Summary

Resolves #505.

A new script, `update_docs_toc.py`, is added; it generates a list of links to level-2 headers and writes that back to `docs/README.md`. It is run in CI via `autogenerate_files.sh`.

## Test Plan

None.


---

_Renamed from "[ty] Update `docs/README.md`'s TOC" to "Update `docs/README.md`'s TOC" by @InSyncWithFoo on 2025-06-02 09:12_

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:22 on 2025-06-02 09:45_

I think this would make it clearer to contributors that this section of the document should not be manually edited:

```suggestion
_TOC_START_MARKER = "<!-- Generated table of contents start -->\n\n"
_TOC_END_MARKER = "\n\n<!-- Generated table of contents end -->"
```

---

_@AlexWaygood reviewed on 2025-06-02 09:45_

---

_@AlexWaygood reviewed on 2025-06-02 09:48_

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:3 on 2025-06-02 09:48_

instead of pinning dependencies here, maybe we could use a script lockfile, similar to [what we do for mdtest.py in the Ruff repo](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/mdtest.py.lock) (more dogfooding!)

---

_@AlexWaygood reviewed on 2025-06-02 09:50_

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:31 on 2025-06-02 09:50_

nit: I wouldn't really prefix any variable names in this file with underscores. It's a script, not a library; I don't think making a distinction between public and private names in this module is meaningful, and the underscores just add visual noise IMO

---

_Label `documentation` added by @MichaReiser on 2025-06-02 09:50_

---

_@AlexWaygood reviewed on 2025-06-02 09:52_

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:6 on 2025-06-02 09:52_

```suggestion
"""Update `docs/README.md`'s table of contents.

This script generates a list of links to level-2 headers and writes that back
to `docs/README.md`. It is run in CI via `autogenerate_files.sh`.
"""
```

---

_@AlexWaygood reviewed on 2025-06-02 09:55_

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:6 on 2025-06-02 09:55_

It would also be great if you could add a short docstring to each function in this file. It doesn't have to be more than a sentence or two. We won't be looking at this code much, so it'll help refresh our memory about how it works when we come back to it at a later date if it's well commented.

---

_@AlexWaygood reviewed on 2025-06-02 09:57_

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:59 on 2025-06-02 09:57_

```suggestion
    try:
        start_marker_index = original.index(_TOC_START_MARKER)
        end_marker_index = original.index(_TOC_END_MARKER)
    except ValueError as e:
        raise RuntimeError(f"No table of contents found in {_DOCS_README}") from e
```

---

_Review comment by @AlexWaygood on `scripts/update_docs_toc.py`:62 on 2025-06-02 09:57_

Can you add a comment explaining why you are ignoring the type-checker and linter errors here?

---

_@AlexWaygood reviewed on 2025-06-02 09:57_

---

_Renamed from "Update `docs/README.md`'s TOC" to "Update `docs/README.md`'s table of contents automatically" by @InSyncWithFoo on 2025-06-02 19:49_

---

_Comment by @zanieb on 2025-06-02 21:43_

This is exactly the kind of investment I wanted to avoid; as mentioned in https://github.com/astral-sh/ty/pull/464#issuecomment-2895485130

I don't really mind that you did it now, but... we really shouldn't be spending time on things that will be obviated by moving to mkdocs in the near future.

What this indicates to me, is that we shouldn't have accepted the ToC in the first place.

---

_Comment by @AlexWaygood on 2025-06-03 09:43_

@InSyncWithFoo, thanks for the PR -- I really appreciate it! If we were planning to stick with having GitHub-hosted docs in the long term, I think we'd definitely want a solution like this.

I was already wondering whether it was worth adding this much code (which will be a maintenance burden itself!) just to keep a pretty short TOC up to date, however. Having a partially generated file in the codebase also has the disadvantage that it could confuse contributors, who have already been confused by our fully generated documentation files (which are conceptually much simpler).

Given @zanieb is also sceptical, I think we'll decline this in favour of manually updating the TOC for now. It won't be too long until we switch to mkdocs, which will mean we won't need a solution like this. And it will anyway never be possible to autogenerate all documentation to guarantee that it's kept up to date -- another recent example is https://github.com/astral-sh/ty/issues/564.

Thanks again for the PR!

---

_Closed by @AlexWaygood on 2025-06-03 09:43_

---
