```yaml
number: 6772
title: Exclude pragma comments from measured line width
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-22T14:32:06Z
updated_at: 2023-09-01T07:39:16Z
url: https://github.com/astral-sh/ruff/issues/6772
synced_at: 2026-01-12T15:54:46Z
```

# Exclude pragma comments from measured line width

---

_@MichaReiser_

Exclude trailing expression end-of-line comments to be included in the measured line width (`type: `, `pyright:` `pylint:`, `noqa`, but NOT `nocoverage` because it is a trailing clause comment that isn't sensitive about its positioning

---

_Label `formatter` added by @MichaReiser on 2023-08-22 14:32_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:32_

---

_Renamed from "Exclude trailing expression end-of-line comments to be included in the measured line width (`type: `, `pyright:` `pylint:`, `noqa`, but NOT `nocoverage` because it is a trailing clause comment that isn't sensitive about its positioning" to "Exclude pragma comments from measured line width" by @MichaReiser on 2023-08-22 14:33_

---

_Removed from milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 15:10_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 15:10_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 15:10_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 15:10_

---

_Comment by @cnpryer on 2023-08-27 00:44_

Curious of what a solution would look like. These are line suffixes, so would we want to just match on the content that'd we want to ignore? Or is there a more robust way to go about this?

---

_Comment by @MichaReiser on 2023-08-27 08:55_

> Curious of what a solution would look like. These are line suffixes, so would we want to just match on the content that'd we want to ignore? Or is there a more robust way to go about this?

No, matching on the content is what we'll need to do here. Similar to how formatter suppression comments:

https://github.com/astral-sh/ruff/blob/0cea4975fcfe09716c084c0a18d1b4c4cd9b8f05/crates/ruff_python_formatter/src/comments/mod.rs#L173-L193

We'll match on the content and set reserved width to 0 if it is a pragma comment.

---

_Comment by @cnpryer on 2023-08-29 22:17_

Sounds very straight-forward then. I can tackle this after #6901.

Reading through https://github.com/astral-sh/ruff/discussions/6670 :)

---

_Comment by @cnpryer on 2023-08-30 00:32_

IIUC there would be no exceptions based on the *values* of the pragmas. 

So something like this would probably work, right?
```rust
      // Don't reserve width for excluded pragma comments.
      let reserved_width = if ["noqa", "type:", "pylint", "pyright:"]
          .iter()
          .any(|prefix| trimmed.starts_with(prefix))
      {
          0
      } else {
          normalized_comment.text_len().to_u32() + 2 // Account for two added spaces
      };
```

Might need some redundant trims to get `trimmed` unless we refactor some of #6901. Not sure how worth it it is.

[This](https://github.com/cnpryer/ruff/blob/3d32f2cf2d442f72911533cd6ec0e58855539ea3/crates/ruff_python_formatter/src/comments/format.rs#L361-L373) is where my mind is at with this currently.

---

_Comment by @MichaReiser on 2023-08-30 08:12_

That looks about right. I don't think we need to handle non-breaking spaces. I don't even think most pragma comments remain valid if you add a non breaking space.

---

_Comment by @cnpryer on 2023-08-30 13:31_

> I don't even think most pragma comments remain valid if you add a non breaking space.

Interesting. I didn't think that a NBSP would invalidate pragmas. I just tested it with 

```py
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  # noqa
a = []  # type: list[str]
```

Both contain a leading NBSP.

> I don't think we need to handle non-breaking spaces

Is the reasoning just because they become invalid pragmas? I'd think the *intention* is that they're there to be treated as pragmas. I'd think from a consistency point of view it might make sense to treat their formatting the same.

I'm imagining a case where a user realizes their pragmas are invalid due to NBPS and corrects them. They might need to address both their pragmas' reinstated functionality **and** now potential formatting changes.

---

_Comment by @MichaReiser on 2023-08-30 13:39_

> Interesting. I didn't think that a NBSP would invalidate pragmas. I just tested it with

What was the result. Did the typechecker pick up the type or not? 

> I'm imagining a case where a user realizes their pragmas are invalid due to NBPS and corrects them. They might need to address both their pragmas' reinstated functionality and now potential formatting changes.

I can see how this is annoying. But I'm not too worried because I don't know how you accidentally end up with a NBPS.  That's why I think this is rare. 

---

_Comment by @cnpryer on 2023-08-30 13:45_

> What was the result. Did the typechecker pick up the type or not?

It did not pick up the type. You called it.

```py
# Lead by a NBSP
a = []  # type: list[str]

# Lead by a space
b = []  # type: list[str]
```
```
❯ mypy scratch.py
scratch.py:2: error: Need type annotation for "a" (hint: "a: List[<type>] = ...")  [var-annotated]
Found 1 error in 1 file (checked 1 source file)
```

If you throw the NBSP into `b`'s pragma you get

```
scratch.py:2: error: Need type annotation for "a" (hint: "a: List[<type>] = ...")  [var-annotated]
scratch.py:5: error: Need type annotation for "b" (hint: "b: List[<type>] = ...")  [var-annotated]
Found 2 errors in 1 file (checked 1 source file)
```

> I can see how this is annoying. But I'm not too worried because I don't know how you accidentally end up with a NBPS. That's why I think this is rare.

Yea this is what I've been trying to understand. In all transparency I wasn't too familiar with NBSP prior to working on this stuff. So I'd have to look into them more to gauge it.

---

_Comment by @MichaReiser on 2023-08-30 13:50_

> Yea this is what I've been trying to understand. In all transparency I wasn't too familiar with NBSP prior to working on this stuff. So I'd have to look into them more to gauge it.

Me neither. I only used `&nbsp;` when writing HTML but that's a long time ago... I don't even know how to insert a NBSP in the editor :laughing: 

---

_Comment by @cnpryer on 2023-08-30 21:54_

So I've looked into this more. Tools like `mypy` and `flake8` will invalidate pragma comments if they are lead by a NBSP (doesn't matter how many). However `ruff` doesn't invalidate the pragmas. Since #5554 these comments are stripped of their whitespace regardless of the *kind* of whitespace that is there.

My preliminary thoughts are that I actually prefer `ruff`'s current behavior. Regardless of how the NBSP ends up there, you're still *intentionally* marking it with a pragma. In fact, I'd think that having a NBSP (for some kind of rendering purposes) actually makes sense. A pragma comment is inherently tied to the comment prefix `#`.

A NBSP is meant to attach two elements together **always** and not break the content. So while this might be rare, it's not *unreasonable* (IMO).

[Here](https://discord.com/channels/1039017663004942429/1073384953741574217/1146453786202751057) is the relevant Discord thread.

[Here](https://play.ruff.rs/00430b93-f281-453a-b2bb-fc8be32d4ad4) is a playground script I've been messing with to better understand NBSP in the context of `ruff` and other tools.

I'll defer to your judgement, but if we decide to move forward with invalidating them then maybe we need to consider updating `ruff` to align with other tools. I have a commit ready:

```diff
diff --git a/crates/ruff/src/noqa.rs b/crates/ruff/src/noqa.rs
index 36cf5105a..62801801c 100644
--- a/crates/ruff/src/noqa.rs
+++ b/crates/ruff/src/noqa.rs
@@ -11,7 +11,7 @@ use log::warn;
 use ruff_text_size::{Ranged, TextLen, TextRange, TextSize};
 
 use ruff_diagnostics::Diagnostic;
-use ruff_python_trivia::indentation_at_offset;
+use ruff_python_trivia::{indentation_at_offset, PythonWhitespace};
 use ruff_source_file::{LineEnding, Locator};
 
 use crate::codes::NoqaCode;
@@ -53,7 +53,7 @@ impl<'a> Directive<'a> {
             let mut comment_start = noqa_literal_start;
 
             // Trim any whitespace between the `#` character and the `noqa` literal.
-            comment_start = text[..comment_start].trim_end().len();
+            comment_start = text[..comment_start].trim_whitespace_end().len();
 
             // The next character has to be the `#` character.
             if text[..comment_start]
```

---

_Closed by @MichaReiser on 2023-09-01 06:34_

---

_Comment by @ndevenish on 2023-09-01 07:38_

> I don't know how you accidentally end up with a NBPS. That's why I think this is rare.

FWIW I _constantly_ type unintentional NBSP. On UK mac Keyboard, Alt-3 is #, and Alt-Space is NBSP. If I'm typing fast sometimes these can blur together (hitting space before releasing the alt). However, I always catch them now, because I set up my IDE to highlight NBSP. I'm not sure how often other people encounter or notice this problem, though.

---
