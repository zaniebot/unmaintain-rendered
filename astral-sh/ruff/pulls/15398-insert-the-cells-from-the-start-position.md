```yaml
number: 15398
title: "Insert the cells from the `start` position"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - notebook
  - server
assignees: []
merged: true
base: main
head: dhruv/fix-notebook-update
created_at: 2025-01-10T12:25:17Z
updated_at: 2025-01-10T13:20:34Z
url: https://github.com/astral-sh/ruff/pull/15398
synced_at: 2026-01-12T15:55:51Z
```

# Insert the cells from the `start` position

---

_@dhruvmanila_

## Summary

The cause of this bug is from https://github.com/astral-sh/ruff/pull/12575 which was itself a bug fix but the fix wasn't completely correct.

fixes: #14768 
fixes: https://github.com/astral-sh/ruff-vscode/issues/644
fixes: https://github.com/astral-sh/ruff-vscode/issues/573

## Test Plan

Consider the following three cells:

1.
```python
class Foo:
    def __init__(self):
        self.x = 1

    def __str__(self):
        return f"Foo({self.x})"
```

2.
```python
def hello():
    print("hello world")
```

3.
```python
y = 1
```

The test case is moving cell 2 to the top i.e., cell 2 goes to position 1 and cell 1 goes to position 2.

Before this fix, it can be seen that the cells were pushed at the end of the vector:

```
  12.643269917s  INFO ruff:main ruff_server::edit::notebook: Before update: [
    NotebookCell {
        document: TextDocument {
            contents: "class Foo:\n    def __init__(self):\n        self.x = 1\n\n    def __str__(self):\n        return f\"Foo({self.x})\"",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "def hello():\n    print(\"hello world\")",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "y = 1",
        },
    },
]
  12.643777667s  INFO ruff:main ruff_server::edit::notebook: After update: [
    NotebookCell {
        document: TextDocument {
            contents: "y = 1",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "class Foo:\n    def __init__(self):\n        self.x = 1\n\n    def __str__(self):\n        return f\"Foo({self.x})\"",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "def hello():\n    print(\"hello world\")",
        },
    },
]
```

After the fix in this PR, it can be seen that the cells are being pushed at the correct `start` index:

```
   6.520570917s  INFO ruff:main ruff_server::edit::notebook: Before update: [
    NotebookCell {
        document: TextDocument {
            contents: "class Foo:\n    def __init__(self):\n        self.x = 1\n\n    def __str__(self):\n        return f\"Foo({self.x})\"",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "def hello():\n    print(\"hello world\")",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "y = 1",
        },
    },
]
   6.521084792s  INFO ruff:main ruff_server::edit::notebook: After update: [
    NotebookCell {
        document: TextDocument {
            contents: "def hello():\n    print(\"hello world\")",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "class Foo:\n    def __init__(self):\n        self.x = 1\n\n    def __str__(self):\n        return f\"Foo({self.x})\"",
        },
    },
    NotebookCell {
        document: TextDocument {
            contents: "y = 1",
        },
    },
]
```

---

_Label `bug` added by @dhruvmanila on 2025-01-10 12:25_

---

_Label `server` added by @dhruvmanila on 2025-01-10 12:25_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-10 12:25_

---

_Label `notebook` added by @AlexWaygood on 2025-01-10 12:27_

---

_Comment by @dhruvmanila on 2025-01-10 12:31_

TODO: I'm going to write a test case for this.

Edit: Added a test case for this scenario.

---

_Comment by @github-actions[bot] on 2025-01-10 12:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/notebook.rs`:139 on 2025-01-10 12:33_

Nit: We could now move the `sell.insert` call out of the `if else` and instead return the `content` and `version` from the `if else`

```rust
let (content, version) = if let Some(text_document) = deleted_cells.remove(&cell.document) {
	let version = text_document.version();
	(text_document.into_contents(), version)
} else {
	(String::new(), 0)
};
```

---

_@MichaReiser approved on 2025-01-10 12:33_

---

_Merged by @dhruvmanila on 2025-01-10 13:11_

---

_Closed by @dhruvmanila on 2025-01-10 13:11_

---

_Branch deleted on 2025-01-10 13:11_

---
