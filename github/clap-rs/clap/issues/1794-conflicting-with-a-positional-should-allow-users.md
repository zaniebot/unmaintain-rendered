---
number: 1794
title: Conflicting with a positional should allow users to skip it, passing in another positional instead
type: issue
state: open
author: nayeemrmn
labels:
  - C-bug
  - S-waiting-on-decision
  - A-help
  - A-parsing
assignees: []
created_at: 2020-04-08T13:18:55Z
updated_at: 2025-08-15T13:44:16Z
url: https://github.com/clap-rs/clap/issues/1794
synced_at: 2026-01-07T13:12:19-06:00
---

# Conflicting with a positional should allow users to skip it, passing in another positional instead

---

_Issue opened by @nayeemrmn on 2020-04-08 13:18_

<!-- Issuehunt Badges -->
[<img alt="Issuehunt badges" src="https://img.shields.io/badge/IssueHunt-%2420%20Funded-%2300A156.svg" />](https://issuehunt.io/r/clap-rs/clap/issues/1794)
<!-- /Issuehunt Badges -->


<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

`rustc 1.42.0 (b8cedc004 2020-03-09)`

### Code

```rust
use clap;
use clap::Arg;
use clap::ArgGroup;

fn main() {
  let app = clap::App::new("hello")
    .bin_name("deno")
    .arg(
      Arg::with_name("option1")
        .long("option1")
        .takes_value(false),
    )
    .arg(
      Arg::with_name("pos1")
        .takes_value(true),
    )
    .group(
      ArgGroup::with_name("arg1")
        .args(&["pos1", "option1"])
        .required(true),
    )
    .arg(
      Arg::with_name("pos2")
        .takes_value(true)
    );

    match app.get_matches_from_safe(std::env::args().collect::<Vec<String>>()) {
      Ok(_) => (),
      Err(err) => err.exit(),
    }
}
```

### Steps to reproduce the issue

`cargo run -- -h`

### Affected Version of `clap*`

2.33.0

### Actual Behavior Summary

```
USAGE:
    deno <pos1|--option1> [pos1]
```

`cargo run -- --option1 abcd` binds `abcd` to `pos1` and complains about conflict between `option1` and `pos1`

Blocks https://github.com/denoland/deno/pull/4635, ref https://github.com/denoland/deno/pull/4635#issuecomment-610548655.

### Expected Behavior Summary

```
USAGE:
    deno <pos1|--option1> [pos2]
```

`cargo run -- --option1 abcd` should bind `abcd` to `pos2`. In other words, it should detect `--option1` having been given as an option and accordingly skip `pos1` which `--option1` is grouped with.

--

If this is actually intended I welcome suggestions as to how I can get the above behaviour. But what else should be the semantics of grouping positional args with options?


<!-- Issuehunt content -->

---

<details>
<summary>
<b>IssueHunt Summary</b>
</summary>


### Backers (Total: $20.00)

- [<img src='https://avatars.githubusercontent.com/u/174703?v=4' alt='pksunkara' width=24 height=24> pksunkara](https://issuehunt.io/u/pksunkara) ($20.00)


#### [Become a backer now!](https://issuehunt.io/r/clap-rs/clap/issues/1794)
#### [Or submit a pull request to get the deposits!](https://issuehunt.io/r/clap-rs/clap/issues/1794)
### Tips

- Checkout the [Issuehunt explorer](https://issuehunt.io/r/clap-rs/clap/) to discover more funded issues.
- Need some help from other developers? [Add your repositories](https://issuehunt.io/r/new) on IssueHunt to raise funds.
</details>
<!-- /Issuehunt content-->

---

_Label `T: bug` added by @nayeemrmn on 2020-04-08 13:18_

---

_Label `C: arg groups` added by @pksunkara on 2020-04-09 08:37_

---

_Label `C: args` added by @pksunkara on 2020-04-09 08:37_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 08:37_

---

_Comment by @pksunkara on 2020-04-09 08:38_

Yeah, this definitely looks like a bug. cc @CreepySkeleton 

---

_Referenced in [denoland/deno#4635](../../denoland/deno/pulls/4635.md) on 2020-04-09 11:33_

---

_Label `M: work in progress` added by @CreepySkeleton on 2020-04-21 11:20_

---

_Comment by @CreepySkeleton on 2020-04-21 11:34_

Yes. I would expect all of the following to be valid invocations:
```
app pos1 pos2
app --option1=val
app --option1 val pos2 <- THIS FAILS
```

Working on the fix, might take a while.

---

_Comment by @nayeemrmn on 2020-04-21 11:39_

> ```
> app --option1=val
> app --option1 val pos2 <- THIS FAILS
> ```

Small correction that `--option1` doesn't take a value in my example but that would make for a better one.

---

_Referenced in [clap-rs/clap#1856](../../clap-rs/clap/pulls/1856.md) on 2020-04-23 17:28_

---

_Label `M: work in progress` removed by @CreepySkeleton on 2020-04-23 17:31_

---

_Closed by @bors[bot] on 2020-04-24 11:18_

---

_Comment by @ldm0 on 2020-11-09 18:27_

Should be reopen. The usage output is still not correct.

---

_Reopened by @pksunkara on 2020-11-09 19:40_

---

_Referenced in [clap-rs/clap#2245](../../clap-rs/clap/pulls/2245.md) on 2020-12-08 17:06_

---

_Closed by @pksunkara on 2020-12-08 17:52_

---

_Comment by @pksunkara on 2021-01-16 14:08_

@ldm0 You were saying that the original fix is wrong. Can you point out a test case why it is wrong?

---

_Referenced in [clap-rs/clap#2297](../../clap-rs/clap/pulls/2297.md) on 2021-01-17 14:19_

---

_Comment by @ldm0 on 2021-01-19 19:11_

@pksunkara  Check this:
```rust
let m = clap::App::new("hello")
    .bin_name("deno")
    .arg(Arg::new("pos1").takes_value(true))
    .arg(Arg::new("option2").long("option2").takes_value(false))
    .arg(Arg::new("pos2").takes_value(true))
    .group(
        ArgGroup::new("arg2")
            .args(&["pos2", "option2"])
            .required(true),
    )
    .try_get_matches();

dbg!(m);
```
with:
```
cargo run -- --option2 abcd
```

You will notice it complains about conflict between `option2` and `pos2`. However the abcd should be placed in pos1.

---

_Referenced in [clap-rs/clap#2309](../../clap-rs/clap/pulls/2309.md) on 2021-01-23 07:50_

---

_Reopened by @pksunkara on 2021-01-27 18:48_

---

_Comment by @issuehunt-oss[bot] on 2021-01-27 18:50_

[@pksunkara](https://issuehunt.io/u/pksunkara) has funded $20.00 to this issue.

---
- Submit pull request via [IssueHunt](https://issuehunt.io/repos/31315121/issues/1794) to receive this reward.
- Want to contribute? Chip in to this issue via [IssueHunt](https://issuehunt.io/repos/31315121/issues/1794).
- Checkout the [IssueHunt Issue Explorer](https://issuehunt.io/issues) to see more funded issues.
- Need help from developers? [Add your repository](https://issuehunt.io/r/new) on IssueHunt to raise funds.

---

_Label `:dollar: Funded on Issuehunt` added by @issuehunt-oss[bot] on 2021-01-27 18:50_

---

_Assigned to @ldm0 by @pksunkara on 2021-02-08 05:34_

---

_Comment by @dmaahs2017 on 2021-02-16 04:57_

Is this issue solved?

---

_Comment by @ldm0 on 2021-02-16 04:58_

> Is this issue solved?

Nope, currently.

---

_Comment by @pksunkara on 2021-05-26 11:13_

@ldm0 Can we focus on finishing this first? Or is it blocked by other stuff?

---

_Comment by @rami3l on 2021-08-05 22:37_

@pksunkara @ldm0 The example above now passes on `master` with `AppSettings::AllowMissingPositional`:

```rs
/// Test for https://github.com/clap-rs/clap/issues/1794.
#[test]
fn positional_args_with_options() {
    let app = clap::App::new("hello")
        .bin_name("deno")
        .setting(AppSettings::AllowMissingPositional)
        .arg(Arg::new("option1").long("option1").takes_value(false))
        .arg(Arg::new("pos1").takes_value(true))
        .group(
            ArgGroup::new("arg1")
                .args(&["pos1", "option1"])
                .required(true),
        )
        .arg(Arg::new("pos2").takes_value(true));

    let m = app.get_matches_from(&["deno", "--option1", "abcd"]);
    assert!(m.is_present("option1"));
    assert!(!m.is_present("pos1"));
    assert!(m.is_present("pos2"));
    assert_eq!(m.value_of("pos2"), Some("abcd"));
}
```

And the usage is correctly shown as:

```yml
USAGE:
    deno <pos1|--option1> [pos2]
```

This `AppSettings` option is required due to the logic here, which indicates that a `pos_counter` bump is only available under certain conditions (which is needed for the example to work):

https://github.com/clap-rs/clap/blob/8b6034e668de0933ab0f88ad14a60a8f1d79a172/src/parse/parser.rs#L483-L484

Is this intended?
- If so, I can make a PR to add this test and finally close this issue.
- If not, maybe we should change the API to allow this special case of `skipping` certain pos args?

---

_Referenced in [clap-rs/clap#2665](../../clap-rs/clap/pulls/2665.md) on 2021-08-05 23:16_

---

_Comment by @pksunkara on 2021-08-06 10:02_

@ldm0 what do you think?

---

_Comment by @ldm0 on 2021-08-07 18:05_

This works:
```rust
use clap;
use clap::Arg;
use clap::ArgGroup;
use clap::AppSettings;

fn main() {
    let app = clap::App::new("hello")
        .bin_name("deno")
        .setting(AppSettings::AllowMissingPositional)
        .arg(Arg::new("option1").long("option1").takes_value(false))
        .arg(Arg::new("pos1").takes_value(true))
        .arg(Arg::new("pos2").takes_value(true))
        .group(
            ArgGroup::new("arg1")
                .args(&["pos1", "option1"])
                .required(true),
        )
        .get_matches();
}
```

But this is not:
```rust
use clap;
use clap::Arg;
use clap::ArgGroup;
use clap::AppSettings;

fn main() {
    let app = clap::App::new("hello")
        .bin_name("deno")
        .setting(AppSettings::AllowMissingPositional)
        .arg(Arg::new("option1").long("option1").takes_value(false))
        .arg(Arg::new("pos1").takes_value(true))
        .arg(Arg::new("pos2").takes_value(true))
        .arg(Arg::new("pos3").takes_value(true))
        .group(
            ArgGroup::new("arg1")
                .args(&["pos1", "pos2", "option1"])
                .required(true),
        )
        .get_matches();
}
```

```rust
error: The argument '--option1' cannot be used with '<pos1>'

USAGE:
    deno [pos3] <pos1|pos2|--option1>

For more information try --help
```

I don't think this issue can be closed now.

---

_Removed from milestone `3.0` by @epage on 2021-10-16 18:08_

---

_Added to milestone `3.1` by @epage on 2021-10-16 18:08_

---

_Referenced in [epage/clapng#151](../../epage/clapng/issues/151.md) on 2021-12-06 20:12_

---

_Label `C: args` removed by @epage on 2021-12-08 19:57_

---

_Label `C: arg groups` removed by @epage on 2021-12-08 19:57_

---

_Label `A-help` added by @epage on 2021-12-08 19:57_

---

_Label `E-medium` added by @epage on 2021-12-09 22:05_

---

_Label `E-medium` removed by @epage on 2022-04-29 13:47_

---

_Label `E-hard` added by @epage on 2022-04-29 13:47_

---

_Comment by @epage on 2022-04-29 13:50_

I'm torn on this.  `AllowMissingPositional` is very limited solution.  The right way of doing this is to check for conflicts for positionals and skip them.  Right now, conflicts are only a validation concern and not a parse concern and I was hoping to keep it that way for our modularization work.  However, being able to control when to skip positionals in this ways is also associated with our desire to make clap more flexible.

Going to put this back in the design/decision status and going to remove the milestone.

---

_Removed from milestone `3.x` by @epage on 2022-04-29 13:50_

---

_Label `E-hard` removed by @epage on 2022-04-29 13:50_

---

_Label `S-waiting-on-decision` added by @epage on 2022-04-29 13:50_

---

_Referenced in [spkenv/spk#775](../../spkenv/spk/pulls/775.md) on 2023-06-29 17:13_

---

_Unassigned @ldm0 by @pksunkara on 2025-05-30 12:40_

---

_Label `:dollar: Funded on Issuehunt` removed by @pksunkara on 2025-05-30 12:40_

---

_Renamed from "Bug when grouping positional args with options" to "Conflicting with a positional should allow users to skip it, passing in another positional instead" by @epage on 2025-08-15 13:44_

---

_Label `A-parsing` added by @epage on 2025-08-15 13:44_

---

_Referenced in [clap-rs/clap#6105](../../clap-rs/clap/issues/6105.md) on 2025-08-15 13:50_

---
