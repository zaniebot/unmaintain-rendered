---
number: 5196
title: "Support for literal `{n}`s in the help message"
type: issue
state: open
author: andreykaere
labels:
  - C-enhancement
  - M-breaking-change
  - A-help
assignees: []
created_at: 2023-11-06T19:41:00Z
updated_at: 2025-08-07T20:10:56Z
url: https://github.com/clap-rs/clap/issues/5196
synced_at: 2026-01-10T01:28:08Z
---

# Support for literal `{n}`s in the help message

---

_Issue opened by @andreykaere on 2023-11-06 19:41_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.6

### Describe your use case

I want to be able to use this help message:
```text
When this option is specified, the program will try to match the given
pattern with the filename and write extracted information to the
metadata.

You can use the following correspondence when writing your pattern
{n}  Artist <-> {a}
{n}  Title  <-> {t}
{n}  Album  <-> {m}
{n}  Year   <-> {y}
{n}  Track  <-> {n}

Default parser patterns are shown below and parsers tries each of them
in the given order:
{n}  1. {n} {a} - {t}
{n}  2. {n} {a} — {t}
{n}  3. {n}. {a} - {t}
{n}  4. {n}. {a} — {t}
{n}  5. {a} - {n} {t}
{n}  6. {a} — {n} {t}
{n}  7. {a} - {n}. {t}
{n}  8. {a} — {n}. {t}
{n}  9. {a} - {t}
{n}  10. {a} — {t}
{n}  11. {n} {t}
{n}  12. {n}. {t}
{n}  13. {t}
```
where first {n} are expected to be interpreted as \n and I want to have ability to "escape" other {n} to be able to see literally `{n}` in the output help message.

### Describe the solution you'd like

To have escaping option to be used: for example `{{n}}` or `\{n\}` or something like that.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @andreykaere on 2023-11-06 19:41_

---

_Comment by @epage on 2023-11-06 19:52_

Note that we want to remove support for `{n}` (#3230).  The main reason it still exists is because of our poor handling of markdown in doc comments for `clap_derive`.  This is being tracked in #2389.

---

_Comment by @andreykaere on 2023-11-06 20:27_

But for some reason `clap` replaces `{n}` even if I use builder pattern instead of deriving it. Any reason why is this the case?

P.S. Using `verbatim_doc_comment` doesn't change this behavior (although it's mentioned as a workaround in #2389).

---

_Comment by @epage on 2023-11-06 20:29_

It exists in the builder because that was the original intended use case.  Rather than partially back it out and then completely remove it, we've left it in as is until we can remove it.

---

_Renamed from "Support for using `{n}` in the help message" to "Support for literal `{n}`s in the help message" by @epage on 2025-08-07 20:10_

---

_Label `M-breaking-change` added by @epage on 2025-08-07 20:10_

---

_Label `A-help` added by @epage on 2025-08-07 20:10_

---

_Comment by @epage on 2025-08-07 20:10_

I'm labeling this issue as being fixed by us removing our special handling for `{n}` so it can literally show up.

---
