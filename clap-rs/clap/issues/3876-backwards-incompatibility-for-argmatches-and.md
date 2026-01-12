```yaml
number: 3876
title: Backwards incompatibility for ArgMatches and UnwindSafe
type: issue
state: closed
author: doivosevic
labels:
  - C-bug
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2022-06-27T17:50:39Z
updated_at: 2022-10-04T15:28:37Z
url: https://github.com/clap-rs/clap/issues/3876
synced_at: 2026-01-12T16:14:15Z
```

# Backwards incompatibility for ArgMatches and UnwindSafe

---

_@doivosevic_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.60

### Clap Version

between 3.1.18 and 3.2.6

### Minimal reproducible code

```rust


fn a(a: ArgMatches) {
    b(a)
}

fn b<T: UnwindSafe>(b: T) {
    
}
```


### Steps to reproduce the bug with the above code

cargo check

### Actual Behaviour

Compile should pass

### Expected Behaviour

Compile fails

### Additional Context

This looks like a backwards incompatibility

### Debug Output

```

error[E0277]: the type `(dyn std::any::Any + Sync + std::marker::Send + 'static)` may contain interior mutability and a reference may not be safely transferrable across a catch_unwind boundary
  --> index/src/utils.rs:31:7
   |
31 |     b(a)
   |     - ^ `(dyn std::any::Any + Sync + std::marker::Send + 'static)` may contain interior mutability and a reference may not be safely transferrable across a catch_unwind boundary
   |     |
   |     required by a bound introduced by this call
   |
   = help: the trait `RefUnwindSafe` is not implemented for `(dyn std::any::Any + Sync + std::marker::Send + 'static)`
   = note: required because of the requirements on the impl of `UnwindSafe` for `Arc<(dyn std::any::Any + Sync + std::marker::Send + 'static)>`
   = note: required because it appears within the type `parser::matches::any_value::AnyValue`
   = note: required because of the requirements on the impl of `UnwindSafe` for `Unique<parser::matches::any_value::AnyValue>`
   = note: required because it appears within the type `alloc::raw_vec::RawVec<parser::matches::any_value::AnyValue>`
   = note: required because it appears within the type `Vec<parser::matches::any_value::AnyValue>`
   = note: required because of the requirements on the impl of `UnwindSafe` for `Unique<Vec<parser::matches::any_value::AnyValue>>`
   = note: required because it appears within the type `alloc::raw_vec::RawVec<Vec<parser::matches::any_value::AnyValue>>`
   = note: required because it appears within the type `Vec<Vec<parser::matches::any_value::AnyValue>>`
   = note: required because it appears within the type `parser::matches::matched_arg::MatchedArg`
   = note: required because it appears within the type `indexmap::Bucket<clap::util::id::Id, parser::matches::matched_arg::MatchedArg>`
   = note: required because of the requirements on the impl of `UnwindSafe` for `Unique<indexmap::Bucket<clap::util::id::Id, parser::matches::matched_arg::MatchedArg>>`
   = note: required because it appears within the type `alloc::raw_vec::RawVec<indexmap::Bucket<clap::util::id::Id, parser::matches::matched_arg::MatchedArg>>`
   = note: required because it appears within the type `Vec<indexmap::Bucket<clap::util::id::Id, parser::matches::matched_arg::MatchedArg>>`
   = note: required because it appears within the type `indexmap::map::core::IndexMapCore<clap::util::id::Id, parser::matches::matched_arg::MatchedArg>`
   = note: required because it appears within the type `indexmap::map::IndexMap<clap::util::id::Id, parser::matches::matched_arg::MatchedArg>`
   = note: required because it appears within the type `ArgMatches`
note: required by a bound in `b`
  --> index/src/utils.rs:34:9
   |
34 | fn b<T: UnwindSafe>(b: T) {
   |         ^^^^^^^^^^ required by this bound in `b`
```

---

_Label `C-bug` added by @doivosevic on 2022-06-27 17:50_

---

_Comment by @epage on 2022-06-27 19:39_

I'm curious, how did you find this?

---

_Label `A-parsing` added by @epage on 2022-06-27 19:39_

---

_Label `S-waiting-on-decision` added by @epage on 2022-06-27 19:39_

---

_Comment by @doivosevic on 2022-06-27 19:40_

I had code which was working which is let's say a wrapper for executing binaries on worker machines and when someone from my org bumped clap in develop my code broke after rebasing

---

_Comment by @epage on 2022-06-27 19:40_

 Note: I'm assuming we'd need to add another trait bound.  It gets a little awkward making a breaking change to fix a breaking change but this would be unlikely to break someone.

---

_Referenced in [clap-rs/clap#3877](../../clap-rs/clap/pulls/3877.md) on 2022-06-28 01:27_

---

_Referenced in [clap-rs/clap#3878](../../clap-rs/clap/pulls/3878.md) on 2022-06-28 02:07_

---

_Comment by @epage on 2022-06-28 02:12_

Some thoughts in working on a fix (#3878)
- Auto-traits are a weird lot for ensuring compatibility on.  Ideally, we could say what auto-traits are assumed and which aren't in a user-facing way.  Instead we have to add compile-time tests for what auto traits we implement and don't.  The big concern is accidentally implementing an auto-trait that we don't provide a guarantee for.
- Its hard to decide whether we should intend a guarantee on this long term
- I seem to be stuck in the implementation with parts dealing with `Arc<dyn>`.  If we can't implement a fix for this, that puts us in an awkward space of either pulling the entire 3.2 release and breaking people or leaving it and letting this breakage continue (if we consider it a breakage).

---

_Comment by @doivosevic on 2022-06-28 08:12_

Can we then get an interface to get all the args from ArgMatches instead so that we can pass the args in some other format across this boundary?

---

_Comment by @epage on 2022-07-16 02:46_

That most likely is blocked on breaking changes, see https://github.com/clap-rs/clap/issues/1206

---

_Referenced in [rust-lang/cargo#374](../../rust-lang/cargo/issues/374.md) on 2022-07-25 12:40_

---

_Referenced in [obi1kenobi/cargo-semver-checks#32](../../obi1kenobi/cargo-semver-checks/issues/32.md) on 2022-08-07 02:31_

---

_Referenced in [obi1kenobi/cargo-semver-checks#33](../../obi1kenobi/cargo-semver-checks/pulls/33.md) on 2022-08-07 03:05_

---

_Referenced in [rust-lang/rust#100252](../../rust-lang/rust/issues/100252.md) on 2022-08-08 00:21_

---

_Comment by @epage on 2022-10-04 15:28_

As clap v4 is now out, I'm going ahead and closing this.

---

_Closed by @epage on 2022-10-04 15:28_

---

_Referenced in [clap-rs/clap#4347](../../clap-rs/clap/issues/4347.md) on 2022-10-04 15:32_

---

_Referenced in [libp2p/rust-libp2p#3312](../../libp2p/rust-libp2p/pulls/3312.md) on 2023-02-25 02:52_

---
