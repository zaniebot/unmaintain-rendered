```yaml
number: 17744
title: "[red-knot] add auto-completion MVP to LSP"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-end-to-end-mvp
created_at: 2025-04-30T18:24:01Z
updated_at: 2025-05-01T16:08:12Z
url: https://github.com/astral-sh/ruff/pull/17744
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] add auto-completion MVP to LSP

---

_Pull request opened by @BurntSushi on 2025-04-30 18:24_

This PR does the wiring necessary to respond to completion requests from
LSP clients.

As far as the actual completion results go, they are nearly about the
dumbest and simplest thing we can do: we simply return a de-duplicated
list of all identifiers from the current module.

Ref astral-sh/ty#86


---

_Review requested from @carljm by @BurntSushi on 2025-04-30 18:24_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-30 18:24_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-30 18:24_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-30 18:24_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-30 18:24_

---

_Comment by @BurntSushi on 2025-04-30 18:24_

Demo:

https://github.com/user-attachments/assets/07b25e50-cd5e-4403-886b-d398f151a2b4



---

_Label `red-knot` added by @BurntSushi on 2025-04-30 18:24_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-30 18:24_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-30 18:24_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-30 18:24_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-04-30 18:24_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-04-30 18:24_

---

_Renamed from "red_knot_server: add auto-completion MVP" to "[red-knot] red_knot_server: add auto-completion MVP" by @BurntSushi on 2025-04-30 18:25_

---

_Label `server` added by @BurntSushi on 2025-04-30 18:25_

---

_Comment by @github-actions[bot] on 2025-04-30 18:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm reviewed on 2025-04-30 18:48_

Looks reasonable to me, but I'd rather have @dhruvmanila or @MichaReiser look at the LSP side.

My main concern here is that I would probably prefer to release the alpha with no autocomplete than with autocomplete that doesn't make use of type information. But we can always ship a quick diff to turn it off right before the alpha release, if we don't get far enough.

---

_Comment by @BurntSushi on 2025-04-30 19:02_

@carljm Would you be okay to have it disabled by default, but allow an env var or some other undocumented toggle enable it? Just for practical purposes of keeping it in tree and being able to iterate.

---

_Comment by @carljm on 2025-04-30 19:06_

> @carljm Would you be okay to have it disabled by default, but allow an env var or some other undocumented toggle enable it? Just for practical purposes of keeping it in tree and being able to iterate.

Oh for sure, that's what I was imagining, definitely not that we'd rip out the code entirely. I'm fine with doing that now, or waiting and seeing where we're at with it end of next week.

---

_Comment by @BurntSushi on 2025-04-30 19:08_

SGTM!

---

_Comment by @dhruvmanila on 2025-04-30 20:32_

We could do something similar to what r-a does with their "experimental" capabilities which requires explicit opt-in from client side. Refer to the first paragraph in https://github.com/rust-lang/rust-analyzer/blob/master/docs/book/src/contributing/lsp-extensions.md#lsp-extensions:

> All capabilities are enabled via the experimental field of `ClientCapabilities` or `ServerCapabilities`.

Specifically, [here](https://github.com/rust-lang/rust-analyzer/blob/5d66d45005fef79751294419ab9a9fa304dfdf5c/editors/code/src/client.ts#L290-L330) they send the capabilities that the VS Code extension supports.

So, for us it could look like below which would be sent when during the server initialization process:
```json
"capabilities": {
	"experimental": {
		"completions": {
			"enabled": true
		}
	}
} 
```

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api.rs`:41 on 2025-04-30 20:38_

I think this should use `BackgroundSchedule::LatencySensitive` as completions should be prioritized than e.g., computing the diagnostics or computing the inlay hints. This wouldn't really affect much right now but might do when we need to compute symbols across the workspace like e.g., suggest users that a possible symbol that matches the text at the current cursor position exists in another module (auto-imports).

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/completion.rs`:27 on 2025-04-30 20:40_

Can we rely on `visit_identifier` here instead? Or, did you encounter any issues when using that method?

---

_@dhruvmanila approved on 2025-04-30 20:40_

Looks good!

---

_Renamed from "[red-knot] red_knot_server: add auto-completion MVP" to "[red-knot] add auto-completion MVP to LSP" by @MichaReiser on 2025-05-01 06:01_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/completion.rs`:21 on 2025-05-01 06:03_

This doesn't have to be as part of this PR because I think it would require fleshing out some API for it but we could do this by resolving the `semantic_index`, iterating over all scopes, and collecting the names from every scope. 

This would be a nice stepping stone to the next step where you'd identify in which scope you are and only suggest the names from that or its ancestor scopes. 



---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/completion.rs`:11 on 2025-05-01 06:04_

This could, probably return a `&'db str` but I'm fine with a `String`.

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server.rs`:232 on 2025-05-01 06:05_

Nit: We probably want to tinker with the options here in the future

---

_@MichaReiser approved on 2025-05-01 06:06_

---

_@BurntSushi reviewed on 2025-05-01 11:55_

---

_Review comment by @BurntSushi on `crates/red_knot_ide/src/completion.rs`:11 on 2025-05-01 11:55_

Yeah it eventually has to get turned into a `String` anyway. And I figured this would all be refactored soon anyway.

---

_@BurntSushi reviewed on 2025-05-01 11:56_

---

_Review comment by @BurntSushi on `crates/red_knot_server/src/server.rs`:232 on 2025-05-01 11:56_

Yup I looked, but they all seemed pretty in the weeds. I also noticed that even if I didn't set this, the client still tried to send completion requests. But this seemed the "most correct" given what exists now.

---

_Review comment by @AlexWaygood on `crates/red_knot_ide/src/completion.rs`:21 on 2025-05-01 12:06_

> we could do this by resolving the `semantic_index`, iterating over all scopes, and collecting the names from every scope.

we already have a query that basically does that in https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/semantic_index/re_exports.rs (but it only visits the global scope, currently at least)

---

_@AlexWaygood reviewed on 2025-05-01 12:06_

---

_Comment by @BurntSushi on 2025-05-01 12:35_

Thank you @dhruvmanila for the tip about an experimental opt-in! I created an issue tracking that for the alpha milestone: https://github.com/astral-sh/ty/issues/74

---

_@BurntSushi reviewed on 2025-05-01 12:55_

---

_Review comment by @BurntSushi on `crates/red_knot_server/src/server/api.rs`:41 on 2025-05-01 12:55_

Yeah makes sense. I did see this and wondered if I should use it, but nothing else did yet so I was unsure. Nice catch!

---

_@BurntSushi reviewed on 2025-05-01 12:57_

---

_Review comment by @BurntSushi on `crates/red_knot_ide/src/completion.rs`:27 on 2025-05-01 12:57_

Yeah! Just didn't know about it. Nice catch. :-) Much simpler.

---

_Review comment by @BurntSushi on `crates/red_knot_ide/src/completion.rs`:21 on 2025-05-01 12:58_

This sounds great, thank you for the idea. I didn't know about this! This looks like a next good step.

---

_@BurntSushi reviewed on 2025-05-01 12:58_

---

_Merged by @BurntSushi on 2025-05-01 16:08_

---

_Closed by @BurntSushi on 2025-05-01 16:08_

---

_Branch deleted on 2025-05-01 16:08_

---
