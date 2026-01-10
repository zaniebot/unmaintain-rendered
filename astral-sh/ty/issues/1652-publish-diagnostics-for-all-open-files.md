---
number: 1652
title: Publish diagnostics for all open files
type: issue
state: open
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-11-27T08:51:27Z
updated_at: 2026-01-02T01:49:36Z
url: https://github.com/astral-sh/ty/issues/1652
synced_at: 2026-01-10T01:51:14Z
---

# Publish diagnostics for all open files

---

_Issue opened by @MichaReiser on 2025-11-27 08:51_

ty's LSP does support the PublishDiagnostics notification, however, it only publishes the diagnostics for the changed file. It never pushes diagnostics for related files. 

Let's say you have

`lib.py`:

```py

class Foo:
    name: str
```

`main.py`:

```py
from lib import Foo

foo = Foo()

print(foo.name)
```

Now, you change `Foo.name` to `Foo.name2`. Note, how ty doesn't show any diagnostics in `main.py`. The new diagnostics only show up once you start making changes to `main.py`


We should send the `PublishNotification` for all open documents. However, doing so on every `didChange` notification seems a bit much and possibly expensive. That's why I think the approach taken by Pyrefly to only do this in `didSave` seems reasonable. 

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-27 08:51_

---

_Label `server` added by @MichaReiser on 2025-11-27 08:51_

---

_Comment by @MichaReiser on 2025-11-27 08:52_

I assigned this to the Stable milestone for now but I think we should prioritize this based on user feedback. Most clients support pull diagnostics and/or workspace diagnostics, in which case the PublishDiagnostic notification isn't used at all (except for notebooks)

---

_Comment by @joaotavora on 2025-12-12 09:31_

> but I think we should prioritize this based on user feedback. Most clients support pull diagnostics and/or workspace diagnostics, in which case the PublishDiagnostic notification isn't used at all (except for notebooks)

Two pieces of feedback:

1. Eglot, the built-in client for Emacs doesn't support pull diagnostics yet, as most servers I work with don't support it. It is superior, and I do plan to add it (maybe using ty as a testbed).

   But I think it would be really nice for you not to let go of publishDiagnostics just yet, as not only should ty work with older clients but also my experience of almost 10 years with close to a hundred of servers it is the overwhelming preference of servers and clients such as the one I maintain are well attuned to it (regardless of whether you send publishDiagnostics for all open files or even unopened files).

2. On a tangent, and perhaps more pressing question, in the current implementation of ty, does the publishDiagnostics only come in when the file changes in the file system or a didSave is sent?  Nothing is published on didChange??  Most servers (with notable exception to rust-analyzer) work like that, I think.  Maybe there is a way to activate this?

   I'd like to make `ty` the default preference of my new LSP server multiplexer thing (https://github.com/joaotavora/rassumfrassum) but without the above change, I have to stick to basedpyright, as it still provides a slightly more stable experience.

---

_Comment by @MichaReiser on 2025-12-12 10:07_

ty does support publish diagnostics already (on `didChange`) but it only publishes diagnostics for the changed file. This is not sufficient if you have `lib.py` and `main.py` where you make a change in `lib.py` but there's now a typing error in `main.py` because of it. This is what this issue is about. 

It's our plan to fix this as part of ty's stable release

---

_Comment by @joaotavora on 2025-12-12 10:52_

> ty does support publish diagnostics already (on didChange) but it only publishes diagnostics for the changed file.

Aha.  I had a better look at the JSONRPC conversation and I can confirm this.  However, something odd happens (which was why I thought id didn't do this).  The `:version` field on these publish diagnostics is always "one behind".  So a newly opened file has version 0, then the client makes a tiny change and sends a new `didChange` and says `"version": 1` in the `textDocument` section.  `ty` duly follows up with a `textDocument/publishDiagnostics`, but for `"version": 0`.  Should be version 1!!   I'm using `INFO Version: 0.0.1-alpha.32` if it helps.  Sorry to shoehorn a bug report here.

> This is not sufficient if you have lib.py and main.py where you make a change in lib.py but there's now a typing error in main.py because of it. This is what this issue is about.  It's our plan to fix this as part of ty's stable release

That's great, and very nice.  Not a showstopper though.  A number of servers have this current behavior IIRC.

---

_Comment by @MichaReiser on 2025-12-12 11:13_

Oh nice, find. I should have a fix any minute :)

