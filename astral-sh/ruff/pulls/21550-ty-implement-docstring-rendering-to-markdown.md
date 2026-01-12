```yaml
number: 21550
title: "[ty] Implement docstring rendering to markdown"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/doc-render
created_at: 2025-11-20T22:57:14Z
updated_at: 2025-11-21T15:47:40Z
url: https://github.com/astral-sh/ruff/pull/21550
synced_at: 2026-01-12T15:57:27Z
```

# [ty] Implement docstring rendering to markdown

---

_@Gankra_

## Summary

This introduces a very bad and naive python-docstring-flavoured-reStructuredText to github-flavor-markdown translator. The main goal is to try to preserve a lot of the formatting and plaintext, progressively enhance the content when we find things we know about, and escape the text when we find things that might get corrupt.

Previously I'd broken this out into rendering each different format, but with this approach you don't really need to?

## Test Plan

Lots of snapshot tests, also messed around in some random stdlib modules.


---

_Review requested from @carljm by @Gankra on 2025-11-20 22:57_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-20 22:57_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-20 22:57_

---

_Review requested from @sharkdp by @Gankra on 2025-11-20 22:57_

---

_Review requested from @dcreager by @Gankra on 2025-11-20 22:57_

---

_@Gankra reviewed on 2025-11-20 22:58_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:197 on 2025-11-20 22:58_

This is the worst parser I've written in a long while so if you tell me "Aria for the love of god make a Parser type with methods and enums" I will. Writing it this way was conducive to experimentation and thinking about the format.

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 22:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @Gankra on 2025-11-20 22:59_

I spent a while salivating over the ruff docstring formatter as a basis for this, but I ultimately concluded it would be a nightmare for both codebases to try to share that logic, and it's not *that* much logic.

---

_Label `server` added by @Gankra on 2025-11-20 23:00_

---

_Label `ty` added by @Gankra on 2025-11-20 23:00_

---

_Renamed from "[ty] implement docstring rendering to markdown" to "[ty] Implement docstring rendering to markdown" by @Gankra on 2025-11-20 23:00_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 23:00_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-11-20 23:03_

The main risk of shipping this implementation is that the previous implementation was so wildly conservative that it "couldn't" ever mess up whereas insufficient escaping or getting confused could complete garble things and eat the docs.

On balance though this just looks so much better I think the risk is worth it (and shipping it will get me a lot of bug reports about what to fix :)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:197 on 2025-11-21 07:00_

You want to use `.universal_newlines` here to get proper `\r` support

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:158 on 2025-11-21 07:01_

What are the two formats that you're referring to here?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:270 on 2025-11-21 07:03_

Can you say why this is necessary, given that it gets rendered as markdown?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:269 on 2025-11-21 07:04_

This won't work for code using tab indentation (which our formatter supports). 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:156 on 2025-11-21 07:05_

What happens for docstrings using the numpy or google format? will they still be rendered correctly?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:204 on 2025-11-21 07:06_

I don't think I understand this. Why is it necessary to add trailing whitespace to any line? This seems very undesired?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:184 on 2025-11-21 07:08_

Let's add a link to [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:233 on 2025-11-21 07:10_

https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-code-block

Defaulting to Python doesn't seem unreasonable but we should respect any override

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:249 on 2025-11-21 07:15_

I'd have to play with restructuredText but the way I read this paragraph

