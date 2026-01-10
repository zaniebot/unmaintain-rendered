```yaml
number: 17639
title: Identifying incomplete input errors
type: issue
state: open
author: aneeshdurg
labels:
  - wish
  - parser
assignees: []
created_at: 2025-04-26T04:45:19Z
updated_at: 2025-04-27T09:50:30Z
url: https://github.com/astral-sh/ruff/issues/17639
synced_at: 2026-01-10T11:09:58Z
```

# Identifying incomplete input errors

---

_Issue opened by @aneeshdurg on 2025-04-26 04:45_

This is a feature request to allow Ruff's parser to indicate when an error might be due to incomplete input. This would allow for projects using Ruff as a python parser to identify cases where future input might make the input valid (e.g. in a REPL).

I think in order to do this, it would likely be necessary to add an extra parameter to the `OtherError` error types with a boolean indicating if the error could be resolved by more input - I think in most cases the other error types can simply be matched to check if future input could make the code valid, but it's possible that some other types could use an additional boolean parameter as well. 

I'm not sure if there's a better way to raise a feature request like this - if there is, please let me know!

---

_Comment by @aneeshdurg on 2025-04-26 04:46_

I'm interested in working on the implementation if this is something that others agree is a worthwhile addition to the project!

---

_Label `question` added by @MichaReiser on 2025-04-26 09:42_

---

_Label `question` removed by @MichaReiser on 2025-04-26 09:42_

---

_Label `needs-info` added by @MichaReiser on 2025-04-26 09:42_

---

_Comment by @MichaReiser on 2025-04-26 09:44_

Can you provide some more information on how this feature would be used and how it would work? 

I'm asking because all syntax errors can be fixed by "more input" where more input either requires inserting or deleting some characters. 

This feature would probably also require awareness of the current cursor position because that's where it's most likely that the user will continue to make changes. 

My last question is: What's preventing you from implementing this logic outside of ruff parser?

---

_Comment by @aneeshdurg on 2025-04-26 13:36_

The usecase is building a REPL using ruff as the parser, so "more input" in this case refers only to adding characters. In terms of how it would work, I think it should be relatively straightforward to audit existing places where errors are returned and check if the error was emitted because it hit the end of the token stream while parsing (i.e. Future tokens could have prevented the error) 

As far as detecting this outside of ruff goes, there's a couple scenarios where determining if the error is recoverable is difficult. One is for triple quoted strings without closing quotes which emits the same error as a single quoted string. Determining the type of string that is unclosed requires reimplementing parts of the parser, as it potentially requires scanning the entire input again (of course this is easily resolvable by introducing a more specific error type, like the errors that fstringerror already has). 
In the case of OtherError it's even harder to analyze outside of ruff since it seems like OtherError is used in a few different ways, and while some are recoverable (e.g. No block after a function/class decl) it's less clear if others are. It's possible to check the error string and then retain an external mapping from error messages to errors that might be recoverable depending on some analysis, but that seems less stable than providing this information from ruff which is already available.

Fwiw, cpython's parser has a similar feature in that SyntaxErrors is further subclassed so that errors that could be resolved by more input are easily identified. 

---

_Comment by @MichaReiser on 2025-04-26 14:43_

> The usecase is building a REPL using ruff as the parser, so "more input" in this case refers only to adding characters. In terms of how it would work, I think it should be relatively straightforward to audit existing places where errors are returned and check if the error was emitted because it hit the end of the token stream while parsing (i.e. Future tokens could have prevented the error)

Would that be limited to the end of the input or anywhere in the program? Can you make a specific example with a syntax error and how the REPL would respond in that case

> As far as detecting this outside of ruff goes, there's a couple scenarios where determining if the error is recoverable is difficult. One is for triple quoted strings without closing quotes which emits the same error as a single quoted string. Determining the type of string that is unclosed requires reimplementing parts of the parser, as it potentially requires scanning the entire input again (of course this is easily resolvable by introducing a more specific error type, like the errors that fstringerror already has).
In the case of OtherError it's even harder to analyze outside of ruff since it seems like OtherError is used in a few different ways, and while some are recoverable (e.g. No block after a function/class decl) it's less clear if others are. It's possible to check the error string and then retain an external mapping from error messages to errors that might be recoverable depending on some analysis, but that seems less stable than providing this information from ruff which is already available.


The string node preserves the quote style. You can also look at the token stream to retrieve the quote style.

> Fwiw, cpython's parser has a similar feature in that SyntaxErrors is further subclassed so that errors that could be resolved by more input are easily identified.

Do you have a reference or link for how that's represented and implemented

---

_Comment by @aneeshdurg on 2025-04-27 01:07_

> Do you have a reference or link for how that's represented and implemented

Here's a usage in cpython of `_IncompleteInputError` - this is how python's `code.InteractiveConsole` runs code and knows to wait for more input for multi-line expressions: https://github.com/python/cpython/blob/4f18916c5c28321f363e85ee7ef3ee29bbf79d8e/Lib/codeop.py#L69. (and here's where it gets raised - https://github.com/python/cpython/blob/4f18916c5c28321f363e85ee7ef3ee29bbf79d8e/Parser/pegen.c#L948)
 
An example would be something like parsing `(1+\n` which is incomplete, but if the next line of text was `2)`, it would be a valid python expression. Similarly, things like `def f():`, `class Foo:`,`"""` , or `"\` all would emit something that is an instance of `_IncompleteInputError` when run through cpython's AST compiler. I could explore looking at the TokenStream, but I think there's just a lot of potential cases, and it seems like it would be much cleaner if that information could just be propagated from `ruff` itself, since this is something that can be known during parsing.

Aside from my own usecases, I've also seen from my recent effort to contribute to it, that RustPython could be a consumer of a similar API - https://github.com/RustPython/RustPython/pull/5743

---

_Comment by @aneeshdurg on 2025-04-27 01:47_

Actually, I thought about it a bit, and it's not needed in any case except for `OtherError` since for all other errors you can check the type and location (though it would be nice if there was a distinction between `UnclosedStringError` and `UnclosedTripleQuotedStringError` - there is for f-strings, but not regular strings). But for `OtherError`, it's less clear if it's sufficient to just check location. I'm hesitant to rely on the message within `OtherError` since that doesn't seem to be a part of the stable API so I still think it's worthwhile to have an additional parameter on `OtherError` to identify errors that could be resolved by appending input.

---

_Comment by @MichaReiser on 2025-04-27 09:50_

What we could consider is to remove `OtherError` and instead use a dedicated error code for each parse error. That should then allow to implement a public method indicating if "more tokens" would fix the error.

---

_Label `needs-info` removed by @MichaReiser on 2025-04-27 09:50_

---

_Label `wish` added by @MichaReiser on 2025-04-27 09:50_

---

_Label `parser` added by @MichaReiser on 2025-04-27 09:50_

---
