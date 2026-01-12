```yaml
number: 1084
title: Full config support in editor settings
type: issue
state: open
author: OliverGuy
labels:
  - configuration
  - server
assignees: []
created_at: 2025-08-21T18:27:19Z
updated_at: 2025-11-14T08:57:38Z
url: https://github.com/astral-sh/ty/issues/1084
synced_at: 2026-01-12T15:54:24Z
```

# Full config support in editor settings

---

_@OliverGuy_

### Summary

It looks like the  [configuration options](https://docs.astral.sh/ty/reference/configuration/) do not seem fully supported within the [editor settings](https://docs.astral.sh/ty/reference/editor-settings/), namely `ty.rules`.

Here's the relevant snippet of my neovim config:
```lua
vim.lsp.config('ty', {
  settings = {
    ty = {
      diagnosticMode = 'workspace',
      experimental = {
        -- this toggle works:
        rename = true,
      },
      rules = {
        -- this one doesn't:
        ['invalid-parameter-default'] = 'ignore',
      },
    },
  },
})
```
Here's a python snippet to trigger the rule I am trying to ignore:
```python
def foo(bar: int = None):
    pass
```

PS: love the project, keep it up 🙏 

### Version

0.0.1-alpha.19 (e9cb838b3 2025-08-19)

---

_Label `configuration` added by @AlexWaygood on 2025-08-21 18:32_

---

_Label `server` added by @AlexWaygood on 2025-08-21 18:32_

---

_Comment by @MichaReiser on 2025-08-22 06:47_

Adding support for configuring ty directly from within the client is something we want to add but had to deprioritize in favor of supporting more language features. But we'll come back to it. 

This is related to https://github.com/astral-sh/ty/issues/786

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:57_

---
