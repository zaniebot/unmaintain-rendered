```yaml
number: 16240
title: "[red-knot] Find conflicting declarations for attribute in class methods"
type: pull_request
state: closed
author: smokyabdulrahman
labels:
  - ty
assignees: []
draft: true
base: main
head: gh-issue-15964
created_at: 2025-02-18T20:20:51Z
updated_at: 2025-05-18T17:08:27Z
url: https://github.com/astral-sh/ruff/pull/16240
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Find conflicting declarations for attribute in class methods

---

_@smokyabdulrahman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Handling one more case for class attributes: checking for conflicting declarations for class attributes in methods.
part of astral-sh/ty#206
## Test Plan
No test for now


---

_Review requested from @carljm by @smokyabdulrahman on 2025-02-18 20:20_

---

_Review requested from @MichaReiser by @smokyabdulrahman on 2025-02-18 20:20_

---

_Review requested from @AlexWaygood by @smokyabdulrahman on 2025-02-18 20:20_

---

_Review requested from @sharkdp by @smokyabdulrahman on 2025-02-18 20:20_

---

_Label `red-knot` added by @MichaReiser on 2025-02-18 20:26_

---

_Renamed from "Find conflicting declarations for attribute in class methods" to "[red-not] Find conflicting declarations for attribute in class methods" by @MichaReiser on 2025-02-18 20:26_

---

_Renamed from "[red-not] Find conflicting declarations for attribute in class methods" to "[red-knot] Find conflicting declarations for attribute in class methods" by @MichaReiser on 2025-02-18 20:27_

---

_Converted to draft by @MichaReiser on 2025-02-19 17:29_

---

_Comment by @MichaReiser on 2025-02-19 17:29_

I put this back in draft as it doesn't seem finished. 

---

_Comment by @sharkdp on 2025-02-19 18:18_

> I put this back in draft as it doesn't seem finished.

Just for additional context, I [suggested splitting](https://github.com/astral-sh/ty/issues/206) the implementation into two parts:  (1) collect all conflicting declared types so we would in principle be able to emit a diagnostic — assuming we could do it from anywhere, and (2) actually create a new diagnostic and make the required changes to be able to emit it.

But even part (1) doesn't seem finished, so keeping this in draft seems fine. @smokyabdulrahman Let us know if you need additional help to proceed here.

---

_Comment by @smokyabdulrahman on 2025-02-19 20:49_

> > I put this back in draft as it doesn't seem finished.
> 
> Just for additional context, I [suggested splitting](https://github.com/astral-sh/ty/issues/206) the implementation into two parts: (1) collect all conflicting declared types so we would in principle be able to emit a diagnostic — assuming we could do it from anywhere, and (2) actually create a new diagnostic and make the required changes to be able to emit it.
> 
> But even part (1) doesn't seem finished, so keeping this in draft seems fine. @smokyabdulrahman Let us know if you need additional help to proceed here.

Hey @sharkdp,
I couldn't think of any other place to tackle and add the logic of collecting conflicting declared types to.
Also, I am not sure how to test for part (1) even in a hacky way.

---

_Comment by @sharkdp on 2025-02-20 22:32_

> I couldn't think of any other place to tackle and add the logic of collecting conflicting declared types to.

We identified two spots in the linked issue. If I understand it correctly, you solved it for one of these, but the more difficult one would be in the change to the loop logic. I can go into more details tomorrow, if that would help.

> Also, I am not sure how to test for part (1) even in a hacky way.

The infrastructure for emitting diagnostics from `Type::member` (and friends) is still not in place. If you're okay with waiting for that, you could still test your implementation by running Red Knot manually on a test file, and "emitting" diagnostics via `eprintln!` or similar.

The important thing for part 1 would be to get the semantics correct, i.e. to identify two or three test cases that would clearly show that we would be able to generate the right diagnostic messages.

If that sounds too complicated, I'm also okay with leaving that open for a bit longer, and revisit it once the diagnostics infrastructure is in place.

---

_Comment by @sharkdp on 2025-02-21 09:54_

> The infrastructure for emitting diagnostics from `Type::member` (and friends) is still not in place

This is being tracked in https://github.com/astral-sh/ty/issues/194

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-15 17:58_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2025-04-15 17:58_

---

_Comment by @MichaReiser on 2025-05-18 16:36_

Are we planning to pick this PR up again or should it be closed?

---

_Comment by @smokyabdulrahman on 2025-05-18 17:08_

> Are we planning to pick this PR up again or should it be closed?

Super sorry for stalling this, I will close this PR.

---

_Closed by @smokyabdulrahman on 2025-05-18 17:08_

---
