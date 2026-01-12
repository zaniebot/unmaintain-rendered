```yaml
number: 12914
title: "[`ruff`] - allow private variables for `RUF006`"
type: pull_request
state: closed
author: diceroll123
labels:
  - rule
  - needs-info
assignees: []
base: main
head: ruf006-allow-private-vars
created_at: 2024-08-16T03:51:36Z
updated_at: 2024-08-27T18:19:53Z
url: https://github.com/astral-sh/ruff/pull/12914
synced_at: 2026-01-12T15:55:42Z
```

# [`ruff`] - allow private variables for `RUF006`

---

_@diceroll123_

## Summary

Changes RUF006 to allow private vars.

I believe this is a reasonable addition, private declarations surely satisfy the underlying needs.

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-08-16 04:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-08-16 06:42_

We should document this pattern and explain the reasoning why we allow it. 

To me it's not entirely clear if allowing it is in the spirit of the rule. I suspect you're coming from rusts `let _ = returns_result()` convention that allows suppressing the `must_use` error?. 

@AlexWaygood any thoughts?

---

_Label `rule` added by @MichaReiser on 2024-08-16 06:42_

---

_@AlexWaygood had review dismissed on 2024-08-16 12:29_

@MichaReiser this makes sense to me. This rule isn't a stylistic rule, it's a correctness rule. You need to have a reference to the `asyncio` task or it will be garbage-collected, which is almost never what you want. From the perspective of Python's garbage collector, it doesn't matter whether you assign it to a private name or a public name.

In my opinion, the docs for this rule already explain the motivation for the rule quite well, so I don't think extra documentation is required here

---

_Comment by @AlexWaygood on 2024-08-16 13:08_

I discussed this offline with @MichaReiser and I think I misunderstood what this PR was trying to do.

@diceroll123, the fixture snippet you've added looks like this:

```py
import asyncio

def f():
    loop = asyncio.get_event_loop()
    _task = loop.create_task(main())
```

Ruff will indeed emit a RUF006 error on this currently, but it's not because it's a private variable. It's because the function could exit immediately after the `loop.create_task()` call, which will lead to the `_task` variable immediately being cleaned up since it's only a local variable. If you change your example to this, Ruff no longer complains about it:

```py
import asyncio

def f():
    global _task
    loop = asyncio.get_event_loop()
    _task = loop.create_task(main())
```

I'm no longer convinced I agree with the change being proposed here. I think the bug that the rule is trying to protect you from applies just as much to tasks assigned to variables that start with underscores. Could you maybe give a little more detail about your motivation for making this PR?

---

_Comment by @diceroll123 on 2024-08-17 05:26_

Hm, good points all around.

The usage in my codebase that made me make this PR is deep within a forever-running async loop, so I suppose I overlooked what Alex has brought up.

---

_Label `needs-info` added by @MichaReiser on 2024-08-17 06:32_

---

_Assigned to @diceroll123 by @MichaReiser on 2024-08-19 10:17_

---

_Comment by @AlexWaygood on 2024-08-27 18:19_

Hey @diceroll123 -- I'm going to close this for now. But please feel free to open a new PR if you still think you have an example of something that's clearly a false positive! (Or just ping me, I'll happily reopen :-)

---

_Closed by @AlexWaygood on 2024-08-27 18:19_

---
