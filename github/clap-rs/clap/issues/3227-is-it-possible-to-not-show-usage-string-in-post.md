---
number: 3227
title: Is it possible to not show usage string in post validation errors?
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-help
  - E-medium
assignees: []
created_at: 2021-12-28T00:40:52Z
updated_at: 2022-02-07T21:37:28Z
url: https://github.com/clap-rs/clap/issues/3227
synced_at: 2026-01-07T13:12:19-06:00
---

# Is it possible to not show usage string in post validation errors?

---

_Issue opened by @epage on 2021-12-28 00:40_

Maintainer's notes:
- If we move error formatting to display time, we can provide all of the information the user needs that the user can build the error as they need (see also #2628)
--
### Discussed in https://github.com/clap-rs/clap/discussions/3226

<div type='discussions-op-text'>

<sup>Originally posted by **ducaale** December 27, 2021</sup>
In Structopt/Clap 2.x, errors from both initial-parsing and post-processing errors didn't include the Usage string. However, in Clap 3.x, post-processing errors include the usage string. Is there a way to opt-out of this new behavior in Clap 3.x?

### Structopt/Clap 2.x
```rust
// structopt = "0.3"
// anyhow = "1.0"

use std::str::FromStr;
use std::time::Duration;

use anyhow::anyhow;
use structopt::clap::{Error, ErrorKind};
use structopt::StructOpt;

pub struct Timeout(Duration);

impl FromStr for Timeout {
    type Err = anyhow::Error;

    fn from_str(sec: &str) -> anyhow::Result<Timeout> {
        let pos_sec: f64 = match sec.parse::<f64>() {
            Ok(sec) if sec.is_sign_positive() => sec,
            _ => return Err(anyhow!("Invalid seconds as connection timeout")),
        };

        let dur = Duration::from_secs_f64(pos_sec);
        Ok(Timeout(dur))
    }
}

#[derive(StructOpt)]
struct Cli {
    #[structopt(long, short)]
    config: Option<String>,

    #[structopt(long, short)]
    timeout: Option<Timeout>,
}

fn main() {
    let _ = Cli::from_args();
    Error::with_description(
        "Can't do relative and absolute version change",
        ErrorKind::ArgumentConflict,
    )
    .exit();
}
```

```
$ cargo run
error: Can't do relative and absolute version change

$ cargo run -- --timeout=-1
error: Invalid value for '--timeout <timeout>': Invalid seconds as connection timeout
```

### Clap 3.x
```rust
// clap = { version = "3.0.0-rc.9", features = ["derive"] }
// anyhow = "1.0"

use std::str::FromStr;
use std::time::Duration;

use anyhow::anyhow;
use clap::{ErrorKind, IntoApp, Parser};

pub struct Timeout(Duration);

impl FromStr for Timeout {
    type Err = anyhow::Error;

    fn from_str(sec: &str) -> anyhow::Result<Timeout> {
        let pos_sec: f64 = match sec.parse::<f64>() {
            Ok(sec) if sec.is_sign_positive() => sec,
            _ => return Err(anyhow!("Invalid seconds as connection timeout")),
        };

        let dur = Duration::from_secs_f64(pos_sec);
        Ok(Timeout(dur))
    }
}

#[derive(Parser)]
struct Cli {
    #[clap(long, short)]
    config: Option<String>,

    #[structopt(long, short)]
    timeout: Option<Timeout>,
}

fn main() {
    let _ = Cli::parse();
    let mut app = Cli::into_app();
    app.error(
        ErrorKind::ArgumentConflict,
        "Can't do relative and absolute version change",
    )
    .exit();
}
```

```
$ cargo run
error: Can't do relative and absolute version change

USAGE:
    temp-clap [OPTIONS]

For more information try --help

$ cargo run -- --timeout=-1
error: Invalid value for '--timeout <TIMEOUT>': Invalid seconds as connection timeout

For more information try --help
```

</div>

---

_Label `C-bug` added by @epage on 2021-12-28 00:40_

---

_Label `A-help` added by @epage on 2021-12-28 00:40_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-28 00:40_

---

_Comment by @epage on 2021-12-28 00:45_

I assume you are specifically referring to errors reported by `App::error`?

`App::error` was added in https://github.com/clap-rs/clap/pull/2890.  In it, I did not have any specific errors in mind to be targeting as a similar use case and saw usage being reported in errors and included it.

I do not see any comments saying why some errors include usage and some do not.  It might be interesting to try to infer this by looking at what errors include usage.

---

_Added to milestone `3.0` by @epage on 2021-12-28 00:45_

---

_Comment by @epage on 2021-12-28 00:46_

Marked this as 3.0.  I don't think we'd consider this breaking to change and doesn't seem the most critical, so we probably won't be blocking 3.0 on this but at least want to track 3.0 related items.

---

_Comment by @epage on 2021-12-28 15:44_

With usage
- argument_conflict
- empty_value
- no_equals
- invalid_value (possible values)
- invalid_subcommand (suggestion available)
- missing_required_argument
- missing_subcommand
- invalid_utf8
- too_many_occurrences
- too_many_values
- too_few_values
- wrong_number_of_values
- unexpected_multiple_usage
- unknown_argument
- unnecessary_double_dash

Without usage
- unrecognized_subcommand (no suggestion available)
- value_validation (custom validator or type conversion)
  - reported by `ArgMatches`, no usage available
  - reported by validator, usage available
- argument_not_found_auto
  - reported by `ArgMatches`, no usage available

Overall, I'm not seeing a pattern
- invalid utf8, invalid possible value, and custom validation fail all deal with value validation but one isn't like the other
- subcommand suggestion vs no suggestion is somewhat similar like possible values vs custom validation
- Most that report a usage will demonstrate what is correct with usage, but not all
- Without usage is mostly where usage isn't available.  One case where usage is available, shares a code with where usage isn't available.  That just leaves "unrecognized_subcommand"

It looks like clap2 is similar, except for `Error::with_description` -> `App::error`.  Note: `Error::raw` (without ever calling `Error::format`) is similar to `Error::with_description` though it will never be colored.

---

_Comment by @ducaale on 2021-12-28 19:35_

>@ducaale mind posting on #3227 what your expectations are for when usage should be shown?

If I were to put myself in the user's shoes, I would find the `USAGE` string most helpful when it references a required positional argument. I also wouldn't mind if errors related to required flags/options display a usage string:

#### Missing positional argument
```
error: The following required arguments were not provided:
    <URL>

USAGE:
    xh [OPTIONS] <METHOD> <URL>

For more information try --help
```

#### Conflict between a positional argument and a flag
```
error: The argument '--delete' cannot be used with '<base>'

USAGE:
    clap-test <base|--delete>

For more information try --help
```

#### Positional argument type conversion error 
(I think it makes sense to print `USAGE` in this case so the user can understand what `<NUMBER>` refers to)
```
error: Invalid value for '<NUMBER>': invalid digit found in string

For more information try --help
```

#### Missing required flag/option
```
error: The following required arguments were not provided:
    --config <CONFIG>

USAGE:
    temp-clap [OPTIONS] --config <CONFIG> <NUMBER>

For more information try --help
```
---
On the other hand, optional flags/options are often condensed in the usage string so I wouldn't find the `USAGE` string that helpful:
```
error: Found argument '--js' which wasn't expected, or isn't valid in this context

USAGE:
    xh [OPTIONS] <URL>

For more information try --help
```

As for custom errors, they might or might not need a `USAGE` string depending on whether the error references a positional argument or not. For example, [xh](https://github.com/ducaale/xh) displays this error if the URL cannot be extracted be after some custom post-processing of positional arguments:

```
$ xh get
error: Missing URL
```

But with Clap 3.x, I like how the previous error message is suddenly more helpful plus the suggestion to use `--help` :
```
$ xh get
error: Missing <URL>

USAGE:
    xh [OPTIONS] <[METHOD] URL> [--] [REQUEST_ITEM]...

For more information try --help
```

But in another case, I found the `USAGE` string to be a bit redundant:
```
$ xh httpbin.org/json a:=--1
error: "a:=--1": invalid number at line 1 column 2

USAGE:
    xh [OPTIONS] <[METHOD] URL> [--] [REQUEST_ITEM]...

For more information try --help
```

---

_Comment by @epage on 2021-12-30 19:47_

@ducaale 
> On the other hand, optional flags/options are often condensed in the usage string so I wouldn't find the `USAGE` string that helpful:
> 
> ```
> error: Found argument '--js' which wasn't expected, or isn't valid in this context
> 
> USAGE:
>     xh [OPTIONS] <URL>
> 
> For more information try --help
> ```

We can fix that:
- We should explore an option to not collapse optional parameters
- For some other errors, we customize their usage based on the users current command line.  We can look into forcing the flag that failed validation to show up.

> But in another case, I found the `USAGE` string to be a bit redundant:
> 
> ```
> $ xh httpbin.org/json a:=--1
> error: "a:=--1": invalid number at line 1 column 2
> 
> USAGE:
>     xh [OPTIONS] <[METHOD] URL> [--] [REQUEST_ITEM]...
> 
> For more information try --help
> ```

if I'm understanding this case, `:=` chooses the parser and `--1` is an invalid value, causing a failure?

Part of me wonders if that is a bit specialized of a case, if `value_name` normally provides enough detail of what is expected (e.g. `NUM`, `PORT`) that it provides a little more of a hint as to what is expected.

Additionally, associating a name with it helps people know where in `--help` to look for more details.

With that said, this depends on how clear it is that `a:=--1` is `REQUEST_ITEM`.  For someone making the command from scratch, I think it works (though maybe the hint won't be needed as much?).  For someone copying from stackoverflow, the linkage might not be fully clear.  I wonder if we should include the `value_name` in the error itself.

---

_Removed from milestone `3.0` by @epage on 2021-12-30 20:37_

---

_Comment by @ducaale on 2021-12-30 21:15_

> We should explore an option to not collapse optional parameters

Sound good to me. I assume you meant the particular optional parameter that failed, correct?

>With that said, this depends on how clear it is that a:=--1 is REQUEST_ITEM. For someone making the command from scratch, I think it works (though maybe the hint won't be needed as much?). For someone copying from stackoverflow, the linkage might not be fully clear. I wonder if we should include the value_name in the error itself.

That is actually a good idea. Here is what the same error would look like if it included the `value_name`:
```
error: Invalid value for '<REQUEST_ITEM>': "a:=--1" invalid number at line 1 column 2

USAGE:
    xh [OPTIONS] <[METHOD] URL> [--] [REQUEST_ITEM]...

For more information try --help
```

Thanks for the tip!

---

_Referenced in [ducaale/xh#216](../../ducaale/xh/pulls/216.md) on 2021-12-30 21:20_

---

_Label `S-waiting-on-design` removed by @epage on 2022-02-02 20:03_

---

_Label `E-medium` added by @epage on 2022-02-02 20:03_

---

_Added to milestone `3.1` by @epage on 2022-02-02 20:04_

---

_Referenced in [clap-rs/clap#3402](../../clap-rs/clap/pulls/3402.md) on 2022-02-04 18:27_

---

_Comment by @epage on 2022-02-04 18:28_

#3402 is exposing the inputs to error formatting so people can customize it as they wish without having to rely on options to control every piece of behavior.

Think this would work for your case?

---

_Closed by @epage on 2022-02-07 21:37_

---
