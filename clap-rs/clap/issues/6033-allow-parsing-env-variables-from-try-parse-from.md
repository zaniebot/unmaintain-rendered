```yaml
number: 6033
title: "Allow parsing env variables from `try_parse_from` parameter or similar"
type: issue
state: closed
author: tysm
labels:
  - C-enhancement
assignees: []
created_at: 2025-06-10T20:14:48Z
updated_at: 2025-06-10T20:24:31Z
url: https://github.com/clap-rs/clap/issues/6033
synced_at: 2026-01-12T16:14:17Z
```

# Allow parsing env variables from `try_parse_from` parameter or similar

---

_@tysm_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.40

### Describe your use case

Although `clap::Parser` provides methods to parse commands from an argv OsString iterator ([`Parser::parse_from`](https://docs.rs/clap/latest/clap/trait.Parser.html#method.parse_from)), the Arg builder itself caches [`std::env::var_os`](https://github.com/clap-rs/clap/blob/master/clap_builder/src/builder/arg.rs#L2196) which is later on used [fill Arg values](https://github.com/clap-rs/clap/blob/master/clap_builder/src/parser/parser.rs#L1418) missing a command line match.

I would like to dynamically parse commands in a multi thread subscriber, but it's [unsafe to set vars](https://doc.rust-lang.org/std/env/fn.set_var.html) in multi thread services.


### Describe the solution you'd like

Ideally, the env feature could be extended for a method (`Parser::parse_from_with_envs`), allowing clap to consume values from a controlled source rather than `std::env::var_os`, like we do for args.

### Alternatives, if applicable

Consume the env at parsing time, rather than build time

### Additional Context

_No response_

---

_Label `C-enhancement` added by @tysm on 2025-06-10 20:14_

---

_Comment by @epage on 2025-06-10 20:24_

This seems to have enough overlap with #4607 that we should probably provide only on solution so I'm closing in favor of that to centralize the conversation.  If there is a reason this should be kept open, let us know!

---

_Closed by @epage on 2025-06-10 20:24_

---

_Referenced in [clap-rs/clap#4607](../../clap-rs/clap/issues/4607.md) on 2025-07-14 19:24_

---
