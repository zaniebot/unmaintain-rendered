```yaml
number: 13883
title: "[red-knot] Jump to definition"
type: pull_request
state: closed
author: rtpg
labels:
  - ty
assignees: []
draft: true
base: main
head: jump-to-definition
created_at: 2024-10-23T06:40:36Z
updated_at: 2024-12-31T07:20:16Z
url: https://github.com/astral-sh/ruff/pull/13883
synced_at: 2026-01-10T20:42:26Z
```

# [red-knot] Jump to definition

---

_Pull request opened by @rtpg on 2024-10-23 06:40_

## Summary

This sets up "jump to definition" functionality in the `red_knot` server.

Here are some challenges in doing this:
- I need to go from a (file, line, row) triple to an AST node where the cursor is
- Given an AST node, I need to look up definitions for them
- Given a definition, I need to figure out the location

I tackled this with a custom trait, `CanLocate`. We can ask implementations of the `CanLocate` trait where their definition is, based on a cursor location. This combines "traverse the AST" with "do the lookup of the definition" because sometimes we'll need to just look up a name in a scope, but for attribute access in particular there's some interplay between types and definition that need to happen so that `a.b.c` is properly done as "find the definition of `c` on the type of `a.b`".

In practice this trait means I can traverse an AST tree, zooming down until I find the smallest node that a cursor is located in.

From there, I have added a couple of methods to help go from an AST node to a `Definition`.

From a `Definition` to a location has proven to be tricky. I have a _most definitely not the right answer_ helper that is simply looking through a hash table to try and find the definition's location based on its hash key (`definition_range`). It's most definitely wrong, and would appreciate some advice there. 

This currently implements "go to definition" based on just names and attribute access (not handling instance attributes just yet, though I believe MRO work is happening in another branch). Import traversal seems to magically work .

Still wanting to clean things up (in particular I think I can simplify some of the `CanLocate` implementations)

## Test Plan

First you need to set up the LSP server. In Emacs I did this with the following with `lsp-mode` (please note I hardcoded the `red_knot` path , you'll have to point to whatever you need)
```
(lsp-register-client
 (make-lsp-client
  :new-connection (lsp-stdio-connection '("/Users/rtpg/proj/ruff/target/debug/red_knot" "server"))
  :activation-fn (lsp-activate-on "python")
  :server-id 'red-knot-ls
  )
 )
```
(in a project's `.dir-locals.el`)
```
((python-mode . (
                 (+format-with . ruff)
                 (lsp-enabled-clients . (red-knot-ls))
                 )))
```

I then was playing around with this file. Some "jump to definition" stuff works, some doesn't.
```
class C:
    a: int = 4

    def __init__(self, a):
        self.a = a

    def foo(self):
        return 0


def f(c: C) -> int:
    return c.a


c: C = C(3)
c.a

c.foo()
```

---

_@rtpg reviewed on 2024-10-23 06:48_

---

_Review comment by @rtpg on `crates/red_knot_server/src/server/api/requests/definition.rs`:7 on 2024-10-23 06:48_

TODO: I should move this to `red_knot_workspace`

---

_@rtpg reviewed on 2024-10-23 06:49_

---

_Review comment by @rtpg on `crates/ruff_db/src/files.rs`:372 on 2024-10-23 06:49_

TODO: rename this based on the signature, decide whether it even makes sense here, and also figure out how to handle the other two cases.

---

_@rtpg reviewed on 2024-10-23 06:50_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/search.rs`:31 on 2024-10-23 06:50_

we hold usizes, LSP server holds u32s. Not sure what the resolution should be.

---

_@rtpg reviewed on 2024-10-23 06:50_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/search.rs`:21 on 2024-10-23 06:50_

Not clear to me if our current system could multiple locations but the LSP can handle multiple definition sites (I imagine in case of ambiguity)

---

_@rtpg reviewed on 2024-10-23 06:51_

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/location.rs`:73 on 2024-10-23 06:51_

I believe I could get away with using `TextRange` or something similar. At the very least, renaming it `CursorPosition` would make it clear-ish what I'm using it for

---

_Label `red-knot` added by @AlexWaygood on 2024-10-23 06:57_

---

_Comment by @MichaReiser on 2024-10-23 12:51_

Thanks for this contribution. This is a very cool feature. 

This is a feature we want long term but it isn't a short-term priority and there are a lot of things that we have to consider: 

* The LSP shouldn't call salsa methods directly. Instead, it should go through a facade (or it panics when another thread changes an input)
* We don't want lsp dependencies in red_knot_python_semantic
* It's unclear to me what the best way is to locate a node. I know that rust analyzer inserts an identifier at the cursor position
* and more...

That's why I don't expect us to get around to reviewing and merging this feature in the short term. That doesn't mean you shouldn't work on it. I just want to avoid frustration if we won't be able to make progress on this feature short term.

---

_Comment by @rtpg on 2024-10-24 01:27_

@MichaReiser yeah, understandable.

 I appreciate how quick you all are for reviews, and believe that's downstream of everyone doing these reviews seriously. So thank you for the expectation setting here. I'll try to avoid this branch becoming a thing people have to consider until the time is right.

I wanted to open this draft PR to publicly state I had something working here (in case someone else was already tackling this), but I am comfortable just keeping this work on-branch until it looks like what people actually want. And if y'all come up with a totally different way to do this and just do it on a clean branch, no harm done (except to my pride 😆 ). I'll still be working through this branch just because _I_ need this functionality for now.

---

_Comment by @github-actions[bot] on 2024-10-24 08:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-12-30 09:26_

Thanks again for your work. I close this PR because it's unlikely that we start working on this feature anytime soon (or review it). But we will pick it back up once we're further along with Red Knot and your work will be a great source of inspiration for it. 



---

_Closed by @MichaReiser on 2024-12-30 09:26_

---

_Comment by @rtpg on 2024-12-31 07:20_

@MichaReiser understood! Looking forward to future work in this space

---
