```yaml
number: 19657
title: "[ty] Implement streaming for workspace diagnostics"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/diagnostic-streaming
created_at: 2025-07-31T08:19:28Z
updated_at: 2025-08-04T09:34:46Z
url: https://github.com/astral-sh/ruff/pull/19657
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Implement streaming for workspace diagnostics

---

_Pull request opened by @MichaReiser on 2025-07-31 08:19_

## Summary

This PR implements diagnostic streaming for the LSP's workspace diagnostic request. Streaming diagnostics has the advantage that they become visible as soon as ty has finished checking a file and not only when the entire workspace has been checked. 

Streaming diagnostics in the LSP doesn't require any sorting because that's a Client concern. 

The way this is implemented is that I extended the `ProgressReporter` trait to also get a reference to the diagnostics, and it's now the reporter's responsibility to collect the diagnostics if desired (which we don't want in the LSP case). 

This change fixes the performance issues that I've seen on home assistant where VS code hangs every time new workspace diagnostics come in. I assume that's because it can now process smaller diagnostic chunks.

## Test Plan

https://github.com/user-attachments/assets/4d687d00-d7ee-416d-8090-17faefd9967c


https://github.com/user-attachments/assets/304dbf3a-1ddd-44ae-94ea-56be83cba045





---

_Label `server` added by @MichaReiser on 2025-07-31 08:19_

---

_Label `ty` added by @MichaReiser on 2025-07-31 08:19_

---

_Comment by @github-actions[bot] on 2025-07-31 08:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-31 12:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser reviewed on 2025-07-31 12:33_

---

_Review comment by @MichaReiser on `crates/ty/tests/cli/rule_selection.rs`:879 on 2025-07-31 12:33_

This is now consistent with how we order other diagnostics without a file: 

* Compare spans
* Compare severity
* Compare id

`division-by-zero` has an error severity, so it should come before `unknown-rule`

---

_Marked ready for review by @MichaReiser on 2025-07-31 12:33_

---

_Review requested from @carljm by @MichaReiser on 2025-07-31 12:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-31 12:33_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-31 12:33_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-31 12:33_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-01 16:31_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/capabilities.rs`:222 on 2025-08-04 04:19_

Could we use an environment variable for this instead? I'm not sure if using client capabilities is correct here.

---

_Review comment by @dhruvmanila on `crates/ty_project/src/lib.rs`:136 on 2025-08-04 04:24_

nit: it might be useful to have this method be called something else like `report_diagnostics_without_file` which is a bit verbose but it could convey the method usage because `report_diagnostics` could mean that any diagnostics can be used to report. Or, it might be useful to expand the documentation of both `report_file` and `report_diagnostics` to specify when it's suppose to be used and link to the other method for a different use case.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:440 on 2025-08-04 04:33_

```suggestion
        // As per the spec:
        //
        // > partial result: The first literal send need to be a WorkspaceDiagnosticReport followed
        // > by `n` WorkspaceDiagnosticReportPartialResult literals
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:146 on 2025-08-04 04:35_

Any specific reason that this number got changed? I don't mind it

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:251 on 2025-08-04 04:47_

nit: these comments doesn't seem useful as it's describing the code, we can remove them

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:415 on 2025-08-04 04:50_

The test part would be useful to document in the `tests/e2e/main.rs` file otherwise it might hard to know how to write tests without looking at the code.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:418 on 2025-08-04 04:51_

The comment says "every ~100ms" but the code is using 50, is that intentional?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:111 on 2025-08-04 04:58_

Just to confirm my understanding, this finish call is mainly used when the reporting mode is "Bulk" to return all of the workspace diagnostics at once and for the diagnostics that are remaining at the end when the reporting mode is "Streaming"?

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:766 on 2025-08-04 04:59_

I think an environment variable would be more suitable for this

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:380 on 2025-08-04 05:04_

nit: I've started a habit of briefly documenting these test cases because they can get quite verbose and long and it might take time to understand what each test case is doing when looking at it after some time. It might be useful to do the same here, it doesn't need to be detailed but just describing what behavior this is testing would be quite useful.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:409 on 2025-08-04 05:05_

I'd probably remove this as it isn't required to "trigger workspace diagnostics" so it's a bit misleading for the test setup

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:436 on 2025-08-04 05:06_

The comment should be "Process the final report", right?

I'm not sure I understand the other part of the comment here -- if it should be a partial report, does it not mean that it should be `WorkspaceDiagnosticReportResult::Partial`?

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:442 on 2025-08-04 05:11_

Can you expand the comment on why this should be 1?

---

_@dhruvmanila approved on 2025-08-04 05:16_

This looks great!

I think I'd find it extremely useful to have some high level documentation for the methods and data structures. I'll probably forget how they all interact when I come back to it in the future ;)

---

_@MichaReiser reviewed on 2025-08-04 06:39_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/capabilities.rs`:222 on 2025-08-04 06:39_

