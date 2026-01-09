---
number: 6031
title: "`required` ignores `default_value`, making some requirements impossible to express"
type: issue
state: open
author: lilyball
labels: []
assignees: []
created_at: 2025-06-10T19:29:51Z
updated_at: 2025-08-07T20:17:49Z
url: https://github.com/clap-rs/clap/issues/6031
synced_at: 2026-01-07T13:12:20-06:00
---

# `required` ignores `default_value`, making some requirements impossible to express

---

_Issue opened by @lilyball on 2025-06-10 19:29_

clap v3.2.0 changed the behavior of required args to ignore default values. For simple cases this can be accounted for (though it’s not properly documented), but in the case of conditional defaults or conditional requires, this leads to some requirements being impossible to express that otherwise would have.

This was previously noticed in https://github.com/clap-rs/clap/issues/3838, which was closed as requiring a v5 to fix, though I believe this could be fixed by adding a new app setting to opt in to having defaults affect requirements.

The problem with the current behavior is conditional defaults and conditional requires combine to make more complicated patterns impossible to express. Here’s an example:

```rust
// Not a contribution.
let cmd = clap::Command::new("cli")
    // --use-color gates all color flags
    .arg(clap::arg!(--"use-color"))
    // --color is required with --use-color, but has a default
    .arg(
        clap::arg!(--color <COLOR>)
            .value_parser(["red", "blue"])
            .default_value_if("use-color", ArgPredicate::IsPresent, "red")
            .requires("use-color"),
    )
    // --shade is required if --color=red
    .arg(
        clap::arg!(--shade <SHADE>)
            .value_parser(["maroon", "pink"])
            .required_if_eq("color", "red")
            .requires("use-color"),
    );
let matches = cmd.get_matches_from(["cli", "--use-color"]);
// this should have failed with an error saying --shade is required,
// but instead it parses just fine.
```

With this config, the `--shade` flag is supposed to be required any time `--color=red`, but you can only write `--color=red` if you also write `--use-color`. As it is today, it’s impossible to express this. If it wasn’t for the `--use-color` flag then I could give color an unconditional default and use `.required_unless_present("color")` on `--shade` to cover the default case, but I can’t do that once I have a `default_value_if`.

---

_Comment by @epage on 2025-06-10 19:50_

The example you gave is not quite clear.  It would help if you followed the issue template, including providing concrete commands that you run and what the out is and what you expect the output to me.

Also, to note #3838 was not closed as requiring a v5.  We'll have a v5 so we'd have left it open in that case.  A simple change was not going to be done for that issue and I'm hesitant about adding new validation related logic at this time because we need to figure out what our general approach is as people come with a lot of different validation requests and to satisfy them all, we'd need balloon the API, binary size, and build time.

---

_Comment by @lilyball on 2025-06-12 00:25_

The example I gave is self-contained and should reproduce against any version of clap v4, and the comments in the example explain what was expected. You can also see it in [the Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=8bb9169f406735da836f7310425dc96b).

More generally, the change from 3.2 that caused requirements to ignore default values is confusing and not properly documented[^1]. If a flag has a default value, then not passing the flag should behave the same as passing it with the default value. Yet as the example I gave demonstrates, `cli --use-color` parses just fine but the equivalent `cli --use-color --color=red` would have errored out saying that the `--shade` flag was expected too. That error is what should have happened with `cli --use-color`, but doesn't because the requirements ignore the default values. And it's very weird that requirements would ignore default values, because behavior shouldn't change just because an implicit flag was made explicit.

In simpler cases, the fact that requirements ignore default values can be worked around, e.g. saying `arg.required_if_eq("color", "red").required_unless_present("color")` can be used to say that the arg is required if `--color` is omitted or is set to `red` (which mimics requirements obeying defaults if `--color` defaults to `red`). But as soon as you use `default_value_if`, there's no way to handle that with current behavior.

The ideal solution here is to revert to the pre-3.2 behavior, where requirements obey defaults[^2]. That's a breaking change though, and so would require doing in a v5. I'm proposing that this can be done via an app setting or arg setting instead, to allow for opting into the fixed behavior (and then also just fixing it for v5, if a v5 happens).

[^1]: `required_if_eq` uses the phrase "is present at runtime" but it's not obvious that this means "was provided explicitly by the user", because a default value affects runtime behavior.
[^2]: I think ideally conflicts would still ignore defaults, I'm not sure offhand what the pre-3.2 behavior there was with conflicts but it makes sense to me that default values would be overridden by explicitly-provided conflicting args, because otherwise setting a default value on an arg would make conflicting args unusable.

---

_Comment by @epage on 2025-08-07 20:17_

A straight revert to the pre-3.2 behavior is not happening.  That had fundamental problems that were being addressed as mentioned in #3838.

I can think of two angles to look at your problem
- This is less about defaults about about implied arguments which may need different semantics.  `Command::replace` (#2836) was one attempt at this but was very crude which led to several blockers.  I thought we had another issue for this but I'm not finding it
- This seems to be less about `required` and more about `required_if_eq` and in that case, it might make sense to check the value, independent of the `ValueSource`.

---
