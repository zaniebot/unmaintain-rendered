```yaml
number: 13878
title: "Add a comment to the top of uv.lock files containing the string @generated"
type: issue
state: open
author: roganartu
labels:
  - enhancement
assignees: []
created_at: 2025-06-06T03:56:17Z
updated_at: 2025-10-21T23:45:07Z
url: https://github.com/astral-sh/uv/issues/13878
synced_at: 2026-01-10T03:23:54Z
```

# Add a comment to the top of uv.lock files containing the string @generated

---

_Issue opened by @roganartu on 2025-06-06 03:56_

### Summary

Some code review tools automatically hide files that contain the text `@generated` notably Phabricator, but I suspect there are others I'm not familiar with.

Examples:
- poetry https://github.com/python-poetry/poetry/issues/2034
- cargo https://github.com/rust-lang/cargo/issues/6180
- Java uses `@Generated` to mark code as generated so linters ignore it (this one isn't always file-wide) https://docs.oracle.com/javase/8/docs/api/javax/annotation/Generated.html

It would be nice if `uv` could add this string as a comment to its lockfile.

### Example

An extra line such as:
```
# This file was @generated with uv version 1.2.3. Do not modify it by hand.
```
at the top of `uv.lock`.

---

_Label `enhancement` added by @roganartu on 2025-06-06 03:56_

---

_Comment by @notatallshaw on 2025-06-06 14:10_

I'd never head of this but it seems to come from the Eclipse Modeling Framework and then Facebook adopted in it's Hack Codegen library, and there is an advocacy website for it to be implemented across a wide range of tools: https://generated.at/

---

_Comment by @konstin on 2025-06-06 15:26_

A counterargument here that `uv.lock` _should_ be reviewed in e.g. phabricator, while I agree that it's good if style tools ignore the file and warn before editing it.

---

_Comment by @roganartu on 2025-06-09 16:52_

I agree this is likely subjective, but I'd argue that lockfiles are (usually?) no more reviewable than say generated protobuf files. We should generally be able to trust the tools to do the right thing. I can see some situations where one might want to review them, such as when doing major version bumps or something, but their format and verbosity doesn't really lend itself to review anyway so I suspect the behaviour has long been reinforced to ignore.

It's certainly not just python that has this problem, lockfiles have to represent so much information that there probably isn't really a succinct and readable representation. Ruby, JS, Go, Rust, all have the same "issue" imo.

---

_Comment by @jackrosenthal on 2025-10-16 19:31_

Hi all, I sent a PR which implements this.  Can we reach a consensus that this should be added?

Here's what my thoughts are:

- `@generated` is a fairly standard way to mark files as machine generated (see https://generated.at/).
- `uv.lock` is, in fact, machine generated.
- The `@generated` marker is used across a wide variety of tools.  Not just code review tools, but also linters, auto-formatters, editors, etc.
- We have no say as to what specific tools should do with the header.  It's up to those tools to do with the information that the file is machine generated as they wish.  For example, Phabricator may have chosen to collapse machine-generated files by default, but it's not up to us to decide if that behavior is correct or not, we just told Phabricator the file was machine generated.  Similarly, suppose a hypothetical IDE decides to show machine-generated files in a different color, it would also not be up to us to decide if doing so was correct or not.  We merely indicate the file is, in fact, machine generated, and tools are allowed to do whatever it is with that information they wish.

---

_Comment by @zanieb on 2025-10-16 20:36_

I think it is relevant what behavior this marker causes downstream while making a decision here. Of course tools can do different things based on that information, but it's up to us whether we think the typical behavior of tools makes sense for this file we control. I don't think you'd convince us to add the marker if it didn't have any downstream effects, and I don't think you'll convince us to add the marker if it degrades common workflows with uv.

There is an entire category of machine generated files that are checked into version control but don't necessarily need to be reviewed, e.g., their contents are deterministically generated from some other files that will be reviewed. This is not the case for the uv lockfile. The contents of this file are important and require direct user interaction to change.

That said, the marker also captures the idea that the file should not be manually edited and does not need to be formatted. I like those aspects and think that's a good fit for our lockfile.

---

_Comment by @samypr100 on 2025-10-21 16:22_

Another potential approach to consider is to start with what cargo did [here](https://github.com/rust-lang/cargo/issues/6180#issuecomment-430708397) adapted to uv.

In other words, implement support in uv to preserve the comment at the top of the file. That is, if uv sees a comment at the top of a lock file, it preserves it. Emitting it automatically can be deferred until a later time.

This could give us better feedback of its usefulness overtime.

---

_Comment by @zanieb on 2025-10-21 17:55_

That's a good idea, I'm in favor of that.

---

_Comment by @jackrosenthal on 2025-10-21 20:14_

Would a `tool.uv` config option be better?  Supporting comment preservation at the top will merely create more maintenance over time if we were to decide to enable the `@generated` comment by default (i.e., do we keep the preservation behavior at that point)?

---

_Comment by @zanieb on 2025-10-21 20:25_

> Supporting comment preservation at the top will merely create more maintenance over time if we were to decide to enable the @generated comment by default (i.e., do we keep the preservation behavior at that point)?

Why would that create more maintenance? I don't follow

---

_Comment by @jackrosenthal on 2025-10-21 20:55_

Suppose we later decide to enable `@generated` (by default), after having already created this comment preservation behavior.

When we see a comment in the `uv.lock`, we could then do any of these things:
- Preserve that comment, even if it doesn't match the `@generated` format.
- Stomp that comment, replacing it with `@generated`.
- Preserve the comment and add our `@generated` comment.

This is already complex and difficult behavior to document.  Now suppose we want to change the `@generated` comment slightly.  Do we have to create something which detects if we're supposed to preserve or replace the comment?  It gets even more hairy.

... all of this just so we can debate over including a single comment that merely indicates the file is machine generated.

OTOH, if we don't introduce comment preservation behavior, and either select to:
- Unconditionally include the `# This file is @generated by uv.  Do not edit by hand`.
- Conditionally include based on a config setting (like `tool.uv.lock-file-generated-comment`).

The behavior for changes down the road is significantly more straightforward.

---

_Comment by @samypr100 on 2025-10-21 22:26_

> Now suppose we want to change the `@generated` comment slightly

Not sure if it was clear, apologies. The approach I was originally thinking for uv specifically was to ignore the comment on the top line if it matches the template sentence **exactly** once we start emitting it. In other words, any other text that does not match the template would be removed on regeneration.

Assuming the template is `# This file is automatically @generated by uv.`, any other variants would be removed / not supported entirely.

Edit:

On second though, the original idea to preserve top-level comments from cargo team is albeit better and more future proof. Inspecting the current [cargo source](https://github.com/rust-lang/cargo/blob/344c4567c634a25837e3c3476aac08af84cf9203/src/cargo/ops/lockfile.rs#L129-L154), once you emit the generated comment any other comments not matching the template get pushed down. I believe that resolves the "suppose we want to change the `@generated` comment slightly" problem (which aligns with your third option).

---
