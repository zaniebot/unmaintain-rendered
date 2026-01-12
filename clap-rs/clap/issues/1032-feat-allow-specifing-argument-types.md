```yaml
number: 1032
title: "feat: Allow specifing argument types"
type: issue
state: closed
author: Binero
labels:
  - A-parsing
assignees: []
created_at: 2017-08-18T20:50:17Z
updated_at: 2018-08-02T03:30:10Z
url: https://github.com/clap-rs/clap/issues/1032
synced_at: 2026-01-12T16:14:10Z
```

# feat: Allow specifing argument types

---

_@Binero_

It would be useful if one could specify the type of an argument, something like so:

```rust
let arg_file = Arg::with_name("file").type(ArgType::Path); // e.g. ./foo
let arg_size = Arg::with_name("size").type(ArgType::FileSize); // e.g. 15MB
let arg_int = Arg::with_name("integer").type(ArgType::Int); // e.g. 0x15
```

---

_Comment by @beardedeagle on 2017-09-27 17:04_

It would be _GREAT_ if we could declare it in the argument declaration itself. ex:
```
        .arg(Arg::with_name("timeout")
                .type(i32)
                .default_value(3)
                .short("t")
                .long("timeout")
                .required(false)
                .takes_value(true)
                .help("timeout limit"))
```

---

_Comment by @Binero on 2017-09-27 17:20_

I don't think using types like `i32` is a good call, because stuff like filesizes are written differently than say time is, yet both are u64. 

---

_Comment by @kbknapp on 2017-10-04 02:59_

This is something I'd like too, but it won't be added until 3.x. I've been playing with ideas on how to accomplish it, and have also landed on your `ArgType` style enum. This will also place very well with completion generation code. 

I can't say *when* this will land, but I *do* want it.

As for using `types` directly, I agree it's probably not the best idea although it could be simulated and using things like `CustomDerive` will basically get you what you're looking for @beardedeagle :+1: 

Stay tuned.

---

_Label `C: parsing` added by @kbknapp on 2017-10-04 03:00_

---

_Label `D: intermediate` added by @kbknapp on 2017-10-04 03:00_

---

_Label `P3: want to have` added by @kbknapp on 2017-10-04 03:00_

---

_Label `T: new feature` added by @kbknapp on 2017-10-04 03:00_

---

_Label `W: 3.x` added by @kbknapp on 2017-10-04 03:00_

---

_Comment by @kbknapp on 2018-07-22 02:38_

Closing this in favor of [`clap_derive`](https://github.com/clap-rs/clap_derive)

---

_Closed by @kbknapp on 2018-07-22 02:38_

---
