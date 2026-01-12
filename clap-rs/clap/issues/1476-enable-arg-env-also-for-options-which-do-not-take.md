```yaml
number: 1476
title: "Enable Arg::env also for options which do not take value"
type: issue
state: closed
author: izderadicka
labels:
  - C-enhancement
  - E-help-wanted
assignees: []
created_at: 2019-05-22T12:52:19Z
updated_at: 2021-05-28T09:42:31Z
url: https://github.com/clap-rs/clap/issues/1476
synced_at: 2026-01-12T16:14:11Z
```

# Enable Arg::env also for options which do not take value

---

_@izderadicka_


### Affected Version of clap
2.33.0

### Feature Request Summary

It would nice if Arg::env would work also for argument that do not take value. In this case is_present will mean that either:
* Argument is present on command line, or
* There is environment variable, with given name, which is not empty (similar to test -n "$MY_VAR" in shell)


### Expected Behavior Summary
Arg::env should not automatically set takes_values (but this is a breaking change, so maybe just leave it as it is).
If Arg::env has defined related env variable and takes_value is false, then ArgMatches::is_present will be true if argument is present on command line or env variable exists and is not empty.


### Actual Behavior Summary

Currently clap Arg::env works only for arguments which take value - environment value is used as default if argument is not provided on command line. Arg::env also automatically sets takes_value to true automatically, which can lead to bit confusing behavior (if you do not know that Arg::env works **only** for Arguments which takes value (and considering that takes_value = false  is default). See short code example below:

```rust
extern crate clap;

use clap::{App,Arg};
use std::env;

fn main() {
    env::set_var("MY_FLAG", "1");
    
    let matches_not_ok = App::new("prog")
        .arg(Arg::with_name("flag")
            .long("flag")
            .env("MY_FLAG")
            .takes_value(false)
            )
        .get_matches_from(vec![
            "prog"
        ]);
        
    // Not working for argument which does not take value :-(
     assert!(! matches_not_ok.is_present("flag"));  
     
     let matches_ok = App::new("prog")
        .arg(Arg::with_name("flag")
            .long("flag")
            .takes_value(false) // switching order of takes_value and env
            .env("MY_FLAG")
            )
        .get_matches_from(vec![
            "prog"
        ]);
        
    // And it looks like it's working 
     assert!(matches_ok.is_present("flag"));  
     
     //but flag will now consumes value, cause env will set takes_value to true behind scenes
     
     env::remove_var("MY_FLAG");
     
     let matches_ok = App::new("prog")
        .arg(Arg::with_name("pos"))
        .arg(Arg::with_name("flag")
            .long("flag")
            .takes_value(false) 
            .env("MY_FLAG")
            )
        .get_matches_from(vec![
            "prog", "--flag", "pos"
        ]);
        
    // now works as expected, however now flag takes value
     assert!(matches_ok.is_present("flag"));  
     // and pos argument is consumed
     assert!(! matches_ok.is_present("pos"));  
     //by flag
     assert_eq!( matches_ok.value_of("flag"), Some("pos"));
     
}
```


---

_Referenced in [TeXitoi/structopt#305](../../TeXitoi/structopt/issues/305.md) on 2019-12-11 15:20_

---

_Referenced in [TeXitoi/structopt#318](../../TeXitoi/structopt/issues/318.md) on 2019-12-30 22:09_

---

_Label `C: environment variable` added by @CreepySkeleton on 2020-02-01 13:40_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 13:40_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 13:40_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 13:40_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 13:40_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 13:41_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-01 13:41_

---

_Referenced in [clap-rs/clap#1362](../../clap-rs/clap/issues/1362.md) on 2020-02-01 15:47_

---

_Comment by @pksunkara on 2020-02-01 19:00_

We don't need to backport this 2.x, so we can remove `W: 2.x` label.

---

_Label `W: 2.x` removed by @CreepySkeleton on 2020-02-02 03:08_

---

_Referenced in [TeXitoi/structopt#372](../../TeXitoi/structopt/issues/372.md) on 2020-04-09 17:56_

---

_Comment by @CreepySkeleton on 2020-07-01 10:45_

Here's what I propose:
* `Arg::env` no longer implies `takes_value(true)`. It's up to user to set it.
* If `env("ENV")` IS set:
  * if `takes_value(true)` IS NOT set (the arg is a flag), than the flag is considered raised if `ENV` is defined. Its precise value doesn't matter, empty or not, its precise value will not be recorded.
  * if `takes_value(true)` IS set, than `ENV` behaves just like it does today.

@pksunkara Any objections? 

---

_Comment by @pksunkara on 2020-07-11 15:08_

Agreed. Marking this for 3.0

---

_Removed from milestone `3.1` by @pksunkara on 2020-07-11 15:08_

---

_Added to milestone `3.0` by @pksunkara on 2020-07-11 15:08_

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-07-11 16:32_

---

_Referenced in [TeXitoi/structopt#428](../../TeXitoi/structopt/issues/428.md) on 2020-09-08 08:55_

---

_Referenced in [meilisearch/meilisearch#958](../../meilisearch/meilisearch/issues/958.md) on 2020-09-08 20:02_

---

_Comment by @pksunkara on 2020-10-26 07:43_

@ldm0 Would you like to take this next?

---

_Comment by @ldm0 on 2020-10-26 07:56_

No problem.

---

_Label `W: 3.x` removed by @pksunkara on 2020-10-26 07:57_

---

_Referenced in [clap-rs/clap#2190](../../clap-rs/clap/pulls/2190.md) on 2020-10-30 18:14_

---

_Closed by @bors[bot] on 2020-11-06 21:59_

---

_Comment by @flavio on 2021-05-28 09:40_

Is there any chance to get this fix available inside of the v2 branch? I can take care of the backport if you are fine with this idea

---

_Comment by @pksunkara on 2021-05-28 09:42_

Unfortunately no, we are accepting only security patches to v2.

---

_Referenced in [kubewarden/policy-server#115](../../kubewarden/policy-server/pulls/115.md) on 2021-10-25 11:43_

---

_Referenced in [Concordium/concordium-node#233](../../Concordium/concordium-node/issues/233.md) on 2022-01-01 16:21_

---
