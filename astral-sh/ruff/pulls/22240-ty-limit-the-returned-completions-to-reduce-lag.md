```yaml
number: 22240
title: "[ty] Limit the returned completions to reduce lag"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/limit-completions
created_at: 2025-12-29T08:58:07Z
updated_at: 2025-12-31T16:39:27Z
url: https://github.com/astral-sh/ruff/pull/22240
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Limit the returned completions to reduce lag

---

_Pull request opened by @MichaReiser on 2025-12-29 08:58_

## Summary

Reduce the number of completions sent to clients to reduce serialization/deserialization overhead, thereby reducing lag.

Serializing large responses can cause noticeable lag because serialization occurs on the server's main thread. 
Not only is serializing large responses expensive on the server, but deserializing them is equally expensive for the client. 

This PR limits the number of completions sent to the client to at most 1k. I picked the number rather arbitrarily, and we could maybe even go as low as 100
But the serialization/deserialization cost between 100 and 1000 is minimal. 


I also made some changes to our tracing setup, which allowed me to debug this more easily:

* Change the response logging from `trace` to `debug`, it includes the end-to-end time it took to process a response (including wait time for an available worker thread and serialization)
* Add spans to `all_symbols` and `workspace_symbols` so that tracing preserves the request information for the spawned tasks running on a worker thread (tracing now logs that `symbols_for_file_globals_only` is part of the `completions `request`)


Mitigates https://github.com/astral-sh/ty/issues/2254

## Test Plan

**before**


https://github.com/user-attachments/assets/ced8ab40-20dd-4291-af08-e8544c574ca2

**After**


https://github.com/user-attachments/assets/3873e4f0-bae8-41d4-a102-6da6fe1e2967



---

_Label `server` added by @MichaReiser on 2025-12-29 08:58_

---

_Label `ty` added by @MichaReiser on 2025-12-29 08:58_

---

_Review requested from @carljm by @MichaReiser on 2025-12-29 08:58_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-29 08:58_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-29 08:58_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-29 08:58_

---

_Label `server` added by @MichaReiser on 2025-12-29 08:58_

---

_Label `ty` added by @MichaReiser on 2025-12-29 08:58_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-29 09:01_

---

_@AlexWaygood approved on 2025-12-29 09:02_

---

_Merged by @MichaReiser on 2025-12-29 09:16_

---

_Closed by @MichaReiser on 2025-12-29 09:16_

---

_Branch deleted on 2025-12-29 09:16_

---

_Comment by @mjpieters on 2025-12-30 15:19_

Could the LSP response please set the [`CompletionList.isIncomplete` flag](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionList) in the response based on wether or not the list has been truncated?

From the LSP 3.17 documentation string for that attribute:

> This list is not complete. Further typing should result in recomputing this list.
>
> Recomputed lists have all their items replaced (not appended) in the incomplete completion sessions.

Also, is there any way a client could opt into getting the full list anyway?

I realise that my use-case is not typical (I'm using the LSP protocol to gather Python project API information) but I was kinda counting on these responses being complete ;-) If `isIncomplete` is set based on wether or not the list was truncated, I can at least programatically detect when to send more detailed queries, as checking for a length of exactly 1000 items feels icky and not very robust.

---

_Comment by @MichaReiser on 2025-12-30 15:23_

My impression is that we already set `is_incomplete`

https://github.com/astral-sh/ruff/blob/f1e6c9c3a0befd642b5a44b78a7df266b0606b79/crates/ty_server/src/server/api/requests/completion.rs#L134-L137

are there cases where it's missing?

---

_Comment by @mjpieters on 2025-12-31 15:33_

> My impression is that we already set `is_incomplete`

Indeed, it is being set regardless of the list actually being complete or not. Could it reflect wether or not the list was truncated instead?

---

_Comment by @MichaReiser on 2025-12-31 16:16_

We can't, because our current fuzzy search returns more results when you type more text, and it's important that clients don't cache the completion results.

> for speed clients should be able to filter an already received completion list if the user continues typing. Servers can opt out of this using a [CompletionList](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionList) and mark it as isIncomplete.



---

_Comment by @mjpieters on 2025-12-31 16:39_

> > for speed clients should be able to filter an already received completion list if the user continues typing. Servers can opt out of this using a [CompletionList](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionList) and mark it as isIncomplete.

Thanks for finding that entry, I had missed that note. So `isIncomplete` is overloaded to mean both 'there can be more results' and 'do not filter this list down yourself, leave that to the server'. Check!


---
