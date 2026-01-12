```yaml
number: 1206
title: "Include dotted module names in `import` completions?"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-09-18T17:08:45Z
updated_at: 2025-11-18T16:10:37Z
url: https://github.com/astral-sh/ty/issues/1206
synced_at: 2026-01-12T15:54:24Z
```

# Include dotted module names in `import` completions?

---

_@AlexWaygood_

It might be cool if ty could include `collections.abc` (a dotted module name) in the list of suggested completions here:

<img width="1282" height="720" alt="Image" src="https://github.com/user-attachments/assets/be5ae43a-9a6d-4e82-b919-2bf365b6a990" />

Thoughts @BurntSushi?

---

_Label `server` added by @AlexWaygood on 2025-09-18 17:08_

---

_Comment by @carljm on 2025-09-18 17:11_

This seems like it could go overboard really quickly -- if a module has ten submodules, and they each have ten submodules, does that expand to a hundred autocomplete options? Or does it only go one level? Or is there some other factor that determines whether a submodule gets grouped with its parent module or not? Seems like querying for all the submodules could also impact performance of autocomplete.

The current behavior is that if you autocomplete `collections` and type a dot, then you get the suggestion for `abc` -- that seems reasonable and what I would expect.

---

_Comment by @AlexWaygood on 2025-09-18 17:22_

> This seems like it could go overboard really quickly -- if a module has ten submodules, and they each have ten submodules, does that expand to a hundred autocomplete options? Or does it only go one level? Or is there some other factor that determines whether a submodule gets grouped with its parent module or not? Seems like querying for all the submodules could also impact performance of autocomplete.

All true! But this must all be weighed against the fact that I import things from `collections.abc` much more often than from `collections` 😆

I feel like we could probably use heuristics to keep things performant. Maybe we could only look for submodules if there aren't very many non-submodule matches. Or we could even consider just doing this for stdlib modules, which I'd wager are imported much more often than third-party modules.

Just throwing some ideas out! It felt mildly annoying that I had to type `import coll<DOWN><TAB>.a<DOWN><TAB>` in the playground just now rather than just `import coll<DOWN><DOWN><TAB>`, considering that this is one of the most-imported modules in typed Python

---

_Comment by @carljm on 2025-09-18 17:29_

> I had to type `import coll<DOWN><TAB>.a<DOWN><TAB>`

You only really need `coll<TAB>.<TAB>` here, because `coll` is enough to make `collections` the first option, and `abc` is the only submodule option, so it's the top option immediately after typing the dot.

---

_Comment by @BurntSushi on 2025-09-18 17:31_

I would be less worried about performance here and more about overwhelming the user.

With that said, I am working on ranking for auto-import right now. Ranking is kind of necessary, otherwise it's really easy for symbols you _probably_ don't want to make it to the top with just a few keystrokes. If we get to a point where we are happy with auto-import's ranking, we could try expanding the auto-completions here and applying ranking to that.

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:49_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:47_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
