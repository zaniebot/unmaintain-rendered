```yaml
number: 18230
title: "[ty] Add `python.ty.disableLanguageServices` config"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/disable-language-services
created_at: 2025-05-20T19:10:25Z
updated_at: 2025-06-17T08:20:47Z
url: https://github.com/astral-sh/ruff/pull/18230
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Add `python.ty.disableLanguageServices` config

---

_Pull request opened by @dhruvmanila on 2025-05-20 19:10_

## Summary

PR adding support for it in the VS Code extension: https://github.com/astral-sh/ty-vscode/pull/36

This PR adds support for `python.ty.disableLanguageServices` to the ty language server by accepting this as server setting.

This has the same issue as https://github.com/astral-sh/ty/issues/282 in that it only works when configured globally. Fixing that requires support for multiple workspaces in the server itself.

I also went ahead and did a similar refactor as the Ruff server to use "Options" and "Settings" to keep the code consistent although the combine functionality doesn't exists yet because workspace settings isn't supported in the ty server.

## Test Plan

Refer to https://github.com/astral-sh/ty-vscode/pull/36 for the test demo.


---

_Label `server` added by @dhruvmanila on 2025-05-20 19:10_

---

_Label `ty` added by @dhruvmanila on 2025-05-20 19:10_

---

_Comment by @github-actions[bot] on 2025-05-20 19:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dhruvmanila reviewed on 2025-05-20 19:53_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/settings.rs`:27 on 2025-05-20 19:53_

An alternative here is to avoid introducing "Python" struct and directly use `disableLanguageServices` in `ClientSettings` like:

```rs
struct ClientSettings {
	disable_language_services: Option<bool>
}
```

The disadvantage for this is that in VS Code the setting would be `python.ty.disableLanguageServices` while in other editors it would just be `disableLanguageServices`.

This PR keeps it consistent across editors and this is what we want as well because once `workspace/didChangeConfiguration` notification support is added, we'd directly query the client (editor) to get this setting value instead of mirroring the structure here.

---

_@dhruvmanila reviewed on 2025-05-20 20:07_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:36 on 2025-05-20 20:07_

I tried doing this centrally in `background_request_task` but that didn't work:

```diff
diff --git a/crates/ty_server/src/server/api.rs b/crates/ty_server/src/server/api.rs
index a9dd9da1d0..9899184fd4 100644
--- a/crates/ty_server/src/server/api.rs
+++ b/crates/ty_server/src/server/api.rs
@@ -143,7 +143,18 @@ fn background_request_task<'a, R: traits::BackgroundDocumentRequestHandler>(
 
         Box::new(move |notifier, responder| {
             let _span = tracing::trace_span!("request", %id, method = R::METHOD).entered();
-            let result = R::run_with_snapshot(&db, snapshot, notifier, params);
+            let result = if snapshot.client_settings().is_language_services_disabled()
+                && matches!(
+                    R::METHOD,
+                    request::GotoTypeDefinitionRequestHandler::METHOD
+                        | request::CompletionRequestHandler::METHOD
+                        | request::InlayHintRequestHandler::METHOD
+                        | request::HoverRequestHandler::METHOD
+                ) {
+                // Return an empty response here ...
+            } else {
+                R::run_with_snapshot(&db, snapshot, notifier, params)
+            };
             respond::<R>(id, result, &responder);
         })
     }))
```

But, then we'd need to somehow be able to get an "empty" response from those handlers which could be `None` or something else.


---

_Comment by @codspeed-hq[bot] on 2025-06-16 09:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fdisable-language-services)

### Merging #18230 will **not alter performance**

<sub>Comparing <code>dhruv/disable-language-services</code> (e6cca67) with <code>main</code> (c22f809)</sub>



### Summary

`✅ 34` untouched benchmarks  





---

_Comment by @dhruvmanila on 2025-06-16 09:36_

This PR is based on top of https://github.com/astral-sh/ruff/pull/18650 because it's just easier to not have to deal with the (now removed) experimental section.

---

_Marked ready for review by @dhruvmanila on 2025-06-16 10:39_

---

_Review requested from @carljm by @dhruvmanila on 2025-06-16 10:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-06-16 10:39_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-06-16 10:39_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-06-16 10:39_

---

_Review requested from @dcreager by @dhruvmanila on 2025-06-16 10:39_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-16 22:35_

---

_Review request for @carljm removed by @carljm on 2025-06-17 01:53_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:36 on 2025-06-17 05:55_

I prefer having this in the specific handlers. It's less surprising, and someone copying an existing handler (which I always do as a starting point) is then also more likely to think about checking this option.

I guess, it's somewhat wasteful to still have the client send us requests but I suspect that unregistering the providers is more complicated and not supported by all clients?

---

_@MichaReiser approved on 2025-06-17 05:56_

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-17 06:38_

---

_@dhruvmanila reviewed on 2025-06-17 06:55_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:36 on 2025-06-17 06:55_

> I guess, it's somewhat wasteful to still have the client send us requests but I suspect that unregistering the providers is more complicated and not supported by all clients?

I think unregistering only works for capabilities that support dynamic registration and I'm not sure if all language capabilities supports that. And, this would also require that we go through the dynamic registration path (via registering each capabilities dynamically) to un-register the capability later as it requires the id that the server used to perform the registration.

---

_Closed by @dhruvmanila on 2025-06-17 07:02_

---

_Reopened by @dhruvmanila on 2025-06-17 07:02_

---

_Merged by @dhruvmanila on 2025-06-17 08:20_

---

_Closed by @dhruvmanila on 2025-06-17 08:20_

---

_Branch deleted on 2025-06-17 08:20_

---
