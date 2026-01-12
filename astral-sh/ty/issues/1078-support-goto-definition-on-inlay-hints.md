```yaml
number: 1078
title: Support goto definition on inlay hints
type: issue
state: closed
author: MatthewMckee4
labels:
  - server
assignees: []
created_at: 2025-08-21T09:29:15Z
updated_at: 2025-11-21T03:22:55Z
url: https://github.com/astral-sh/ty/issues/1078
synced_at: 2026-01-12T15:54:24Z
```

# Support goto definition on inlay hints

---

_@MatthewMckee4_

I can write this up more later.

Rust supports this and I think it is a very useful feature

---

_Label `server` added by @AlexWaygood on 2025-08-21 09:46_

---

_Comment by @MichaReiser on 2025-08-21 10:15_

I took a quick look at the LSP specification. This requires setting `location` on the inlay label. 

I think it's fine for a first implementation to only have a link for the outer most type, but it would be nice if we could support clicking on sub types as well (`list[User]`, I want to click `User`). 

Somewhat related, we could also set tooltips on inlay labels

https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#inlayHintLabelPart

---

_Comment by @MatthewMckee4 on 2025-08-26 12:17_

I think that this may require a refactor of the `display.rs` file.

What I'm thinking is that we should represent the "display" of a type as a list of `DisplayPart` enum or similar. Then in inlay hints we can iterate over that and identify what parts we can create a goto from, and we should be able to keep the display output exactly the same.

I will have a play around with this. But otherwise i think i would have to duplicate a lot of code from display as i want to be able to display in the same was that `display.rs` does whilst also getting information from the displayed output (like where each goto definition should be placed within the inlay hint part).

---

_Comment by @dhruvmanila on 2025-08-26 15:15_

I can spend some time thinking about this tomorrow morning (it's end of day for me :)) unless @Gankra has some thoughts on how to approach this?

---

_Comment by @Gankra on 2025-08-26 18:20_

The new DisplaySettings type could *maybe* be refactored to hold onto mutable state that gets written back... but maybe the real conclusion here is we should stop going through the Actual Display Trait...

---

_Comment by @dhruvmanila on 2025-08-28 11:51_

> What I'm thinking is that we should represent the "display" of a type as a list of `DisplayPart` enum or similar. Then in inlay hints we can iterate over that and identify what parts we can create a goto from, and we should be able to keep the display output exactly the same.

And the reason for this would be that, considering Micha's example above, the "list" and "User" would need to be separate so that they can have their own location value? So, the `: list[User]` would actually be a list containing `: `, `list`, `[`, `User`, `]`.

The other thing to note which seems more important is that some of the types wouldn't display valid Python syntax. For example, `x = Foo` where `Foo` is a user defined class would display `x: <class 'Foo'> = Foo`. I also quickly checked what Pylance is doing and it seems that it's only providing the location for the outermost type, so in `list[Foo]`, it would only be able to goto definition on `list` and not on `Foo`.

This is what Micha also suggested and I think I agree that we can start with just resolving the information on the outermost name node. This can be done by manually collecting all the characters that form up a valid Python identifier if the inlay hint starts with it (thus excluding the class syntax or any other invalid syntax) and when we encounter any character which isn't valid we stop and the collected characters would form up the identifier for which we should look up additional information.

I also wonder what's the use of the `tooltip` field.

I'd also recommend if we can lazily resolve this information if the client supports it via https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#inlayHint_resolve. You can refer to the logic for resolving code actions lazily in the Ruff language server for reference.

Does this help?

---

_Comment by @MatthewMckee4 on 2025-08-28 16:30_

Thank you.

> This is what Micha also suggested and I think I agree that we can start with just resolving the information on the outermost name node. This can be done by manually collecting all the characters that form up a valid Python identifier if the inlay hint starts with it (thus excluding the class syntax or any other invalid syntax) and when we encounter any character which isn't valid we stop and the collected characters would form up the identifier for which we should look up additional information.

This seems like a fair approach.

For `<class 'Foo'>` would we not want to allow the use to goto on `Foo` within that. I think that is useful and when using that approach we would also be able to got on `(int, str, /) -> bool` for example.

I'll have a go at this anyway.

---

_Comment by @dhruvmanila on 2025-08-29 10:31_

> For `<class 'Foo'>` would we not want to allow the use to goto on `Foo` within that. I think that is useful and when using that approach we would also be able to got on `(int, str, /) -> bool` for example.

I agree that it would be useful but I want to avoid increasing the scope here, so we could just start with a simple implementation but I don't mind if you want to try it.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:56_

---

_Comment by @Gankra on 2025-11-20 14:42_

Done in https://github.com/astral-sh/ruff/pull/21533 although there's plenty of things that could be followed up.

---

_Closed by @Gankra on 2025-11-20 14:42_

---

_Comment by @MatthewMckee4 on 2025-11-21 01:25_

@Gankra would you want to make an issue for the follow ups you want to support.

I'm interested to see and would love to help in any way.

---

_Comment by @Gankra on 2025-11-21 03:22_

Alex and I got very excited about the feature and burned down all the really obvious stuff. The remaining bits are:
* #1598
* #1605 

But I think maybe a good followup now would be:

* #317

In particular the machinery I setup should be a good foundation for it, I'll leave some notes in there.

---
