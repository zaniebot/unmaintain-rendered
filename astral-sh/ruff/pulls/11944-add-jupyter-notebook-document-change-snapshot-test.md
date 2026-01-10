```yaml
number: 11944
title: Add Jupyter Notebook document change snapshot test
type: pull_request
state: merged
author: snowsignal
labels:
  - server
  - testing
assignees: []
merged: true
base: main
head: jane/server/snapshots/jupyter-notebook
created_at: 2024-06-20T01:37:59Z
updated_at: 2024-06-21T05:39:07Z
url: https://github.com/astral-sh/ruff/pull/11944
synced_at: 2026-01-10T21:56:00Z
```

# Add Jupyter Notebook document change snapshot test

---

_Pull request opened by @snowsignal on 2024-06-20 01:37_

## Summary

Closes #11914.

This PR introduces a snapshot test that replays the LSP requests made during a document formatting request, and confirms that the notebook document is updated in the expected way.

---

_Label `server` added by @snowsignal on 2024-06-20 01:37_

---

_Label `testing` added by @snowsignal on 2024-06-20 01:37_

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-20 01:37_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-06-20 01:37_

---

_Comment by @github-actions[bot] on 2024-06-20 01:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/tests/notebook.rs`:14 on 2024-06-20 06:41_

Can we use a more generic name for the notebook. For some reason, my brain tries to give meaning to the notebook name (e.g. that it is relevant for this specific test case) where I think it isn't. Maybe just `test_notebook.ipynb`?

---

_Review comment by @MichaReiser on `crates/ruff_server/tests/snapshots/notebook__changed_notebook.snap`:5 on 2024-06-20 07:10_

Would it make sense in your view to instead render a visual representation of the notebook (by e.g. calling `to_string()` instead of snapshoting all internal fields? Snapshoting all fields seems brittle to internal changes of the notebook (which is exactly when we want to ensure that we didn't break anything, but the tests all change in that case, and become useless)

---

_@MichaReiser approved on 2024-06-20 07:11_

What I like about the snapshot tests is that they're in the `ruff-server` crate. That means, we dont' need to update `ruff-server` in the `ruff-vscode` repository to see if the tests are passing. 

What makes me worried about this kind of tests is that they encode our understanding of the LSP protocol and directly call into low level functionalities, which makes them brittle to changes and requires exposing internals. 

I think my ideal setup would be that we have a way to record LSP messages. This doesn't have to be automated. I'm fine copying those messages from the tracing panel for now. 

> To fully capture LSP messages between the editor and the server, set "rust-analyzer.trace.server": "verbose" config and check Output > Rust Analyzer Language Server Trace.

The second piece in the puzzle would be a test-runner that reads all the messages from the input file and passes them to the server. But I don't know how complicated it is to build a test bed like this. I think there's some complication involved once the server needs to respond to client requests because it then becomes necessary to map the requests? 

An alternative to this is to rely on VS code directly. It's unclear how we would run such tests in CI, especially as part of the ruff repository but it would give us the best coverage. See https://github.com/prettier/prettier-vscode/blob/main/CONTRIBUTING.md

I'm okay merging this specific test but I think I'm hesitant of adding too many "hand crafted" tests that snapshot internal data structures because they encode our LSP understanding and will break whenever we make changes to the internal representation.

---

_Review comment by @dhruvmanila on `crates/ruff_server/tests/notebook.rs`:44 on 2024-06-20 08:02_

What's the process of ensuring that any changes in the actual notebook is being reflected here? I assume it needs to be manually updated so it might be useful for a future developer to provide a brief documentation on how to update them.

---

_Review comment by @dhruvmanila on `crates/ruff_server/resources/test/fixtures/super_resolution_overview.ipynb`:1 on 2024-06-20 08:04_

I think it will be more useful to have minimal code in the Notebook (could be hand-written) as the test is about whether the edits are correct. This will make it quicker to verify whether the ranges of the edits themselves are correct or not. Otherwise, a developer might need to read the notebook and extract the relevant ranges to make sure the updates are in the correct place.

An example notebook would be around 3-5 cells mixed with markdown cells where some cells would need formatting while others don't. The number of lines doesn't really matter.

---

_@dhruvmanila approved on 2024-06-20 08:04_

---

_@snowsignal reviewed on 2024-06-20 18:35_

---

_Review comment by @snowsignal on `crates/ruff_server/tests/snapshots/notebook__changed_notebook.snap`:5 on 2024-06-20 18:35_

I'm fine with that! It's probably easier to understand a diff like that, too.

---

_@snowsignal reviewed on 2024-06-20 18:36_

---

_Review comment by @snowsignal on `crates/ruff_server/tests/notebook.rs`:44 on 2024-06-20 18:36_

These changes were copied over from custom traces I had set up temporarily in the server. It's far from ideal, but I can add some documentation on how I went about it.

---

_@snowsignal reviewed on 2024-06-20 18:37_

---

_Review comment by @snowsignal on `crates/ruff_server/resources/test/fixtures/super_resolution_overview.ipynb`:1 on 2024-06-20 18:37_

For this snapshot I wanted a 'real world' test case, and one that comes from performing an operation that can easily be replicated (running a full-document format). For future notebook snapshots I think what you suggested makes sense.

---

_Comment by @snowsignal on 2024-06-20 18:42_

> I think my ideal setup would be that we have a way to record LSP messages. This doesn't have to be automated. I'm fine copying those messages from the tracing panel for now.

@MichaReiser I agree completely - I eventually want to have a system in place that can fully capture the back-and-forth with VS Code and build snapshot tests automatically.

> The second piece in the puzzle would be a test-runner that reads all the messages from the input file and passes them to the server. But I don't know how complicated it is to build a test bed like this. I think there's some complication involved once the server needs to respond to client requests because it then becomes necessary to map the requests?

Mapping requests shouldn't be too hard since they have IDs.

---

_@dhruvmanila reviewed on 2024-06-21 05:10_

---

_Review comment by @dhruvmanila on `crates/ruff_server/tests/notebook.rs`:44 on 2024-06-21 05:10_

Thank you. No need to go into too much detail, some high level instructions are fine. It can be done as a follow-up as well.

---

_@snowsignal reviewed on 2024-06-21 05:26_

---

_Review comment by @snowsignal on `crates/ruff_server/tests/notebook.rs`:44 on 2024-06-21 05:26_

I'll do this as a follow-up, then!

---

_Merged by @snowsignal on 2024-06-21 05:29_

---

_Closed by @snowsignal on 2024-06-21 05:29_

---

_Branch deleted on 2024-06-21 05:29_

---
