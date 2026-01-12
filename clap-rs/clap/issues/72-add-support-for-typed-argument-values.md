```yaml
number: 72
title: Add support for typed argument values
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
assignees: []
created_at: 2015-04-14T01:15:46Z
updated_at: 2018-08-02T03:29:38Z
url: https://github.com/clap-rs/clap/issues/72
synced_at: 2026-01-12T16:14:08Z
```

# Add support for typed argument values

---

_@kbknapp_

Initially support `Int`, and `Float`. Implementation should allow additions down the road as well, based on usage, maybe a `File` or `Directory` too... 


---

_Label `enhancement` added by @kbknapp on 2015-04-14 01:15_

---

_Label `feature request` added by @kbknapp on 2015-04-14 01:15_

---

_Referenced in [clap-rs/clap#73](../../clap-rs/clap/issues/73.md) on 2015-04-14 01:17_

---

_Comment by @kbknapp on 2015-04-14 14:45_

Initial implementation may simply be a convenience macro, something like

``` rust
let num = value_t!(matches.value_of("length"), u32);
```

This would allow far more types initially simply using Rust's `&str.parse()`, without crossing any lines (i.e where does `clap` validation stop, and developer validation start? What about parsing IP address, MAC addresses, etc....where is the line?). The more validation `clap` does, the bigger a performance hit _everyone_ takes whether they want the validation or not.

What's unclear is if failure to parse should constitute ending the process? I'm leaning towards returning an `Option<T>` or `Result<T,&str>` which gives `clap` consumers the option to use `.unwrap_or()` for a default, or also deciding what to do upon a failed parse. I'm going to continue thinking about this before implementing...

The other nice thing about a macro initially is I could potentially include something like a `decode_t!` to decode a &str into a user defined `enum`. Now this would open the door for changing `.possible_values()` but I'm unsure about how to best implement this part without just requiring the consumer to manually type out each possibility of the `enum` into a possible values vec as is currently done.


---

_Referenced in [clap-rs/clap#74](../../clap-rs/clap/pulls/74.md) on 2015-04-14 18:39_

---

_Closed by @kbknapp on 2015-04-14 18:42_

---

_Comment by @kbknapp on 2015-04-14 20:17_

While this isn't the best solution, it does work for the time being. I'm still unsure how to best implement this in a way that is _least_ taxing on the developer (i.e. I don't want to force people to implement custom traits, or anything like that...)

So this will remain TBD, but for now `value_t!` and `value_t_or_exit!` will at least get the job done in a somewhat efficient manner.


---

_Referenced in [clap-rs/clap#3124](../../clap-rs/clap/issues/3124.md) on 2021-12-09 16:16_

---

_Referenced in [clap-rs/clap#3130](../../clap-rs/clap/issues/3130.md) on 2021-12-09 16:34_

---
