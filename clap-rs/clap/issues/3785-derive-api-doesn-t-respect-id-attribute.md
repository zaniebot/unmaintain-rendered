```yaml
number: 3785
title: "Derive API doesn't respect `id` attribute"
type: issue
state: closed
author: survived
labels:
  - C-bug
  - E-easy
  - A-derive
assignees: []
created_at: 2022-06-03T11:32:49Z
updated_at: 2022-08-09T19:31:46Z
url: https://github.com/clap-rs/clap/issues/3785
synced_at: 2026-01-12T16:14:15Z
```

# Derive API doesn't respect `id` attribute

---

_@survived_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

3.1.18

### Minimal reproducible code

[playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=3823c43cec7e251150faf0af365e1e2b)
```rust
#[derive(clap::Parser)]
struct App {
    #[clap(id = "some-argument", long = "some-argument")]
    field: String,
}

fn main() {
    let _parsed: App = clap::Parser::parse_from(["app", "--some-argument", "123"]);
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

Panics with a message:

```
thread 'main' panicked at '`field` is not a name of an argument or a group.
Make sure you're using the name of the argument itself and not the name of short or long flags.', src/main.rs:5:5
```

### Expected Behaviour

In the code above, you can replace `id` with `name` and it will work just fine. [`Arg::id`] was introduced in 3.1.0, it's supposed to replace `Arg::name`. However, derive api doesn't work with `id` attribute, it keeps supporting `name` attribute that refers to deprecated method.

I expect that `id` attribute will have the same behaviour as `name`. Moreover, to be aligned with changes in library API, derive API ideally should yield a warning that `name` attribute is deprecated.

[`Arg::id`]: https://docs.rs/clap/latest/clap/struct.Arg.html#method.id

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @survived on 2022-06-03 11:32_

---

_Label `A-derive` added by @epage on 2022-06-03 13:30_

---

_Label `E-easy` added by @epage on 2022-06-03 13:30_

---

_Comment by @danielparks on 2022-07-31 04:55_

I wrote a quick fix for this, but I have a couple of questions before I submit a PR:

1. Should the `name` and `id` attributes really do the same thing? I see that `Arg` and `Command` both have separate `name` and `id` fields, though I haven’t taken the time to figure out what exactly they do.
2. Should this change only apply to fields? Given that `Command` has an `id` field as well I’m inclined to make it universal, which seems simpler.
3. Would you prefer a separate PR to update documentation, or should I combine them into one?

---

_Comment by @epage on 2022-08-01 17:11_

> Should the name and id attributes really do the same thing? I see that Arg and Command both have separate name and id fields, though I haven’t taken the time to figure out what exactly they do.

While this is murkier in clap v3, clap v4 is working to make this more obvious (some deprecations move us in this direction): `name` and `id` are distinct.  a command's name is both an identifier and a user-visible value.  An argument's ID is mostly just an identifier though it might be used as a default for `Arg::value_name`. 

Us conflating the two has led to maintainer confusion in some of the design and user confusion in how they are used.

> Should this change only apply to fields? Given that Command has an id field as well I’m inclined to make it universal, which seems simpler.

Could you clarify what you mean?  Are you speaking in terms of should this apply to both Arg and Command?  Yes, we should respect explicit overrides in both

> Would you prefer a separate PR to update documentation, or should I combine them into one?

Commits should be atomic.  If the code is deviating from the documentation, the documentation should be updated at the same time.

What documentation updates do you see being needed?

---

_Comment by @danielparks on 2022-08-03 03:31_

Thanks for the quick response. I‘m not very familiar with clap’s internals, so I appreciate your clarifications.

It sounds like the attribute behavior should be different for Command and Arg. Specifically:

1. Command: set only the `id` field. This will probably involve adding a public `id()` method to the `Command` struct.
2. Arg: set both `id` and `name`, much as the `name` attribute does now.

There are a few derive reference doc updates needed:

1. Add `id` attribute for Args.
2. Depreciate `name` attribute on Args.
3. Add `id` attribute for Commands.

---

_Comment by @epage on 2022-08-03 13:11_

> Command: set only the id field. This will probably involve adding a public id() method to the Command struct.

From a users perspective, `Command` only has `name`.  Ignore the `id` field (it will likely go away in clap v4).

> Arg: set both id and name, much as the name attribute does now.

From a users perspective, `Arg` only has `id`.  Ignore the `name` field (it will likely go away in clap v4)

Something that confuses the matter further is `Arg` also has `value_name[s]`, e.g. https://github.com/clap-rs/clap/issues/3709

---

_Comment by @danielparks on 2022-08-04 03:25_

Seems like we should disallow the `id` attribute on `Command`, then? Unfortunately, that’s going to be either messy or complicated.

[Here is the solution](https://github.com/clap-rs/clap/compare/master...danielparks:clap:issue-3785-derive-id-attr) I have that effectively just recognizes the `id` attribute as an alias for `name`.

---

_Comment by @epage on 2022-08-09 15:57_

I'd say that solution is good enough for now.  Note that we would merge for master first which would has all deprecated functionality removed.  We'd want to add back in the `name` attribute in the docs if/when its backported to v3-master.

If we want to go for the error, short term, we could augment `push_attrs` with what type of attribute it is `Command`, `Arg`, etc) and do error checking in the case for `PushMethod`

Longer term, I'm going to be splitting up the attributes like `#[command(name = "foo")]` and it'll be easier to do such checks

---

_Referenced in [clap-rs/clap#4049](../../clap-rs/clap/pulls/4049.md) on 2022-08-09 18:50_

---

_Comment by @danielparks on 2022-08-09 19:02_

Thanks for all of your clarifications and help. I’ve submitted PR #4049; let me know if it needs updates.

---

_Closed by @epage on 2022-08-09 19:31_

---

_Referenced in [clap-rs/clap#4068](../../clap-rs/clap/pulls/4068.md) on 2022-08-12 04:52_

---
