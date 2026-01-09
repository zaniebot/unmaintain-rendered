---
number: 7744
title: "Feature: Automatically detect and enable virtual environments"
type: issue
state: open
author: aspeddro
labels: []
assignees: []
created_at: 2024-09-27T18:01:37Z
updated_at: 2025-05-03T21:48:38Z
url: https://github.com/astral-sh/uv/issues/7744
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature: Automatically detect and enable virtual environments

---

_Issue opened by @aspeddro on 2024-09-27 18:01_

The OCaml package manager `opam` has the concept of [`switch`](https://ocaml.org/docs/opam-switch-introduction) which are like virtual environments.

The environment (`switch`) is activated when entering the directory. I think `uv` could have this feature. If `uv` finds an environment it should automatically activate it when executing `uv run python` for example.

https://ocaml.org/docs/opam-switch-introduction#selecting-a-switch

---

_Comment by @thomasaarholt on 2025-02-03 06:58_

I think this would be a lovely feature to have, but it is perhaps also treading on the edge of being in-scope? 

I currently use [`mise`](https://mise.jdx.dev/lang/python.html) for for this. It's a general software management tool which has support for [automatic venv activation on cd](https://mise.jdx.dev/lang/python.html#automatic-virtualenv-activation) (with specific support for `uv`). If `uv` supported this feature directly, I probably wouldn't use `uv` for Python.

---

_Comment by @rsyring on 2025-02-21 05:46_

FWIW, I also use mise for this and agree it's probably out of scope for uv.  It would involve things like shell activations, accounting for the various shell's people use, etc.  If you want an idea of the work involved, review how much mise does to get venv activation working and all the edge cases that need to be handled.  Even simple stuff like knowing that the shell is no longer in a given directory and deactivating the venv.

---

_Comment by @maphew on 2025-05-03 21:46_

I don't always want automatic venv activation on entering the dir and would be satisfied with:

 `uv venv` - creates a virtual environment, unchanged from current behaviour

`uv shell` (or `uv activate` or `uv venv activate`) - invokes the correct activate command for current platform/project.

---
