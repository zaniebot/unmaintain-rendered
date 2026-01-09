---
number: 1433
title: "Allow styled text in `template`"
type: issue
state: closed
author: ssokolow
labels:
  - C-enhancement
  - A-help
  - S-blocked
assignees: []
created_at: 2019-03-19T00:17:43Z
updated_at: 2023-03-28T02:45:24Z
url: https://github.com/clap-rs/clap/issues/1433
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow styled text in `template`

---

_Issue opened by @ssokolow on 2019-03-19 00:17_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.32.0 (9fda7c223 2019-01-16)`

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

When using a custom template to work around papercuts in Clap (#1432) and StructOpt (TeXitoi/structopt#172) which break `help2man`, i can find no documented way to apply `AppSettings::ColoredHelp` to the "USAGE:" header, though I've managed to reconstruct the desired bits of all of the rest of the default template.

### Expected Behavior Summary

I should have some means of reintroducing the conditionally and portably colored `USAGE:` header, like this:

![desired](https://user-images.githubusercontent.com/46915/54571592-61dc3e00-49db-11e9-94cc-d1b09d698bae.png)

**NOTE:** Image created as a composite of two screenshots.

### Actual Behavior Summary

I was able to recreate every desired element of the default template *except* the coloring on the `USAGE:` header:

![actual](https://user-images.githubusercontent.com/46915/54571565-3d806180-49db-11e9-823b-55048f4436fc.png)

### Steps to Reproduce the issue

1. Copy the example from the README into a new project
2. Use the following custom template in concert with `AppSettings::ColoredHelp`:

```rust
const HELP_TEMPLATE: &str = "{bin} {version}

{about}

USAGE:
    {usage}

{all-args}
";
```

### Sample Code or Link to Sample Code

https://gist.github.com/ssokolow/9df8c92b24d94116c5eb57fccad2b1a2

---

_Comment by @ssokolow on 2019-03-19 00:33_

...and, aesthetics aside, having a `USAGE:` header is necessary for `help2man` to recognize the usage line.

---

_Comment by @danwilliams on 2019-05-05 13:45_

Equally, if you use, as a guideline, the "full help template" that can be found here:

https://github.com/clap-rs/clap/blob/master/tests/example1_tmpl_full.txt

...there is no way to apply the ColoredHelp formatting to the various other headings if, for instance, you wish to change their order or rename them (e.g. for language translations).

Unsure at this point of the best approach to getting coloured help output when using custom help templates.

---

_Comment by @ssokolow on 2019-05-06 02:03_

**EDIT:** Sorry if you saw the original form of this via e-mail notification. I initially misunderstood the intent of your response.

True, but, In my testing, aside from the USAGE: header not being colourized, the template I gave is equivalent to removing the author placeholder from the default template and inserting a newline before `{about}` so `help2man` understands it properly.

`example1_tmpl_full.txt` has the additional problem of not replicating the "hide headers for empty sections" behaviour of the default template.

That said, what I'd suggest is some kind of placeholder that resolves to either changing the colour if colourization is enabled or nothing if it's disabled.

---

_Label `C: colorizing` added by @CreepySkeleton on 2020-02-01 14:47_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 14:47_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-01 14:47_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 14:47_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:49_

---

_Comment by @CreepySkeleton on 2020-07-09 13:10_

> That said, what I'd suggest is some kind of placeholder that resolves to either changing the colour if colourization is enabled or nothing if it's disabled.

I was thinking along these lines in the context of https://github.com/clap-rs/clap/issues/1790. Something like
```
{color:yellow:USAGE}
```

or more generally
```
{color:color_name-bold:text}
 ----- ---------- ---- ----
 |        |         |
 keyword  |       optional bold switch
       the color
```
And maintain the list of supported colors. 

I don't want to support any RGB-or-something notations because Windows CMD has only a set of predefined colors and implementing the RGB code => winapi color mapping is something I very much like to avoid.



---

_Renamed from "No `template` placeholder for colorized "USAGE:" header" to "Allow styled text in `template`" by @pksunkara on 2020-10-26 08:20_

---

_Comment by @pksunkara on 2020-10-26 08:21_

The original `usage` issue is fixed in #2131. I am repurposing this issue for general styled text in templates.

---

_Referenced in [clap-rs/clap#2196](../../clap-rs/clap/issues/2196.md) on 2020-10-31 21:10_

---

_Referenced in [epage/clapng#116](../../epage/clapng/issues/116.md) on 2021-12-06 18:34_

---

_Label `C: colorizing` removed by @epage on 2021-12-08 20:23_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:23_

---

_Referenced in [clap-rs/clap#3108](../../clap-rs/clap/issues/3108.md) on 2021-12-09 19:52_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 19:53_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 19:53_

---

_Referenced in [clap-rs/clap#1790](../../clap-rs/clap/issues/1790.md) on 2021-12-09 21:23_

---

_Referenced in [clap-rs/clap#2389](../../clap-rs/clap/issues/2389.md) on 2021-12-30 12:32_

---

_Comment by @epage on 2022-08-18 13:37_

My plan is for the API to accept ANSI colored strings.  When we output, we use a special stdout/stderr that adapt the ANSI color codes to the capabilities of the terminal.

Blocked on https://github.com/epage/anstyle/issues/5

---

_Label `S-waiting-on-design` removed by @epage on 2022-08-18 13:37_

---

_Label `S-blocked` added by @epage on 2022-08-18 13:37_

---

_Referenced in [clap-rs/clap#4110](../../clap-rs/clap/pulls/4110.md) on 2022-08-24 15:27_

---

_Referenced in [clap-rs/clap#4114](../../clap-rs/clap/pulls/4114.md) on 2022-08-25 18:44_

---

_Added to milestone `4.x` by @epage on 2022-08-30 21:21_

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-08-31 13:57_

---

_Referenced in [sharkdp/bat#2327](../../sharkdp/bat/pulls/2327.md) on 2022-10-03 20:21_

---

_Referenced in [clap-rs/clap#3234](../../clap-rs/clap/issues/3234.md) on 2022-10-07 15:36_

---

_Referenced in [clap-rs/clap#4444](../../clap-rs/clap/pulls/4444.md) on 2022-11-03 02:08_

---

_Referenced in [clap-rs/clap#4431](../../clap-rs/clap/issues/4431.md) on 2022-11-05 03:22_

---

_Referenced in [clap-rs/clap#4611](../../clap-rs/clap/issues/4611.md) on 2023-01-09 16:06_

---

_Referenced in [clap-rs/clap#4765](../../clap-rs/clap/pulls/4765.md) on 2023-03-17 14:48_

---

_Closed by @epage on 2023-03-28 02:45_

---
