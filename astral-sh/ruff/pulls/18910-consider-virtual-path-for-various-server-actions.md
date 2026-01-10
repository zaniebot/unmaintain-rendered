```yaml
number: 18910
title: Consider virtual path for various server actions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/server-virtual-path
created_at: 2025-06-24T08:48:30Z
updated_at: 2025-06-24T12:30:48Z
url: https://github.com/astral-sh/ruff/pull/18910
synced_at: 2026-01-10T18:39:09Z
```

# Consider virtual path for various server actions

---

_Pull request opened by @dhruvmanila on 2025-06-24 08:48_

## Summary

Ref: https://github.com/astral-sh/ruff/issues/14820#issuecomment-2996690681

This PR fixes a bug where virtual paths or any paths that doesn't exists on the file system weren't being considered for checking inclusion / exclusion. This was because the logic used `file_path` which returns `None` for those path. This PR fixes that by using the `virtual_file_path` method that returns a `Path` corresponding to the actual file on disk or any kind of virtual path.

This should ideally just fix the above linked issue by way of excluding the documents representing the interactive window because they aren't in the inclusion set. It failed only on Windows previously because the file path construction would fail and then Ruff would default to including all the files.

## Test Plan

On my machine, the `.interactive` paths are always excluded so I'm using the inclusion set instead:

```json
{
  "ruff.nativeServer": "on",
  "ruff.path": ["/Users/dhruv/work/astral/ruff/target/debug/ruff"],
  "ruff.configuration": {
    "extend-include": ["*.interactive"]
  }
}
```

The diagnostics are shown for both the file paths and the interactive window:

<img width="1727" alt="Screenshot 2025-06-24 at 14 56 40" src="https://github.com/user-attachments/assets/d36af96a-777e-4367-8acf-4d9c9014d025" />

And, the logs:

```
2025-06-24 14:56:26.478275000 DEBUG notification{method="notebookDocument/didChange"}: Included path via `extend-include`: /Interactive-1.interactive
```

And, when using `ruff.exclude` via:

```json
{
	"ruff.exclude": ["*.interactive"]
}
```

With logs:

```
2025-06-24 14:58:41.117743000 DEBUG notification{method="notebookDocument/didChange"}: Ignored path via `exclude`: /Interactive-1.interactive
```


---

_Label `server` added by @dhruvmanila on 2025-06-24 08:48_

---

_Comment by @github-actions[bot] on 2025-06-24 08:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dhruvmanila on 2025-06-24 09:30_

The diagnostics remain even after I close the interactive window:

```
2025-06-24 14:59:51.053545000 DEBUG notification{method="textDocument/didClose"}: Unable to close document with key vscode-notebook-cell:/Interactive-1.interactive#W1sdW50aXRsZWQ%3D - the snapshot was unavailable
```

TODO: Currently debugging

---

_Marked ready for review by @dhruvmanila on 2025-06-24 10:23_

---

_Comment by @dhruvmanila on 2025-06-24 10:49_

> TODO: Currently debugging

Ugh, it seems that VS Code is not sending the request in the order that we would get for other notebook documents.

When I close the interactive window, I get the following two requests:

```
[Trace - 3:56:51 PM] Sending notification 'notebookDocument/didClose'.
Params: {
    "notebookDocument": {
        "uri": "untitled:/Interactive-1.interactive"
    },
    "cellTextDocuments": [
        {
            "uri": "vscode-notebook-cell:/Interactive-1.interactive#W1sdW50aXRsZWQ%3D"
        }
    ]
}


[Trace - 3:56:51 PM] Sending notification 'textDocument/didClose'.
Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:/Interactive-1.interactive#W1sdW50aXRsZWQ%3D"
    }
}
```

Here, VS Code first sends the request to close the notebook document, so we remove it from the internal index. And, then when it sends us the request to close the cell document, we fail because the notebook document doesn't exists in the index:

https://github.com/astral-sh/ruff/blob/7f5d7b5dd1f41e63ddb94ec991ad26b03a8a317e/crates/ruff_server/src/session/index.rs#L248-L248

But, when I close a regular Jupyter Notebook (`.ipynb` file), it sends the request in reverse order:

```
[Trace - 4:13:58 PM] Sending notification 'textDocument/didClose'.
Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:/private/tmp/ruff-interactive-test/play.ipynb#W0sZmlsZQ%3D%3D"
    }
}


[Trace - 4:13:58 PM] Sending notification 'notebookDocument/didClose'.
Params: {
    "notebookDocument": {
        "uri": "file:///private/tmp/ruff-interactive-test/play.ipynb"
    },
    "cellTextDocuments": [
        {
            "uri": "vscode-notebook-cell:/private/tmp/ruff-interactive-test/play.ipynb#W0sZmlsZQ%3D%3D"
        }
    ]
}
```


---

_Review requested from @MichaReiser by @dhruvmanila on 2025-06-24 11:26_

---

_Comment by @dhruvmanila on 2025-06-24 11:28_

Discussing this internally, I think the above mentioned inconsistency can be fixed in a separate PR as it isn't related to the issue.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/fix.rs`:49 on 2025-06-24 11:57_

Should we use the normal path for `package` detection because we know that virtual documents can never belong to a package (I would also feel better about `parent` not throwing for real paths. Not sure if there's a case with a virtual path where that panics)

---

_@MichaReiser approved on 2025-06-24 11:58_

---

_Comment by @MichaReiser on 2025-06-24 11:58_

Thanks for looking into this issue

---

_@dhruvmanila reviewed on 2025-06-24 12:16_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/fix.rs`:49 on 2025-06-24 12:16_

Yeah, that makes sense, thanks for catching that!

---

_@MichaReiser reviewed on 2025-06-24 12:24_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:84 on 2025-06-24 12:24_

Same here, we can use `file_path` here

---

_Merged by @dhruvmanila on 2025-06-24 12:24_

---

_Closed by @dhruvmanila on 2025-06-24 12:24_

---

_Branch deleted on 2025-06-24 12:24_

---

_@dhruvmanila reviewed on 2025-06-24 12:29_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:84 on 2025-06-24 12:29_

Sorry! I missed this, created https://github.com/astral-sh/ruff/pull/18914

---
