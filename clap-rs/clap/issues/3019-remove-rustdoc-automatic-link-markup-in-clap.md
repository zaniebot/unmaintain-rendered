---
number: 3019
title: Remove rustdoc automatic link markup in clap_derive-generated help messages
type: issue
state: closed
author: AntonHermann
labels:
  - A-derive
assignees: []
created_at: 2021-11-12T19:09:21Z
updated_at: 2021-11-14T01:04:16Z
url: https://github.com/clap-rs/clap/issues/3019
synced_at: 2026-01-10T01:27:30Z
---

# Remove rustdoc automatic link markup in clap_derive-generated help messages

---

_Issue opened by @AntonHermann on 2021-11-12 19:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.5

### Describe your use case

When using URLs in doc comments like "see https://repo.com for details", rustdoc emits a warning suggesting the automatic link syntax: "see \<https://repo.com\> for details".

When clap uses this doc comment to generate help messages, it outputs the automatic link markup (angle brackets) as it was written instead of removing the markup, leaving only the url in place.

### Describe the solution you'd like

Removing the automatic link markup and just using the URL in the help message would be a good solution in my opinion.

One could also consider emitting the ansi control sequence to markup links in the terminal on linux (see [this writeup](https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda) for details, terminal support, etc.) so they are clickable in many terminal emulators.

### Alternatives, if applicable

Leaving everything as is, meaning that users who want to use `rustdoc`s automatic links but don't want them to show up in help output would have to duplicate their help message and pass it to `#[clap(about = "...")]` or `#[clap(long_about = "...")]`.
(I haven't checked whether those take preference over doc-comments, if they do not then it would be impossible to have working links in rustdoc while not having the link markup in the clap output?)

### Additional Context

_No response_

---

_Label `T: new feature` added by @AntonHermann on 2021-11-12 19:09_

---

_Label `C: derive macros` added by @epage on 2021-11-12 19:31_

---

_Comment by @epage on 2021-11-12 19:33_

Thanks for the feedback!

I suspect there are other improvements to be made in our post-porcessing.  The challenge will be in finding the right balance between implementation cost and quality.

---

_Referenced in [ouch-org/ouch#196](../../ouch-org/ouch/pulls/196.md) on 2021-11-12 21:18_

---

_Comment by @pksunkara on 2021-11-14 00:32_

This can be closed since #2389 covers it already.

---

_Closed by @AntonHermann on 2021-11-14 01:04_

---
