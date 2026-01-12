```yaml
number: 1619
title: Allow the server in buffer not tied to actual file
type: issue
state: closed
author: klonuo
labels:
  - needs-info
assignees: []
created_at: 2025-11-24T12:08:10Z
updated_at: 2025-11-24T14:36:40Z
url: https://github.com/astral-sh/ty/issues/1619
synced_at: 2026-01-12T15:54:25Z
```

# Allow the server in buffer not tied to actual file

---

_@klonuo_

Hi,

If I open empty buffer in editor, set the syntax to Python and paste some code, the server doesn't do anything.
If I create empty file and save it, then paste some code server works even saved file is empty, just following the code in the buffer.

This is the same for `ruff` too, but for example other LSP server allows working on empty buffer, i.e. LSP json.

So I wanted to suggest this as a feature request, if easily possible.

---

_Comment by @MichaReiser on 2025-11-24 12:24_

Hi @klonuo 

This should work with ty. Can you tell me more about what editor you're using? Can you share an example of what isn't working. E.g are diagnostics missing? Are completions not working? Do you have a code example you could share?

---

_Label `needs-info` added by @MichaReiser on 2025-11-24 12:26_

---

_Comment by @klonuo on 2025-11-24 12:32_

Tried both in Zed and in Sublime Text.

I noticed this while I copied part of the logs which was Python dictionary and wanted to format it with ruff to see whats happening, for example:

```python
{'merge_action': 'UPDATE', 'updated': datetime.datetime(2025, 11, 24, 10, 24)}
```

Just copy paste this in empty buffer.

In Sublime Text LSP options are disabled (although as previously mentioned if I do the same with json code in empty buffer it works fine).
In Zed I can run LSP command but nothing happens nor anything is written in LSP log, until I create new file and do the same.

---

_Comment by @MichaReiser on 2025-11-24 12:34_

Okay, so this is about Ruff and not ty? 

What formatting woudl you expect for `{'merge_action': 'UPDATE', 'updated': datetime.datetime(2025, 11, 24, 10, 24)}` ? The formatting looks correct, at least when you've configured single quotes over double quotes

---

_Comment by @klonuo on 2025-11-24 12:36_

It's same with ty - if you hover on datetime for example nothing will happen, nor it would complain that datetime is not defined.

The dictionary was much larger, this is just for example :)

---

_Comment by @MichaReiser on 2025-11-24 12:38_

I just tested this with Zed and I can confirm that formatting doesn't work for untitled files but it does for VS Code. I took a look at the logs and it seems that Zed never sends a message to Ruff or ty when the untitled file is open. So this seems like an upstream bug in Zed rather than ruff or ty

---

_Comment by @klonuo on 2025-11-24 12:45_

Same for Sublime Text, but in VS Code it indeed works for some reason. I dont have neovim here to check also...

---

_Comment by @MichaReiser on 2025-11-24 12:58_

I don't have Sublime but the follwing upstream zed issues seem relevant:

* https://github.com/zed-industries/zed/issues/17098
* https://github.com/zed-industries/zed/issues/20839

I don't know how sublime handles unsaved buffers but they are supported in Ruff and ty.

---

_Comment by @klonuo on 2025-11-24 13:26_

Ok thanks, in vscode both work obviously. I'll close this issue

---

_Closed by @klonuo on 2025-11-24 14:36_

---