I don't think so. You can't change environment variables for a running program (it requires unsafe code) and we would only want to change it for a single test. 

---

_@MichaReiser reviewed on 2025-08-04 06:40_

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:136 on 2025-08-04 06:40_

I think `report_diagnostics_without_file` is misleading. The diagnostics have a file. But I can see if I can improve the documentation

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:146 on 2025-08-04 06:41_

Sending that many `$/progress` notifications slowed down the overall check time especially when we recheck a project. We may want to implement a time based solution too 

---

_@MichaReiser reviewed on 2025-08-04 06:41_

---

_@MichaReiser reviewed on 2025-08-04 06:42_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:111 on 2025-08-04 06:42_

Yes, that's correct

---

_@MichaReiser reviewed on 2025-08-04 06:42_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:766 on 2025-08-04 06:42_

Let's discuss this above

---

_@MichaReiser reviewed on 2025-08-04 06:43_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:436 on 2025-08-04 06:43_

It should be a `Partial` report, but partial and full reports have the same shape, so that a serialized `Partial` result gets deserialized as a `Full` result. That's why we can't test it here

---

_Comment by @MichaReiser on 2025-08-04 06:45_

> I think I'd find it extremely useful to have some high level documentation for the methods and data structures. I'll probably forget how they all interact when I come back to it in the future ;)

I added a summary in https://github.com/astral-sh/ruff/pull/19670 Is this sufficient or are you looking for more?

---

_@dhruvmanila reviewed on 2025-08-04 06:53_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/capabilities.rs`:222 on 2025-08-04 06:53_

Maybe we could add another entrypoint to create the server like `Server::new_for_test` that sets this flag?

---

_@dhruvmanila reviewed on 2025-08-04 06:56_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:436 on 2025-08-04 06:56_

We use our own fork of `lsp-types` which we can update to accommodate this change (https://github.com/astral-sh/ruff/blob/b53424d6b17ccb7efcfc8ea52e1995797a4e921c/Cargo.toml#L119-L121)

---

_@dhruvmanila reviewed on 2025-08-04 06:57_

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:436 on 2025-08-04 06:57_

We could also add the `PartialWorkspaceProgress` in the fork

---

_Comment by @dhruvmanila on 2025-08-04 06:59_

> I added a summary in #19670 Is this sufficient or are you looking for more?

I think that documentation is great and it provides a good overview of all the things that have been implemented for workspace diagnostics. But, I think it'd still be useful to document the working parts (methods).

---

_@MichaReiser reviewed on 2025-08-04 09:17_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/workspace_diagnostic.rs`:415 on 2025-08-04 09:17_

I added a comment to the test where the behavior matters (and explain the motivation). I decided against adding it to `main.rs` because I'm worried that it will easily get outdated.

---

_@MichaReiser reviewed on 2025-08-04 09:20_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/pull_diagnostics.rs`:436 on 2025-08-04 09:20_

I thought about that but it feels like more work. 

---

_Merged by @MichaReiser on 2025-08-04 09:34_

---

_Closed by @MichaReiser on 2025-08-04 09:34_

---

_Branch deleted on 2025-08-04 09:34_

---
