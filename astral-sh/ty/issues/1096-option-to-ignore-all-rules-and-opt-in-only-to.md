```yaml
number: 1096
title: option to ignore all rules and opt-in only to specific ones
type: issue
state: closed
author: vishds
labels:
  - feature
  - configuration
assignees: []
created_at: 2025-08-25T14:51:14Z
updated_at: 2025-08-26T16:02:18Z
url: https://github.com/astral-sh/ty/issues/1096
synced_at: 2026-01-12T15:54:24Z
```

# option to ignore all rules and opt-in only to specific ones

---

_@vishds_

### Question

I couldn't find any option in the CLI to narrow down the rules that ty checks for, similar to `ruff check --select`.
I did notice that we can use ignore flag multiple times, but I'm trying to use `ty` to fix a specific issue in an old codebase with lot of type errors.

### Version

ty 0.0.1-alpha.17

---

_Label `question` added by @vishds on 2025-08-25 14:51_

---

_Label `configuration` added by @carljm on 2025-08-25 14:52_

---

_Comment by @carljm on 2025-08-25 14:56_

I'm not the expert on the configuration(that's @MichaReiser, and he is out for a few weeks), but as far as I can tell you're right, we don't currently have any way to ignore all rules by default and select just a few to enable.

I think this is a bit more dangerous in general with a type checker than it is with a linter, since in a type checker, rules are less independent of each other -- it's often the case that one type error can cause cascading `Unknown` type that may then hide other errors. So looking at your codebase through the filter of only a single rule may hide other issues that would impact accurate detection of that single rule.

So I am not entirely sure what our plan is here, but I think it makes sense to keep this issue open to track the feature request.

---

_Label `question` removed by @carljm on 2025-08-25 14:56_

---

_Label `feature` added by @carljm on 2025-08-25 14:57_

---

_Renamed from "Selection of Rules" to "option to ignore all rules and opt-in only to specific ones" by @carljm on 2025-08-25 14:57_

---

_Comment by @AlexWaygood on 2025-08-25 16:07_

Thanks for the report! I think this is a duplicate of https://github.com/astral-sh/ty/issues/174 -- once that's implemented you'll be able to do `ty check --ignore=ALL --select=unresolved-reference`, etc.

---

_Closed by @AlexWaygood on 2025-08-25 16:07_

---

_Comment by @vishds on 2025-08-26 13:10_

Thank you for your reply.

> So looking at your codebase through the filter of only a single rule may hide other issues that would impact accurate detection of that single rule.

I'm curious about the implementation of `--ignore`. Does it suffer from the same issue ?
I tried reading the source code, but I couldn't figure out whether the suppression happens at the output stage or the detection stage 😅.

In my case I parsed out the specific rules. So a filter at the output stage would suffice for me. But I do understand the issue with that implementation.


---

_Comment by @carljm on 2025-08-26 15:54_

> I'm curious about the implementation of `--ignore`. Does it suffer from the same issue ?

Yes, the issue is inherent to the nature of type checking. It doesn't mean you can't still ignore some rules if you want to, it just means you have to be aware that by ignoring a rule you may miss some problems. For an example, consider that you may want to detect `invalid-assignment`, but suppress `invalid-type-form`. The consequence is that we won't tell you if you have `x: InvalidAnnotation = something`, because `InvalidAnnotation` would emit `invalid-type-form` and then we resolve the declared type of `x` to `Unknown`, meaning there won't be any `invalid-assignment` on that assignment. This is what I mean about rules not always being independent of each other. (Some rules of course are more independent and suppressing them is less likely to cause silent cascading `Unknown`.)

> I couldn't figure out whether the suppression happens at the output stage or the detection stage

It's at the detection stage, but it doesn't really matter, since there isn't significant processing between detection and output. The issue I'm talking about isn't about how we handle diagnostics (we don't have logic that explicitly suppresses one diagnostic in favor of another, or anything like that), it's about the inherent nature of how type information flows through your program.

---
