```yaml
number: 11690
title: "Docs: refactor/enhance dependency groups section"
type: pull_request
state: closed
author: rsyring
labels: []
assignees: []
base: main
head: docs-dependency-groups
created_at: 2025-02-21T06:22:07Z
updated_at: 2025-08-14T15:57:28Z
url: https://github.com/astral-sh/uv/pull/11690
synced_at: 2026-01-10T06:44:32Z
```

# Docs: refactor/enhance dependency groups section

---

_Pull request opened by @rsyring on 2025-02-21 06:22_

## Summary

- Add example of dependency group includes
- Refactor section to to show pyproject.toml config & explanation of dependency resolution first and move CLI implementation and options into it's own section

It took me a decent amount of searching through the issues and multiple attempts to resist just posting a "question" issue to ask how to do group includes before I finally stumbled upon the answer pointing to pep 735.  Since I had been over the docs in that section multiple times, I thought it would help to just add the answer front and center.

Also refactored the section to separate config from cli.  The part about resolution all being done together was something I had to search for a couple days ago and it seems like that's an important bit that should be closer to the top of the section instead of mixed in after the CLI option discussion.

Feel free to ask for adjustments if desired.

## Test Plan

I started the docs server and reviewed manually.


---

_Review comment by @Gankra on `docs/concepts/projects/dependencies.md`:638 on 2025-02-21 16:41_

```suggestion
dev = [
  {include-group = "lint"},
  "pytest",
]
```

---

_Review comment by @Gankra on `docs/concepts/projects/dependencies.md`:672 on 2025-02-21 16:41_

```suggestion
dev = [
  {include-group = "lint"},
  "pytest",
]
```

---

_Review comment by @Gankra on `docs/concepts/projects/dependencies.md`:617 on 2025-02-21 16:45_

Uncertain this is the "right" thing to cite? (genuine question, need to check with someone else...)

---

_Review comment by @Gankra on `docs/concepts/projects/dependencies.md`:673 on 2025-02-21 16:49_

This seems like a distracting/strange thing to include in the documentation.

---

_@Gankra approved on 2025-02-21 16:51_

This seems like a reasonable tweak but I'm gonna get a second opinion.

---

_@rsyring reviewed on 2025-02-21 18:06_

---

_Review comment by @rsyring on `docs/concepts/projects/dependencies.md`:617 on 2025-02-21 18:06_

The docs already reference [PEP 735](https://peps.python.org/pep-0735/).  I was going to reference it again but noticed that the pep itself points to the dependency groups spec as the canonical version.  So I linked to it.  

However, upon reviewing the docs, I realized there was further room for improvement so pushed up another commit with some additional refactoring which removes this sentence.

However, it's up for debate on whether or not the existing PEP 735 reference should be kept or replaced with the link to the spec.  I'd be +0 towards the latter since the PEP points there.

---

_@rsyring reviewed on 2025-02-21 18:07_

---

_Review comment by @rsyring on `docs/concepts/projects/dependencies.md`:673 on 2025-02-21 18:07_

Since there was an issue open about it, I thought it might save someone from creating another issue.  :)

I moved the notice to a comment in the example but wouldn't mind at all if your preference is to remove that too.

---

_@rsyring reviewed on 2025-02-21 18:08_

---

_Review comment by @rsyring on `docs/concepts/projects/dependencies.md`:638 on 2025-02-21 18:08_

fixed, thanks

---

_Comment by @rsyring on 2025-02-21 18:09_

Forced pushed a new commit (would you prefer separate commits when addressing PR comments?) addressing a couple of the above issues as well as some additional refactoring.

---

_Comment by @Gankra on 2025-02-21 18:10_

You can put the commits up in any form you prefer, we always squash-merge so don't worry about it too much.

---

_Comment by @rsyring on 2025-08-14 15:57_

This PR was primarily concerned with adding a section in the docs for `include-group`.  That's there now under "Nesting groups".  Given the age of this PR, it can likely be closed as stale.

---

_Closed by @rsyring on 2025-08-14 15:57_

---
