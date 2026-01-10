---
number: 2340
title: "Many keystrokes are required before a `Literal` auto-import completion suggestion is offered"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2026-01-05T11:48:11Z
updated_at: 2026-01-07T17:25:28Z
url: https://github.com/astral-sh/ty/issues/2340
synced_at: 2026-01-10T01:51:14Z
---

# Many keystrokes are required before a `Literal` auto-import completion suggestion is offered

---

_Issue opened by @AlexWaygood on 2026-01-05 11:48_

### Summary

Take a look at this video. The `Literal` auto-import suggestion is only provided after I have typed `Liter`, despite there being only one suggestion offered after typing `Lite`:

https://github.com/user-attachments/assets/4aafddf2-4a27-4b0a-b1fd-56f49e0ed4cc

@RasmusNygren and @MichaReiser observe:

> We seem to lose that suggestions when we truncate to the top 1000 completions: https://github.com/astral-sh/ruff/blob/8464aca795bc3580ca15fcb52b21616939cea9a9/crates/ty_ide/src/completion.rs#L122. Revert that and the behaviour is as expected, which suggests that the suggestion has very low ranking.
> 
> There's probably some improvements that can be made to the ranking heuristics, I think everything that's not yet imported is being ranked lower.
> I'm a bit surprised that's not in the top 1000 either way, though.
>
> The name is used last when ranking autocompletion suggestions. Maybe it's something we should give a higher weight to.
>
> It seems like we might need a slightly more sophisticated ranking heuristic when we limit the completions to some top `N` completions. You can't rely on fallbacks quite as much in that situation

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2026-01-05 11:48_

---

_Label `server` added by @AlexWaygood on 2026-01-05 11:48_

---

_Label `completions` added by @AlexWaygood on 2026-01-05 11:48_

---

_Comment by @MichaReiser on 2026-01-05 12:03_

In general, I think we should make our fuzzing matching more strict. E.g. only add a `.*` pattern after uppercase letters to match `TypeInferenceBuilder` when writing `TIBuilder` 

---

_Comment by @AlexWaygood on 2026-01-05 12:33_

I'm guessing there's the same root cause for why an auto-import suggestion for `ty_extensions.TypeOf` is not provided here:

<img width="2012" height="528" alt="Image" src="https://github.com/user-attachments/assets/6c3c3ee3-c8fa-41e9-8c65-f071bd7f370e" />

This seems to be quite a big usability regression for completions

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-05 12:34_

---

_Comment by @MichaReiser on 2026-01-05 12:44_

Would you mind adding a playground :) So that I don't need to copy paste your code?

I'm a bit surprised that we run into this in the playground because the truncation only happens if there are more than 1000 suggestions. I would expect this to mainly be an issue when working with many third-party dependencies (or a system python with many packages installed)

---

_Comment by @AlexWaygood on 2026-01-05 12:59_

> Would you mind adding a playground :) So that I don't need to copy paste your code?

oops, sorry 😶 here you go: https://play.ty.dev/48e66663-66d3-4ba5-ba83-cd46f2ca38a3

I would expect `ty_extensions.TypeOf` to be the top autoimport suggestion for this situation, but only `ty_extensions.CallableTypeOf` is provided as a suggestion:

```py
from ty_extensions import is_subtype_of
from typing import reveal_type

reveal_type(bool(is_subtype_of(type[type], TypeO<CURSOR>)))
```

---

_Comment by @AlexWaygood on 2026-01-06 13:20_

Another example: `Enum` is not offered as an auto-import autocomplete suggestion at all here:

<img width="1496" height="1054" alt="Image" src="https://github.com/user-attachments/assets/8aa69f67-e5ac-496b-9c77-441af03eb5fb" />

https://play.ty.dev/ebb3aa40-b1e6-4381-ac33-105fcd2f381e

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-06 19:56_

---

_Comment by @BurntSushi on 2026-01-07 17:14_

OK, so the actual root cause here is indeed the limiting to 1,000 completions (as already said above), but the reason is more subtle. In particular, the problems identified here do not occur in VS Code. Indeed, it looks like this might be a playground specific problem. More to the point, the problem (using the `Literal` example in the OP) disappears when you re-request completions. You can see it in the video here:

https://github.com/user-attachments/assets/7524fc38-15c1-462b-bb4d-4718beb0a592

What I believe is happening here is that the playground isn't respecting [`CompletionList::isIncomplete`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionList), which we set here unconditionally:

https://github.com/astral-sh/ruff/blob/60038709927aa5d133b8428de2deda4667fbe812/crates/ty_server/src/server/api/requests/completion.rs#L134-L137

Basically, `isIncomplete` is _supposed_ to tell LSP clients to re-request completions on each keystroke. When it's `false` (or not respected), then subsequent keystrokes after the initial completion request specifically _won't_ re-request completions. This lets the LSP client do the entirety of the filtering on the client side without involving the server. The obvious downside here is that if you start with a very broad query like `L`, then the LSP client is fundamentally limited to whatever suggestions are returned in that initial request.

Before the limiting was put in place, we'd just return everything. So the LSP client filtering would still work. But with the limit put in place, anything beyond that cut-off will never have a chance of being shown as the user refines their query.

But wait... where we set `isIncomplete` isn't used by the playground. So my claim is perhaps not true. Checking if Monaco has [`isIncomplete`](https://microsoft.github.io/monaco-editor/typedoc/interfaces/languages.CompletionList.html) reveals that it does. And indeed, we do not set it.

So, the fix is simple: https://github.com/astral-sh/ruff/pull/22442

---

_Comment by @BurntSushi on 2026-01-07 17:17_

I've confirmed that https://github.com/astral-sh/ruff/pull/22442 fixed all three of the issues identified by @AlexWaygood 

(You can tell because if you re-request completions after typing, the expected completions show up.)

---

_Closed by @BurntSushi on 2026-01-07 17:25_

---
