```yaml
number: 21772
title: "[ty] Don't send publish diagnostics for clients supporting pull diagnostics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/did-changed-watched-files-publish-diagnostics
created_at: 2025-12-03T07:55:31Z
updated_at: 2025-12-04T07:12:07Z
url: https://github.com/astral-sh/ruff/pull/21772
synced_at: 2026-01-12T15:57:33Z
```

# [ty] Don't send publish diagnostics for clients supporting pull diagnostics

---

_@MichaReiser_

## Summary

fixes: https://github.com/astral-sh/ty/issues/1727

This PR fixes the reported duplicated diagnostic in https://github.com/astral-sh/ty/issues/1727 by skipping `publish_diagnostics` 
if pull diagnostics are enabled, similar to how we do this in other places. 

While this PR fixes the duplicate diagnostics, there's still the question why vim sends `didChangeWatchedFiles` for 
a file that's currently open in the editor. The issue with that is that the server is now in this awkward spot
that it has to decide whether it should use the new content on disk OR the content that's currently open in the editor (as reported by `didOpen`, `didChange`). For now, ty uses the content sent by `didOpen`/`didChange`,
meaning, ty will ignore the new content when checking the project. 

I think this is the sensible thing to do here. The client either has to tell the server that the file is now closed,
in which case ty will use the content from disk, or that the user accepted the on-disk changes over the in-editor changes
by sending a `didChange` notification. 

## Test Plan

Added E2E test


---

_Review requested from @carljm by @MichaReiser on 2025-12-03 07:55_

---

_Label `bug` added by @MichaReiser on 2025-12-03 07:55_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-03 07:55_

---

_Label `server` added by @MichaReiser on 2025-12-03 07:55_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-03 07:55_

---

_Label `bug` added by @MichaReiser on 2025-12-03 07:55_

---

_Label `server` added by @MichaReiser on 2025-12-03 07:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-03 07:57_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 07:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-03 08:14_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-03 08:25_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-03 08:37_

---

_Label `ty` added by @AlexWaygood on 2025-12-03 08:44_

---

_Review request for @carljm removed by @carljm on 2025-12-04 00:33_

---

_Review requested from @dhruvmanila by @carljm on 2025-12-04 00:34_

---

_Comment by @dhruvmanila on 2025-12-04 05:49_

> While this PR fixes the duplicate diagnostics, there's still the question why vim sends `didChangeWatchedFiles` for
> a file that's currently open in the editor.

Yeah, I'm not sure either. I looked at the request payload:

```
{
  jsonrpc = '2.0',
  method = 'workspace/didChangeWatchedFiles',
  params = {
    changes = {
      { type = 3, uri = 'file:///Users/dhruv/dotfiles/4913' },
      { type = 1, uri = 'file:///Users/dhruv/dotfiles/t.py' },
      { type = 3, uri = 'file:///Users/dhruv/dotfiles/t.py~' },
    },
  },
}
```

It seems to be sending an entry stating that the `t.py` file was created which seems weird because the file is already open in the editor and it already exists on the filesystem.

---

_@dhruvmanila approved on 2025-12-04 05:50_

---

_Merged by @MichaReiser on 2025-12-04 07:12_

---

_Closed by @MichaReiser on 2025-12-04 07:12_

---

_Branch deleted on 2025-12-04 07:12_

---
