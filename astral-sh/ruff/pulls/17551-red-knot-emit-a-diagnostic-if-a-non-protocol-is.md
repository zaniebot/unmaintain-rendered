```yaml
number: 17551
title: "[red-knot] Emit a diagnostic if a non-protocol is passed to `get_protocol_members`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/get-protocol-members-validation
created_at: 2025-04-22T13:55:10Z
updated_at: 2025-04-23T10:14:09Z
url: https://github.com/astral-sh/ruff/pull/17551
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Emit a diagnostic if a non-protocol is passed to `get_protocol_members`

---

_Pull request opened by @AlexWaygood on 2025-04-22 13:55_

## Summary

Passing a non-protocol class to `get_protocol_members` fails at runtime. This PR adds logic to red-knot so that we detect that error and warn the user about it.

Stacked on top of #17550

## Test Plan

Existing mdtests updated and extended. Snapshots added!

Screenshot of what the diagnostics look like in the terminal: 
![image](https://github.com/user-attachments/assets/3298df31-f612-4215-a1f4-55f264056328)



---

_Label `red-knot` added by @AlexWaygood on 2025-04-22 13:55_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-22 13:55_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-22 13:55_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-22 13:55_

---

_Comment by @github-actions[bot] on 2025-04-22 14:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-22 14:14_

I don't know why the ruff (not red-knot!) ecosystem check is failing, but it doesn't seem related to this PR.

---

_Comment by @dhruvmanila on 2025-04-22 14:25_

> I don't know why the ruff (not red-knot!) ecosystem check is failing, but it doesn't seem related to this PR.

It's because you're using stacked PRs, once the PR that's below this one has the ecosystem check completed, you can re-run here so that it can find the base ecosystem report which would be part of that PR.

---

_Comment by @AlexWaygood on 2025-04-22 16:53_

> > I don't know why the ruff (not red-knot!) ecosystem check is failing, but it doesn't seem related to this PR.
> 
> It's because you're using stacked PRs, once the PR that's below this one has the ecosystem check completed, you can re-run here so that it can find the base ecosystem report which would be part of that PR.

Hrm, I've tried rerunning the job several times now (and the PR below this one has definitely completed its CI!).

I think it's possibly because the PR below this one (#17550) had the Linux build skipped entirely in its CI run? I'm not sure why -- PRs that only change `.md` files don't seem to have any tests run on them right now, which is sorta problematic for red-knot PRs 😆

---

_Comment by @dhruvmanila on 2025-04-22 17:01_

> I think it's possibly because the PR below this one (#17550) had the Linux build skipped entirely in its CI run?

Ah, yeah that's probably the case. The Ruff ecosystem checks needs a base report to compare against. This is usually from `main` but as this is a stacked PR, it'll fetch it from the previous PR (which is correct) but that doesn't exists and so this workflow fails.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/protocols.md_-_Protocols_-_Invalid_calls_to_`get_protocol_members()`.snap`:80 on 2025-04-22 19:39_

it's kinda weird to link to the typing spec here, as it's not designed as user-facing documentation. But the typing-module docs don't go into the details here; PEP 544 is a snapshot of a decision that was taken years ago rather than up-to-date documentation; and it feels like an admission of defeat to link to the mypy docs 😆 I think the typing spec is maybe the least bad option

---

_@AlexWaygood reviewed on 2025-04-22 19:39_

---

_@dcreager reviewed on 2025-04-22 20:55_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/snapshots/protocols.md_-_Protocols_-_Invalid_calls_to_`get_protocol_members()`.snap`:80 on 2025-04-22 20:55_


> it feels like an admission of defeat to link to the mypy docs 😆

That suggests that the ideal solution is to eventually describe this in our own (public-facing) documentation. So I think linking to the typing spec is fine as a placeholder, maybe with a TODO comment in the code to update it to the public docs once they're written.

---

_@AlexWaygood reviewed on 2025-04-22 21:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/protocols.md_-_Protocols_-_Invalid_calls_to_`get_protocol_members()`.snap`:80 on 2025-04-22 21:01_

Yes. Or for us to invest resources into improving type-checker-agnostic docs for `Protocol`! A TODO here sounds like a good idea whatever the case, though (I'll add tomorrow; it's late here)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:1360 on 2025-04-22 23:57_

This is a lovely diagnostic, and a great example for what we can be doing with the new diagnostic framework in other cases -- but it's also a lot of code for an extremely narrow case (misuse of one specific function)! Not really suggesting we change anything here, just not sure it will be worth investing this much in every narrow edge-case diagnostic...

---

_@carljm approved on 2025-04-22 23:59_

Nice!

---

_@AlexWaygood reviewed on 2025-04-23 10:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:1360 on 2025-04-23 10:06_

Yeah, I know what you mean.

I think there might be ways to make the diagnostic APIs a little less verbose? The code here isn't that complicated, just a little boilerplate-y. And I think the way you get a reputation for having "excellent error messages" like rustc is by paying attention to lots of little details like this.

---

_Merged by @AlexWaygood on 2025-04-23 10:13_

---

_Closed by @AlexWaygood on 2025-04-23 10:13_

---

_Branch deleted on 2025-04-23 10:13_

---
