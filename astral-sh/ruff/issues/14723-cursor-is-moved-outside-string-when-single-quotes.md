```yaml
number: 14723
title: cursor is moved outside string when single quotes are formatted to double quotes on save
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2024-12-02T01:21:55Z
updated_at: 2024-12-02T05:31:10Z
url: https://github.com/astral-sh/ruff/issues/14723
synced_at: 2026-01-10T11:09:56Z
```

# cursor is moved outside string when single quotes are formatted to double quotes on save

---

_Issue opened by @DetachHead on 2024-12-02 01:21_

when the cursor is on the last character of a single-quoted string, it gets moved outside of the string when formatting.
# before formatting
![Image](https://github.com/user-attachments/assets/4f7c41f0-2cd3-4589-ba9d-8175b7892eb1)

# after formatting
## actual
![Image](https://github.com/user-attachments/assets/55a25f7f-d472-446a-9d40-ba4360f4b320)

## expected
![Image](https://github.com/user-attachments/assets/cf95e0d1-3166-4623-9ab1-b45d9f0f86ee)

# video
https://github.com/user-attachments/assets/e1099722-e05a-4942-b47a-a793ac2c258a


---

_Comment by @dhruvmanila on 2024-12-02 04:24_

Thanks for opening this issue but I don't think this is in our control. The only information that Ruff extension provides is the formatted text which then the editor uses to update the file:

```
2024-12-02 09:52:59.856 [info] [Trace - 9:52:59 AM] Received response 'textDocument/formatting - (55)' in 0ms.
2024-12-02 09:52:59.856 [info] Result: [
    {
        "newText": "\"asdf\" + \"asdf\"\n",
        "range": {
            "end": {
                "character": 15,
                "line": 0
            },
            "start": {
                "character": 0,
                "line": 0
            }
        }
    }
]
``` 

---

_Comment by @DetachHead on 2024-12-02 05:20_

it does seem to have some knowledge of where to move the cursor to in some cases:

![Image](https://github.com/user-attachments/assets/c683b9a9-8fdf-4ef4-b6a9-d9181d7c5c6a)
![Image](https://github.com/user-attachments/assets/6be60241-774e-4e37-abf7-0f93e7ec8802)

though i have no idea how this works and [there doesn't seem to be any way to specify the cursor position in the lsp](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textEdit) so i guess this issue can be closed

---

_Comment by @dhruvmanila on 2024-12-02 05:31_

Yeah, I don't think the language server can specify the cursor position for an edit. It's probably some heuristic that the editors follow which might even be different for each of them.

---

_Closed by @dhruvmanila on 2024-12-02 05:31_

---
