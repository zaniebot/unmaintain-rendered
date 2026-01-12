```yaml
number: 21459
title: "Limit `eglot-format` hook to eglot-managed Python buffers"
type: pull_request
state: merged
author: thamer
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-11-14T17:34:39Z
updated_at: 2025-11-17T13:52:32Z
url: https://github.com/astral-sh/ruff/pull/21459
synced_at: 2026-01-12T15:57:25Z
```

# Limit `eglot-format` hook to eglot-managed Python buffers

---

_@thamer_

Running `eglot-format` in buffers not managed by Eglot causes a `jsonrpc-error` in Emacs 30. It may also display a `documentFormattingProvider` warning when the server does not support formatting. Add checks for both.

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-14 17:35_

---

_@ntBre reviewed on 2025-11-14 18:52_

---

_Review comment by @ntBre on `docs/editors/setup.md`:411 on 2025-11-14 18:52_

I think another option could be to add the hook with the `LOCAL` argument within a python-mode-hook. Something like:

```suggestion
  (add-hook 'python-mode-hook
            (lambda ()
              (add-hook 'after-save-hook 'eglot-format nil t)))
```

but I haven't tested this. I'm still on lsp-mode rather than eglot and don't use format-on-save.

This could also be combined with the `python-mode-hook` above:

```elisp
  (add-hook 'python-mode-hook
            (lambda ()
              (eglot-ensure)
              (add-hook 'after-save-hook 'eglot-format nil t)))
```

---

_Label `documentation` added by @ntBre on 2025-11-14 18:54_

---

_@thamer reviewed on 2025-11-14 19:15_

---

_Review comment by @thamer on `docs/editors/setup.md`:411 on 2025-11-14 19:15_

That's better. But it should be `python-base-mode-hook` to also work on the tree-sitter mode.

---

_@ntBre reviewed on 2025-11-14 19:17_

---

_Review comment by @ntBre on `docs/editors/setup.md`:411 on 2025-11-14 19:17_

Ah nice, we should fix that on line 403 too then. And maybe 406?

---

_@thamer reviewed on 2025-11-14 20:25_

---

_Review comment by @thamer on `docs/editors/setup.md`:411 on 2025-11-14 20:25_

Done.

---

_Review requested from @ntBre by @MichaReiser on 2025-11-17 07:28_

---

_Comment by @dhruvmanila on 2025-11-17 08:57_

I'd prefer if @ntBre could look at this as I'm not familiar with Emacs plugins :)

---

_Review request for @dhruvmanila removed by @MichaReiser on 2025-11-17 09:01_

---

_@ntBre approved on 2025-11-17 13:49_

Thank you!

I tested with this configuration in an `init.el` file in a temporary directory and:

```shell
emacs --init-directory .
```

Editing a Python file started eglot, and format-on-save was working. Editing a different file type and saving didn't show any errors.

---

_Renamed from "Limit Eglot format to eglot-managed python buffers" to "Limit `eglot-format` hook to eglot-managed Python buffers" by @ntBre on 2025-11-17 13:50_

---

_Merged by @ntBre on 2025-11-17 13:52_

---

_Closed by @ntBre on 2025-11-17 13:52_

---
