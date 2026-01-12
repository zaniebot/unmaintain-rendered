```yaml
number: 87
title: Go to definition
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:17:20Z
updated_at: 2025-07-31T19:23:10Z
url: https://github.com/astral-sh/ty/issues/87
synced_at: 2026-01-12T15:54:22Z
```

# Go to definition

---

_@MichaReiser_

Implement go to definition in the LSP

---

_Label `server` added by @MichaReiser on 2025-03-14 10:18_

---

_Comment by @MichaReiser on 2025-03-19 17:27_

The LSP specification supports three different operations:

* [Go to definition](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_definition)
* [Go to declaration](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_declaration)
* [Go to type definition](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_typeDefinition)

See [reddit](https://www.reddit.com/r/neovim/comments/11u3sx3/lsp_differences_between_definition_declaration/) and the [sorbet documentation](https://sorbet.org/docs/go-to-def)

@AlexWaygood mentioned that pyright (and mypy?) support navigating to the typing stub or the definition in the python source. I don't think we can support the second yet because our module resolver doesn't support resolving both.

---

_Comment by @AlexWaygood on 2025-03-19 17:32_

> (and mypy?)

mypy doesn't have an LSP integration; it's just a CLI tool. There are plugins that integrate it into VSCode but it doesn't have any features like "go to definition" in the same way that pyright does or the way that we aim to.

> I don't think we can support the second yet because our module resolver doesn't support resolving both.

Agreed. It would be a lovely feature to have eventually, especially since pyright already implements it, but it definitely isn't essential for an alpha or beta release.

---

_Comment by @MichaReiser on 2025-03-20 16:18_

[Sorbet](https://sorbet.org/docs/go-to-def#jumping-to-the-definition-took-me-to-a-webpage-instead-of-a-ruby-file) has a custom command to support "jump to definition" for their equivalent of vendored typeshed stubs

---

_Comment by @AlexWaygood on 2025-03-20 16:19_

Jumping to a definition in a stub feels more like "go to declaration" rather than "go to definition" to me: stubs contain declarations rather than definitions

---

_Comment by @MichaReiser on 2025-03-20 17:50_

> Jumping to a definition in a stub feels more like "go to declaration" rather than "go to definition" to me: stubs contain declarations rather than definitions

Yeah, the distinction between go to definition and go to declaration is certainly very vague in Python. However, I'd expect *Go to definition* for `os.name` to jump to the typing stub simply because there's no other place we can jump to. I would be annoyed if Red Knot simply does *nothing* (Go to definition is the default if you Cmd/Ctrl click a name).

---

_Comment by @AlexWaygood on 2025-03-20 17:55_

In the long term I'd like "go to definition" to jump to https://github.com/python/cpython/blob/ce79274e9f093bd06d2285c9af48dbcbc92173de/Lib/os.py#L53 for whatever Python interpreter is in use for `os.name`, but for "go to declaration" to jump to the typing stub. That's what pyright does, and it makes sense to me. I realise we can't support that for red-knot right away, but I think that should be our long-term goal.

---

_Comment by @AlexWaygood on 2025-03-20 17:57_

However, I'm okay with it if you want to implement jumping to the stub definition as "go to definition" rather than "go to declaration" initially! I can see that it would indeed be annoying if red-knot did nothing at all when you CTRL-clicked a name.

---

_Comment by @MichaReiser on 2025-03-20 17:59_

> However, I'm okay with it if you want to implement jumping to the stub definition as "go to definition" rather than "go to declaration" initially! I can see that it would indeed be annoying if red-knot did nothing at all when you CTRL-clicked a name.

I think it's less a "want to" but more a: Not doing it because it is a serious scope increase ;)

I also realised that `os` isn't a good example. The point is, go to definition should jump to the typing stub if no source file is available (because it's a native module or, for now, because we don't support resolving both stubs and source files at the same time)

---

_Comment by @AlexWaygood on 2025-03-20 18:00_

> I also realised that `os` isn't a good example. The point is, go to definition should jump to the typing stub if no source file is available (because it's a native module or, for now, because we don't support resolving both stubs and source files at the same time)

Yeah, and that's also what pyright does. The `sys` module _is_ a good example, as it's all written in C. CTRL-clicking on `sys`-module APIs in pyright jumps to the stub, because there's no runtime source available.

---

_Comment by @MichaReiser on 2025-03-20 18:04_

I talked with @carljm and I might start with *go to type definition*, because, it turns out, that's easier with the infrastructure we have today.

---

_Label `server` added by @MichaReiser on 2025-05-07 15:08_

---

_Renamed from "[red-knot] Go to definition" to "Go to definition" by @MichaReiser on 2025-05-07 15:17_

---

_Comment by @MichaReiser on 2025-07-31 19:23_

An initial version of go to definition and go to declaration is now implemented, see https://github.com/astral-sh/ruff/pull/19371

---

_Closed by @MichaReiser on 2025-07-31 19:23_

---