---

_Comment by @joaotavora on 2025-12-14 22:13_

Hello again @MichaReiser and sorry in advance if I keep hijacking this issue with tangents.  Eglot now supports "pull diagnostics".  I found that `ty server` is much snappier in this mode (maybe just an impression, but why would that be? ¯\_(ツ)_/¯) 

Anyway  I was expecting this mode to address precisely the use case you described in the beginning of this issue, which is to provide diagnostics for documents directly affected by a change to a given document.

As far as I can read in the LSP spec, there is the 'relatedDocumentSupport' client sub-capability, but no server that I know consults it or does anything useful with it.  

Is supporting this part of the plans for the `textDocument/diagnostics` request, too, or only `textDocument/publishDiagnostics`?

---

_Comment by @MichaReiser on 2025-12-15 07:01_

> As far as I can read in the LSP spec, there is the 'relatedDocumentSupport' client sub-capability, but no server that I know consults it or does anything useful with it

No, we don't plan to support this unless there's clear evidence that clients need it. The reason is that we actually don't have that information. There are ways we could compute it but it's non-trivial. VS Code doesn't need it, it simply re-pulls diagnostics for all open files when the server announces `interFileDependencies: true`. I think the same is true for Zed. 

ty also supports workspace diagnostics, which is closer to the traditional push diagnostics where the server pushes new diagnostics across the entire workspace. 

> Anyway I was expecting this mode to address precisely the use case you described in the beginning of this issue, which is to provide diagnostics for documents directly affected by a change to a given document.

Can you say more about what you tried and what isn't working? Ideally, in a separate issue because this is unrelated to push diagnostics.

---

_Comment by @joaotavora on 2025-12-15 09:42_

On Mon, Dec 15, 2025 at 7:01 AM Micha Reiser ***@***.***>
wrote:

> As far as I can read in the LSP spec, there is the
'relatedDocumentSupport' client sub-capability, but no server that I know
consults it or does anything useful with it
>
> No, we don't plan to support this unless there's clear evidence that
clients need it. The reason is that we actually don't have that
information. There are ways we could compute it but it's non-trivial. VS
Code doesn't need it, it simply re-pulls diagnostics for all open files
when the server announces interFileDependencies: true. I think the same is
true for Zed.

I naively expected the language server to maintain a dependency graph
between files.  Not only do I not care about VSCode (as you may or may not
expect), I think brute-forcing this from the client is pretty inefficient.
clangd seems to maintain this, for example.  It publishes diagnostics for
affected files and affected files only (though only on didSave).

> Can you say more about what you tried and what isn't working?

Not much more to add.  It'd say it's fairly on-topic, it matches EXACTLY
the case which _you_ described in the opening post, down to the file names
:-)  I expected a breaking change to lib.py followed by a
`textDocument/diagnostic` of lib.py to bring in "diagnostics for related
files", just as you put it.  Naively, I expected this to come in a
'relatedDocuments' of the response to the pull.

>  Ideally, in a separate issue because this is unrelated to push
diagnostics.

Sorry, wouldn't know where to start.  Feel free to convert me into a
discussion, another issue (it's what I do in my repos).  Or just ignore me
:-)

Happy holidays,
João


---

_Comment by @MichaReiser on 2025-12-15 10:30_

>  I think brute-forcing this from the client is pretty inefficient.
clangd seems to maintain this, for example.  It publishes diagnostics for
affected files and affected files only (though only on didSave).


