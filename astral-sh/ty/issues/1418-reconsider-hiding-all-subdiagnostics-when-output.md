```yaml
number: 1418
title: "Reconsider hiding all subdiagnostics when `--output-format=concise` is specified"
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-10-23T13:33:38Z
updated_at: 2025-11-14T08:25:11Z
url: https://github.com/astral-sh/ty/issues/1418
synced_at: 2026-01-10T02:06:25Z
```

# Reconsider hiding all subdiagnostics when `--output-format=concise` is specified

---

_Issue opened by @AlexWaygood on 2025-10-23 13:33_

Many of our existing diagnostics are unactionable when using `--output-format=concise`. For example:

```
~/dev [1] % cat foo.py                      
sum(1, 2)
~/dev % uvx ty check foo.py --output-format=concise
Checking ------------------------------------------------------------ 1/1 files
foo.py:1:1: error[no-matching-overload] No overload of function `sum` matches arguments
Found 1 diagnostic
```

Some other type checkers do a much better job here: mypy defaults to concise diagnostics (you have to enable verbose annotations using the `--pretty` flag), but even with mypy's default, concise, output format it displays subdiagnostics that tell you what the available overloads are:

```
~/dev [1] % uvx mypy foo.py                            
foo.py:1: error: No overload variant of "sum" matches argument types "int", "int"  [call-overload]
foo.py:1: note: Possible overload variants:
foo.py:1: note:     def sum(Iterable[bool], /, start: int = ...) -> int
foo.py:1: note:     def [_SupportsSumNoDefaultT: _SupportsSumWithNoDefaultGiven] sum(Iterable[_SupportsSumNoDefaultT], /) -> _SupportsSumNoDefaultT | Literal[0]
foo.py:1: note:     def [_AddableT1: SupportsAdd[Any, Any], _AddableT2: SupportsAdd[Any, Any]] sum(Iterable[_AddableT1], /, start: _AddableT2) -> _AddableT1 | _AddableT2
Found 1 error in 1 file (checked 1 source file)
```

There are many situations where it's implausible to put all information to make a diagnostic actionable on a single line; I think there will often be situations when important information needs to be shunted to a subdiagnostic. We should add a way to specify for some subdiagnostics that they should be displayed (in a concise format) even if `--output-format=concise` is specified on the CLI.

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-23 13:33_

---

_Comment by @sharkdp on 2025-10-27 08:32_

