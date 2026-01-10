---
number: 3221
title: Add an env_prefix derive option for a consistent prefix for environment variables
type: issue
state: open
author: gibfahn
labels:
  - C-enhancement
  - E-medium
  - A-derive
assignees: []
created_at: 2021-12-27T01:12:19Z
updated_at: 2025-04-01T15:11:43Z
url: https://github.com/clap-rs/clap/issues/3221
synced_at: 2026-01-10T01:27:36Z
---

# Add an env_prefix derive option for a consistent prefix for environment variables

---

_Issue opened by @gibfahn on 2021-12-27 01:12_

Maintainer's notes
- Proposed fix is to add a `Command::next_env_prefix` and `Arg::env_prefix` and is modeled after `help_heading`
  - `Command::next_env_prefix`applies to all future `Arg`s and is set during `.arg()`
  - An explicit `Arg::env_prefix` takes precedence over `Command::next_env_prefix`
  - Derives will need to capture and restore state
  - https://github.com/clap-rs/clap/issues/1807 will allow derives to change the `Command::next_env_prefix` part-way through struct definition
  - Either we generate the env variables during `_build` (`env` will internally need to be a `Cow`), leaving help generation and env variable look up unchanged or we do it on lookup/display
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

v3.0.0-rc.8

### Describe your use case

This was previously discussed in https://github.com/TeXitoi/structopt/issues/370.

Basically the request is to be able to allow a specific prefix to be appended to all `#[clap(env)]` derived environment variable names.

### Describe the solution you'd like

Before:

```rust
#[derive(Parser)]
pub struct Opts {
    #[clap(long, env = "FOO_CONFIG_DIR")]
    pub config_dir: String,
    #[clap(long, env = "FOO_TEMP_DIR")]
    pub temp_dir: String,
    #[clap(long, env = "FOO_CACHE_DIR")]
    pub cache_dir: String,
    #[clap(long)]
    pub isolated: bool
}
```

After:

```rust
#[derive(Parser)]
#[clap(env_prefix)]
pub struct Opts {
    #[clap(long, env)]
    pub config_dir: String,
    #[clap(long, env)]
    pub temp_dir: String,
    #[clap(long, env)]
    pub cache_dir: String,
    #[clap(long)]
    pub isolated: bool
}
```

(where a custom prefix could be set by `#[clap(env_prefix="foo")]`, but the default was the `#[clap(name)]`)

### Alternatives, if applicable

I don't think there's an alternative solution to manually setting the environment variable for every option that needs it.

There was a suggestion in the above issue about allowing this to be calculated via a function vs a string literal, but I can't think of a good use-case for it, so it seems unnecessary for this feature.

### Additional Context

Examples from the above issue:

