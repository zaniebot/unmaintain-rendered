```yaml
number: 86
title: Code completion
type: issue
state: open
author: MichaReiser
labels:
  - server
  - completions
assignees: []
created_at: 2025-03-14T10:18:02Z
updated_at: 2025-12-31T15:21:15Z
url: https://github.com/astral-sh/ty/issues/86
synced_at: 2026-01-12T15:54:22Z
```

# Code completion

---

_@MichaReiser_

Implement auto-completion support

# Plan

At time of writing (2025-05-02), the plan below represents my best effort guess as to how we want to proceed here. The first few items are meant to correspond to lower hanging fruit.

One thing that is not really captured by the plan here, but I think may require some effort, is figuring out how best to identify what the current context is. I suspect there may be an easy way to start for specific cases, but the extent to which that works more generally is not totally clear to me yet.

* [x] Add built-ins to code completions.
* [x] Return all names from scopes in current module. [ref](https://github.com/astral-sh/ruff/pull/17744#discussion_r2069886060)
* [x] Identify current scope and return names from just that scope.
* [x] Identify a `Object.<typing>` context and return attributes on `Object`.
* [x] Identify a `from module import <typing>` context and return items from `module`.
* [x] Identify `import <typing>` and `import module.<typing>` contexts and return available modules.
* [x] #1550
* [x] [BETA] Add type signatures to completion suggestions
* [x] [BETA] [For scope based completions, offer unimported symbols as suggestions. (And upon selection, add the required import.)](https://github.com/astral-sh/ty/issues/535)
* [ ] Offer string literals based on type context.
* [ ] Offer string literals based on `TypedDict` context.
* [ ] Offer contextual language keywords in completions (e.g., `def` and `match`).
* [ ] Offer completions for cases in `match` statements.


---

_Label `server` added by @MichaReiser on 2025-03-14 10:18_

---

_Comment by @MichaReiser on 2025-03-16 16:21_

I think this is a pretty big feature and not one that specifically plays to my strengths. Why I think it's big:

* Code completion is context sensitive: It needs to list all modules when in an `import <here>` position but list a module's symbols when at the start of an identifier, or types when in a typing position (oh right, knowing whether we're in a typing position isn't trivial in Python, thanks to stringified type annotations and legacy type aliases)
* We lack the necessary queries: The module resolver allows individual module lookups but it has no API to list all modules. The same is true for all other APIs. We have APIs to lookup a single attribute but we don't have one that lists the possible attributes of a class, taking descriptor protocol, inheritance, and what not into account (and hide attributes that are hidden because of inheritance). 
* It's important to rank suggestions because you want to avoid re-experts if you can import the symbol directly.

I think we can implement something basic, but the project itself is probably a larger one that spans multiple months.

Edited:

Next steps:

* What's the minimum amount of work we can do to ship something useful?
* If we had to ship this in one week (or in two weeks), what would we do?

---

_Assigned to @BurntSushi by @BurntSushi on 2025-04-04 13:53_

---

_Renamed from "[red-knot] Code completion" to "Code completion" by @MichaReiser on 2025-05-07 15:16_

---

_Comment by @MichaReiser on 2025-05-28 11:37_

Completing signature might actually be a separate feature in the LSP specification and could be worth tracking separately: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_signatureHelp

---

_Comment by @MichaReiser on 2025-07-08 06:30_

@BurntSushi it might be worth updating the issue summary with the next tasks.

---

_Removed from milestone `Beta` by @BurntSushi on 2025-08-15 13:03_

---

_Added to milestone `GA` by @BurntSushi on 2025-08-15 13:03_

---

_Comment by @sharkdp on 2025-08-26 07:19_

> Offer contextual language keywords in completions

The fact that this feature is still missing makes typing code quite painful in some cases. For example, open an empty [playground](https://play.ty.dev/) and type `x = None<Enter>`. Since `None` is not one of the completions, you'll end up with `x = NotImplemented`. For the playground in particular, I can't even get rid of the autocompletion suggestions using `Esc`, so I need to manually move the cursor or use other tricks to be able to enter this without being modified.

---

_Comment by @AlexWaygood on 2025-08-26 09:17_

Similarly, the new `import <CURSOR>` completions are awesome, but it keeps annoying me that `importlib` is suggested in cases like this 😆 from a human's perspective, it's pretty clear that `import` is going to be the next token here

<img width="1452" height="288" alt="Image" src="https://github.com/user-attachments/assets/e8c33b43-114d-44ea-90b2-8003b7d9a436" />

---

<img width="1338" height="302" alt="Image" src="https://github.com/user-attachments/assets/bc49bad7-625e-4256-8ee8-50763cf6acbc" />

---

_Comment by @MichaReiser on 2025-09-29 09:32_

Thread with feedback on completions: https://github.com/astral-sh/ty/issues/1274

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:51_

---

_Comment by @sharkdp on 2025-10-15 11:29_

https://github.com/astral-sh/ruff/pull/20807 supposedly added support for `None`, but I don't see it in the provided completions in the playground and still have trouble writing `x = None<Enter>`:

<img width="705" height="332" alt="Image" src="https://github.com/user-attachments/assets/c9d810d7-5f0f-4e37-a197-2dbb19c8434e" />

---

_Comment by @AlexWaygood on 2025-10-15 11:47_

@sharkdp I think that's because there hasn't been a successful ty playground deploy since https://github.com/astral-sh/ruff/pull/20807 landed: see the list of runs in https://github.com/astral-sh/ruff/actions/workflows/publish-ty-playground.yml. The playground deploy was broken by https://github.com/astral-sh/ruff/pull/20863 and then fixed again in https://github.com/astral-sh/ruff/pull/20885, but none of the commits that have been merged to `main` since https://github.com/astral-sh/ruff/pull/20885 landed have touched the parts of the codebase that would lead to a fresh ty playground deploy from `main` being triggered.

I tried out a local build of the playground as part of my review of https://github.com/astral-sh/ruff/pull/20807 and confirmed that it fixed the issue with `None` locally for me



---

_Comment by @sharkdp on 2025-10-15 11:51_

That makes sense, thank you!

---

_Comment by @MichaReiser on 2025-11-14 08:34_

@BurntSushi I think it would be useful to split out some smaller tasks so that we can be explicit about which feature should be part of stable and which completions feature are fine to defer until after stable. But this can wait until after the beta is out

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 15:21_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:21_

---
