```yaml
number: 18984
title: "[ty] Request configuration from client"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/configuration-request
created_at: 2025-06-27T13:26:31Z
updated_at: 2025-07-02T09:01:44Z
url: https://github.com/astral-sh/ruff/pull/18984
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Request configuration from client

---

_Pull request opened by @MichaReiser on 2025-06-27 13:26_

## Summary

This PR makes the necessary changes to the server that it can request configurations from the client using the `configuration` request. 
This PR doesn't make use of the request yet. It only sets up the foundation (mainly the coordination between client and server)
so that future PRs could pull specific settings. 

I plan to use this for pulling the Python environment from the Python extension. 

Deno does something very similar to this.

## Test Plan

Tested that diagnostics are still shown. 


---

_Review requested from @carljm by @MichaReiser on 2025-06-27 13:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-27 13:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-27 13:26_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-27 13:26_

---

_Label `server` added by @MichaReiser on 2025-06-27 13:27_

---

_Label `ty` added by @MichaReiser on 2025-06-27 13:27_

---

_@MichaReiser reviewed on 2025-06-27 13:29_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:230 on 2025-06-27 13:29_

I decided to change all the `Client` methods to not return a `Result` if the only reason why they can fail is if the channel `Receiver` gets dropped. The main motivation is that this should never happen. The `main_loop` receiver is always alive for as long as the `Server` is alive. The client receiver could die due to a panic in that thread. That's why we keep writing a log message but we no longer propagate the `Result`. 

---

_Comment by @github-actions[bot] on 2025-06-27 13:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB
-     memo fields = ~129MB
+     memo fields = ~117MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~129MB
+     memo fields = ~142MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~276MB
+     memo fields = ~304MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~304MB
+     memo fields = ~334MB

meson (https://github.com/mesonbuild/meson)
-     memo fields = ~304MB
+     memo fields = ~334MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~490MB
+     memo fields = ~445MB

```
</details>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-27 13:32_

---

_Review request for @carljm removed by @MichaReiser on 2025-06-27 14:13_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-06-27 14:13_

---

_Review request for @dcreager removed by @MichaReiser on 2025-06-27 14:13_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-06-27 14:13_

---

_Comment by @MichaReiser on 2025-06-28 16:25_

Uh, I think this actually breaks the settings configured by the client because the fetched settings override the settings from the `initialize` request. I won't have time to fix this before my PTO. 

I think the "proper" fix is that `Workspaces::register` registers the workspace with default options and that we move retrieve all options from the `configuration` request.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-06-30 06:53_

---

_Comment by @dhruvmanila on 2025-07-02 08:30_

> Uh, I think this actually breaks the settings configured by the client because the fetched settings override the settings from the `initialize` request. I won't have time to fix this before my PTO.
> 
> I think the "proper" fix is that `Workspaces::register` registers the workspace with default options and that we move retrieve all options from the `configuration` request.

Why is this a problem? Isn't this correct? The fetched settings from `workspace/configuration` should be overriding the ones coming from `InitializationOptions`. I think they're going to be the same until the user changes the setting in the editor.


---

_@dhruvmanila reviewed on 2025-07-02 08:59_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/main_loop.rs`:199 on 2025-07-02 08:59_

I think this would need to be moved in a standalone method or something as it'll be used for handling `didChangeConfiguration` and probably `didChangeWorkspaceFolders` but it doesn't need to be done now.

---

_@dhruvmanila approved on 2025-07-02 09:01_

---

_Merged by @dhruvmanila on 2025-07-02 09:01_

---

_Closed by @dhruvmanila on 2025-07-02 09:01_

---

_Branch deleted on 2025-07-02 09:01_

---
