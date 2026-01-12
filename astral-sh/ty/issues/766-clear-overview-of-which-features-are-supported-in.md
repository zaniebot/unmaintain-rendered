```yaml
number: 766
title: Clear overview of which features are supported in ty
type: issue
state: closed
author: vlashada
labels:
  - documentation
  - question
assignees: []
created_at: 2025-07-04T17:47:26Z
updated_at: 2025-12-15T13:14:39Z
url: https://github.com/astral-sh/ty/issues/766
synced_at: 2026-01-12T15:54:23Z
```

# Clear overview of which features are supported in ty

---

_@vlashada_

### Question

I tried switching fully to ty from pyright, but as stated in the README, it is still early. I'm now planning on running a hybrid between ty and pyright. It would be really useful if there was some page where it clearly shows which features are supported by ty, which are unstable, which are upcoming and which are not yet started. If this was on the website or the README that would make it much easier for people to start testing it and providing feedback.

### Version

_No response_

---

_Label `question` added by @vlashada on 2025-07-04 17:47_

---

_Label `documentation` added by @zanieb on 2025-07-04 17:54_

---

_Comment by @dhruvmanila on 2025-07-07 04:19_

Are you specifically looking for the features that the language server provides or the typing features? For the language server, we've something here: https://github.com/astral-sh/ty-vscode/?tab=readme-ov-file#features but I agree that it'd be useful to have it in the official docs similar to https://docs.astral.sh/ruff/editors/features/.

---

_Comment by @smartinellimarco on 2025-07-09 00:04_

@dhruvmanila is the go to definition feature only in the vscode extension? I'm unable to make it work in nvim or helix

---

_Comment by @dhruvmanila on 2025-07-09 04:23_

> [@dhruvmanila](https://github.com/dhruvmanila) is the go to definition feature only in the vscode extension? I'm unable to make it work in nvim or helix

Currently, ty only supports go to _type_ definition and not go to definition. They're different. In Neovim, that would be `vim.lsp.buf.type_definition`.

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:26_

---

_Comment by @Joe-Zer0 on 2025-08-21 18:31_

@smartinellimarco - would you mind sharing how you setup ty in helix?  Or possibly even add instructions here - https://github.com/helix-editor/helix/wiki/Language-Server-Configurations#python.  Thanks!

---

_Comment by @smartinellimarco on 2025-08-22 13:21_

@Joe-Zer0 It should run OOTB in helix:
- https://github.com/helix-editor/helix/blob/4b40b45527ae28e055d10814575bd7f7a8747826/languages.toml#L131
- https://github.com/helix-editor/helix/blob/4b40b45527ae28e055d10814575bd7f7a8747826/languages.toml#L1010

you may want to add to your `~/.config/helix/languages.toml`:

```
[language-server.ty.config]
ty.experimental.completions.enable = true
ty.diagnosticMode = "workspace"
```

---

_Assigned to @sharkdp by @MichaReiser on 2025-12-05 15:48_

---

_Comment by @sharkdp on 2025-12-15 13:14_

The current status of various type system features can now be tracked in https://github.com/astral-sh/ty/issues/1889.

---

_Closed by @sharkdp on 2025-12-15 13:14_

---
