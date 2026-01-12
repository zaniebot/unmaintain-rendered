```yaml
number: 712
title: Allow getting values from environmental args if not passed in via the command line
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
assignees: []
created_at: 2016-10-28T02:06:03Z
updated_at: 2021-01-12T19:28:28Z
url: https://github.com/clap-rs/clap/issues/712
synced_at: 2026-01-12T16:14:09Z
```

# Allow getting values from environmental args if not passed in via the command line

---

_@kbknapp_

Priority should be passed in manually, then env args, then default values.


---

_Label `T: new feature` added by @kbknapp on 2016-10-28 02:06_

---

_Label `D: intermediate` added by @kbknapp on 2016-10-28 02:06_

---

_Label `P3: want to have` added by @kbknapp on 2016-10-28 02:06_

---

_Label `C: parsing` added by @kbknapp on 2016-10-28 02:06_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-28 02:06_

---

_Comment by @kbknapp on 2016-10-28 02:26_

Should add a `App::env_args(name)` which essentially extends the command line args with whatever is inside `name`. In this scenario name would contain a full set of arg(s) such as `--foo bar`

Should also add an `Arg::env_value(name)` where `name` can be either just a value i.e. `bar` in `--foo bar`, or the whole arg, i.e. `--foo bar` but only for a single argument, whereas `App::env_args` could theoretically contain `--foo bar --baz` etc.


---

_Comment by @quodlibetor on 2016-12-13 14:14_

Something that I would love to see is supporting the (semi-standard?) technique of having env vars (and config vars) be option/argument names, screaming snake cased and prefixed with the name of the app. To support this it would be nice if there was a `App::env_arg_prefix(name)` so that you could do (in pseudo-clap):

```
    App::env_arg_prefix("myapp").Arg("--some").Arg("required")
```

and be able to run your app like:

```
MYAPP_SOME=thing MYAPP_REQUIRED=yes myapp
```

This also maps pretty cleanly to loading things from config files (you could just specify a `config_arg_section` and have everything work theoretically the same.)

---

_Referenced in [clap-rs/clap#814](../../clap-rs/clap/issues/814.md) on 2017-01-10 20:09_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:56_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 18:56_

---

_Comment by @kbknapp on 2018-02-02 01:54_

This has already been implemented in v2 via [`Arg::env`](https://docs.rs/clap/2.29.2/clap/struct.Arg.html#method.env) and [`Arg::env_os`](https://docs.rs/clap/2.29.2/clap/struct.Arg.html#method.env_os). Guess I forgot to close this?

---

_Closed by @kbknapp on 2018-02-02 01:54_

---

_Comment by @quodlibetor on 2018-02-02 19:20_

Neat!

It would still be nice to have a similar `App::fallback_args_from_os(PREFIX)` flag that would make it so that every arg that has a `long` option would also accept `PREFIX_${to_screaming_snake_case(long_name)}`. It's a really useful paradigm for 12FA, and it's nice to be consistent.

---

_Comment by @kbknapp on 2018-02-05 03:42_

@quodlibetor I like the idea, but for now I'd rather stick with having to declare the args one would like to use with ENV vals. If an application wants to support what you're proposing, it's as simple as adding:

```
Arg::with_name("some")
    .long("some")
    .env("MYAPP_SOME")
```

I'm not sure I'm comfortable adding code that goes through ENV vars matching a prefix, converting longs to screaming snake case, doing a match, etc. This is something that could be handled pretty easily in an external crate once v3 is out (due to `App::mut_arg`).

---

_Comment by @bestouff on 2018-02-05 08:16_

Automatically reading env vars would be really nice, but guarded by a global switch, like `.auto_2fa(true)` or something like that.

---

_Comment by @kbknapp on 2018-02-05 15:16_

I'm not *necessarily* opposed to that feature/setting. But with the upcoming v3 and `mut_arg` I would like to start pairing back some settings that could be external crates and moving them out of the core clap code, or even creating a `clap_extras` crate that provides these one off, modification-only of an `App` or `Arg` instance and that don't rely on core clap internals to function. 

If you'd like to open an issue with those features in mind so I can track it separately, that's fine. However, I can't promise I'll get to it anytime soon as getting v3 out the door is my priority right now.

---

_Comment by @quodlibetor on 2018-02-05 16:49_

@kbknapp yup, that's what I'm currently doing, it would just be nice if instead of:

```rust
App::new("cool")
    .arg(
        Arg::with_name("config")
            .env("COOL_CONFIG"))
    .arg(
        Arg::with_name("out-file")
            .env("COOL_OUT_FILE"))
```

I could just do:

```rust
App::new("cool")
    .map_env_with_prefix("COOL_")
    .arg(Arg::with_name("config"))
    .arg(Arg::with_name("out-file"))
```

It's not a *huge* deal, it's mostly a "every now and then I am lazy about
correctly specifying env and then it's annoying" deal.

---

_Comment by @pronebird on 2018-11-12 16:39_

I have a use case where env variable is not backed by an argument. I am currently using `after_help` to outline the available environment variables. It would be great to support `env` and collect environment variables under `ENV` section just like `SUBCOMMANDS` or `FLAGS`, i.e:

```rust
App::new("cool")
  .env("MAGIC_COOKIE", "Does something cool")
```

---

_Comment by @quodlibetor on 2021-01-12 19:28_

Something that recently came up in support of having this be a global property on `App` is it would allow linting or errorring if there are any env vars that match the prefix, but do not match any known args:

```
App::new("COOL")
    .env_prefix("COOL_")
    .strict_env_prefix()
    .arg(Arg::with_name("check"))
```

Could fail if `COOL_CHCEK` is in the environment, which I'm sure would have saved me hours of debugging over the course of the career.

The `mut_arg` feature in combination with `get_arguments` does seem like it would make it pretty easy to build this in to an external crate, although I'm not sure if that would/could work with the derive macro.

---
