```yaml
number: 784
title: Gray out unreachable code
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-07-08T16:02:47Z
updated_at: 2025-11-14T08:39:44Z
url: https://github.com/astral-sh/ty/issues/784
synced_at: 2026-01-10T02:06:24Z
```

# Gray out unreachable code

---

_Issue opened by @MichaReiser on 2025-07-08 16:02_

This can be done by creating a diagnostic with the `UnnecessaryCode` tag. 

Our reachability infrastructure should be powerful enough to support this but recording reachability constraints for every node is probably too expensive and we have to do it at the basic block level instead. 

The diagnostic reported by ty should also cover the maximum span that's unreachable instead of having a diagnostic for each node (which would be very verbose. It's worth looking into how other type checker handle this). 

There's an UX question for what we want to do about code that is unreachable with the current configuration settings (e.g. for a newer python version). Ideally, this code gets grayed out in the LSP but reporting a diagnostic in the CLI might be annoying (but can be useful to detect code that's no longer revelant after upgrading to a newer Python version)

---

_Label `server` added by @MichaReiser on 2025-07-08 16:02_

---

_Comment by @erictraut on 2025-07-08 16:24_

For reference, [here is the code](https://github.com/microsoft/pyright/blob/5ba9dcf9802382b4db529221ba353ae3edc597ad/packages/pyright-internal/src/analyzer/checker.ts#L2832) in pyright that does this. It works roughly like you surmised. Once it discovers an unreachable statement, it extends the range to the last statement in the block.

One consideration: If you plan to also emit a "real" diagnostic (an error or warning) for unreachable code in addition to emitting a "tagged hint" diagnostic, you need to make sure that these diagnostics use different ranges. If the ranges are identical, clients like VS Code will drop (i.e. ignore) the tagged hint. To work around this, pyright adopts a trick from the TypeScript language server. It uses the full range for the tagged hint but uses a much shorter range (corresponding to the the first token of the first statement) for the "real" diagnostic.

Here's how that appears in VS Code:

<img width="227" height="96" alt="Image" src="https://github.com/user-attachments/assets/9b789113-7fc2-4245-b4ba-7af6bbb6dfe2" />

---

_Comment by @sharkdp on 2025-07-08 17:49_

Just for reference: We have existing tests for this here: https://github.com/astral-sh/ruff/blob/fda188953f9b739e4baf42bad1e88e812d1f1d78/crates/ty_python_semantic/resources/mdtest/unreachable.md?plain=1#L8-L104

> There's an UX question for what we want to do about code that is unreachable with the current configuration settings (e.g. for a newer python version). Ideally, this code gets grayed out in the LSP but reporting a diagnostic in the CLI might be annoying (but can be useful to detect code that's no longer revelant after upgrading to a newer Python version)

Tests for this are also already available: https://github.com/astral-sh/ruff/blob/fda188953f9b739e4baf42bad1e88e812d1f1d78/crates/ty_python_semantic/resources/mdtest/unreachable.md?plain=1#L106-L203

---

_Comment by @erictraut on 2025-07-08 18:50_

> There's an UX question for what we want to do about code that is unreachable with the current configuration settings (e.g. for a newer python version).

