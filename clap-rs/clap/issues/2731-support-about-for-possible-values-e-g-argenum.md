```yaml
number: 2731
title: "Support `about` for `possible_values` e.g. `ArgEnum`"
type: issue
state: closed
author: ModProg
labels: []
assignees: []
created_at: 2021-08-21T00:15:08Z
updated_at: 2021-09-19T10:29:10Z
url: https://github.com/clap-rs/clap/issues/2731
synced_at: 2026-01-12T16:14:13Z
```

# Support `about` for `possible_values` e.g. `ArgEnum`

---

_@ModProg_

### Clap Version

master

### Describe your use case

Sometimes the value itself my not give enough information to the user, therefore it would be helpful to be able to show and additional about in e.g. fish completions.
Fish does that for example for `__fish_complete_users`:
![grafik](https://user-images.githubusercontent.com/11978847/130304017-ed419f48-2321-4517-a67d-9c00d0d99f6a.png)


### Describe the solution you'd like

Support adding abouts/documentation for the possible completion values.

For `ArgEnum`:
```rs
#[derive(Clone, Copy, ArgEnum)]
pub enum ShellType {
    /// the default Shell
    Bash,
    /// the friendly interactive Shell
    Fish
}
```
This should result in the following completions in fish for example:
![grafik](https://user-images.githubusercontent.com/11978847/130304172-dee7c631-950a-4d52-843b-1cab6b6feda8.png)

### Additional Context

For fish this is somewhat related to #2730 

---

_Label `T: new feature` added by @ModProg on 2021-08-21 00:15_

---

_Renamed from "Support `abbout` for `possible_values` e.g. `ArgEnum`" to "Support `about` for `possible_values` e.g. `ArgEnum`" by @ModProg on 2021-08-21 00:16_

---

_Comment by @epage on 2021-08-21 02:05_

The challenge is the derive API is built on top of the builder API, so we need to come up with a way to specify this within the builder API.

Possibly a variant of `possible_values` that takes a slice of tuples.

---

_Comment by @ModProg on 2021-08-21 08:58_

> Possibly a variant of possible_values that takes a slice of tuples.

Tho this is not very builder like :D but as that is probably the only information you would want to attach to a possible_value it makes sense.

I could also imagine somthing more like this (somewhat verbose if there can only be `about`:
```
.possible_value(PVal::new("bash").about("the default Shell"))
.possible_value(PVal::new("fish").about("the friendly interactive Shell"))
.possible_value(PVal::new("sh"))
```
Another option would be to do it via `possible_value` and `possible_value_about`:
```
.possible_value_about("bash","the default Shell")
.possible_value_about("fish","the friendly interactive Shell")
.possible_value("sh")
```

When using `possible_values_about`:
```
.possible_values_about(
    &[
        ("bash", "the default Shell"), 
        ("fish", "the friendly interactive Shell"),
        ("sh", "")
    ]
)
// or
.possible_values_about(
    &[
        ("bash", "the default Shell"), 
        ("fish", "the friendly interactive Shell")
    ]
)
.possible_value("sh")
```

Internally this would need to be stored in a Vec of Tuples.


---

_Referenced in [clap-rs/clap#2733](../../clap-rs/clap/pulls/2733.md) on 2021-08-21 16:17_

---

_Comment by @ModProg on 2021-08-21 19:17_

I drafted an Implementation in #2733 using the `.possible_values_about`. It works for zsh and fish, because those currently implement possible_args completion.

---

_Label `C: args` added by @pksunkara on 2021-08-31 10:54_

---

_Referenced in [clap-rs/clap#2758](../../clap-rs/clap/pulls/2758.md) on 2021-09-07 21:42_

---

_Referenced in [clap-rs/clap#2762](../../clap-rs/clap/pulls/2762.md) on 2021-09-09 12:52_

---

_Closed by @pksunkara on 2021-09-19 10:29_

---
