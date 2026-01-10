---
number: 3912
title: "Allow a field's type to be different than the value parser type"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-07-12T14:33:15Z
updated_at: 2022-08-24T02:48:17Z
url: https://github.com/clap-rs/clap/issues/3912
synced_at: 2026-01-10T01:27:49Z
---

# Allow a field's type to be different than the value parser type

---

_Issue opened by @epage on 2022-07-12 14:33_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.8

### Describe your use case

Some value parsers (`Count`, `SetTrue`, `SetFalse`) only support one specific data type but the user might want to support a different user type.

For example, the following `parse(from_flag)` test cannot be ported to `value_parser`
```rust
#[test]
fn non_bool_type_flag() {
    fn parse_from_flag(b: bool) -> std::sync::atomic::AtomicBool {
        std::sync::atomic::AtomicBool::new(b)
    }

    #[derive(Parser, Debug)]
    struct Opt {
        #[clap(short, long, parse(from_flag = parse_from_flag))]
        alice: std::sync::atomic::AtomicBool,
        #[clap(short, long, parse(from_flag))]
        bob: std::sync::atomic::AtomicBool,
    }

    let falsey = Opt::try_parse_from(&["test"]).unwrap();
    assert!(!falsey.alice.load(std::sync::atomic::Ordering::Relaxed));
    assert!(!falsey.bob.load(std::sync::atomic::Ordering::Relaxed));
}
```
(a `SetConst` might also work to a degree)

Others might want a different count type than `u8`.

Someone even brought up processing a `Vec<String>` into a `String`, see https://github.com/clap-rs/clap/discussions/3905#discussioncomment-3125768

### Describe the solution you'd like

The user should be able to
- Specify the `ValueParser` output type to use with `get_one` / `get_many`
  - Defaults to the field's type
- Optionally a conversion function from value parser type to field type
  - Defaults to `TryInto` (or `Into`?)

This will only be called when extracting the fields and will only be called once.  We will not do a proper validation step for this.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @epage on 2022-07-12 14:33_

---

_Label `A-derive` added by @epage on 2022-07-12 14:33_

---

_Label `S-waiting-on-design` added by @epage on 2022-07-12 14:33_

---

_Comment by @epage on 2022-07-12 14:34_

What would we call these new attributes?

---

_Comment by @chipsenkbeil on 2022-07-12 16:46_

