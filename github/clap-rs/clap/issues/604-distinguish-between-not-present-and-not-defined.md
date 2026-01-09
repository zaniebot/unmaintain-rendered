---
number: 604
title: Distinguish between not present and not defined Args
type: issue
state: closed
author: SuperFluffy
labels:
  - C-enhancement
  - E-hard
assignees: []
created_at: 2016-07-26T14:05:27Z
updated_at: 2021-10-27T10:32:30Z
url: https://github.com/clap-rs/clap/issues/604
synced_at: 2026-01-07T13:12:19-06:00
---

# Distinguish between not present and not defined Args

---

_Issue opened by @SuperFluffy on 2016-07-26 14:05_

Right now `ArgMatches::is_present(s)` checks whether the option with name `s` is present in the list of arguments or not. What about `Arg`s that have not been defined, and which I erroneously check for? Maybe `is_present()` could return an `Option<bool>` instead, with `Some(bool)` in cases where a defined `Arg` was set/not set, and `None` in cases where the option does not appear in the list of defined `Arg`s.

Assume the following example:

``` rust
let m = App::new("myprog")
    .arg(Arg::with_name("debug")
        .long("debug")
        .short("d"))
    .get_matches_from(vec![
        "myprog", "-d"
    ]);

// Check with typo returns false
let is_present = m.is_present("dbeug");
```

This function will of course always return false. However, `dbeug` was never actually defined in the first place, so it would be great if it complained about that.


---

_Renamed from "Distinguish between not present and not defined flags/options" to "Distinguish between not present and not defined Args" by @SuperFluffy on 2016-07-26 14:05_

---

_Comment by @kbknapp on 2016-07-28 00:56_

I agree this is an issue, and something I'm hoping to solve by proxy with the ability to use enums instead of strings (see #510). But this is all a breaking change and will be made on the 3.x branch which I'm hoping to get some time to work on in the next few weeks and actually be able to put out soon.

Having `is_present` return an `Option<bool>` could also be one of the breaking 3.x changes. I'm not opposed to it.


---

_Label `T: RFC / question` added by @kbknapp on 2016-07-28 00:56_

---

_Label `C: matches` added by @kbknapp on 2016-07-28 00:56_

---

_Label `W: 3.x` added by @kbknapp on 2016-07-28 00:56_

---

_Added to milestone `3.0` by @kbknapp on 2016-08-26 15:28_

---

_Label `W: postponed` added by @kbknapp on 2016-11-02 02:59_

---

_Label `W: postponed` removed by @kbknapp on 2017-04-05 18:40_

---

_Referenced in [clap-rs/clap#929](../../clap-rs/clap/issues/929.md) on 2017-04-18 00:41_

---

_Removed from milestone `3.0` by @kbknapp on 2018-02-02 01:52_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:52_

---

_Comment by @kbknapp on 2018-07-22 00:58_

I think I'd prefer to `panic!` on an undefined arg. This will mean keeping a list of all defined args in the matches, which increases the memory usage quite a bit but perhaps only doing this in debug mode or behind a feature flag would be a good way to go about this.

---

_Label `T: enhancement` added by @kbknapp on 2018-07-22 00:59_

---

_Label `D: easy` added by @kbknapp on 2018-07-22 00:59_

---

_Label `P3: want to have` added by @kbknapp on 2018-07-22 00:59_

---

_Label `M: mentored` added by @kbknapp on 2018-07-22 00:59_

---

_Label `good first issue` added by @kbknapp on 2018-07-22 00:59_

---

_Label `T: RFC / question` removed by @kbknapp on 2018-07-22 00:59_

---

_Comment by @spwitt on 2018-10-16 02:23_

@kbknapp I've started working on this. I should have a pull request tomorrow after I update according to contribution guide

---

_Comment by @vijfhoek on 2019-06-08 15:11_

For the record, I agree with using `panic!` for this. I think it is a really fair assumption that calling this on an undefined argument is a programming error, and not just a runtime error.

---

_Removed from milestone `v3-alpha.2` by @pksunkara on 2020-02-01 07:45_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:45_

---

_Label `C: asserts` added by @pksunkara on 2020-04-09 11:45_

---

_Label `Z: good first issue` removed by @pksunkara on 2020-04-10 13:59_

---

_Label `Z: mentored` removed by @pksunkara on 2020-04-10 13:59_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-10 14:06_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-10 14:06_

---

_Removed from milestone `3.1` by @pksunkara on 2020-04-10 14:06_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-10 14:06_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-10 14:06_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-10 14:06_

---

_Removed from milestone `3.1` by @pksunkara on 2020-04-10 14:06_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-10 14:06_

---

_Label `D: easy` removed by @pksunkara on 2020-04-10 14:06_

---

_Label `D: hard` added by @pksunkara on 2020-04-10 14:06_

---

_Label `C: asserts` removed by @pksunkara on 2020-04-10 14:08_

---

_Comment by @CreepySkeleton on 2020-06-30 11:29_

> This will mean keeping a list of all defined args in the matches, which increases the memory usage quite a bit but perhaps only doing this in debug mode or behind a feature flag would be a good way to go about this.

Emphasis on:
> ... only doing this in debug mode ...

This is pretty much the same as what we do for `Id`. Piece o'cake, I'll take it.

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-06-30 11:29_

---

_Referenced in [clap-rs/clap#2000](../../clap-rs/clap/pulls/2000.md) on 2020-07-03 04:55_

---

_Referenced in [clap-rs/clap#1104](../../clap-rs/clap/issues/1104.md) on 2021-07-20 17:22_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Removed from milestone `3.0` by @epage on 2021-10-16 18:21_

---

_Added to milestone `4.0` by @epage on 2021-10-16 18:21_

---

_Referenced in [clap-rs/clap#2897](../../clap-rs/clap/issues/2897.md) on 2021-10-16 19:48_

---

_Removed from milestone `4.0` by @pksunkara on 2021-10-16 19:49_

---

_Unassigned @CreepySkeleton by @pksunkara on 2021-10-26 20:46_

---

_Assigned to @pksunkara by @pksunkara on 2021-10-26 20:46_

---

_Added to milestone `3.0` by @pksunkara on 2021-10-27 09:23_

---

_Closed by @bors[bot] on 2021-10-27 10:32_

---

_Referenced in [epage/clapng#82](../../epage/clapng/issues/82.md) on 2021-12-06 16:37_

---
