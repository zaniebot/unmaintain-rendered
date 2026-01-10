```yaml
number: 1774
title: Filter out language-keyword completions in places where only an expression can be valid syntax
type: issue
state: closed
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T14:33:32Z
updated_at: 2025-12-18T17:28:22Z
url: https://github.com/astral-sh/ty/issues/1774
synced_at: 2026-01-10T01:53:59Z
```

# Filter out language-keyword completions in places where only an expression can be valid syntax

---

_Issue opened by @AlexWaygood on 2025-12-05 14:33_

For example, in this situation, `raise` should not be included as a completion suggestion, because `for x in raise` is invalid syntax (Python's grammar dictates that only an expression can follow the sequence `for <EXPR> in `):

<img width="1332" height="734" alt="Image" src="https://github.com/user-attachments/assets/26dd05d9-bd11-4fc1-a14d-d1b329cf3ac1" />

---

_Label `server` added by @AlexWaygood on 2025-12-05 14:33_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 14:33_

---

_Renamed from "Filter out keyword completions in places where only an expression can be valid syntax" to "Filter out language-keyword completions in places where only an expression can be valid syntax" by @AlexWaygood on 2025-12-13 21:41_

---

_Comment by @AlexWaygood on 2025-12-13 21:42_

Similarly, it's a bit annoying that `finally` is at the top of the list here when syntactically it would only ever be invalid syntax for a keyword to appear in this position:

<img width="1360" height="288" alt="Image" src="https://github.com/user-attachments/assets/2aad15ea-da36-466f-8b19-480b05d92b9b" />

---

_Comment by @BurntSushi on 2025-12-15 18:59_

https://github.com/astral-sh/ruff/pull/21979 did this for `for ... in <CURSOR>`.

I wonder if it make senses to approach this issue more methodically. That is, enumerate all classes of "must be an expression" locations, and then try to convert that to code so that we can more holistically rule out things that aren't expressions (or can't become expressions).

Otherwise it seems likely this will just be a whack-a-mole kind of thing.

---

_Comment by @gablank on 2025-12-17 07:43_

> enumerate all classes of "must be an expression" locations

Why not just use the formal Python grammar for this, then you can avoid suggesting something that leads to invalid syntax in all cases? Should also make it easy to support different Python versions (maintaining https://github.com/astral-sh/ruff/pull/22002 as the grammar grows is going to lead to a lot of conditionals, I imagine).

---

_Comment by @BurntSushi on 2025-12-17 13:00_

@gablank Can you elaborate? What specifically are you suggesting?

---

_Comment by @gablank on 2025-12-17 13:46_

I am making quite a few assumptions on how `ty` works here, but reading https://github.com/astral-sh/ruff/pull/22002 it seems to me like what `ty` lists as valid next keywords is just based on "hardcoding", i.e. "if the current state is `for x in <iter>:<CURSOR>`, then yield, return, await etc. are valid keywords".

What I'm asking is why the grammar itself isn't used to generate up all this, instead of encoding that logic by hand?

I guess you're kind of suggesting the same in this comment, although maybe a special case of what I'm asking about?

>  I wonder if it make senses to approach this issue more methodically. That is, enumerate all classes of "must be an expression" locations, and then try to convert that to code so that we can more holistically rule out things that aren't expressions (or can't become expressions).

---

_Comment by @AlexWaygood on 2025-12-17 13:59_

> What I'm asking is why the grammar itself isn't used to generate up all this, instead of encoding that logic by hand?

generating some kind of schema from the formal grammar at https://github.com/python/cpython/blob/3.14/Grammar/python.gram is an interesting idea. It sounds hard, though

---

_Comment by @BurntSushi on 2025-12-18 17:28_

I believe this should now be closed by https://github.com/astral-sh/ruff/pull/22002

---

_Closed by @BurntSushi on 2025-12-18 17:28_

---
