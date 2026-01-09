---
number: 1030
title: Support argument aliases that set multiple arguments
type: issue
state: closed
author: darnir
labels:
  - C-enhancement
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2017-08-15T07:17:19Z
updated_at: 2022-01-11T18:46:08Z
url: https://github.com/clap-rs/clap/issues/1030
synced_at: 2026-01-07T13:12:19-06:00
---

# Support argument aliases that set multiple arguments

---

_Issue opened by @darnir on 2017-08-15 07:17_

## Problem Description

In my projects, I often have a scenario where there is a single shortcut switch which sets values for multiple switches. For example, in my current project, I have (taken directly from my code):

```rust
        .arg(
            Arg::with_name("UTILIZATION")
                .long("utilization")
                .short("u")
                .takes_value(true)
                .overrides_with_all(&["UBEGIN", "USTEP", "UEND"])
                .help("Utilization of generated taskset"),
        )
        .arg(
            Arg::with_name("UBEGIN")
                .long("utilization-begin")
                .takes_value(true)
                .default_value("0.025")
                .help("Begin Utilization Range"),
        )
        .arg(
            Arg::with_name("USTEP")
                .long("utilization-step")
                .default_value("0.1")
                .help("Step for Utilization Range"),
        )
        .arg(
            Arg::with_name("UEND")
                .long("utilization-end")
                .default_value("1.0")
                .help("End of Utilization Range"),
        )
        .group(
            ArgGroup::with_name("UTILIZATION_RANGE")
                .conflicts_with("UTILIZATION")
                .multiple(true),
        )
```

Now what I would like it for the argument `UTILIZATION` to explicitly set values for `UBEGIN`, `USTEP` and `UEND`.

As you can see, I currently have a overrides and conflicts rule and then later in the application code, `UTILIZATION` is tested for and the other values are set.

Another way to implement this would be by allowing the user to manipulate values in the `ArgMatches` struct. Then one could simply set the values there before returning to the main client for consumption.

This is not a very obscure feature but is often used in many programs. For a more real-world usage of such a feature, please look at GNU Wget, quoting from its manual here (Emphasis mine):

>       --mirror
>            Turn on options suitable for mirroring.  This option turns on recursion and time-stamping, sets infinite recursion depth and keeps FTP
>            directory listings.  It is currently equivalent to **-r -N -l inf --no-remove-listing**.


---

_Comment by @kbknapp on 2017-10-04 03:03_

Using [`Arg::default_value_if`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.default_value_if) will probably get you about 90% of what you're looking for. Otherwise, you'll have to go with defining these aliases in user code. I'm not against adding something like this feature in the future, but it's not something I can work on right now as there are about 1,000 other things I need to finish first :stuck_out_tongue_winking_eye: 

---

_Label `C: parsing` added by @kbknapp on 2017-10-04 03:03_

---

_Label `P4: nice to have` added by @kbknapp on 2017-10-04 03:03_

---

_Label `T: new feature` added by @kbknapp on 2017-10-04 03:03_

---

_Label `W: 3.x` added by @kbknapp on 2017-10-04 03:03_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#78](../../epage/clapng/issues/78.md) on 2021-12-06 16:35_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Comment by @epage on 2021-12-09 17:09_

Between `default_value_if` and `replace` (https://github.com/clap-rs/clap/issues/2836), I think we've got this covered.  If there is feedback on `replace, please put it there.

---

_Closed by @epage on 2021-12-09 17:09_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:46_

---