> Literal code blocks ([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#literal-blocks)) are introduced by ending a paragraph with the special marker ::. The literal block must be indented

is that what comes after is only a literal block if there's at least one blank line, and text is indented. 

I think your code correctly handles the empty line. But it ends up pushing an empty markdown code block if the next paragraph isn't correctly indented.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:196 on 2025-11-21 07:16_

I'm not sure about mixing the parsing of docstrings with the formatting logic. It seems harder to follow. I'd be inclined to parse the docstring into an intermediate representation and then do a second pass to format it. 

---

_@MichaReiser reviewed on 2025-11-21 07:16_

---

_@Gankra reviewed on 2025-11-21 12:29_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:197 on 2025-11-21 12:29_

I need to signpost that better in the comments but this function assumes `documentation_trim` (in the same file) was already run, so our leading whitespace is only spaces, and our newlines are only `\n` (and unlike ruff we don't need to care about authentically preserving the flavour of whitespace).

---

_@Gankra reviewed on 2025-11-21 12:29_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:158 on 2025-11-21 12:29_

reStructuredText and markdown

---

_@Gankra reviewed on 2025-11-21 12:41_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:270 on 2025-11-21 12:41_

Yes so the idea here is that docstrings really like using indentation for structure that we do not (care to) parse but we also don't want markdown turning into code-blocks. By replacing leading whitespace with `&nbsp;` outside of codeblocks, we preserve the native indent of the docstring and stuff we don't understand still is rendered clearly and pretty.
 
<img width="638" height="864" alt="Screenshot 2025-11-21 at 7 40 49 AM" src="https://github.com/user-attachments/assets/56aa0370-4960-4892-8a9c-58328a4867cf" />


---

_@Gankra reviewed on 2025-11-21 12:42_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:269 on 2025-11-21 12:42_

(another thing handled by `documentation_trim`)

---

_@MichaReiser reviewed on 2025-11-21 12:44_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:158 on 2025-11-21 12:44_

Oh, that's not clear to me from the comment

---

_@Gankra reviewed on 2025-11-21 12:45_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:156 on 2025-11-21 12:45_


<img width="529" height="524" alt="Screenshot 2025-11-21 at 7 44 21 AM" src="https://github.com/user-attachments/assets/2dd7ab55-9858-44f2-84cf-c2b15c4b9ee4" />

<img width="699" height="662" alt="Screenshot 2025-11-21 at 7 44 48 AM" src="https://github.com/user-attachments/assets/ce1f99f4-3c4e-4ed5-bae7-d72badde3b3b" />

<img width="605" height="447" alt="Screenshot 2025-11-21 at 7 45 11 AM" src="https://github.com/user-attachments/assets/1d3ec998-9cf6-4c81-9453-77db03bb9e55" />


---

_@Gankra reviewed on 2025-11-21 12:47_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:204 on 2025-11-21 12:47_

It's part of the approach I'm taking where I want to preserve the wrapping/indentation of the original docstring.

It prevents e.g. `param1` and `param2` from being merged together as a single line in:

```
'param1' -- The first parameter description
'param2' -- The second parameter description
            This is a continuation of param2 description.
'param3' -- A parameter without type annotation
```

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-21 12:48_

---

_@Gankra reviewed on 2025-11-21 12:49_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:249 on 2025-11-21 12:49_

Haha yes you caught me this part of the parser is distinctly janky. I'll tweak it to make it Spec Compliant (although I can't help but wonder if being sloppy will be the name of the game since we need to render whatever we find).

---

_@Gankra reviewed on 2025-11-21 12:51_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:196 on 2025-11-21 12:51_

Dang, ok if the captain of the performance team wants more intermediate allocations and representations who am I to complain 😄 

(I was half expecting to be told to remove some intermediate Strings)

---

_@Gankra reviewed on 2025-11-21 12:52_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:250 on 2025-11-21 12:52_

Found an in-retrospect-obvious place where this is broken: this mangles inline code `__init__` (I kept seeing it without any code markup, so that was my primary concern)

---

_@MichaReiser reviewed on 2025-11-21 12:54_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:196 on 2025-11-21 12:54_

Lol. I'm not too concerned here, given that we don't render markdown strings that frequently and having a more structured representation would ultimately be useful for Ruff too. Although I don't think we should aim to make it reusable for now.

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:270 on 2025-11-21 12:56_

(Even if I had a perfect parser for this format, the only clear way to render this to markdown, imo, is by rendering these blocks as indented)

---

_@Gankra reviewed on 2025-11-21 12:56_

---

_@MichaReiser reviewed on 2025-11-21 13:50_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:270 on 2025-11-21 13:50_

Oh, I think I misread it as *in a code block*. This makes a ton of sense for indents outside code blocks.

---

_@MichaReiser reviewed on 2025-11-21 13:52_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:270 on 2025-11-21 13:52_

I wonder if we should use the unicode code point directly so that we don't rely on the markdown renderer supporting HTML?

---

_Merged by @Gankra on 2025-11-21 15:47_

---

_Closed by @Gankra on 2025-11-21 15:47_

---

_Branch deleted on 2025-11-21 15:47_

---
