```yaml
number: 1701
title: "\"was provided more than once\" when combining RIPGREP_CONFIG_PATH with command-lines using condensed short option syntax"
type: issue
state: closed
author: ssokolow
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-10-10T03:40:53Z
updated_at: 2023-11-21T04:51:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1701
synced_at: 2026-01-12T16:13:24Z
```

# "was provided more than once" when combining RIPGREP_CONFIG_PATH with command-lines using condensed short option syntax

---

_@ssokolow_

#### What version of ripgrep are you using?

    ripgrep 12.1.1
    +SIMD -AVX (compiled)
    -SIMD -AVX (runtime)

#### How did you install ripgrep?

    RUSTFLAGS="-C target-cpu=native" cargo +nightly install ripgrep --features 'simd-accel'

#### What operating system are you using ripgrep on?

```
$ lsb_release -a                                                                             
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.7 LTS
Release:        16.04
Codename:       xenial
```

#### Describe your bug.

ripgrep exits with the following error...

    error: The argument '--pretty' was provided more than once, but cannot be used multiple times

...if `-p` or `--pretty` is in the configuration file and condensed short option syntax is used on the command line.

#### What are the steps to reproduce the behavior?

1. Set up `RIPGREP_CONFIG_PATH` and a configuration file containing `-p` or `--pretty`
2. Run `ripgrep` with `-p -z` or `--pretty -z` at the command line.
3. Notice that it works as expected, as it should for use in scripts.
4. Run `ripgrep` with `-p -z -p` at the command line
5. Notice that it works in an `alias`-compatible way as it should.
6. Run `ripgrep` with `-pz` at the command line.
7. Get an error message.

#### What is the actual behavior?

```
$ rg -pz foo /usr/share/doc 
error: The argument '--pretty' was provided more than once, but cannot be used multiple times

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
```

#### What is the expected behavior?

It shouldn't matter what form it takes.

#### What do you think ripgrep should have done?

`-pz` should behave identically to `-p -z`. 

---

_Comment by @okdana on 2020-10-10 04:00_

I think the config file is a red herring. I get the same issue with just the command line. The option-stacking thing does seem relevant though

These fail:

```
% RIPGREP_CONFIG_PATH= \rg -p -pz x <<< xyz
% RIPGREP_CONFIG_PATH= \rg -ppz x <<< xyz
```

But these are OK:

```
% RIPGREP_CONFIG_PATH= \rg -pz -p x <<< xyz
% RIPGREP_CONFIG_PATH= \rg -pzp x <<< xyz
% RIPGREP_CONFIG_PATH= \rg -zpp x <<< xyz
% RIPGREP_CONFIG_PATH= \rg -pp -z x <<< xyz
% RIPGREP_CONFIG_PATH= \rg -p -p -z x <<< xyz
```

It does it with all other options i tried, too, not just `--pretty`. I assume it's a Clap bug...?

---

_Comment by @okdana on 2020-10-10 04:17_

Yeah, i think so, since `fd` also does it:

```
% fd -1 -pp -L README
README.md

% fd -1 -ppL README
error: The argument '--full-path' was provided more than once, but cannot be used multiple times ...
```

I couldn't find a related issue in the [clap repo](https://github.com/clap-rs/clap/), i guess it needs reported there

---

_Comment by @ssokolow on 2020-10-10 04:24_

OK. I'll try to find time to put together a minimal reproducer tomorrow.

**EDIT:** The day didn't go well. Tomorrow maybe.

---

_Comment by @ssokolow on 2020-10-12 06:15_

I've tried to put together a reproducer, but I can't remember what the proper way is to ask clap for that `alias`-compatible behaviour where you can specify an argument multiple times but the additional copies have no semantic significance within the program.

---

_Comment by @BurntSushi on 2020-10-12 12:04_

@ssokolow Thanks for looking into this! I believe the setting you're looking for is [`clap::AppSettings::AllArgsOverrideSelf`](https://docs.rs/clap/2.33.3/clap/enum.AppSettings.html#variant.AllArgsOverrideSelf). ripgrep sets it [here](https://github.com/BurntSushi/ripgrep/blob/def993bad1a9275cdc249f04048e5b2065b79f05/crates/core/app.rs#L62).

---

_Label `bug` added by @BurntSushi on 2020-10-12 12:05_

---

_Comment by @ssokolow on 2020-10-12 13:43_

Thanks. I've now got a reproducer:

```rust
use clap::{Arg, App, AppSettings};

fn main() {
    let schema = App::new("ripgrep#1701 reproducer")
        .setting(AppSettings::AllArgsOverrideSelf)
        .arg(Arg::with_name("pretty")
            .short("p")
            .long("pretty"))
        .arg(Arg::with_name("search_zip")
            .short("z")
            .long("search-zip"));

    let test_args = &[
       vec!["reproducer", "-pz", "-p"],
       vec!["reproducer", "-pzp"],
       vec!["reproducer", "-zpp"],
       vec!["reproducer", "-pp", "-z"],
       vec!["reproducer", "-p", "-p", "-z"],

       vec!["reproducer", "-p", "-pz"],
       vec!["reproducer", "-ppz"],
    ];


    for argv in test_args {
        let matches = schema.clone().get_matches_from_safe(argv);
        match matches {
            Ok(_) => println!(" OK: {:?}", argv),
            Err(e) => println!("ERR: {:?} ({:?})", argv, e.kind),
        }
    }
}
```

```
 OK: ["reproducer", "-pz", "-p"]
 OK: ["reproducer", "-pzp"]
 OK: ["reproducer", "-zpp"]
 OK: ["reproducer", "-pp", "-z"]
 OK: ["reproducer", "-p", "-p", "-z"]
ERR: ["reproducer", "-p", "-pz"] (UnexpectedMultipleUsage)
ERR: ["reproducer", "-ppz"] (UnexpectedMultipleUsage)
```

I'm not feeling motivated to open a bug right now but I'll do it later if someone else doesn't beat me to it. (It's near the end of my day and anything which introduces a new social component feels tiring.)

---

_Comment by @ssokolow on 2020-10-13 10:53_

Reported as clap-rs/clap#2171

---

_Label `rollup` added by @BurntSushi on 2023-11-21 00:57_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
