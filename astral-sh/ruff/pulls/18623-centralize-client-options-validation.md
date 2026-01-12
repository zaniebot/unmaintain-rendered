```yaml
number: 18623
title: Centralize client options validation
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: micha/server-options
created_at: 2025-06-11T09:30:03Z
updated_at: 2025-06-12T16:58:37Z
url: https://github.com/astral-sh/ruff/pull/18623
synced_at: 2026-01-12T15:56:22Z
```

# Centralize client options validation

---

_@MichaReiser_

## Summary

The main motivation for this refactor was to port the ty LSP refactors to Ruff. However, I ran into the problem that `ResolvedClientSettings::new` uses `show_err_msg` that now would require a `Client`, but I don't easily have access to a `Client` in `make_document_ref` (which is called from `take_snapshot`). 

Now, the refactor in this PR goes beyond what's required to remove the `Client` dependency but I only realized this when I was done with the larger refactor. I'm interested in second opinions on if I should narrow the refactor or if, what I've done here, overall improves the code (I think it does). 

The changes required to remove the `Client` dependency are:

1. Change `ResolvedClientSettings::new` to return a `Result`. This way, it's up to the caller to call `show_err_msg`
2. Introduce a new `GlobalClientSettings` that validates the global options exactly once (instead everytime they're needed). We can store a `Client` on `GlobalClientOptions` instead of having to pass it to `make_document_ref`. 

Now, the other refactor that I did was to align the settings handling with Ruff/ty:

* `Options` is the unresolved configuration as specified by the users. All values use `Option<T>` (`ClientSettings` -> `ClientOptions`, `AllSettings` -> `AllOptions`, ...)
* `Settings` are the resolved configurations with defaults filled (`ResolvedClientSettings` -> `ClientSettings`)
* Split resolving the options to settings and combining options from different sources into two steps:
  1. Implement `Combine` on `ClientOptions`. Options from different sources can be *combined* together
  2. Add the `Options::into_settings` method that resolves (and validates) the options into settings

I find this two staged process easier to reason about and using the same terminology as in Ruff and ty helps when navigating the code.


## Test plan

* I tested that toggling global options toggles the corresponding behavior in the server
* I verified that settings can be overriden at the workspace level
* I verified that the server shows no error message when using an invalid rule code at the global level if the same setting is overriden at the workspace level



---

_Label `server` added by @MichaReiser on 2025-06-11 09:30_

---

_@MichaReiser reviewed on 2025-06-11 09:31_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:41 on 2025-06-11 09:31_

Somewhat scary that we clone the settings for every request! We should probably use an `Arc` here

---

_@MichaReiser reviewed on 2025-06-11 09:34_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:233 on 2025-06-11 09:34_

This resulted in constructing and validating the global settings on every keystroke (worst case) which then could result in Ruff spamming the user with errors. We can now simply cache settings.

---

_Comment by @github-actions[bot] on 2025-06-11 09:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-06-11 11:22_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/options.rs`:442 on 2025-06-11 11:22_

It's a bit unfortunate that this is now our third version of `Combine`. I tried to use Ruff's `CombineOptions` but had to realise that the server's combine logic is closer to ty's but also inconsistent. Making it impossible to use either ty's nor Ruff's implementation

* Unlike Ruff, the server deeply merges options. E.g., setting `lint` in the workspace settings doesn't override all `lint` settings from the global settings.
* Unlike ty, the server doesn't merge vectors. 
* The server was inconsistent about how to handle `.configuration`: Before, it tried to resolve the field from the workspace, but it fell back to the `configuration` field from the global settings if, e.g., the file didn't exist. Now, we'll use the `configuration` value from the workspace without falling back to the global settings `configuration` if the server fails to resolve the configuration. This seems more correct in my view (and consistent with how we handle configurations in other places)

---

_Renamed from "Refactor Ruff-server options" to "Centralize client options validation" by @MichaReiser on 2025-06-11 11:30_

---

_Marked ready for review by @MichaReiser on 2025-06-11 11:39_

---

_@MichaReiser reviewed on 2025-06-11 11:57_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:41 on 2025-06-11 11:57_

I fixed this

---

_@MichaReiser reviewed on 2025-06-11 11:58_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/options.rs`:1 on 2025-06-11 11:58_

The additions in this file primarily consist of the types that I moved from `settings` to here. 

The main notable changes are:

* The `Combine` implementations
* The `into_settings` implementations

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:80 on 2025-06-12 07:52_

Rename to align with the new enum name
```suggestion
        let mut all_options = AllOptions::from_value(
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:233 on 2025-06-12 08:25_

Thanks! This makes sense, I have a half baked branch doing something similar locally ;)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/options.rs`:442 on 2025-06-12 08:30_

Yeah, there are two parts to settings resolution in the server:
1. The server needs to pick the value from either the global or the workspace setting.
2. Combine the options from different sources like inline configuration / configuration file and the editor settings

In (1), this split between the global and workspace mainly exists in VS Code and is one of the reason I want to move us away from this model and pull the settings for a workspace from the client directly so that the client gives us the final value that we need to use and the only thing we need to do is (2).

> * The server was inconsistent about how to handle `.configuration`: Before, it tried to resolve the field from the workspace, but it fell back to the `configuration` field from the global settings if, e.g., the file didn't exist. Now, we'll use the `configuration` value from the workspace without falling back to the global settings `configuration` if the server fails to resolve the configuration. This seems more correct in my view (and consistent with how we handle configurations in other places)

Yeah, I agree.

---

_@dhruvmanila approved on 2025-06-12 08:44_

Looks good! Thanks for making this change. I think what you're calling "combine" is actually "choosing" between the workspace and global options :)

---

_Merged by @MichaReiser on 2025-06-12 16:58_

---

_Closed by @MichaReiser on 2025-06-12 16:58_

---

_Branch deleted on 2025-06-12 16:58_

---

_Label `internal` added by @MichaReiser on 2025-06-12 16:58_

---
