---
number: 6087
title: Improve parsing and formatting inside  after_help
type: issue
state: open
author: Agent-Hellboy
labels:
  - C-enhancement
  - S-waiting-on-design
  - A-man
assignees: []
created_at: 2025-08-04T07:51:38Z
updated_at: 2025-08-04T15:35:42Z
url: https://github.com/clap-rs/clap/issues/6087
synced_at: 2026-01-07T13:12:20-06:00
---

# Improve parsing and formatting inside  after_help

---

_Issue opened by @Agent-Hellboy on 2025-08-04 07:51_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Hi , I am coming from https://github.com/uutils/coreutils/issues/8362
clap_mangen is outputting after_help content as plain text, causing list items to appear inline instead of properly formatted.
https://github.com/clap-rs/clap/blob/583ba4ad9a4aea71e5b852b142715acaeaaaa050/clap_mangen/src/render.rs#L248C15-L248C25





### Describe the solution you'd like

Fix suggestions to discuss 
- Parse list items (lines starting with - )
- Extract options and descriptions
- Use proper roff formatting (.TP, .B, .I)

I will add a PR.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Agent-Hellboy on 2025-08-04 07:51_

---

_Referenced in [clap-rs/clap#6088](../../clap-rs/clap/pulls/6088.md) on 2025-08-04 14:00_

---

_Comment by @epage on 2025-08-04 14:42_

@pksunkara at https://github.com/clap-rs/clap/pull/6088#issuecomment-3150968893

> @epage I don't have much context here, but we specifically say that the help text is not markdown, right? IIRC, I had some discussions about this back in the days. Can you please let me know if something changed?



---

_Comment by @epage on 2025-08-04 14:43_

Correct, we do not specify the output is markdown.

What we are missing is turning of ANSI escape codes into ROFF.  I'm playing around with it but its showing some limitations in `roff` and `anstyle-roff`.

---

_Label `A-man` added by @epage on 2025-08-04 14:44_

---

_Label `S-waiting-on-design` added by @epage on 2025-08-04 14:44_

---

_Comment by @Agent-Hellboy on 2025-08-04 14:57_

> Correct, we do not specify the output is markdown.
> 
> What we are missing is turning of ANSI escape codes into ROFF. I'm playing around with it but its showing some limitations in `roff` and `anstyle-roff`.

could you please explain it in details with some context? and how it's related to the current issue.

---

_Comment by @pksunkara on 2025-08-04 15:11_

> What we are missing is turning of ANSI escape codes into ROFF.

I am not sure if ROFF has special syntax for display lists. But if it does:

If we implement #5900, Doc comments written in markdown with lists -> ANSI just prints indented list -> ROFF needs to parse it again. In this scenario, isn't it better to pass direct markdown from docstring to generators?

---

Without that decision and implementing #5900 first, I am not sure this issue can be resolved since we don't want to have any custom syntax support for help strings.

---

_Comment by @epage on 2025-08-04 15:34_

> could you please explain it in details with some context? and how it's related to the current issue.

While ANSI escape codes can't help with bulleted lists, they can help with other forms of formatting.

---

_Comment by @epage on 2025-08-04 15:35_

Not mentioned in this thread is that #6088 replaced blank lines with `.PP`.  We do that for `long_about` but none of our other user-provided text.  It likely deserves an issue on its own.

---

_Referenced in [uutils/coreutils#9005](../../uutils/coreutils/pulls/9005.md) on 2025-10-26 17:34_

---
