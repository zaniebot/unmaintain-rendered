```yaml
number: 3741
title: "Request: an env-variable-only version of clap::Parser::parse() that doesn't interact with env::args"
type: issue
state: closed
author: wimax-grapl
labels:
  - C-enhancement
  - S-wont-fix
assignees: []
created_at: 2022-05-21T19:46:51Z
updated_at: 2022-05-23T17:00:13Z
url: https://github.com/clap-rs/clap/issues/3741
synced_at: 2026-01-12T16:14:15Z
```

# Request: an env-variable-only version of clap::Parser::parse() that doesn't interact with env::args

---

_@wimax-grapl_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

2.34.0

### Describe your use case

Hi folks!

We are.... well, if we're being honest, we're probably misusing Clap. 

We use it to define a hierarchical config for our program, but we never inject arguments by command line flag, just by environment variable.
That is to say, all of our Derives look like
```
#[derive(clap::Parser, Debug)]
pub struct ClientCertConfig {
    #[clap(env)]
    pub public_certificate_pem: Option<String>,
    #[clap(env)]
    pub some_other_thing: u64,
}
```

I've encountered two situations where using Clap this way has been painful:

1. When you start including default values into the mix, Clap understandably starts complaining about the ordering of your arguments from a CLI point of view. However, for our use cases, this doesn't really matter and just adds extra complexity. **The workaround we ended up using was adding clap(env, long) to every field to avoid any ordering complexity.**
2. I have a test case that calls ::parse() to grab values from environment variables. I also try passing `--nocapture` to the test binary. [This is a known issue. ](https://github.com/rust-lang/cargo/issues/8916)


### Describe the solution you'd like

it'd be really nice if there were a batteries-included `Parser::parse_from_env()` or something. It can throw as many errors as it wants if your struct doesn't strictly have `env` fields. 

### Alternatives, if applicable

It's possible we should just switch to something like [config-rs](https://github.com/mehcode/config-rs/). 

### Additional Context

-

---

_Label `C-enhancement` added by @wimax-grapl on 2022-05-21 19:46_

---

_Comment by @wimax-grapl on 2022-05-21 20:18_

(note: at least for the #2 case, `.parse_from(some_empty_iter)` seems to do the trick.)

---

_Referenced in [grapl-security/grapl#1737](../../grapl-security/grapl/pulls/1737.md) on 2022-05-21 20:49_

---

_Comment by @epage on 2022-05-21 23:59_

Yes, I would recommend `.parse_from`.  Depending on what you are doing, you may also want to set [`#[clap(no_binary_name = true)]`](https://docs.rs/clap/latest/clap/struct.App.html#method.no_binary_name) as that will let you just do `parse_from([])` and not need to accept any arguments.

For the fact that clap is focused on command-line argument parsing rather than environment reading and that we at least provide those building blocks I'm going to close this.  We do not want to set the precedence that we are a general environment reader and all of the API requests that may come with that.

---

_Closed by @epage on 2022-05-21 23:59_

---

_Label `S-wont-fix` added by @epage on 2022-05-22 00:00_

---

_Comment by @wimax-grapl on 2022-05-23 17:00_

Thanks for the followup / workarounds!

---
