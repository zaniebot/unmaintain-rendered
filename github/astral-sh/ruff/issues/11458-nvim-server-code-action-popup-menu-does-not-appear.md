---
number: 11458
title: "[nvim/server] code action popup menu does not appear "
type: issue
state: closed
author: baggiponte
labels:
  - question
assignees: []
created_at: 2024-05-17T09:51:55Z
updated_at: 2024-05-20T07:47:06Z
url: https://github.com/astral-sh/ruff/issues/11458
synced_at: 2026-01-07T13:12:15-06:00
---

# [nvim/server] code action popup menu does not appear 

---

_Issue opened by @baggiponte on 2024-05-17 09:51_

Ciao!

I am using neovim with ruff (and/or ruff-lsp) as LSP server, in tandem with pyright. Yesterday I update to nvim 0.10.0 and noticed ruff odes not display the code action popup menu (`vim.lsp.buf.code_action()`). I was just using ruff, so I reinstalled ruff-lsp and in the logs found this:

```
[ERROR][2024-05-17 11:38:37] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/luca/.local/share/nvim/mason/bin/ruff"	"stderr"	"  10.131295s ERROR ruff_server::server::api An error occurred with result ID 4: failed to deserialize diagnostic data: missing field `range`\n"

[ERROR][2024-05-17 11:38:37] .../vim/lsp/rpc.lua:770	"rpc"	"/Users/luca/.local/share/nvim/mason/bin/ruff-lsp"	"stderr"	"Exception occurred for message \"3\": KeyError: 'location'\nTraceback (most recent call last):\n  File \"/Users/luca/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/pygls/protocol/json_rpc.py\", line 194, in _execute_request_callback\n    self._send_response(msg_id, result=future.result())\n                                       ^^^^^^^^^^^^^^^\n  File \"/Users/luca/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/ruff_lsp/server.py\", line 1000, in code_action\n    edit=_create_workspace_edit(\n         ^^^^^^^^^^^^^^^^^^^^^^^\n  File \"/Users/luca/.local/share/nvim/mason/packages/ruff-lsp/venv/lib/python3.12/site-packages/ruff_lsp/server.py\", line 1490, in _create_workspace_edit\n    line=edit[\"location\"][\"row\"] - 1,\n         ~~~~^^^^^^^^^^^^\nKeyError: 'location'\n"
```

The first log should be the error within ruff server, the second within ruff-lsp. Am finding it really hard to dissect, any suggestion on how I can improve the report? Thanks! I guess I should s/o @dhruvmanila

---

_Comment by @baggiponte on 2024-05-17 12:17_

Don't know why but this solved itself.

---

_Closed by @baggiponte on 2024-05-17 12:17_

---

_Comment by @dhruvmanila on 2024-05-17 12:20_

Thank you for updating the issue. Feel free to re-open if you face this problem again, I can debug it.

---

_Comment by @baggiponte on 2024-05-17 13:04_

Thanks for your answer! Now I have this small issue:

<img width="629" alt="image" src="https://github.com/astral-sh/ruff/assets/57922983/837acf26-5616-4942-b7bc-f06c214cf659">

Set aside the error message (don't know what this is due to), I see a duplicated message for errors 1-2 and no option to ignore the rule. Maybe nvim can only display 4 options?

---

_Comment by @dhruvmanila on 2024-05-20 05:33_

I don't think there's any limit as to how many code actions Neovim can display. I'm not exactly sure what I'm seeing here. Is it a notification popup like the one in [`nvim-notify`](https://github.com/rcarriga/nvim-notify)? Is it some other plugin which shows a floating window and then you type an appropriate number?

---

_Label `question` added by @dhruvmanila on 2024-05-20 05:33_

---

_Comment by @baggiponte on 2024-05-20 07:47_

> I don't think there's any limit as to how many code actions Neovim can display. I'm not exactly sure what I'm seeing here. Is it a notification popup like the one in [`nvim-notify`](https://github.com/rcarriga/nvim-notify)? Is it some other plugin which shows a floating window and then you type an appropriate number?

Ops, sorry. It's [`noice.nvim`](https://github.com/folke/noice.nvim). It uses notify under the hood. It might simply be an error on their side. Will open a new issue if I see that pop up again 😊

---
