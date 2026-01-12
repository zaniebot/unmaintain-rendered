```yaml
number: 1780
title: Support rewriting relative image URLs to absolute in docstrings?
type: issue
state: open
author: mflova
labels:
  - server
  - wish
assignees: []
created_at: 2025-12-05T17:41:24Z
updated_at: 2025-12-06T18:53:33Z
url: https://github.com/astral-sh/ty/issues/1780
synced_at: 2026-01-12T15:54:25Z
```

# Support rewriting relative image URLs to absolute in docstrings?

---

_@mflova_

It would be great if VS Code could render images embedded in Python docstrings, similar to how Pylance currently does.

As a reference, Pylance currently supports:
- Absolute paths (via `file://`)
- Web-based URLs

It does not support relative paths, as discussed here: [Pylance issue #7755](https://github.com/microsoft/pylance-release/issues/7755).

Is this something that must be done at the level of ty-vscode? Or does it have to be implemented somewhere else? Thanks!

---

_Comment by @MichaReiser on 2025-12-05 17:52_

> Is this something that must be done at the level of ty-vscode? Or does it have to be implemented somewhere else? Thanks!

Not sure, that depends on VS Code's markdown support

Reading through the linked issue, this might just work if we detect them:

> This is really a function of how VS code renders the markdown (and not how Pylance generates it). Although maybe we could forcefully put the file:// prefix into the markdown...

Can you share a sample markdown example that currently works in pylance?

---

_Label `server` added by @MichaReiser on 2025-12-05 17:52_

---

_Comment by @mflova on 2025-12-06 18:23_

> > Is this something that must be done at the level of ty-vscode? Or does it have to be implemented somewhere else? Thanks!
> 
> Not sure, that depends on VS Code's markdown support
> 
> Reading through the linked issue, this might just work if we detect them:
> 
> > This is really a function of how VS code renders the markdown (and not how Pylance generates it). Although maybe we could forcefully put the file:// prefix into the markdown...
> 
> Can you share a sample markdown example that currently works in pylance?

I am a bit confused here. I tried with `ty` LSP and the image seems to be rendering properly

<img width="566" height="388" alt="Image" src="https://github.com/user-attachments/assets/53027158-b267-4642-95b0-45c1be93db3f" />

Maybe I did something wrong back then when I tried. But now I can get the very same functionalities that `pylance` is supporting (absolute paths and URL-based). So maybe there would be no action here. The only thing that would be great to have is the local relative paths. For private tools or libraries that need to use images, none of the currently supported options are feasible, since you cannot make the image public and an absolute path would be meaningless.

Would there be a possibility to work on this? Or is this something that purely depends on VSCode?

---

_Comment by @Gankra on 2025-12-06 18:30_

Clarifying: you're saying we have feature parity with pylance here?

I agree relative images would be really nice, although I wonder how often the relevant images are bundled in wheels (so this would only benefit docs for the thing you are working on). Also It would be helpful to have an example of a convention that appears in the actual python ecosystem.

---

_Comment by @mflova on 2025-12-06 18:39_

> Clarifying: you're saying we have feature parity with pylance here?
> 
> I agree relative images would be really nice, although I wonder how often the relevant images are bundled in wheels (so this would only benefit docs for the thing you are working on). Also It would be helpful to have an example of a convention that appears in the actual python ecosystem.

If I did not make any mistake, the image-related one should be same as pylance. By mistake I mean using `pylance` when I thought I was using `ty`. But I double checked multiple times

About the convention, I am not sure if we will find any. As far as I am aware, Python LSPs cannot render images from a relative path (at least `pylance` nor `ty`). As a consequence, I am not sure if I can find any "convention". I assume that the two main ways would be 1) a relative path with respect to the root or 2) a relative path with respect to the current file where the docstring is defined

---

_Comment by @Gankra on 2025-12-06 18:46_

Yeah if there's a convention I would assume it's more of a Sphinx thing

(can confirm ty renders images in vscode) ((we don't do anything special we just let the markdown through to vscode))

---

_Label `wish` added by @Gankra on 2025-12-06 18:52_

---

_Renamed from "Support rendering images in docstrings" to "Support rewriting relative image URLs to absolute in docstrings?" by @Gankra on 2025-12-06 18:53_

---
