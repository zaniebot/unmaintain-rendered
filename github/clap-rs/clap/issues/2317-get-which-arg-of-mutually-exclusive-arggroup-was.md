---
number: 2317
title: Get which arg of mutually exclusive ArgGroup was passed
type: issue
state: closed
author: Qyriad
labels:
  - C-enhancement
  - A-validators
  - S-waiting-on-mentor
assignees: []
created_at: 2021-01-28T20:44:43Z
updated_at: 2022-08-12T22:00:50Z
url: https://github.com/clap-rs/clap/issues/2317
synced_at: 2026-01-07T13:12:19-06:00
---

# Get which arg of mutually exclusive ArgGroup was passed

---

_Issue opened by @Qyriad on 2021-01-28 20:44_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

One of the use cases the documentation lists for `ArgGroup` is to have a set of mutually exclusive arguments (with `.multiple(false)`), but the only way to get which of those arguments was passed is to call `matches.is_present(name)` for every argument in that group. It would be really nice if I could just get which argument of a mutually exclusive argument group was passed. Currently, to do this you have to check each argument individually, doing something like:
```rust
let directions = &["left", "right", "forward"];

let matches = App::new("ArgGroup test")
    .arg(Arg::with_name("left")
        .short("-l")
        .long("--left"))
    .arg(Arg::with_name("right")
        .short("-r")
        .long("--right"))
    .arg(Arg::with_name("forward")
        .short("-f")
        .long("--forward"))
    .group(ArgGroup::with_name("direction")
        .required(true)
        .multiple(false)
        .args(directions))
    .get_matches();

let mut passed_direction = None;
for direction in directions {
    if matches.is_present(direction) {
        passed_direction = Some(direction);
    }
}
let passed_direction = passed_direction.unwrap();
```
Which is kind of tedious, and not very clean.

### Describe the solution you'd like
Being able to write something like:
```rust
let matches = App::new("ArgGroup test")
    .arg(Arg::with_name("left")
        .short("-l")
        .long("--left"))
    .arg(Arg::with_name("right")
        .short("-r")
        .long("--right"))
    .arg(Arg::with_name("forward")
        .short("-f")
        .long("--forward"))
    .group(ArgGroup::with_name("direction")
        .required(true)
        .multiple(false)
        .args(&["left", "right", "forward"]))
    .get_matches();

let passed_direction = matches.value_of("direction");
```
would be really nice. `value_of()` is just an example, there — I'm not particular about the exact API is to get this :slightly_smiling_face: 

---

_Label `T: new feature` added by @Qyriad on 2021-01-28 20:44_

---

_Label `C: arg groups` added by @pksunkara on 2021-01-29 00:10_

---

_Comment by @ldm0 on 2021-01-29 16:44_

I think [`conflicts_with_all`](https://docs.rs/clap/3.0.0-beta.2/clap/struct.Arg.html#method.conflicts_with_all) might matches your use case.

---

_Comment by @pksunkara on 2021-01-29 18:05_

But that's not the thing he is asking for.

---

_Comment by @Qyriad on 2021-01-29 18:39_

she*, but I believe you're correct. `ArgGroup::multiple()` already sets the mutual exclusivity. I'm looking for a way to get *which* argument in an `ArgGroup` was passed, from an `ArgMatches`. I don't see how `conflicts_with_all()` helps me there.

---

_Comment by @ldm0 on 2021-01-30 04:21_

Sorry for disturbing, haven't slept much yesterday and my mind isn't clear.

So we need to design a new api specifically for fetching the triggered argument and also the value? @pksunkara .

---

_Comment by @pksunkara on 2021-01-30 10:30_

Yes, but this is not a priority for 3.0

---

_Added to milestone `3.1` by @pksunkara on 2021-01-30 10:30_

---

_Comment by @Qyriad on 2021-01-30 15:54_

> So we need to design a new api specifically for fetching the triggered argument and also the value?

Ideally, though I would also be fine with an API just for fetching the triggered argument — which would mean that getting the value of that argument would be a separate step.

---

_Comment by @DocKDE on 2021-10-26 18:15_

I'd like this as well but I'll be patient :)

---

_Referenced in [epage/clapng#177](../../epage/clapng/issues/177.md) on 2021-12-06 20:21_

---

_Label `C: arg groups` removed by @epage on 2021-12-08 19:57_

---

_Label `A-builder` added by @epage on 2021-12-08 19:57_

---

_Label `A-parsing` added by @epage on 2021-12-08 19:57_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:11_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:11_

---

_Label `A-builder` removed by @epage on 2021-12-13 15:55_

---

_Label `A-parsing` removed by @epage on 2021-12-13 15:55_

---

_Label `A-validators` added by @epage on 2021-12-13 15:55_

---

_Removed from milestone `3.1` by @epage on 2021-12-13 16:16_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 16:17_

---

_Label `S-waiting-on-design` removed by @epage on 2021-12-13 16:18_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 16:18_

---

_Referenced in [clap-rs/clap#3748](../../clap-rs/clap/issues/3748.md) on 2022-05-25 12:19_

---

_Comment by @epage on 2022-05-25 12:27_

Currently, when we parse an argument and store its value in ArgMatches, we also store that value under the ArgGroup's name,, as was previously mentioned.

With #3732, we've changed some things related to storing the value which is having me re-think it.  In #3405, we are looking at defining actions to allow users to describe how a value should be written to the ArgMatches.  #3748 is considering doing the same for arg groups which could include writing out which argument in an arg group was active.  This doesn't let you access both the name and value but, as was mentioned, the user can look up the value.

---

_Referenced in [clap-rs/clap#4072](../../clap-rs/clap/pulls/4072.md) on 2022-08-12 21:48_

---

_Closed by @epage on 2022-08-12 22:00_

---
