---
number: 4589
title: Help heading descriptions
type: issue
state: open
author: IgnisDa
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2022-12-30T17:09:26Z
updated_at: 2024-08-07T09:00:56Z
url: https://github.com/clap-rs/clap/issues/4589
synced_at: 2026-01-10T01:27:58Z
---

# Help heading descriptions

---

_Issue opened by @IgnisDa on 2022-12-30 17:09_

### Discussed in https://github.com/clap-rs/clap/discussions/4588

<div type='discussions-op-text'>

<sup>Originally posted by **IgnisDa** December 30, 2022</sup>
Hello. Awesome project!

I want to display a help string after the help heading.

<img width="895" alt="image" src="https://user-images.githubusercontent.com/60938164/210093863-1eaf0148-653a-483d-8b0b-045228edfb9a.png">

For reference, this is what it should end up looking like:

<img width="900" alt="image" src="https://user-images.githubusercontent.com/60938164/210094017-753abcfa-d853-44c9-8a21-3adb4d0f7183.png">


Is this possible?

This is the code: https://github.com/IgnisDa/rust-libs/blob/53fcfb13510e69297ff98173239f962b3f4e97d5/crates/reflector/src/cli.rs#L83</div>

---

_Label `C-enhancement` added by @epage on 2022-12-30 17:27_

---

_Label `A-help` added by @epage on 2022-12-30 17:27_

---

_Label `S-waiting-on-design` added by @epage on 2022-12-30 17:27_

---

_Comment by @epage on 2022-12-30 17:31_

At the moment, help headings are defined in an ad hoc fashion unlike in Python.  So the first question is whether we just add `Command::help_heading_description` or make it like `ArgGroup` and allow mixing ad-hoc with structured definitions.

Second, is what to call this (description, help, etc).

We also need to decide whether we support this only for long help of if we provide both types.

Lastly, we'd need to decide what the whitespace situation should be, both newlines and indentation.,


Ideally, we would compare the user-visible aspects of this with other CLI parsers to see what lessons we can learn from them

---

_Referenced in [clap-rs/clap#2222](../../clap-rs/clap/issues/2222.md) on 2023-03-30 18:56_

---

_Comment by @crowlKats on 2024-08-07 09:00_

just throwing in that Deno is extremely interested in having this feature, mainly for linking to pages of our documentation related to the heading section

---

_Referenced in [clap-rs/clap#5995](../../clap-rs/clap/issues/5995.md) on 2025-05-09 14:30_

---
