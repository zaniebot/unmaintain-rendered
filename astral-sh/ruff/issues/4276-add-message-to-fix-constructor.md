```yaml
number: 4276
title: "Add message to `Fix` constructor"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
  - help wanted
assignees: []
created_at: 2023-05-08T10:25:24Z
updated_at: 2024-01-10T17:07:27Z
url: https://github.com/astral-sh/ruff/issues/4276
synced_at: 2026-01-10T11:09:47Z
```

# Add message to `Fix` constructor

---

_Issue opened by @MichaReiser on 2023-05-08 10:25_

Depends on #4183 

Add a, for now, optional `title` or `message` field to `Fix` that will be used to replace the `DiagnosticKind::autofix_title`. 

The motivation is that a diagnostic can have multiple potential fixes. For example, the unused variable diagnostic can recommend to either:

* Prefix the variable with an underscore, to intentionally mark it as "this is ok"
* Remove the variable 
* Add a noqa suppression comment. 

Adding the title to the `Fix` instead of specifying it on the `DiagnosticKind` will allow us to model the different diagnostics. 

It also has the advantage that the Rust compiler can enforce the requirement that each fixable `DiagnosticKind` has a message, because it is not stored as part of the `Fix`. 


## Tasks

* [ ] Add new `title` or `message` field to `Fix` (`Option<String>`). 
* [ ] Change the constructor methods to take `message: String` as first argument, except for `unspecified*`, to avoid having to refactor all calls at once
* [ ] Serialize the `title`/`message` as part of the `Fix` in the JSON Emitter 
* [ ] Change the `MessageCodeFrame` implementation to show `DiagnosticKind::suggestion` if present, or fall back to `Fix::title` if the `Diagnostic` has one `Fix`.

---

_Label `help wanted` added by @MichaReiser on 2023-05-08 10:33_

---

_Label `internal` added by @MichaReiser on 2023-05-08 10:33_

---

_Comment by @zanieb on 2023-05-10 15:35_

Feel free to assign me to this one

---

_Assigned to @zanieb by @charliermarsh on 2023-05-10 15:38_

---

_Comment by @zanieb on 2023-05-14 03:56_

I've started work on this at https://github.com/charliermarsh/ruff/compare/main...madkinsz:ruff:internal/4276 and I could use a bit more context on the plan for replacing `autofix_title`.

For example, there's a fix message generated at https://github.com/charliermarsh/ruff/blob/c10a4535b9549d8dab6dec024ce3231b74bf5e18/crates/ruff/src/rules/pyflakes/rules/imports.rs#L47-L54

Is the plan to duplicate that logic and generate a message at https://github.com/charliermarsh/ruff/blob/0a68636de33b3398c61cac2bead42101fcb6022e/crates/ruff/src/checkers/ast/mod.rs#L5329-L5332

For the following todo

> Change the MessageCodeFrame implementation to show DiagnosticKind::suggestion if present, or fall back to Fix::title if the Diagnostic has one Fix.

Did you mean to say that the other way around? We should show the new `Fix::message` and fall back to the diagnostic kind suggestion if it's there is no fix message yet? Otherwise we'll need to remove the `autofix_title` before any of these new changes are displayed?

For including the message in the JSON emitter, we're already showing the diagnostic kind suggestion and it seems like it would be duplicative to include both messages. We should just show one with the same logic we decide on above, right?
https://github.com/charliermarsh/ruff/blob/853d8354cb4ad3f0a77c3a916efc0956e7e5d83e/crates/ruff/src/message/json.rs#L43 

<details>

<summary>Some additional notes...</summary>

Should update this TODO https://github.com/charliermarsh/ruff/blob/a2b8487ae3c3efa99deb21376dcd504a81276a3f/crates/ruff_diagnostics/src/violation.rs#L33-L34

Autofix title is populated as the diagnostic kind suggestion at https://github.com/charliermarsh/ruff/blob/c10a4535b9549d8dab6dec024ce3231b74bf5e18/crates/ruff_macros/src/violation.rs#L62 
</details>


---

_Comment by @MichaReiser on 2023-05-15 07:28_

Thanks and sorry that the issue is a bit less clear. I must admit, I haven't fully figured out the details myself yet and it will probably require some exploration. 

> Is the plan to duplicate that logic and generate a message at 

My idea is to remove the `autofix_title` for the majority of fixes and instead move them into `Fix::title`. There are different ways how we can do this. We can either inline the message, create a function that generates the fix title, or keep an `autofix_title` method function on the `DiagnosticKind` of a specific fix (but remove it from the trait). I recommend that we do whatever works best for a specific fix. 

There may be a few use cases where we have to duplicate the logic or use a different phrasing. This is mainly for diagnostics with `AutofixKind::SOMETIMES` because we want to show a suggestion even if we don't provide a fix. Ideally, the advice text reads more like a suggestion to the user about what they could try whereas the `Fix::title` describes what this specific fix does.  I don't like this much but I haven't figured out a better solution yet.

> Did you mean to say that the other way around? We should show the new Fix::message and fall back to the diagnostic kind suggestion if it's there is no fix message yet? Otherwise we'll need to remove the autofix_title before any of these new changes are displayed?

No, that's the order I had in mind. The `MessageCodeFrame` currently only shows the `autofix_title` because we don't have a proper suggestion field. But, what I just noticed is that the description above fails to mention that we want a new `DiagnosticKind::suggestion` or `advice` method that returns a message that guides the user towards a fix, regardless of whether Ruff offers an autofix. Does the order then make more sense? I recommend just renaming `autofix_title` to `suggestion` / `advice` to ease the refactoring and then migrating the `suggestion` to the `Fix::title` on a rule-per-rule basis. The reason why we may want to have an `advice` and a `title` is that the title is phrased in imperative mode and must be short. The advice should probably not be written in imperative mode and may want to go into a little more detail. My plan for the future is to expand advice to support rich content like code frames, diffs, messages with markdown, etc. But that's a long way down the road ;)

> For including the message in the JSON emitter, we're already showing the diagnostic kind suggestion and it seems like it would be duplicative to include both messages. We should just show one with the same logic we decide on above, right?

I'm fine with duplicating the information for now. Ultimately, the fields will store different messages. 

---

_Comment by @zanieb on 2023-06-02 00:21_

Thanks for your long comment and apologies for the slow reply. I've had a busy couple of weeks ;) 

I've made some additional progress over at https://github.com/charliermarsh/ruff/compare/main...madkinsz:ruff:internal/4276

It seems unlikely that I'll make significant progress on this in the near future — if you want it sooner rather than later feel free to pick up where I left off. Otherwise I can take this up with gusto in July.

---

_Comment by @MichaReiser on 2023-06-02 06:17_

@madkinsz thanks for the update and no worries. We can easily wait till July, except if there's someone else interested in picking this up before then. 

---

_Comment by @MichaReiser on 2024-01-10 17:07_

I'll close this. I think it's best to reconsider this if we plan to change our fix infrastructure. The one we have right now seems to be working reasonably well. 

---

_Closed by @MichaReiser on 2024-01-10 17:07_

---

_Closed by @MichaReiser on 2024-01-10 17:07_

---