If we use serde as an example, it has [`into`](https://serde.rs/container-attrs.html#into), [`from`](https://serde.rs/container-attrs.html#from), and [`try_from`](https://serde.rs/container-attrs.html#try_from) as attributes at the struct level for converting to a different type before serializing and deserializing. So my thought would be to use similar naming for the conversion function that you can place on an individual field. Heck, you could just have it use the corresponding trait implementations if you wanted something simpler than an arbitrary function.

In terms of output type, would just having `#[clap(type = "...")]` be too simple?

---

_Comment by @itsxaos on 2022-08-23 20:41_

> Others might want a different count type than `u8`.

I'm the guy that wants a different count type. Specifically, I want a bigger type because I think `u8` is too small.

I'm trying to rewrite something I wrote in Python with [click](https://click.palletsprojects.com/) in Rust and it bothers me that the counted option only goes up to 255 because not only have I gone over that when using the Python version of my program, but it was actually quite ergonomic.

I understand that it's a rather niche use-case and maybe `u8` is the right "default", but it also seems there's no way at all (using clap) to implement a counted option "manually". (I'm probably wrong on that because I only tried with the Derive API, but I did try for quite a while.)

Also, I found it a little surprising that the count just saturates when it would exceed 255.
That doesn't seem unreasonable given the alternatives, but it should probably be documented somewhere.


---

_Comment by @epage on 2022-08-23 21:15_

> not only have I gone over that when using the Python version of my program

Could you expand on use case when you go over 255?  What range are you typically in?

> but it was actually quite ergonomic.

I assume a "not" was missing in here.  What ergonomic problems have you had with `u8`?

>  it also seems there's no way at all (using clap) to implement a counted option "manually"

We have not opened ArgAction up for expansion yet as we work through how it should behave internall first.

---

_Comment by @itsxaos on 2022-08-23 23:20_

> I assume a "not" was missing in here. What ergonomic problems have you had with `u8`?

No I did mean it like that, it was surprisingly ergonomic. It's a bit silly to go into this much detail because to be honest, I think it happened to be useful once, maybe twice.
But hey, here goes:

To keep it simple, let's say the command in question prints log entries for a given time span. Without any arguments, it prints all the entries that occurred that day, and if provided with `-l` / `--last` all the entries that occurred the *last* day (i.e. yesterday), `-ll` entries from the day before yesterday, and so on. It also takes an argument for specifying the time span (i.e. `week` or `month`), so that it will show entries from this (or last, etc.) week or month instead.

It's a somewhat unusual way of doing things but it has proven to be very convenient, because usually I don't go more than 5 days back (`-lllll`), and if I do, I'll go by weeks instead.
However, at some point it did occur that I wanted to view the entries from ~450 days back and I was surprised by how ergonomic it was, because…

Recent(ish) versions of bash and zsh make it very easy to enter a specific (large) number of a character.
By holding Alt and typing in a number (or by pressing Esc before each digit), the shell can be made to enter the next key that many times. So, if I hold Alt and type `10`, release Alt, and then press `a`, I get `aaaaaaaaaa`.
(Two notes: a) bash even shows the number while it's being entered and b) on windows, it probably doesn't work with the numpad number keys).

Doing that was more convenient than calculating and looking up the date that works out to.
And with the 450 `l`s, "scrolling" through the days is actually also more convenient than if I was using a numeric argument: just doing up-arrow, `l` and enter (or backspace and enter) instead of having to edit the number.

> Could you expand on use case when you go over 255? What range are you typically in?

So that's the use case where it happened to be useful to go over 255. I'll freely admit it's unusual and uncommon, and far outside the typical range, which is less than 5, but I think it shows that it's not unthinkable and (occasionally) reasonable to do so.

255 characters really isn't that many and, if it were `u16`, 65535 is more than can fit on any reasonable terminal window. As far as I understand there's also no significant costs to having it be the bigger type.

Obviously it's a matter of compatibility now, but I think it's rather arbitrary to limit it to `u8` and ideally it really should be a `u16`.

---

_Comment by @epage on 2022-08-24 02:46_

@itsxaos at this point we should probably move this discussion some place else.  Mind opening a new issue if you want this changed?  This issue is specifically about allowing a type in a struct's field that is different than the type of the value parser.

---

_Comment by @epage on 2022-08-24 02:47_

> Some value parsers (Count, SetTrue, SetFalse) only support one specific data type but the user might want to support a different user type.

As an update, we've loosened this restriction for SetTrue/SetFalse in the 4.0 release, see  https://github.com/clap-rs/clap/pull/4095 for the latest version of it

---

_Referenced in [clap-rs/clap#4347](../../clap-rs/clap/issues/4347.md) on 2022-10-04 15:32_

---

_Referenced in [clap-rs/clap#3864](../../clap-rs/clap/issues/3864.md) on 2022-10-24 01:48_

---

_Referenced in [clap-rs/clap#4417](../../clap-rs/clap/issues/4417.md) on 2022-10-24 13:10_

---

_Referenced in [clap-rs/clap#4679](../../clap-rs/clap/issues/4679.md) on 2023-02-06 15:49_

---

_Referenced in [clap-rs/clap#3114](../../clap-rs/clap/issues/3114.md) on 2023-02-13 16:54_

---

_Referenced in [clap-rs/clap#4830](../../clap-rs/clap/issues/4830.md) on 2023-04-10 20:46_

---
