---
number: 16504
title: "[red-knot] tracking issue for diagnostic overhaul"
type: issue
state: closed
author: BurntSushi
labels:
  - tracking
  - ty
  - diagnostics
assignees: []
created_at: 2025-03-04T18:06:17Z
updated_at: 2025-05-02T14:14:22Z
url: https://github.com/astral-sh/ruff/issues/16504
synced_at: 2026-01-10T01:22:57Z
---

# [red-knot] tracking issue for diagnostic overhaul

---

_Issue opened by @BurntSushi on 2025-03-04 18:06_

This issue is meant to track work on overhauling Red Knot's diagnostic system. Red Knot's diagnostic system (as of 2025-03-04) was is intentionally simplistic with the knowledge that we would beef it up later. This issue tracks that work.

This issue can be closed when we feel that Red Knot's diagnostic infrastructure is in a state where we can do the following with minimal friction:

* Create diagnostics with multiple code snippets, potentially from different files.
* Diagnostics can have an arbitrary number of optionally labeled annotations on said snippets.
* Diagnostics can have an arbitrary number of additional "sub-diagnostics," like helpful notes or tips, to add clarifying context to the "main" diagnostic.
* It is easy to create new diagnostics, perhaps with a proc macro.
* Some portion of Red Knot diagnostics are _actually_ using the above features in order to gain confidence that the infrastructure put into place serves us well.

# Specific work

Not all of these have issues created for them yet.

* [x] #16505
* [x] #16506
* [x] #16808
* [x] #16809
* ~~[ ] Build a proc macro for easily defining a structured representation of a `Diagnostic`.~~
* [x] Choose some diagnostics in Red Knot to improve with our new data model.

# Data model

This section documents the data model for diagnostics. This isn't fixed in stone, but it's meant to give some guidance on how we think about the data that make up diagnostics.

What *is* a diagnostic? Let’s try a definition: a diagnostic is a collection of information gathered by a *tool* intended for presentation to an end user, and which describes a group of related *deficiencies* in the inputs given to the *tool*. This definition is quite vague and probably not that useful. Instead, we can try to outline the specific kinds of *data* that a diagnostic contains. We’ll call this our *diagnostic data model*, and this data model will be what *defines* a diagnostic in Astral tooling.    

We will start a high level, which requires mentioning things that haven’t been defined yet. We’ll define those in subsequent sections. With that said, it’s worth defining some lower level primitives first:

- **message** - A short end user readable phrase or sentence.
- **span** - A triple of a file path, the file contents and a single contiguous range of characters in the file contents.
- **annotation** - A **span** with a **message**.

### Diagnostic

A diagnostic has *all* of the following things:

- A **unique identifier** that can be converted to an end user visible string. Identifiers should be relatively short. The purpose of an identifier is to distinguish one diagnostic from others and to permit referring to an individual diagnostic in a succinct and unambiguous way. For example, end user bug reports could reference diagnostic ids without copying the entire output. Additionally, diagnostic identifiers could be useful as a means to link end users to more documentation about a diagnostic.
- A single **primary message** meant to be read by end users. The primary message is meant to be a single terse description (usually a short phrase) describing the group of related deficiencies that the diagnostic describes. Stated differently, if only one thing from a diagnostic can be shown to an end user in a particular context, it is the primary message.
- A **severity** that indicates, roughly, the diagnostic’s assumed level of importance to an end user. A severity is an enumeration of the following values:
    - **bug** - The *tool* has found a deficiency with itself, and it should be identified as such to avoid giving the impression to the end user that it was a deficiency with their inputs.
    - **error** - The *tool* has found a definitive deficiency that must be corrected.
    - **warning** - The *tool* has found a deficiency that *likely* warrants attention from an end user.
    - **note** - The *tool* has relevant context to share about a characteristic.
    - **help** - The *tool* has a suggestion to resolve a deficiency.

A diagnostic *optionally* contains any combination of the following things:

- Zero or more **annotations**. When non-zero, at least one *should* be **primary**.
- Zero or more **spans**. (That is, annotations but without a message.) These are useful for when you want to point to a specific thing in a code snippet, but without a message attached to it.
- Zero or more **sub diagnostics**.
- Zero or more **fixes**.
- Zero or one **formatting diff**.

There may be other *optional* properties we want to add to a diagnostic. For example:

- Whether the diagnostic refers to *deprecated* code or techniques. This can be useful in tooling that consumes our diagnostics (like an LSP) for special visual rendering.

### Sub diagnostic

A sub diagnostic is like a diagnostic, but can only contain a subset of information:

- A single parent **diagnostic**.
- A single **primary message**.
- A **severity**.
- Zero or more **annotations**. When non-zero, at least one *should* be **primary**.
- Zero or more **spans**.

Sub diagnostics cannot be arbitrarily nested. Their main purpose is to add clarifying information to a diagnostic, usually with a different **severity**. For example, adding a **note** sub diagnostic to an **error** diagnostic.

### Fix

A **fix** is a change to part of the input identified by a *tool* that is meant to resolve or mitigate an identified *deficiency*. A fix consists of:

- A single **primary message** describing the fix.
- One or more **substitutions**. A substitution is a **span** and a string, where the span identifies the part of the input to delete and the string is the content to replace it with.
- An **applicability** indicating the strength of the fix. This is an enumeration of the following values:
    - **display-only** - The fix is unsafe and should only be displayed for manual application by the user.
    - **unsafe** - The fix is unsafe and should only be applied with user opt-in.
    - **safe** - The fix is definitely what the user intended and can always be applied.

### Formatting Diff

A **formatting diff** is a change to a part of the input as a result of a stylistic reformatting. It is shown to an end user as a *diff* between what was in the input and the result of reformatting. A formatting diff consists of:

