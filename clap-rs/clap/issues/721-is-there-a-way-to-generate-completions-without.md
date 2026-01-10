---
number: 721
title: Is there a way to generate completions without consuming the App struct?
type: issue
state: closed
author: matthiasbeyer
labels: []
assignees: []
created_at: 2016-10-31T13:44:44Z
updated_at: 2020-08-15T17:45:12Z
url: https://github.com/clap-rs/clap/issues/721
synced_at: 2026-01-10T01:26:34Z
---

# Is there a way to generate completions without consuming the App struct?

---

_Issue opened by @matthiasbeyer on 2016-10-31 13:44_

The following case cannot be implemented (without using `App::clone()`) with clap, or am I missing something?

```rust
let app = get_app();
if app.get_matches().is_present("generate-completion") {
    app.gen_completions() /* ... */
}
```

as `get_matches()` is consuming. Is there another way besides `clone()`?

---

_Comment by @matthiasbeyer on 2016-10-31 13:45_

My title is a bit misleading maybe... :-/


---

_Comment by @kbknapp on 2016-10-31 14:35_

You can use `App::get_matches_from_safe_borrow` which doesn't consume the `App`.

Alternatively you could also just abstract away the building of the `App` and just re-call it. Even for relatively large CLIs like `rustup` [this isn't an issue since it's so fast anyways](https://github.com/rust-lang-nursery/rustup.rs/blob/607ef078ab939da30f2dfbc895ad1b85295472b2/src/rustup-cli/rustup_mode.rs#L103-L107).


---

_Label `T: RFC / question` added by @kbknapp on 2016-10-31 14:35_

---

_Renamed from "Cannot generate completion if user passes flag" to "Is there a way to generate completions without consuming the App struct?" by @kbknapp on 2016-10-31 14:35_

---

_Closed by @kbknapp on 2016-11-02 02:56_

---

_Comment by @NilsIrl on 2020-08-15 14:41_

Why does `get_matches()` consume the app?

---

_Comment by @CreepySkeleton on 2020-08-15 17:45_

Why shouldn't it?

Actually, this question is harder than it might seem. Long story short: There's a hidden "build" step which needs the app to be mutable. If this was sorted out at type system level (aka `Arg & App & BuiltApp & BuiltArg`), we could avoid that and use `&self`, but that's quite an endeavor I'm  not sure I'm comfortable committing to before 3.0 is out.

---
