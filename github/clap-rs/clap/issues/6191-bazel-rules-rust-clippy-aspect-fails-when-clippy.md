---
number: 6191
title: "Bazel rules_rust clippy aspect fails when clippy::allow_attributes_without_reason rule set to forbid"
type: issue
state: closed
author: George-Hulme
labels:
  - C-bug
  - A-derive
  - S-triage
assignees: []
created_at: 2025-12-02T15:14:35Z
updated_at: 2025-12-02T19:04:58Z
url: https://github.com/clap-rs/clap/issues/6191
synced_at: 2026-01-07T13:12:20-06:00
---

# Bazel rules_rust clippy aspect fails when clippy::allow_attributes_without_reason rule set to forbid

---

_Issue opened by @George-Hulme on 2025-12-02 15:14_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.91.1 (ed61e7d7e 2025-11-07)

### Clap Version

4.5.53

### Minimal reproducible code

The bazel aspect, @rules_rust//rust:defs.bzl%rust_clippy_aspect, fails to build when `#[derive(clap::Parser)]` is used on a struct in our repository. This is due to us setting `clippy::allow_attributes_without_reason` and `clippy::unwrap_used` to "forbid" to help ensure all allow cases are documented properly and to help ensure all results / options are handled properly.

The root cause appears to be the `#[allow(clippy::restriction)]` annotation in `clap_derive` (See: [here](https://github.com/clap-rs/clap/blob/53a69215e481c237bff5599f93bbcc7bdc6218c5/clap_derive/src/derives/into_app.rs#L40)).
There are many allowed lint groups here which I imagine will cause similar issues with many other lint groups and rules. Perhaps we should hide these behind a feature and use `cfg_attr` to add them.

### Steps to reproduce the bug with the above code

Setup a bazel `rust_binary` target using `rules_rust`. Set the following in ".bazelrc":
```
build --aspects=@rules_rust//rust:defs.bzl%rust_clippy_aspect
build:clippy --output_groups=clippy_checks
build --@rules_rust//rust/settings:clippy_flags=-Fclippy::allow_attributes_without_reason
```
In the rust project source, derive `clap::Parser` for a struct.
Finally, run `bazel build --config=clippy //...` and observe error.

### Actual Behaviour

`bazel build --config=clippy //...` fails with:
```
error[E0453]: allow(clippy::restriction) incompatible with previous forbid
 --> service/src/main.rs:4:10
  |
4 | #[derive(Parser)]
  |          ^^^^^^ overruled by previous forbid
  |
  = note: `forbid` lint level was set on command line
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)

error: aborting due to 1 previous error
```

### Expected Behaviour

`bazel build --config=clippy //...` builds successfully.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @George-Hulme on 2025-12-02 15:14_

---

_Label `S-triage` added by @George-Hulme on 2025-12-02 15:14_

---

_Comment by @epage on 2025-12-02 15:32_

Is there a reason you are doing `forbid`, rather than `deny`?

The use of `forbid` generally doesn't compose well with macros that can't always generate pristine code (especially under every clippy lint option that may exist).

In this particular case, `reason` I believe would require an MSRV bump.

---

_Label `A-derive` added by @epage on 2025-12-02 15:32_

---

_Comment by @George-Hulme on 2025-12-02 15:54_

The reason we use forbid is to ensure that these can't be overridden within our internal packages.
Adding a reason here wouldn't fix the issue. The root cause is that the derive always adds the `allow(clippy::restrictions)` attribute which includes the lint rules we have forbidden.
Using `#[cfg_attr(feature = "...", allow(...))]` and enabling said feature by default would allow us, and others, to opt-out of adding these allow attributes and manage which rules we allow ourselves, without affecting the majority of users.

---

_Comment by @epage on 2025-12-02 15:58_

Features have a cost and we're not going to add a feature for controlling our lints

---

_Comment by @George-Hulme on 2025-12-02 18:54_

I suppose the workaround of not using `clap::Parser` and saying that it's forbid-incompatible for these lints will have to do. We have worked around it by deriving the command structure manually

---

_Comment by @epage on 2025-12-02 19:04_

As I'm not seeing a good path forward for accommodating bazel's choice to forbid some lints, I'm going to go ahead and close this.  If people can come up with an acceptable path forward in the future, we can re-evaluate then

---

_Closed by @epage on 2025-12-02 19:04_

---