- One **substitution**, defined the same as for a **fix**.

**NOTE**: The need for a formatting diff that is distinct from a fix is still somewhat hazy. At least from a data modeling perspective, it seems like they ought to be unified. Whether that’s actually something that’s practical to do in the code is somewhat less clear (to me).

---

_Label `diagnostics` added by @BurntSushi on 2025-03-04 18:06_

---

_Label `red-knot` added by @BurntSushi on 2025-03-04 18:06_

---

_Label `tracking` added by @BurntSushi on 2025-03-04 18:06_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-03-04 18:06_

---

_Referenced in [astral-sh/ruff#16505](../../astral-sh/ruff/issues/16505.md) on 2025-03-04 18:07_

---

_Referenced in [astral-sh/ruff#16241](../../astral-sh/ruff/issues/16241.md) on 2025-03-04 18:10_

---

_Comment by @cretz on 2025-03-07 20:30_

Unsure if accepting feedback here (sorry if not), but one thing I have missed when authoring diagnostics in some analysis libraries is "arbitrary facts" assuming there are not plans to store those elsewhere. These are often cached and/or made available to dependent diagnostics. I understand there would be concern that arbitrary data could be overused, but there is a lot of value in e.g. coloring methods across libraries when building a call graph (though I understand extensibility is not an initial goal, I have a selfish use case to use call graphs to transitively prevent calling thread-blocking primitives in `async def` methods). [.NET](https://learn.microsoft.com/en-us/dotnet/api/microsoft.codeanalysis.diagnostic.properties) and [Go](https://pkg.go.dev/golang.org/x/tools/go/analysis#Fact) provide this kind of data and I have especially used the latter for similar use cases. MyPy's `metadata` for their plugins is only at a type level which was too restrictive.

---

_Comment by @BurntSushi on 2025-03-07 20:47_

Feedback is welcome!

@cretz Can you give some examples? I'm not sure I fully understand what you mean by "arbitrary facts."

---

_Comment by @cretz on 2025-03-07 21:27_

Just arbitrary state that can attach to typed nodes for reading by later diagnostics. I suppose in Rust this would be a map with boxed dyn trait values or something serde-able if you have a cache.

For instance, we had a use case where I had to prevent certain illegal calls in Go from certain types of methods for determinism reasons (e.g. `time.Now()`). So we wrote https://github.com/temporalio/sdk-go/tree/master/contrib/tools/workflowcheck and that is able to use https://pkg.go.dev/golang.org/x/tools/go/analysis#Fact to store, for every method analyzed, whether it made an illegal call. So it could check whether the fact on the method referenced in an invocation was considered illegal and if so mark itself illegal with the facts of the other one. This allowed us to track invalid calls across a call graph no matter how many transitive levels deep. So finally when analyzing the method that disallowed illegal calls (in our cases a "workflow method"), I could build up an illegal call stack/graph when there was a violation however many levels deep using these facts. Obviously this doesn't survive interface indirection (same as it might not here with `typing.Protocol` indirection) and there are circular call graph issues, but the ability for me to store something about a node when analyzing it was the key and is what I consider a "fact".

I am unsure if this applies to diagnostics or some other aspect, but it is specific analyzers that want to store something on a node for itself to read back later when referencing. For my use case in Python, I wanted to be able to color certain known thread-blocking calls as "async unsafe" in the arbitrary storage place (aka a "fact") of those nodes and do the same to any callers thereof. So when analyzing an `async def` function I can look at typed invocations within for the stored data/fact to see if they are considered "async unsafe" (circular call graph issues notwithstanding). I built a PoC in MyPy but had to hack attributes on functions to build this pseudo call graph. I know that cross-function diagnostic data is not a common use case of these kinds of static analysis tools, but in Python I can really only rely on static typing-based analyzers to build the call graphs I need (and mypy, pyright, etc are not extensible enough).

(pardon the wall of text)

---

_Comment by @BurntSushi on 2025-03-08 01:25_

Ah I see, no, that makes sense. I get it. That seems more useful for, say, a plugin mechanism or for someone looking to do their own analysis using an existing system, i.e., a way to "hook" into it somehow. For diagnostics that are hard-wired _into_ the system, probably this sort of general approach to attaching arbitrary data to nodes isn't as useful.

All that said, I'm pretty sure this would definitely be something outside of the scope of the diagnostics work. This seems more like a feature of Red Knot itself. My guess is that we wouldn't consider something like this until we're further along.

---

_Comment by @MichaReiser on 2025-03-08 17:55_

I'm familiar with Roslyn's (C#'s) feature to attach arbitrary data and I agree that it can be useful for analysis runs to store information across analysing different nodes. BiomeJS took a different approach there where it instead would allow you to write a stateful visitor for the analysis of an entire file. It's unclear if that will scale well to multi-file analysis or if an approach as you describe would be better. Either way, it's something that's on my mind but I agree with @BurntSushi, that it better fits into the system orchestrating an analysis and propagating the analysis results through the system rather than the diagnostic system because you may also want to propagate information for nodes that don't have errors (diagnostics) per se.

---

_Comment by @BurntSushi on 2025-05-02 14:14_

We ended up cutting scope on the proc macro here, but otherwise everything that I wanted to get done to overhaul Red Knot's diagnostics here is now done. There's still plenty of other work to do over time:

* Improving individual diagnostics by taking advantage of the new `Diagnostic` API.
* Consider a proc macro. Although I do think one of the side effects of cutting scope on the proc macro is that we tried harder to make the "by hand" API pretty convenient. I think a proc macro could be even more convenient, but I don't really have a good pulse on how important this is.
* A `Diagnostic` doesn't have a way of representing a "fix" yet. That still needs to be added. cc @ntBre 

---

_Closed by @BurntSushi on 2025-05-02 14:14_

---
