```yaml
number: 2151
title: "Provide access to the original error for `ValueValidation` errors"
type: issue
state: closed
author: Nemo157
labels: []
assignees: []
created_at: 2020-10-05T10:38:25Z
updated_at: 2020-10-14T06:06:23Z
url: https://github.com/clap-rs/clap/issues/2151
synced_at: 2026-01-12T16:14:12Z
```

# Provide access to the original error for `ValueValidation` errors

---

_@Nemo157_

### Describe your use case

I have a `FromStr` implementation that returns a chained error providing more context when decoding an argument fails. When this error is returned `clap` (using v3's `derive` and `Clap::try_parse`) serializes _just_ the outer context into a string and attaches that to the `ValueValidation` error returned, removing the more important information contained inside the inner error.

Here's a (not that small) minimal representation of the issue:

```rust
use std::{fmt, str::FromStr, error::Error};
use clap::Clap;

#[derive(Debug, Copy, Clone)]
struct Foo;

#[derive(Debug, Copy, Clone)]
struct InnerError;

#[derive(Debug, Copy, Clone)]
struct OuterError(InnerError);

impl Error for InnerError {}
impl Error for OuterError {
    fn source(&self) -> Option<&(dyn Error + 'static)> {
        Some(&self.0)
    }
}

impl fmt::Display for InnerError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.write_str("inner error")
    }
}

impl fmt::Display for OuterError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        f.write_str("outer error")
    }
}

impl FromStr for Foo {
    type Err = OuterError;

    fn from_str(_: &str) -> Result<Self, Self::Err> {
        Err(OuterError(InnerError))
    }
}

#[derive(Debug, Clap)]
struct Args {
    foo: Foo,
}

fn main() -> Result<(), Box<dyn Error>> {
    dbg!(Args::try_parse())?;

    Ok(())
}
```

Running this with `cargo run -- test` we can see that the returned error only contains:

```
        kind: ValueValidation,
        info: [
            "<foo>",
            "test",
            "outer error",
        ],
```

losing the `InnerError` completely.

### Describe the solution you'd like

Add an `Option<Box<dyn Error>>` onto the `Error` struct and support the `Error::source` method to get access to this error.

This would cause (additional) issues with the current `Display` implementation, when using an error reporter such as `anyhow::Error` which chains the causes on; you would get duplicate error messages because of `impl Display for clap::Error` printing the `cause` already. (Additional because you already get duplicate `Error: ` prefixes from `clap::Error` printing this prefix itself).

---

_Label `T: new feature` added by @Nemo157 on 2020-10-05 10:38_

---

_Comment by @TeXitoi on 2020-10-05 13:33_

For me, the solution is to change the Display implementation or your OuterError to what you want.

---

_Comment by @Nemo157 on 2020-10-05 13:47_

That would be against the modern error handling standards where an error should only print its own details and rely on an error reporting implementation (such as `anyhow` or `eyre`, or `clap`'s internal reporting when using `Args::parse()`) to chain on the context when the error is being shown to a user.

EDIT: And be against long-time error guidelines such as [C-GOOD-ERR](https://rust-lang.github.io/api-guidelines/interoperability.html#c-good-err) which says

> The error message given by the Display representation of an error type should be lowercase without trailing punctuation, and typically concise.

---

_Comment by @TeXitoi on 2020-10-05 15:12_

Then, returns a anyhow::Result in FronStr, and if that's still not good, use a custom validator/parser that returns the kind of error you want. I don't think that's clap work to do complicated error formating.

---

_Comment by @Nemo157 on 2020-10-05 15:23_

I do return an `anyhow::Result` in my real code, but that obeys the error standards and doesn't print the `source` in its `Display` impl either (it only prints the source-chain when actually printing to the user).

> I don't think that's clap work to do complicated error formating.

That's precisely why I believe `clap` should give access to the entire error structure, so that other code can perform complicated error formatting using all the details available to it, rather than being reduced to printing a string that `clap` has formatted from the internal error.

---

_Added to milestone `3.0` by @pksunkara on 2020-10-05 15:24_

---

_Comment by @Nemo157 on 2020-10-05 15:29_

Quickly looking into what would be necessary to implement this, there would need to be at least one breaking change of changing `Arg::validator` to require `E: Error + Send + Sync` instead of `E: ToString` (I'm not sure whether `validator_os` would change as well, it seems odd that it requires a `String` error currently).

---

_Comment by @pksunkara on 2020-10-05 15:30_

I tagged this as 3.0 in case we want to decide on changing the API before release.

---

_Comment by @Nemo157 on 2020-10-05 16:01_

If you want I can prepare a PR with this change to look at, I just quickly hacked it in to test the behaviour and it works well with both `anyhow` and `color-eyre` error reporting.

---

_Comment by @pksunkara on 2020-10-05 16:10_

Sounds good

---

_Comment by @pksunkara on 2020-10-09 16:35_

@Nemo157 Any update with the PR?

---

_Label `C: errors` added by @pksunkara on 2020-10-09 23:25_

---

_Comment by @Nemo157 on 2020-10-12 12:26_

Sorry, I [prepared the change](https://github.com/clap-rs/clap/compare/master...Nemo157:error-source) and got it passing tests, but I don't remember if there was anything else I needed to check before opening a PR, I'll try to get it open tonight.

---

_Referenced in [clap-rs/clap#2169](../../clap-rs/clap/pulls/2169.md) on 2020-10-12 17:56_

---

_Closed by @bors[bot] on 2020-10-14 06:06_

---

_Referenced in [rust-lang/project-error-handling#27](../../rust-lang/project-error-handling/issues/27.md) on 2020-12-25 10:32_

---
