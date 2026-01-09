---
number: 1506
title: Premium typed arguments support
type: issue
state: closed
author: ColinPitrat
labels:
  - A-validators
assignees: []
created_at: 2019-06-23T07:53:23Z
updated_at: 2021-11-04T16:12:34Z
url: https://github.com/clap-rs/clap/issues/1506
synced_at: 2026-01-07T13:12:19-06:00
---

# Premium typed arguments support

---

_Issue opened by @ColinPitrat on 2019-06-23 07:53_

One of the main features I'd expect from clap is to be able to specify the type of an argument, typically int, and have it provide an helpful error message is the argument cannot be parsed properly.

Currently, either one has to parse the argument string and provide an error message or move the logic to a custom validator that tries to parse and display a helpful message if it fails, but still need to parse after that (but can unwrap() as the validator already ensure the value is convertible).

I see two main options to do that:
 - provide "standard" generic validators like is_type<T>() or is_type_between<T>(min: T, max: T). Then the user still has to parse but can be sure it will succeed (so unwrap()) and knows, if needed, some value checks have been done already
 - provide a new type() method to the builder. The args would be checked in a similar ways as validator do (to provide a helpful message) but on top of that, a generic typed_value_of method<T> would return the arg with the proper type provided. It would panic if the type T is not the same as the one provided for the argument.

Ideally this would also cover flags like enum, vectors (e.g comma separated) as long as the proper trait is provided (typically TryFrom/TryInto) etc...

What do you think?

---

_Comment by @ColinPitrat on 2019-06-23 08:16_

Currently, what I do when I need this:

```
fn is_type<T: FromStr>(val: String) -> Result<(), String>
where <T as std::str::FromStr>::Err : std::string::ToString
{
    match val.parse::<T>() {
        Ok(_) => Ok(()),
        Err(m) => Err(m.to_string()),
    }
}
```

Used like this:

```
        .arg(Arg::with_name("some_uint")
                .required(true)
                (...)
                .validator(is_type::<u32>))
(...)

    let some_uint = match matches.value_of("some_uint").parse::<u32>().unwrap();
```

---

_Comment by @omarabid on 2019-07-17 18:44_

I know it doesn't solve your problem but: https://github.com/clap-rs/clap_derive/issues/4

I think it's better to get the CustomDerive done by now. It could enable a bunch of stuff: Like handling configuration through clap-rs

---

_Label `clap-derive-related` added by @CreepySkeleton on 2020-02-01 13:24_

---

_Label `C: derive macros` added by @pksunkara on 2020-02-03 09:19_

---

_Label `clap-derive-related` removed by @pksunkara on 2020-02-03 09:19_

---

_Comment by @pksunkara on 2020-04-09 08:03_

You should be able to achieve this with `clap_derive` now. For normal clap use, this issue about feature request to add some helpers.

---

_Label `C: derive macros` removed by @pksunkara on 2020-04-09 08:04_

---

_Label `C: validators` added by @pksunkara on 2020-04-09 08:04_

---

_Label `D: medium` added by @pksunkara on 2020-04-09 08:04_

---

_Label `P4: nice to have` added by @pksunkara on 2020-04-09 08:04_

---

_Label `T: new feature` added by @pksunkara on 2020-04-09 08:04_

---

_Comment by @epage on 2021-11-04 16:12_

Didn't know we had this issue.  We've started requirements gathering and high level design of this at https://github.com/clap-rs/clap/issues/2683 which we are hoping to implement for clap 4.

Closing this out in favor of https://github.com/clap-rs/clap/issues/2683

---

_Closed by @epage on 2021-11-04 16:12_

---
