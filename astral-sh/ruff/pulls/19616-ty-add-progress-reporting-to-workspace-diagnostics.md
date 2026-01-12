```yaml
number: 19616
title: "[ty] Add progress reporting to workspace diagnostics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/workspace-diagnostics-progress
created_at: 2025-07-29T13:13:57Z
updated_at: 2025-07-30T10:27:35Z
url: https://github.com/astral-sh/ruff/pull/19616
synced_at: 2026-01-12T15:56:43Z
```

# [ty] Add progress reporting to workspace diagnostics

---

_@MichaReiser_

## Summary

This PR adds a nice little progress bar when ty is checking an entire workspace. 

This turned out to be trickier than I thought. I initially thought that it would be as easy as taking the `work_done_token` from the workspace diagnostic request and sending the right `$/progress` notifications using that token. However, that doesn't work for VS Code nor Zed. It took me quite a while to find out that VS Code doesn't support this. Instead, a server has to use the server initiated work done token registration. With this, it finally works but it requires some awkward concurrent code because we aren't supposed to use that token until the client replied that it's ok to do so... but we don't want to delay checking only so that we can report progress. 




## Test Plan


https://github.com/user-attachments/assets/0fefea21-69b9-4c7a-803c-476d72a1bbaa


https://github.com/user-attachments/assets/f2ff1d4c-9924-413d-bdb9-1dcdb845f7ce

Disclaimer: I used a debug build to test this feature. A release build is much faster :)



---

_Label `server` added by @MichaReiser on 2025-07-29 13:13_

---

_Label `ty` added by @MichaReiser on 2025-07-29 13:13_

---

_Label `server` added by @MichaReiser on 2025-07-29 13:13_

---

_Label `ty` added by @MichaReiser on 2025-07-29 13:13_

---

_Comment by @github-actions[bot] on 2025-07-29 13:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-29 13:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-07-29 15:42_

---

_Review requested from @carljm by @MichaReiser on 2025-07-29 15:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-29 15:42_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-29 15:42_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-29 15:42_

---

_Review request for @dcreager removed by @MichaReiser on 2025-07-29 15:42_

---

_Review request for @carljm removed by @MichaReiser on 2025-07-29 15:42_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-07-29 15:42_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-07-29 15:42_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-07-29 15:42_

---

_Renamed from "[ty] Add progress report to workspace diagnostics" to "[ty] Add progress reporting to workspace diagnostics" by @MichaReiser on 2025-07-29 15:45_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server.rs`:61 on 2025-07-30 03:45_

Does this show up? Because, tracing hasn't been initialized yet.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/lazy_work_done_progress.rs`:32 on 2025-07-30 03:52_

```suggestion
/// ## Server Initiated Progress
///
/// The implementation initiates a work done progress report lazily when no token is provided in the request.
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/lazy_work_done_progress.rs`:14 on 2025-07-30 03:55_

nit: I usually move the references at the end so as to avoid breaking the reading flow

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/lazy_work_done_progress.rs`:151 on 2025-07-30 04:07_

This can only happen when the create progress failed somehow, right? Would it be useful to add a debug message in this case?

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:200 on 2025-07-30 04:41_

nit: use `let ... else { return Err() }`?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:78 on 2025-07-30 04:42_

Should this be _not_ `pub(crate)` as, I think, it's internal now given that we have `send_request` and `send_deferred_request`?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:44 on 2025-07-30 04:44_

(Can't comment on the actual lines)

The "Note" part of `send_request` is now implemented via `send_deferred_request`, we could either remove it or change it to reflect the current logic.

---

_@dhruvmanila approved on 2025-07-30 04:47_

This looks great!

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:62 on 2025-07-30 04:50_

I'm still a bit unsure on why this is required. Do you mind explaining in brief on the reason this is required?

---

_@dhruvmanila reviewed on 2025-07-30 04:52_

---

_Comment by @dhruvmanila on 2025-07-30 05:02_

This is awesome to see. Here's a demo in Neovim:

https://github.com/user-attachments/assets/4d1a7eab-a7c1-421f-b4dd-889723d77e2c

Ignore the end message, that's an issue with the formatting logic that I've implemented.


---

_@MichaReiser reviewed on 2025-07-30 06:50_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server.rs`:61 on 2025-07-30 06:50_

Huh, I thought I deleted this line. Thanks for catching this

---

_@MichaReiser reviewed on 2025-07-30 06:51_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/lazy_work_done_progress.rs`:151 on 2025-07-30 06:51_

This can also happen if the client doesn't support work done progress reporting or if the server initiated request didn't complete before `Drop` is called

---

_@MichaReiser reviewed on 2025-07-30 06:53_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/client.rs`:62 on 2025-07-30 06:53_

`send_request` requires a `&Session`. We don't have a `&Session` in the background worker. We only have a `SessionSnapshot`.

---

_@dhruvmanila reviewed on 2025-07-30 08:09_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/client.rs`:62 on 2025-07-30 08:09_

Ah ok, so basically what the note mentioned. I thought there must be something else as well :)

---

_@MichaReiser reviewed on 2025-07-30 10:14_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/client.rs`:78 on 2025-07-30 10:14_

It's also called from `main_loop` which requires it to be `pub(crate)`

---

_Merged by @MichaReiser on 2025-07-30 10:27_

---

_Closed by @MichaReiser on 2025-07-30 10:27_

---

_Branch deleted on 2025-07-30 10:27_

---