[`rust-analyzer`](https://github.com/rust-analyzer/rust-analyzer/pull/4424) looks for `RA_LOG` var, [`rustc`](https://internals.rust-lang.org/t/rust-log-vs-rustc-log/4297) looks for `RUSC_LOG` var, [`sccache`](https://github.com/mozilla/sccache#storage-options) looks for `SCCACHE_ENDPOINT`, `SCCACHE_CACHE_SIZE`, `SCCACHE_REDIS` and other....


---

_Label `C-enhancement` added by @gibfahn on 2021-12-27 01:12_

---

_Comment by @epage on 2021-12-27 14:50_

Wanted to double check.  Did you mean for your "after" to be:
```rust
#[derive(Parser)]
#[clap(env_prefix)]
pub struct Opts {
    #[clap(long, env)]
    pub config_dir: String,
    #[clap(long, env)]
    pub temp_dir: String,
    #[clap(long, env)]
    pub cache_dir: String,
    #[clap(long)]
    pub isolated: bool
}
```
- `#[clap(env)]` is being specified
- I added `--isolated` to contrast with the use of `env`

---

_Label `A-derive` added by @epage on 2021-12-27 14:50_

---

_Label `S-waiting-on-author` added by @epage on 2021-12-27 14:50_

---

_Comment by @epage on 2021-12-27 14:52_

If we do this, we'd need to make clear that `env_prefix` would **not** propagate through `flatten` / `subcommand` unlike most other attributes that get set.  I'm assuming this limitation isn't enough to hold back making it easier to follow common env variable practices.

---

_Comment by @gibfahn on 2021-12-30 19:17_

>Wanted to double check. Did you mean for your "after" to be:

Whoops, yes my bad, you'd still specify `env`. I updated the original example to match your correction.

> If we do this, we'd need to make clear that env_prefix would not propagate through flatten / subcommand unlike most other attributes that get set.

Why wouldn't we want it to propagate? I can see that making the user be explicit is probably a better idea, but I could also see `mycmd mysubccmd --myarg` being converted to `$MYCMD_MYSUBCMD_MYARG` (for example), and I'm not sure what the issue would be with flatten used outside of subcommands.

>I'm assuming this limitation isn't enough to hold back making it easier to follow common env variable practices.

Definitely not an issue to specify `env_prefix` in more places either way, especially if the default values mostly work.

---

_Comment by @epage on 2021-12-30 19:34_


> > If we do this, we'd need to make clear that env_prefix would not propagate through flatten / subcommand unlike most other attributes that get set.
> 
> Why wouldn't we want it to propagate? I can see that making the user be explicit is probably a better idea, but I could also see `mycmd mysubccmd --myarg` being converted to `$MYCMD_MYSUBCMD_MYARG` (for example), and I'm not sure what the issue would be with flatten used outside of subcommands.

Sorry I didn't go into more details on this.  We can't make derives for different structs interact at build-time.  We have to do all of this at runtime.

Our options are
- Balloon out the derive API for each new piece of information we want to communicate, either breaking compatibility or having the new functions on the trait call into the old ones with defaults
- Try to figure out which arguments were added during the flatten and modify them accordingly
- Make this a part of the builder API.

In thinking about this, maybe it could be worthwhile to add this to the builder API.

---

_Label `S-waiting-on-author` removed by @epage on 2021-12-30 20:36_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-30 20:36_

---

_Comment by @gibfahn on 2022-01-17 14:59_

>In thinking about this, maybe it could be worthwhile to add this to the builder API.

Adding this to the builder API would basically mean that `#[clap(env)]` would call something like `.env(default_env!())` the way `#[clap(version)]` calls `.version(crate_version!())` today?

If so that seems reasonable to me.

>Sorry I didn't go into more details on this. We can't make derives for different structs interact at build-time. We have to do all of this at runtime.

For what it's worth I think it would be fine to just have `#[clap(env)]` work for top-level options, and leave it as a future enhancement to deal with propagating.

---

_Comment by @epage on 2022-01-17 15:17_

> For what it's worth I think it would be fine to just have `#[clap(env)]` work for top-level options, and leave it as a future enhancement to deal with propagating.

We want to avoid this.  Someone refactoring their app from a single `Struct` to multiple `Struct`s would then get surprised by it breaking.

---

_Label `S-waiting-on-design` removed by @epage on 2022-01-25 15:43_

---

_Label `S-waiting-on-mentor` added by @epage on 2022-01-25 15:43_

---

_Label `S-waiting-on-mentor` removed by @epage on 2022-01-25 15:44_

---

_Label `S-waiting-on-design` added by @epage on 2022-01-25 15:44_

---

_Comment by @epage on 2022-01-25 15:51_

Alright, so the proposal is we add an `App::env_prefix`.

Questions
- How should `Arg`s override this, e.g. reading from a [general purpose env variable](https://clig.dev/#environment-variables)
- Should we allow changing the env prefix?  Maybe like we do with `help_heading` where its stateful?
  - Modeling after `help_heading` would allow overriding the prefix in limited cases
  - For derive users, in https://github.com/clap-rs/clap/issues/1807 we propose a `clap::app` attribute on fields so `App::env_prefix` can be called
- Should we apply this during `_build` or when doing look ups
  - `_build` is better for sharing logic with help, man pages, etc
  - On lookup is better for performance
  - We could switch to a two-tier build model where `get_matches` does a minimal build and everything else (like https://github.com/clap-rs/clap/issues/2911) gets a full build.

So in writing up these questions, I think we've got our answers.

---

_Label `S-waiting-on-design` removed by @epage on 2022-01-25 15:52_

---

_Label `E-medium` added by @epage on 2022-01-25 15:52_

---

_Referenced in [clap-rs/clap#3513](../../clap-rs/clap/issues/3513.md) on 2022-02-26 10:14_

---

_Referenced in [clap-rs/clap#3517](../../clap-rs/clap/issues/3517.md) on 2022-02-28 03:31_

---

_Comment by @Stargateur on 2022-05-20 11:44_

> If we do this, we'd need to make clear that `env_prefix` would **not** propagate through `flatten` / `subcommand` unlike most other attributes that get set. I'm assuming this limitation isn't enough to hold back making it easier to follow common env variable practices.

It's would be amazing that flatten to do it automatically for example I would like:

```rust
#[derive(Debug, Args, Deserialize)]
pub struct PostgreSQL {
  #[clap(long, env)]
  pub host: Option<String>,
}

#[derive(Parser, Debug)]
#[clap(author, version, about)]
#[clap(env_prefix)]
pub struct ConfigArgs {
  #[clap(flatten)]
  pub postgresql: PostgreSQL,
}
```

This mean host would have the env `{NAME}_POSTGRESQL_HOST`, if it's not possible it would be great to be able to use `env_prefix` more than one like:

```rust
#[derive(Debug, Args, Deserialize)]
#[clap(env_prefix(name, struct_name))]
pub struct PostgreSQL {
  #[clap(long, env)]
  pub host: Option<String>,
}
```


---

_Comment by @epage on 2022-05-20 12:22_

> It's would be amazing that flatten to do it automatically for example I would like:

Note that my later post shifted this from being a derive feature to a builder feature which means it could work across `flatten`.

---

_Referenced in [MaterializeInc/materialize#12765](../../MaterializeInc/materialize/pulls/12765.md) on 2022-05-30 13:14_

---

_Comment by @Maher4Ever on 2022-07-18 09:05_

@epage Is this planned to be implemented soon? 

---

_Comment by @epage on 2022-07-18 12:29_

It has the `E-medium` tag which means the current design is accepted for moving forward.   However, my focus is on other area (iterating on the API for reducing binary size, reducing compile times, and improving flexibility).  If someone is willing to step up to implement it, I'd be willing to review and merge the PRs.

---

_Comment by @eighty4 on 2023-02-28 21:29_

I've been using `#[arg(...)]` macro yet have seen use of `clap` interchangeably in other sources. Although it's not on docs.rs, switching out `arg` for `clap` works. However, neither `arg` nor `clap` permits an `env` flag (yet I've also seen `env` used in other sources). Quite confused!

Is there currently a way to permit a required option to be specified with an environment variable or is the environment variable feature included with `env_prefix`?

Here's an example of my current usage:

https://github.com/eighty4/cquill/blob/main/src/main.rs

Looking for this `env_prefix` feature led me to this ticket, hoping to have `CQUILL_CQL_DIR=` override the `--cql-dir` arg. Is this currently possible with an `env` flag that would support `CQL_DIR=`?

---

_Comment by @epage on 2023-02-28 22:02_

> I've been using #[arg(...)] macro yet have seen use of clap interchangeably in other sources. Although it's not on docs.rs, switching out arg for clap works. 

`clap` is deprecated in favor of the more specialized attributes

> However, neither arg nor clap permits an env flag (yet I've also seen env used in other sources). Quite confused!

If you search on docs.rs, you will find [`Arg::env`](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.env) which is documented as being behind the `env` feature flag.  If you run `cargo add clap -F env`, it should start to work.  As for any derive aspect of this documentation, see the [discussion on the topic](https://github.com/clap-rs/clap/discussions/4090)

---

_Referenced in [cartesi/rollups#27](../../cartesi/rollups/issues/27.md) on 2023-04-20 01:55_

---

_Comment by @epage on 2023-08-01 00:57_

Alternatively, #5050 would allow doing this without any other new feature

---

_Referenced in [clap-rs/clap#5050](../../clap-rs/clap/issues/5050.md) on 2023-08-01 01:06_

---

_Referenced in [surrealdb/surrealdb#2696](../../surrealdb/surrealdb/issues/2696.md) on 2023-09-14 17:04_

---

_Referenced in [clap-rs/clap#4556](../../clap-rs/clap/issues/4556.md) on 2023-11-09 16:11_

---

_Comment by @SpiderUnderUrBed on 2025-04-01 10:00_

Any update? I think a env option as gibfahn suggested would be the easiest and best solution, I dont know how exactly #5050 would solve this.

---
