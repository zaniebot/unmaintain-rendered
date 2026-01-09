---
number: 5459
title: Make visible_alias print in a way that fits convention
type: issue
state: open
author: lolbinarycat
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2024-04-15T21:12:45Z
updated_at: 2024-07-08T21:52:42Z
url: https://github.com/clap-rs/clap/issues/5459
synced_at: 2026-01-07T13:12:20-06:00
---

# Make visible_alias print in a way that fits convention

---

_Issue opened by @lolbinarycat on 2024-04-15 21:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.4

### Describe your use case

print aliases in an easily understood way

### Describe the solution you'd like

currently, aliases are printed at the end of the flag's description.  i believe it would be much more natural to print them as `-f, --flag, --alias   description`

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @lolbinarycat on 2024-04-15 21:12_

---

_Label `A-help` added by @epage on 2024-04-16 00:46_

---

_Label `S-waiting-on-design` added by @epage on 2024-04-16 00:46_

---

_Comment by @epage on 2024-04-16 00:54_

Could you go into more detail on this as this is meant to be the jumping off point in the conversation.

For example
- "print aliases in an easily understood way": It would help to show the current behavior and explain why it isn't easy to understand
- "fits convention": while not in the issue body, it is in the title.  What prior art have you seen for this?  What are examples of that output?  Is there prior art in other directions that you know of?

I assume you are somewhat connected with @tertsdiepraam who opened #5454.  In that, concerns raised on the proposed output include
- Alignment issues.  While #5454 had them misaligned, there are other approaches, each with their own trade offs, including
  - Indenting the block further: reducing the space for help text
  - Splitting it across lines, taking up more vertical space (could be limited to long-help).  If the help is aligned with the first entry and overflows onto other lines, then it wouldn't take up more space.
- Unclear association between flag and value though this already exists for short flags.
  - [clap-help](https://crates.io/crates/clap-help) prints the short and long with their value

---

_Comment by @tertsdiepraam on 2024-04-16 05:44_

Nope, I don't know @lolbinarycat. But we could use this issue as it seems to he about the same thing. Thanks for the mention and I'll follow this issue.

---

_Comment by @lolbinarycat on 2024-04-16 08:44_

>  What prior art have you seen for this?

essentially every program that has aliases either prints them together, or prints them entirely separately, with the alias having a description like "alias for --some-other-flag"

for an example of the former see gnu tar (literally the first command i tried when you asked for an example)

---

_Comment by @epage on 2024-04-16 16:31_

I had tried three or so commands and couldn't find examples of aliases.  

Its also good to get from a variety of design spaces to explore multiple options.  If you could find more prior art, that would be helpful.

Relevant parts from `tar --help`

Sometimes they use next line help
```
  -w, --interactive, --confirmation
                             ask for confirmation for every action
```

Sometimes they affect alignment
```
  -A, --catenate, --concatenate   append tar files to an archive
  -c, --create               create a new archive
      --delete               delete from the archive (not on mag tapes!)
  -d, --diff, --compare      find differences between archive and file system
  -r, --append               append files to the end of an archive
      --test-label           test the archive volume label and exit
  -t, --list                 list the contents of an archive
  -u, --update               only append files newer than copy in archive
  -x, --extract, --get       extract files from an archive
```

When they take a value, they don't list the value for the short but they do for each long
```
  -F, --info-script=NAME, --new-volume-script=NAME
                             run script at end of each tape (implies -M)
```

---

_Referenced in [clap-rs/clap#5571](../../clap-rs/clap/issues/5571.md) on 2024-07-08 19:54_

---

_Comment by @Dyredhead on 2024-07-08 21:52_

I would love for this to be implemented, such that it is easier to see from the --help dialouge. `eza` also does this, albiet only in their manpage:
```

       --color=WHEN, --colour=WHEN
              When to use terminal colours (using ANSI escape code to colorize the output).

       Valid settings are ‘always', ‘automatic' (or ‘auto' for short), and ‘never'.  The default value is ‘automatic'.

       The  default behavior (‘automatic' or ‘auto') is to colorize the output only when the standard output is connected to a real terminal.  If the output of eza is redirected to a file
       or piped into another program, terminal colors will not be used.  Setting this option to ‘always' causes eza to always output terminal color, while ‘never' disables the use of ter‐
       minal color.

       Manually setting this option overrides NO_COLOR environment.

       --color-scale, --colour-scale
              highlight levels of field distinctly.  Use comma(,) separated list of all, age, size

       --color-scale-mode, --colour-scale-mode
              Use gradient or fixed colors in --color-scale.

       Valid options are fixed or gradient.  The default value is gradient.

```
In terms of ordering I think:
1. short
2. short aliases
3. long
4. long aliases

Would make the most sense.


---

_Referenced in [clap-rs/clap#5454](../../clap-rs/clap/pulls/5454.md) on 2024-07-10 16:01_

---

_Referenced in [clap-rs/clap#5995](../../clap-rs/clap/issues/5995.md) on 2025-05-09 14:30_

---
