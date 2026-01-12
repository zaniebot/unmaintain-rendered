```yaml
number: 1991
title: goto-declaration to go to import instead of definition
type: issue
state: open
author: sirfz
labels:
  - server
assignees: []
created_at: 2025-12-17T09:40:44Z
updated_at: 2025-12-18T09:20:29Z
url: https://github.com/astral-sh/ty/issues/1991
synced_at: 2026-01-12T15:54:26Z
```

# goto-declaration to go to import instead of definition

---

_@sirfz_

Congrats on the Beta release!

This is a feature that existed forever in jedi since before LSPs existed and it's something that I've been missing (A LOT tbh) since moving on with (based)pyright which don't implement this LSP feature at all (iirc).

Basically, if I call goto-declaration (not goto-definition) on an imported class/object/etc, it would jump to the import statement in the same file instead of the source definition.

Pyrefly already added this feature after I mentioned it on HN, which was cool. Would be great to also have it in Ty if possible (it already works in Zuban, which is the successor of Jedi).

---

_Label `server` added by @AlexWaygood on 2025-12-17 09:57_

---

_Comment by @MichaReiser on 2025-12-18 09:08_

TypeScript does a slightly different distinction (at least in their non LSP based extension) but I liked having the option to jump to both. 

I think what's important is that clicking go to declaration on the imported name should still resolve to the declaration in the other file.

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-18 09:08_

---

_Comment by @sirfz on 2025-12-18 09:20_

From memory, I believe pyright doesn't implement `textDocument/declaration` at all (I just used to get an error that it's not provided by the lsp). The behavior as it is in zuban/pyrefly right now is to behave like `textDocument/definition` for anything within the same file (just jumps to where a variable/class is declared) but if it's in a different file, it would jump to the import line which is very useful to check from were a class/function/constant is being imported.

---
