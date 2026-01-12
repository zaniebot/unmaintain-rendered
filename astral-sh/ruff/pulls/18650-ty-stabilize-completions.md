```yaml
number: 18650
title: "[ty] Stabilize completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - breaking
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/stabilize-completions
created_at: 2025-06-12T19:00:06Z
updated_at: 2025-06-16T11:44:11Z
url: https://github.com/astral-sh/ruff/pull/18650
synced_at: 2026-01-12T15:56:23Z
```

# [ty] Stabilize completions

---

_@BurntSushi_

Specifically, this PR reverts "Make completions an opt-in LSP feature (#17921)",
corresponding to commit 51e2effd2d6b6cf61bd245a3219168fe661d1a7d.

In practice, this means you don't need to opt into completions working
by enabling experimental features. i.e., I was able to remove this from
my LSP configuration:

```
"experimental": {
    "completions": {
        "enable": true
    }
},
```

There's still a lot of work left to do to make completions awesome, but
I think it's in a state where it would be useful to get real user
feedback. It's also meaningfully using ty to provide completions that
use type information.

Ref astral-sh/ty#86


---

_Review requested from @carljm by @BurntSushi on 2025-06-12 19:00_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-12 19:00_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-12 19:00_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-12 19:00_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-12 19:00_

---

_Comment by @github-actions[bot] on 2025-06-12 19:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @dcreager removed by @BurntSushi on 2025-06-12 19:03_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-06-12 19:03_

---

_Comment by @MichaReiser on 2025-06-13 07:05_

I think @dhruvmanila expressed some concerns around stabilizing the feature. We should make sure that he reviewed this PR before merging.

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/settings.rs`:38 on 2025-06-13 07:06_

This is, technically, a breaking change. I think it's fine, considering where we're at with ty. 

We'll also need to update the `ty-vscode` extension to no longer list this option and update the documentation as well.

---

_@MichaReiser approved on 2025-06-13 07:06_

---

_Label `server` added by @MichaReiser on 2025-06-13 07:06_

---

_Label `ty` added by @MichaReiser on 2025-06-13 07:06_

---

_Label `breaking` added by @MichaReiser on 2025-06-13 07:06_

---

_Comment by @dhruvmanila on 2025-06-13 10:06_

At this stage, the ty language server doesn't have all the capabilities that a developer might use on a daily basis e.g., go to definition, rename, etc. This means that it's more than likely that the ty language server will be used alongside another language server (e.g., Pyright) to provide other capabilities. Now, if we stabilize this i.e., show completions without opt-in, it would mean that the completion menu would contain duplicate entries from both the language server.

One way to solve this, and is a solution that Pyrefly has implemented, would be to provide a `python.ty.disableLanguageServices` option that disables all the language services from ty and then the server will be used only for providing type checking diagnostics. This is something that I've implemented [here](https://github.com/astral-sh/ty-vscode/pull/36) but is currently blocked on an upstream change (https://github.com/microsoft/vscode-python/pull/25041). This would mainly be useful in VS Code where we would add support for recognizing this option in the Python extension so that it can disable the Pylance language server when the user wants to use ty language server exclusively.

The above solution is not granular as it would either enable all language features or disable them, nothing in between. Another solution would be to provide granular configuration, like allowing users to use completions from ty but navigation capabilities like goto definition, declaration, etc. from another language server. Or, an even granular options that allows users to enable / disable each capability.

**tl;dr** The main issue here is how do we allow users to keep using ty server's capabilities even when another language server is active. I'd prefer if we can wait until we have the `python.ty.disableLanguageServices` option in, so that users can still use ty for type checking and keep using another language server for other capabilities.

That said, I think we could still merge the PRs for adding the above config option on ty's side so that there's still an option for users to opt-out of language services like auto-completions.

---

_Comment by @BurntSushi on 2025-06-13 13:29_

Hmmm okay. Do we already have a problem today with conflicting LSPs running simultaneously? e.g., If you do a goto type definition request with ty and pyright enabled, which one services it?

Another issue I see is that even if we offer granular enable/disable options, that still seemingly requires the _other_ LSP you use to _also_ have granular enable/disable options.

I'm fine waiting here for `python.ty.disableLanguageServices`, but I'd prefer not doing that if it's truly blocked on upstream.

---

_Comment by @dhruvmanila on 2025-06-13 14:08_

I think what we can do is:
1. Merge this PR stabilizing completions
2. I'll make the `python.ty.disableLanguageServices` PRs ready for review for the ty server and ty VS Code extension
3. I'll open a PR in the Python extension to at least detect this option
4. I'll see if I can fix the issue as stated in https://github.com/microsoft/vscode-python/pull/25041 in that PR

Pyrefly has a hack that does the refresh on their VS Code extension. I'd prefer and see if I can just fix the issue on the Python extension side. I just didn't prioritized it as it wasn't that helpful earlier but given that completions would be available it might be useful to prioritize that now.

---

_Comment by @dhruvmanila on 2025-06-13 14:11_

So, all in all, I'm fine moving ahead with this PR.

We should also remove this option from https://github.com/astral-sh/ty/blob/main/docs/reference/editor-settings.md#experimental.

---

_Comment by @dhruvmanila on 2025-06-13 14:13_

> Hmmm okay. Do we already have a problem today with conflicting LSPs running simultaneously? e.g., If you do a goto type definition request with ty and pyright enabled, which one services it?

I think this depends on client implementation. For example, Neovim supports multiple servers for navigation capabilities like goto type definition where it'll collect all the locations for the request from all the available language servers and provide them as quickfix list so you can navigate through all available options.

---

_@dhruvmanila approved on 2025-06-13 16:06_

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-14 07:39_

---

_Merged by @BurntSushi on 2025-06-16 11:44_

---

_Closed by @BurntSushi on 2025-06-16 11:44_

---

_Branch deleted on 2025-06-16 11:44_

---