It's pretty okay. The only thing clients have to do is request new diagnostics for all open files. You can add a little debounce for the files that aren't focused and should get very good performance. If that's not sufficient, the solution here really is workspace diagnostics where the server pushes new diagnostics. 

> Not much more to add.  It'd say it's fairly on-topic, it matches EXACTLY
the case which _you_ described in the opening post, down to the file names
:-)  I expected a breaking change to lib.py followed by a
`textDocument/diagnostic` of lib.py to bring in "diagnostics for related
files", just as you put it.  Naively, I expected this to come in a
'relatedDocuments' of the response to the pull.

The expected flow here without related document support but with `interFileDependencies` is that the client requests diagnostics for `main.py`, 

---

_Comment by @joaotavora on 2025-12-15 11:57_

Micha Reiser ***@***.***> writes:

> *
> MichaReiser left a comment (astral-sh/ty#1652)
>
>  I think brute-forcing this from the client is pretty inefficient.
>  clangd seems to maintain this, for example. It publishes diagnostics for
>  affected files and affected files only (though only on didSave).
>
> It's pretty okay. The only thing clients have to do is request new diagnostics for all open files. You can add a little
> debounce for the files that aren't focused and should get very good performance. If that's not sufficient, the solution
> here really is workspace diagnostics where the server pushes new
> diagnostics.

My guess is the level of okayness would depend on how many files are
open...  A "little debounce" is high complexity for marginal reward, as
Emacs users frequently have a window that lists diagnostics in visited
files, focused, visible or not.  Falling back to workspace diagnostics
(if the server even supports them) is admitting defeat, likely bringing
in loads of uninteresting information.  If this is how it's supposed to
work, it makes sense for me now that clangd hasn't moved to pull
diagnostics, and my opinion is that it's much better to use its
exclusive knowledge of the dependency graph to trace the implication of
a given document change.  In fact I don't see any other way you're going
solve this issue as you describe it.

> The expected flow here without related document support but with
> interFileDependencies is that the client requests diagnostics for
> main.py,

Okay, will do that.  Possibly when you implement this issue, I'll turn
off diagnostic pulls for 'ty' entirely and that should solve things
nicely.


---

_Comment by @ficd0 on 2025-12-19 20:12_

> Most clients support pull diagnostics and/or workspace diagnostics, in which case the PublishDiagnostic notification isn't used at all (except for notebooks)

Chiming in here: [kakoune-lsp](https://github.com/kakoune-lsp/kakoune-lsp) is to Kakoune what eglot is to Emacs, and it also doesn't support pull diagnostics. I'm actually not sure about Helix, but I suspect that may be the case too.

The suggestion to push on `didSave` but not `didChange` seems reasonable. Perhaps pushing on `didChange` could be enabled via a config option if there's a good reason for it?

---

_Comment by @ficd0 on 2025-12-19 20:51_

I don't mind giving this feature a shot, actually, if it's not being worked on already. I am new to programming in Rust, however, so I would very much appreciate some input on whether my thinking is correct here.

Assuming we're talking about the `ty_server` crate in the `ruff` repository:

It seems like we're [not handling the `didSave` notification](https://github.com/astral-sh/ruff/blob/1a18ada931c3a2de2b11abe3f01461c6032e7503/crates/ty_server/src/server/api/notifications.rs) from the client, so we'd need to add a handler that's wired to `lsp_types::notification::DidSaveTextDocument` and called from the notification dispatcher

Its `run` implementation would probably just need to iterate all the document handles in the session and call `publish_diagnostics_if_needed` for each of them. 

If I could get some assurance that I'm on the right track here, I'm happy to give this a shot :)

---

_Comment by @MichaReiser on 2025-12-19 20:53_

That sounds about right. 

---

_Label `bug` added by @MichaReiser on 2025-12-31 16:50_

---

_Comment by @ficd0 on 2026-01-02 01:49_

I've opened a PR with my fix :) 

---
