```yaml
number: 3344
title: allow_invalid_utf8(true) required only in debug build
type: issue
state: closed
author: njaard
labels:
  - C-bug
  - A-validators
  - S-wont-fix
assignees: []
created_at: 2022-01-25T18:17:08Z
updated_at: 2022-02-02T20:00:36Z
url: https://github.com/clap-rs/clap/issues/3344
synced_at: 2026-01-12T16:14:14Z
```

# allow_invalid_utf8(true) required only in debug build

---

_@njaard_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.58.1

### Clap Version

3.0.12

### Minimal reproducible code

```rust
	use clap::Arg;
	let matches = clap::App::new("app")
		.arg(Arg::new("f")
			.takes_value(true)
		)
		.get_matches();
	matches.value_of_os("f");
```

This program fails like so:
```
thread 'main' panicked at 'Must use `Arg::allow_invalid_utf8` with `_os` lookups at `f`', src/main.rs:12:13
```

But only in a debug build.

### Steps to reproduce the bug with the above code

Succeeds: `cargo run --release -- blah`
Fails: `cargo run -- blah`

### Actual Behaviour

Is `allow_invalid_utf8(true)` actually required? i don't know

### Expected Behaviour

It should just work.

### Additional Context

Why is this option even required? It's a giant footgun.

If you really want to do that check, maybe change `takes_value` to `takes_string` and `takes_path`. But as it is, it serves no purpose.

### Debug Output

_No response_

---

_Label `C-bug` added by @njaard on 2022-01-25 18:17_

---

_Comment by @epage on 2022-01-25 18:44_

> Why is this option even required? It's a giant footgun.

Clap was more of a footgun before this existed

In v2, if you did `value_of` and the user passed an invalid utf-8 string, clap would panic.  Your options were one of
- Pass `AppSettings::StrictUtf8` which prevented invalid utf-8 when it was intended
- Always use `value_of_os` and do the conversion yourself
- Write your own validators and make sure your accesses matched your definitions with those validators

Now, `StrictUtf8` is the default and you can opt-out with `allow_invalid_utf8` on a per-argument basis.

The asserts serve a couple of purposes
- Help users know that  `value_of_os` should be paired with `allow_invalid_utf8` to be effective (if you have strict utf-8, `value_of_os` offers you nothing that `value_of` gives you, if you use allow invalid utf-8, `value_of` will panic on invalid characters
  - This eases the transition from clap2, eases general on-boarding, and lower maintenance effort (can make changes more confidently)
- Prepare the way for https://github.com/clap-rs/clap/issues/2683 which will be the ultimately solution for this

99% of clap's error reporting is done through debug asserts
- Clap assumes these are development errors, that there isn't a reason to recover from them, and that the majority of testing happens in debug mode (check out [App::debug_assert](https://docs.rs/clap/latest/clap/struct.App.html#method.debug_assert) though it doesn't help in this case, and [trycmd](https://docs.rs/trycmd/))
- We limit it to `debug` for faster smaller binary sizes and faster runtime performance

---

_Label `A-validators` added by @epage on 2022-01-25 18:45_

---

_Label `S-triage` added by @epage on 2022-01-25 18:45_

---

_Comment by @epage on 2022-02-02 20:00_

As this is by design and there aren't any more comments, I'm closing this out.  If there are any concerns, feel free to raise them

---

_Closed by @epage on 2022-02-02 20:00_

---

_Label `S-triage` removed by @epage on 2022-02-02 20:00_

---

_Label `S-wont-fix` added by @epage on 2022-02-02 20:00_

---

_Referenced in [FamilySearch/pewpew#99](../../FamilySearch/pewpew/pulls/99.md) on 2022-09-09 15:53_

---
