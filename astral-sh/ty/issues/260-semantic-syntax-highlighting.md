```yaml
number: 260
title: Semantic syntax highlighting
type: issue
state: closed
author: kiyoon
labels:
  - server
  - wish
assignees: []
created_at: 2025-05-08T02:59:34Z
updated_at: 2025-07-13T14:13:41Z
url: https://github.com/astral-sh/ty/issues/260
synced_at: 2026-01-12T15:54:22Z
```

# Semantic syntax highlighting

---

_@kiyoon_

I would like to know if anything is planned for semantic highlighting.

It is a feature provided in basedpyright, pylance and even typescript language server that highlights based on the semantics, like known variables by the LSP are highlighted.

This has been very important in my workflow because I set syntax highlighting to very minimal and prefer to use mainly semantic highlighting. More colour in my code means more type-complete, making programming more fun kind of like painting.

I appreciate your work!

---

_Comment by @kiyoon on 2025-05-08 03:01_

For example, the first function call would be coloured and the second would be white if I used basepyright's semantic highlighting.

<img width="243" alt="Image" src="https://github.com/user-attachments/assets/6e08cdcd-ae42-4156-832e-70b673fe2d1c" />

<img width="254" alt="Image" src="https://github.com/user-attachments/assets/7b72d4c1-7ecc-42a4-b6b5-3b1209606cef" />

---

_Comment by @carljm on 2025-05-08 04:27_

That's a cool feature! I think we have a fair amount of higher-priority things we need to do first, but this seems like an interesting feature to consider in future.

---

_Label `wish` added by @carljm on 2025-05-08 04:27_

---

_Label `server` added by @dhruvmanila on 2025-05-08 04:33_

---

_Comment by @kiyoon on 2025-05-08 04:35_

Thanks for considering!

As a reference this is what basedpyright looks like:

<img width="337" alt="Image" src="https://github.com/user-attachments/assets/9b5d1d7c-e7cd-4d85-9314-ceaac86447cc" />

The `var_outside_scope` has a semantic highlight as well, it just happens to be white in this case. So the variable within and outside of function have different colours.

---

_Renamed from "feat: semantic highlighting" to "Semantic syntax highlighting" by @MichaReiser on 2025-05-28 11:29_

---

_Comment by @MichaReiser on 2025-07-13 14:13_

ty now supports basic semantic syntax highlithing

---

_Closed by @MichaReiser on 2025-07-13 14:13_

---