For reference, pyright distinguishes between three types of [unreachable code](https://microsoft.github.io/pyright/#/type-concepts-advanced?id=reachability):
1. Structural: due to control flow statements like `return` and `raise`
2. Static conditions: based on the use of [statically-evaluated conditional expressions](https://microsoft.github.io/pyright/#/type-concepts-advanced?id=reachability) like version or platform checks, `TYPE_CHECKING`, etc.
3. Type analysis: based on full type analysis

All three of these result in "grayed out" text in the editor, but only categories 1 and 3 generate "unreachable code" diagnostics. I only recently added the "unreachable code" diagnostic, so I haven't received much feedback on it yet. This check is not enabled by default, even in pyright's strict mode, because there are many legitimate cases where unreachable code appears in code.

---

_Comment by @AlexWaygood on 2025-07-08 18:56_

> This check is not enabled by default, even in pyright's strict mode, because there are many legitimate cases where unreachable code appears in code.

That's interesting. My instinct is that we should at least emit diagnostics by default for category (1). The reason is the amount of confusion I've seen among users of type checkers -- on the mypy issue tracker, on Stack Overflow, and similar -- due to the fact that many errors which would normally be caught by a type checker are (for very good reasons) not emitted on code in unreachable branches. It's often easy to introduce code that would never be reachable by accident; I think users deserve to know that we won't be able to provide a good analysis of those regions of their codebase.

---

_Comment by @erictraut on 2025-07-08 19:51_

Yeah, there are many directions ty could go here. One could even make the argument that category 1 isn't something that a type checker should ever report. It's arguably more of a linter concern.

---

_Comment by @DetachHead on 2025-10-05 11:02_

IMO unreachable code is almost always unintentional (with the exception of `TYPE_CHECKING` blocks), and therefore should be an error, not just a greyed out hint.

regarding python version/platform checks, if the type checker considers those unreachable when they aren't, that often means the type checker is wrong. for example:

```py
if sys.version_info < (3, 13):
    print("you are on an old version of python :(") # error: unreachable
else:
    print("you're on the latest python version :)")
```
the fact that the developer wrote such a check means they expect their code to be run on more than one python version. but i don't think any of the type checkers currently support this (#1309) 

sidenote: it also seems that unlike [other type checkers](https://basedpyright.com/?pythonVersion=3.13&typeCheckingMode=all&code=JYWwDg9gTgLgBAZwJ4IFDuAM0SgdANwFMoFgIA7AfWHMwjgB44AKAZgBo4BGVgSgC5UcYdzgBqOACJJcAMRxiUaPzhRCkWAFVyagIYBjABa6ARgBtCLBPSxwArjsIHj5y-ogATSwHddCcgDk8LrkClBKUJwwhsAIqg4woG5QfoZw3hB2Zh5wAOb07uAWMIRmSPbkXiX6JR68QA), [ty is able to type check unreachable code](https://play.ty.dev/b5064cbc-d00d-43d1-ba51-7fc520bc72ab). i'm not sure if this is intentional (i hope it is) or if this functionality will go away once ty is able to detect unreachable code, but if the latter is the case, that means it's even more important to have the option to report unreachable code as an error because otherwise invvalid code that would normally be caught by the type checker will go undetected.

---

_Comment by @AlexWaygood on 2025-10-05 11:10_

> sidenote: it also seems that unlike [other type checkers](https://basedpyright.com/?pythonVersion=3.13&typeCheckingMode=all&code=JYWwDg9gTgLgBAZwJ4IFDuAM0SgdANwFMoFgIA7AfWHMwjgB44AKAZgBo4BGVgSgC5UcYdzgBqOACJJcAMRxiUaPzhRCkWAFVyagIYBjABa6ARgBtCLBPSxwArjsIHj5y-ogATSwHddCcgDk8LrkClBKUJwwhsAIqg4woG5QfoZw3hB2Zh5wAOb07uAWMIRmSPbkXiX6JR68QA), [ty is able to type check unreachable code](https://play.ty.dev/b5064cbc-d00d-43d1-ba51-7fc520bc72ab). i'm not sure if this is intentional (i hope it is) or if this functionality will go away once ty is able to detect unreachable code

We already have the capability to detect unreachable code internally, and we already suppress many diagnostics in code blocks we understand as unreachable (to prevent false-positive errors that would occur when type-checking unreachable code).

As you've spotted, we don't currently suppress _all_ diagnostics in unreachable blocks; there are some diagnostics that we think we can reliably check for even in unreachable blocks. It remains to be seen whether this is sustainable, though, as we're still coming across situations where we can't reliably type-check code in unreachable blocks, and where we'll therefore likely need to suppress our diagnostics more aggressively. See https://github.com/astral-sh/ty/issues/1165 for a recent example.

---

_Comment by @sharkdp on 2025-10-06 07:47_

> sidenote: it also seems that unlike [other type checkers](https://basedpyright.com/?pythonVersion=3.13&typeCheckingMode=all&code=JYWwDg9gTgLgBAZwJ4IFDuAM0SgdANwFMoFgIA7AfWHMwjgB44AKAZgBo4BGVgSgC5UcYdzgBqOACJJcAMRxiUaPzhRCkWAFVyagIYBjABa6ARgBtCLBPSxwArjsIHj5y-ogATSwHddCcgDk8LrkClBKUJwwhsAIqg4woG5QfoZw3hB2Zh5wAOb07uAWMIRmSPbkXiX6JR68QA), [ty is able to type check unreachable code](https://play.ty.dev/b5064cbc-d00d-43d1-ba51-7fc520bc72ab). i'm not sure if this is intentional (i hope it is)

Our current behavior is intentional, yes. The reasoning is described in this test suite: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/unreachable.md#no-incorrect-diagnostics-in-unreachable-code

This test case demonstrates something very close to your example: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/unreachable.md#emit-diagnostics-for-definitely-wrong-code

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:39_

---
