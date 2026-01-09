---
number: 1910
title: Compute argument possible values based on other arguments
type: issue
state: closed
author: fbecart
labels:
  - C-enhancement
  - A-validators
  - S-wont-fix
assignees: []
created_at: 2020-05-06T12:31:09Z
updated_at: 2022-01-11T18:30:50Z
url: https://github.com/clap-rs/clap/issues/1910
synced_at: 2026-01-07T13:12:19-06:00
---

# Compute argument possible values based on other arguments

---

_Issue opened by @fbecart on 2020-05-06 12:31_

I would like to be able to compute the possible values of an argument dynamically, based on the values provided for some other argument(s).

### Use case

I'm working on a CLI which has a goal similar to `make`.

```sh
$ make --help
Usage: make [options] [target] ...
Options:
  -f FILE, --file=FILE, --makefile=FILE
                              Read FILE as a makefile.
```

The file lists the available targets. Given a value of `FILE`, I am able to load a list of available target names. I would like to use that list as the possible values for `target`.

This would affect the validation of `target`. Ideally, this would also affect its autocompletion.

Currently, `possible_values` is expected to be provided as an array slice (`&[&str]`) as the `App` is being built.
https://github.com/clap-rs/clap/blob/9d03c8497ce17486e31b19fecbe5bec97ac82d09/src/build/arg/mod.rs#L1712
At that time, the value of `FILE` is not available yet.

### Wanted solution

I would like instead to be able to provide a closure that would compute the possible values for my argument. This closure would take the values of the other app arguments as a parameter.

### Workaround

It is already possible to obtain this behavior by parsing the arguments twice (the second time with refined validation):

```rust
fn get_app(allowed_target_names: Option<Vec<&str>>) -> App {
    let target_arg = Arg::with_name("target");

    let target_arg = if let Some(allowed_target_names) = allowed_target_names {
        target_arg.possible_values(&allowed_target_names)
    } else {
        target_arg
    };

    App::new("make")
        .arg(target_arg)
        [...]
}

fn main() {
    let arg_matches = get_app(None).get_matches();

    let config = Config::load(arg_matches.value_of("config_file"));
    let all_target_names = config.list_target_names();

    let app_args = get_app(Some(all_target_names)).get_matches();

    [...]
}
```

However, this workaround doubles the overhead of parsing arguments. Also, this solution does not cover the autocompletion part.

### Related issues

- https://github.com/clap-rs/clap/issues/568
- https://github.com/clap-rs/clap/pull/1793

---

_Label `T: new feature` added by @fbecart on 2020-05-06 12:31_

---

_Added to milestone `3.1` by @pksunkara on 2020-05-06 15:11_

---

_Comment by @pksunkara on 2020-05-06 15:11_

I think this can be achieved by the hooks proposal mentioned in #1471.

---

_Comment by @fbecart on 2020-05-07 06:55_

Thanks @pksunkara for looking into it. I see how https://github.com/clap-rs/clap/issues/1471 is related, as it suggests to have a closure returning the default value of an argument. 

This issue remains unique in the following ways:

- It is about computing the possible values of an argument.
- It requires access to the arguments that have already been provided.
- It should influence auto-completion.
- It is not related to prompting the user -- which is the primary motive for default value hooks.

---

_Comment by @pksunkara on 2020-05-07 12:24_

It is related because we can extend the hook proposal to allow defining possible values too. Prompts is a tangential issue.

Auto completion will probably be tied into the dynamic completion issue.

---

_Comment by @fbecart on 2020-05-08 20:27_

To illustrate the issue, here's a link to the project I'm working on: https://github.com/fbecart/zinoma/blob/9d2ea034d566a92a7464a7ca58c0b75cb87d7072/src/main.rs#L32-L37

---

_Comment by @kbknapp on 2020-05-11 18:43_

Also potentially related #1880 

---

_Referenced in [fbecart/zinoma#7](../../fbecart/zinoma/issues/7.md) on 2020-05-14 11:55_

---

_Comment by @fbecart on 2020-08-05 17:19_

Closely related to https://github.com/clap-rs/clap/issues/1232, if not duplicate.

---

_Referenced in [epage/clapng#157](../../epage/clapng/issues/157.md) on 2021-12-06 20:15_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:13_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:13_

---

_Label `A-validators` added by @epage on 2021-12-08 21:13_

---

_Referenced in [clap-rs/clap#568](../../clap-rs/clap/issues/568.md) on 2021-12-10 21:44_

---

_Comment by @epage on 2021-12-10 21:49_

In https://github.com/clap-rs/clap/discussions/2832, I layout an idea for generalizing clap.  This has the potential for opening the door to something like this.

With that said, I would generally recommend not doing this.  Parsing a file sufficiently to know what future args should be *during* the parsing of the args feels like there is a can of worms of problems with it.  I'm assuming `make` most other tools do it.

If you really want `clap` like errors, in 3.0.0, we are offering an `App::error`.  For more native solutions, I feel like #1404 and #568 are more of what we want to provide.  I did note this on #568 so we can remember to consider whether we should support this case or not.

In favor of those options I listed, I'm leaning towards closing this.  If there are concerns that are not covered, feel free to speak up!

---

_Closed by @epage on 2021-12-10 21:49_

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2021-12-13 17:56_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:30_

---

_Referenced in [clap-rs/clap#5621](../../clap-rs/clap/pulls/5621.md) on 2024-08-02 16:51_

---