I agree: our concise diagnostics are too concise in many situations. Sometimes I look at the ecosystem diff and don't even understand what the problem is:
```py
# lots of code

assert_type(x, int)  # Argument does not have asserted type `int`
```
(ok, cool, but what is the actual type of `x`? — it's only shown in the "full" message)


I think there's probably no good one-size-fits-all heuristic here (should the info text always be included or not? should subdiagnostics be extra sentences or not? should annotations be added somehow?). We might need to actually design the "full" and the "concise" diagnostic *separately* for the best UX. We wouldn't need to do that for *every* lint rule that we have. But some rules are more important than others. And it might be a valuable investment to have a bespoke construction of the concise message for those cases.

---

_Comment by @carljm on 2025-10-29 19:55_

I suspect that as a default rule we should show all sub-diagnostics in concise mode. The bulk of the "concision" comes from eliminating code frames.

---

_Comment by @AlexWaygood on 2025-10-29 19:58_

> I suspect that as a default rule we should show all sub-diagnostics in concise mode.

We should also omit the

```
info: rule `foo-bar-baz` enabled by default
```

subdiagnostics that are added to every diagnostic, when we're in concise mode. But yes, I agree that the default for subdiagnostics should probably be for them to be shown in concise mode, unless configured otherwise (and we could configure it otherwise for these specific subdiagnostics)

---

_Comment by @BurntSushi on 2025-10-30 00:00_

Do we have a good understanding of when and how often the concise output format is being used? I acknowledge that we use it heavily for the ecosystem diff check, but where else?

My main concern is mostly with making the construction of diagnostics more complicated/difficult by exposing "concise or not" in that API. I think it's important that diagnostics are really easy to construct without much mental burden.

(My concern is mitigated by @carljm's suggestion, FWIW.)

---

_Comment by @sharkdp on 2025-10-30 00:27_

I'm mostly concerned by the fact that we use the concise format for diagnostics in the LSP. So it might actually be the format that is seen more often by users than our pretty/"full" format?

---

_Comment by @AlexWaygood on 2025-10-30 00:28_

For me personally (and this is only my personal perspective!), I find our current default output format much too verbose for CLI usage if there are more than even one or two diagnostics. Usually if I'm running a type checker on Python code from the CLI, I want to be able to easily see the different kinds of errors being emitted by the type checker across a codebase and to be able to scroll between different diagnostics. Our current default output format is too verbose for either of those, and our concise output format is too concise!

I love all the annotations and extra information we add in the VSCode context, where I can use the "click to see full diagnostic" feature to view each diagnostic in isolation in a separate tab, but for me it's just too much right now when using ty from the CLI.

For all our output formats, I do also think we need to think a bit harder about the UX in situations where we're emitting hundreds or thousands of diagnostics on a codebase. Spewing them all onto the terminal is pretty overwhelming, and I think will be quite a negative experience for people using ty for the first time. (First-time users will often have lots of diagnostics, even if they've previously used other type checkers on their codebase!)

---

_Comment by @BurntSushi on 2025-10-30 00:41_

> I'm mostly concerned by the fact that we use the concise format for diagnostics in the LSP. So it might actually be the format that is seen more often by users than our pretty/"full" format?

In my LSP setup, I can only see one line anyway. So would adding multiple lines for each concise diagnostic end up helping?

(I wouldn't be too surprised to discover that my setup is hampered in some way.)

OK, @AlexWaygood's comment answers that. I guess there is a way to see the full diagnostic in VS Code. But even then, wouldn't you want the full diagnostic and not the concise one?

> For all our output formats, I do also think we need to think a bit harder about the UX in situations where we're emitting hundreds or thousands of diagnostics on a codebase. Spewing them all onto the terminal is pretty overwhelming, and I think will be quite a negative experience for people using ty for the first time. (First-time users will often have lots of diagnostics, even if they've previously used other type checkers on their codebase!)

I'm thinking about rustc here, which has always very heavily relied on the verbose diagnostic output format. I've never used its concise format and I don't ever see anyone else doing so. Is there a fundamental difference here between rustc and ty? Is it that we expect ty to be emitting lots of diagnostics that folks won't seek to actually fix? I guess in rustc's case, you can't really move forward until you satisfy the compiler. But that's not really the case for Python.

Another lever to pull on here is whether it makes sense to limit output to the first N diagnostics by default. If we're emitting thousands of diagnostics, for example, then that surely isn't useful (unless you have a specific use case where you really want to see everything). So maybe it would be better to limit it by default.

Is it possible we (as in, the people working on ty) have a different use case than typical end users? I can see us caring a lot more about things _in aggregate_, "how many lints are this kind and how many are that kind," where as maybe end users don't care about that as much?

---

_Comment by @AlexWaygood on 2025-10-30 00:48_

> I guess in rustc's case, you can't really move forward until you satisfy the compiler. But that's not really the case for Python.

Yeah, I think that's a pretty big difference! A single rustc diagnostic means the project won't run. But you can have a Python project that's been run in production for years, and might even be clean under other, established Python type checkers, that still produces thousands of ty diagnostics.

> Is it possible we (as in, the people working on ty) have a different use case than typical end users? I can see us caring a lot more about things _in aggregate_, "how many lints are this kind and how many are that kind," where as maybe end users don't care about that as much?

No, I think I've very much cared about these things when writing things in Python and using type checkers to check my Python code! Often many similar diagnostics can be caused by a single error, and it's useful to know that do that you can go and fix that error first before bothering with any of the rest of them

---

_Comment by @BurntSushi on 2025-10-30 00:49_

To clarify my position here: I agree and acknowledge that our concise diagnostics are less useful than they could be.

I am fully in favor of any kind of tweak we can make that doesn't require individual diagnostic tailoring. I'm not _opposed_ to individual diagnostic tailoring, but I want to understand whether the use cases we improve with that tailoring are worth that work.

---

_Comment by @AlexWaygood on 2025-10-30 00:53_

> Another lever to pull on here is whether it makes sense to limit output to the first N diagnostics by default. If we're emitting thousands of diagnostics, for example, then that surely isn't useful (unless you have a specific use case where you really want to see everything). So maybe it would be better to limit it by default.

I agree that we may want to do this as well, yes. It might make sense to print some kind of summary at the top (rather than at the bottom) if there are _many_ diagnostics, and then print the remaining output to a pager, rather than printing many thousands of lines of output to the terminal.

---

_Comment by @BurntSushi on 2025-10-30 00:54_

I like the idea of printing a summary.

I also like the idea of just turning on all sub-diagnostics (like @carljm suggested) and seeing how that feels. It might get us to a "good enough" state.

---

_Comment by @AlexWaygood on 2025-10-30 00:57_

Currently _every_ diagnostic has >1 subdiagnostic due to https://github.com/astral-sh/ty/issues/1418#issuecomment-3463638012, so I do think we need a way to ensure that _some_ subdiagnostics can be switched off when `--output-format=concise` is specified. I think it's too much for _every_ diagnostic to take >=2 lines of space on the terminal when `--output-format=concise` is specified.

---

_Comment by @MichaReiser on 2025-10-30 00:58_

I have the same sentiment as @BurntSushi and I question that defaulting to always showing the subdiagnostic is necessary what users want. E.g. I would not want to see every available overload. I'd be more than happy to use my IDE to see the type of the overload or switch to `--output-format=full`.

I also think we shouldn't over-optimize for the case where projects have many diagnostics. Instead, we should provide a way to reduce those diagnostics, either by having a baseline or `--add-noqa` feature. 



---

_Comment by @BurntSushi on 2025-10-30 01:01_

> Currently _every_ diagnostic has >1 subdiagnostic due to [#1418 (comment)](https://github.com/astral-sh/ty/issues/1418#issuecomment-3463638012), so I do think we need a way to ensure that _some_ subdiagnostics can be switched off when `--output-format=concise` is specified. I think it's too much for _every_ diagnostic to take >=2 lines of space on the terminal when `--output-format=concise` is specified.

Yeah but I think we can do this via a general rule and without individual diagnostic tailoring. We can even hack this at first (perhaps with a substring search in diagnostic output rendering) just so we can see what it feels like before doing something more principled.

---

_Comment by @AlexWaygood on 2025-10-30 01:03_

> I'd be more than happy to use my IDE to see the type of the overload

Not all users will use ty inside an IDE. I think it's much more common in Python than in Rust to use a lightweight text editor and more-or-less live inside the terminal rather than live inside an IDE. That obviously doesn't scale to larger Python applications, but lots of Python applications aren't that large; that doesn't mean that you don't want to run a type checker on them.

---

_Comment by @MichaReiser on 2025-10-30 01:09_

> Not all users will use ty inside an IDE. I think it's much more common in Python than in Rust to use a lightweight text editor and more-or-less live inside the terminal rather than live inside an IDE. 

That's where you can use `--output-format=full` ;) 

> Yeah but I think we can do this via a general rule and without individual diagnostic tailoring. We can even hack this at first (perhaps with a substring search in diagnostic output rendering) just so we can see what it feels like before doing something more principled.

There's at least the python version diagnostic (where we inferred the python version from) that probably needs to be suppressed. I also worry that many sub-diagnostics are framed in a way that it may not make sense to show them without the code frame. But I guess that's something we can solve. 

I don't mind exploring this but I suspect that the outcome is that users will ask for a "proper-concise" output. 

---

_Comment by @AlexWaygood on 2025-10-30 01:12_

> I don't mind exploring this but I suspect that the outcome is that users will ask for a "proper-concise" output.

I can't recollect this ever being an issue on the mypy issue tracker, and, as I say, mypy generally prints all its subdiagnostics even as part of its (default) output format. (Not saying there isn't a mypy issue somewhere asking for this -- there may well be. But I can't remember it coming up much at all, from my time triaging mypy issues.)

---

_Comment by @AlexWaygood on 2025-10-30 01:13_

> There's at least the python version diagnostic (where we inferred the python version from) that probably needs to be suppressed.

IDK, even these might be usefully displayed by default even with `--output-format=concise` if they only take up one additional line on the terminal. We only add them in situations where we have strong reason to suspect that the user could get rid of the error by adjusting their configuration to change their inferred Python version.

---

_Comment by @BurntSushi on 2025-10-30 01:17_

I do think there is a fundamental impedance mismatch between concise and "full" output. Particularly where the full output includes the code frame. That code frame is incredibly rich and detailed context that doesn't need to be repeated in the prose of the diagnostic. A good diagnostic should generally keep that prose very short, often relying on the position in the code frame to do a lot of the talking. So if you render the prose _without_ the code frame you lose that context and the resulting prose _can_ become nonsense or at least much more difficult to understand. The extent to which the prose becomes hard to understand might vary greatly from diagnostic to diagnostic.

We could try to write all prose in diagnostics while being mindful that it won't always be shown along with the code frame. IDK how I feel about that.

---

_Comment by @MichaReiser on 2025-10-30 01:21_

An easy way to filter the sub-diagnostics would be by severity. E.g. hide all `note` level diagnostics.

> We could try to write all prose in diagnostics while being mindful that it won't always be shown along with the code frame. IDK how I feel about that.

I share this concern and I think the python version is a good example of this:

```py
Python {version} assumed due to this configuration setting
```

Which will be fairly pointless. 

Unless we say we skip any sub-diagnostics with annotations and only render simple notes. But it definitely makes writing diagnostics trickier because you have to keep all this in mind when authoring them.

---

_Comment by @AlexWaygood on 2025-10-30 01:40_

> I share this concern and I think the python version is a good example of this:
> 
> ```
> Python {version} assumed due to this configuration setting
> ``` 
> 
> Which will be fairly pointless.

Yes, that's a good illustration of the problem. Possible solutions would be to make it possible to have a subdiagnostic be rendered differently depending on the specified output format, or even have an entirely different subdiagnostic be rendered depending on the output format.

Both of these would result in our diagnostic model changing and becoming more complicated, yes. My perspective, however, is that I've found concise output formats very useful when using type checkers on Python code, and I therefore don't think it's an acceptable outcome for us to have a noticeably worse concise output format to existing type checkers such as mypy. What works well for rustc isn't necessarily a great match for Python. If fixing this issue requires us to complicate our current diagnostic model somewhat, for me that's a price I'm willing to pay.

I love our "full" output format and I think it's far better UX than what mypy or pyright are able to provide. However, I don't think that having a much better verbose output format means that we can get away with a noticeably worse concise output format than what they provide.

---

_Comment by @carljm on 2025-10-30 14:44_

Isn't "filename and line number" (maybe we add column numbers too?) the concise-output way of representing the "same information" as the code frame, but concisely? It's certainly a less _accessible_ representation (though that's somewhat mitigated by shells/editors supporting filename:lineno as a click target), but it's not like the information is absent.

---

_Comment by @carljm on 2025-10-30 14:45_

> I suspect that the outcome is that users will ask for a "proper-concise" output.

I think the prior experience of mypy and pyright suggests that this is not really a risk.

What users probably will ask for (either way) is machine-parseable output, but I don't think "concise format restricted to one line per diagnostic" is the right answer to that request.

---

_Comment by @BurntSushi on 2025-10-30 15:00_

> Isn't "filename and line number" (maybe we add column numbers too?) the concise-output way of representing the "same information" as the code frame, but concisely? It's certainly a less _accessible_ representation (though that's somewhat mitigated by shells/editors supporting filename:lineno as a click target), but it's not like the information is absent.

The code frames are richer than that. They aren't just a subset of lines from a source file. They also include position information into those lines (which I guess you can get with column numbers, although there may be multiple such column _ranges_ per line) along with annotation text (which seems harder to represent in a concise output mode). That information density influences (or at least, I think it ought to) how one writes the rest of the messages in the diagnostic.

I agree that the context in the concise output doesn't need to be completely absent, but I'm thinking about _quality_ here and the extent to which we're willing to abide "worse" diagnostics in the concise output format.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:25_

---
