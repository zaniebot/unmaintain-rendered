```yaml
number: 317
title: "Support double clicking to insert inlay hints in the editor (\"bake inlay hints\")"
type: issue
state: closed
author: DetachHead
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-05-11T14:09:40Z
updated_at: 2025-11-24T19:48:32Z
url: https://github.com/astral-sh/ty/issues/317
synced_at: 2026-01-10T01:58:59Z
```

# Support double clicking to insert inlay hints in the editor ("bake inlay hints")

---

_Issue opened by @DetachHead on 2025-05-11 14:09_

before:

![Image](https://github.com/user-attachments/assets/f5fd5144-12a4-4414-a438-8cc30aced4ff)

after:

![Image](https://github.com/user-attachments/assets/a12651c2-6689-4e1a-b8f4-cd8aa4505391)

---

_Comment by @MichaReiser on 2025-05-12 06:39_

What's asked here is to support the [`inlayHints`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#inlayHint) `textEdits` property. 

This is mostly straightforward with the exception that our `type.display(db)` doesn't always return valid Python syntax.



---

_Label `server` added by @MichaReiser on 2025-05-12 06:39_

---

_Label `wish` added by @MichaReiser on 2025-05-12 06:39_

---

_Comment by @DetachHead on 2025-05-12 06:49_

yeah. pyright's type printer handles this with [a `PythonSyntax` flag](https://github.com/DetachHead/basedpyright/blob/8dc617d466cc17c281bae5bae0a38dd5420e1037/packages/pyright-internal/src/analyzer/typePrinter.ts#L76) that can be passed to the type printer. that flag is used for inlay hints but not hover:

![Image](https://github.com/user-attachments/assets/a7f16138-7f5a-4e67-9081-d6ca59aa0671)

[playground](https://basedpyright.com/?typeCheckingMode=all&code=GYexAIF5wGwQwLYCMAmcBc4ByIB2BTIA)

---

_Renamed from "support double clicking to insert inlay hints" to "Support double clicking to insert inlay hints in the editor" by @dhruvmanila on 2025-06-25 09:51_

---

_Renamed from "Support double clicking to insert inlay hints in the editor" to "Support double clicking to insert inlay hints in the editor ("bake inlay hints")" by @AlexWaygood on 2025-09-23 08:22_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:50_

---

_Comment by @MatthewMckee4 on 2025-11-21 01:22_

Do we want to support this?

---

_Comment by @carljm on 2025-11-21 01:47_

@MatthewMckee4 If you look at the metadata in the right-hand column, you'll see that this issue in the Stable milestone, with priority P2. That indicates that yes, we would like to support it, though it's not considered a critical feature. But I think that means a PR for it would be welcome, if that was your question :)

---

_Comment by @Gankra on 2025-11-21 03:25_

With the new machinery we introduced for recording metadata about inlay hints this should be a lot more viable. In particular while we're rendering types we can record in the TypeDetails that we emitted "invalid syntax", and disable the feature.

Also, we can use the TypeDetails to potentially empower auto-imports on click? Maybe that one's impossible, I'm not sure what the lsp protocol gives us here.

---

_Label `wish` removed by @Gankra on 2025-11-21 03:29_

---

_Label `help wanted` added by @Gankra on 2025-11-21 03:29_

---

_Comment by @Gankra on 2025-11-21 03:33_

You want to add like `set_invalid_syntax()` here:

https://github.com/astral-sh/ruff/blob/6b7adb0537d1a57f26cf462a637af483e0ba2c75/crates/ty_python_semantic/src/types/display.rs#L176

And set it in places like here where we know we're about to emit invalid type syntax:

https://github.com/astral-sh/ruff/blob/6b7adb0537d1a57f26cf462a637af483e0ba2c75/crates/ty_python_semantic/src/types/display.rs#L1572

(I don't think you need to be perfect about finding all the places, it's the kind of thing that will be easy to iterate on)

---

_Comment by @MatthewMckee4 on 2025-11-23 01:38_

Perfect, thank you.

I'll have a bash at this tomorrow.

One thing I think is interesting to think about is how we insert annotations with unknowns in them.

```py
x[: list[int | Unknown]] = [1, 2, 3]
```

If I doubly clicked on this, I'd want to get `list[int]`.

---

_Comment by @DetachHead on 2025-11-23 02:32_

Ideally that shouldn't be an issue once #1240 / #1473 is resolved 

---

_Closed by @Gankra on 2025-11-24 19:48_

---
