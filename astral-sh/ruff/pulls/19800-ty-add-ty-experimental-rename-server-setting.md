```yaml
number: 19800
title: "[ty] Add `ty.experimental.rename` server setting"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - configuration
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/experimental-rename
created_at: 2025-08-07T10:32:58Z
updated_at: 2025-08-07T12:55:24Z
url: https://github.com/astral-sh/ruff/pull/19800
synced_at: 2026-01-12T15:56:47Z
```

# [ty] Add `ty.experimental.rename` server setting

---

_@dhruvmanila_

## Summary

This PR is a follow-up from https://github.com/astral-sh/ruff/pull/19551 and adds a new `ty.experimental.rename` setting to conditionally register for the rename capability. The complementary PR in ty VS Code extension is https://github.com/astral-sh/ty-vscode/pull/111.

This is done using dynamic registration after the settings have been resolved. The experimental group is part of the global settings because they're applied for all workspaces that are managed by the client.

## Test Plan

Add E2E tests.

In VS Code, with the following setting:
```json
{
	"ty.experimental.rename": "true",
	"python.languageServer": "None"
}
```

I get the relevant log entry:
```
2025-08-07 16:05:40.598709000 DEBUG client_response{id=3 method="client/registerCapability"}: Registered rename capability
```

And, I'm able to rename a symbol. Once I set it to `false`, then I can see this log entry:

```
2025-08-07 16:08:39.027876000 DEBUG Rename capability is disabled in the client settings
```

And, I don't see the "Rename Symbol" open in the VS Code dropdown.

https://github.com/user-attachments/assets/501659df-ba96-4252-bf51-6f22acb4920b



---

_Label `configuration` added by @dhruvmanila on 2025-08-07 10:32_

---

_Label `server` added by @dhruvmanila on 2025-08-07 10:32_

---

_Label `ty` added by @dhruvmanila on 2025-08-07 10:32_

---

_Comment by @github-actions[bot] on 2025-08-07 10:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-07 10:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-08-07 10:40_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-07 10:40_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-08-07 10:40_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-07 10:40_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-07 10:40_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-08-07 10:40_

---

_Review request for @carljm removed by @dhruvmanila on 2025-08-07 10:40_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-08-07 10:40_

---

_Review requested from @UnboundVariable by @dhruvmanila on 2025-08-07 10:40_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:574 on 2025-08-07 11:07_

This isn't incorrect but the LSP supports sending multiple registrations in a single request. Should we change the `regsiter_*` to a `register_dynamic_capability` method that registers all dynamic capabilities (collects the one it must remove and then collects the one it needs to add)

---

_@MichaReiser approved on 2025-08-07 11:12_

I think it's safe to assume that clients that don't support dynamic registration probably also don't support the configuration request.

Should we register the rename provider for those statically if the experimental rename feature is enabled in the initialization options? We should probably do the same for workspace diagnsotics unless we're already doing this.

---

_Comment by @dhruvmanila on 2025-08-07 11:47_

> I think it's safe to assume that clients that don't support dynamic registration probably also don't support the configuration request.

I'm not sure if we can assume that as dynamic registrations are scoped to individual capabilities and workspace configuration is an independent capability. So, a client might have dynamic registration for one capability while it might not for another.

For workspace diagnostics, the server checks whether dynamic registration is supported. If not, it will statically register for the diagnostic capability during initialization but without looking at the `ty.diagnosticMode` value. It will always advertise as supporting workspace diagnostics.

We could do the same for the rename capability which is that we would check whether dynamic registration is supported for the rename capability and if it's not supported, statically register it during initialization. We could then check in the rename request handler whether the user has enabled this or not and return early if not.

Does this make sense?

---

_@dhruvmanila reviewed on 2025-08-07 11:48_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:574 on 2025-08-07 11:48_

Yeah, I wanted to do something like that but the difference here is that for the rename capability we _do_ want to unregister the capability but then we should _avoid_ re-registering the capability. This scenario is only valid when we support `didChangeConfiguration` but I don't want to make any assumptions that might be incorrect later on.

---

_@dhruvmanila reviewed on 2025-08-07 11:49_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:574 on 2025-08-07 11:49_

Oh, I could have two separate methods `unregister_dynamic_capability` and `register_dynamic_capability` which would solve this.

---

_Comment by @MichaReiser on 2025-08-07 11:50_

> We could do the same for the rename capability which is that we would check whether dynamic registration is supported for the rename capability and if it's not supported, statically register it during initialization. We could then check in the rename request handler whether the user has enabled this or not and return early if not.

I didn't see a meaningful value that the client could return in that case. That's why I suspect the best second solution is to only register the provider if the settings (resolved at this point) also enable it. It seems a better experience than one where users can't opt into the setting at all.

---

_Comment by @dhruvmanila on 2025-08-07 11:58_

> I didn't see a meaningful value that the client could return in that case. That's why I suspect the best second solution is to only register the provider if the settings (resolved at this point) also enable it. It seems a better experience than one where users can't opt into the setting at all.

Ok, so are you saying that the server would check the resolved initialization options and check whether the client supports dynamic registration for the relevant capability (rename and diagnostic) and set the server capability according the relevant server setting (`ty.experimental.rename` and `ty.diagnosticMode`)?

---

_@dhruvmanila reviewed on 2025-08-07 12:52_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/capabilities.rs`:306 on 2025-08-07 12:52_

This is the behavior that was discussed in the review feedback.

---

_@dhruvmanila reviewed on 2025-08-07 12:52_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:574 on 2025-08-07 12:52_

I'm going to do this as a follow-up.

---

_Merged by @dhruvmanila on 2025-08-07 12:54_

---

_Closed by @dhruvmanila on 2025-08-07 12:54_

---

_Branch deleted on 2025-08-07 12:54_

---
