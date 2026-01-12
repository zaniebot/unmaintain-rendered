```yaml
number: 15695
title: "[red-knot] Explore alternative notation for declaring file names in mdtests"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - testing
  - ty
assignees: []
created_at: 2025-01-23T17:15:00Z
updated_at: 2025-02-04T07:27:18Z
url: https://github.com/astral-sh/ruff/issues/15695
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Explore alternative notation for declaring file names in mdtests

---

_@MichaReiser_

### Description

Our mdtest framework allows multifile tests. The file's path is declared as an attribute in the code snippet:

``````
```pyi path=/typeshed/stdlib/builtins.pyi
class Custom: ...

custom_builtin: Custom
```
``````

The downside of this is that the paths arent visible when rendering the markdown file on Github, which makes it harder to understand the test.

![Image](https://github.com/user-attachments/assets/d0124b42-5919-4b5d-9228-1826df6f6ffe)

We should explore if there are other notations for specifying a file's path that is visible when rendered on GitHub. We probably want to support both notations (the old and new one) for now to avoid having to rewrite all tests. 

E.g. we could use a bold text right before a code snippet as the file path. 

``````
**/typeshed/stdlib/builtins.pyi**

```pyi
class Custom: ...

custom_builtin: Custom
```
``````

The ideal solution would be if we can get GitHub to render the file's attributes because even the information that it is a `pyi` file is relevant. 

---

_Label `help wanted` added by @MichaReiser on 2025-01-23 17:15_

---

_Label `testing` added by @MichaReiser on 2025-01-23 17:15_

---

_Label `red-knot` added by @MichaReiser on 2025-01-23 17:15_

---

_Comment by @MichaReiser on 2025-01-23 17:29_

Github feature request https://github.com/orgs/community/discussions/77414

---

_Comment by @sharkdp on 2025-01-24 11:44_

Another thing to keep in mind is that the original plan for MDTests envisioned other metadata fields besides `path=`. Like `stage=` if we ever want to write tests for incremental computation:

https://github.com/astral-sh/ruff/blob/7778d1d64631cef107e98b634d2899d4cf0aab0b/crates/red_knot_test/README.md?plain=1#L407-L410



---

_Closed by @sharkdp on 2025-02-04 07:27_

---
