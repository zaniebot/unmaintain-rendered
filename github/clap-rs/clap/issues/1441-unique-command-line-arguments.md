---
number: 1441
title: "`unique` command line arguments"
type: issue
state: closed
author: suhr
labels: []
assignees: []
created_at: 2019-03-29T17:05:47Z
updated_at: 2020-07-01T02:53:31Z
url: https://github.com/clap-rs/clap/issues/1441
synced_at: 2026-01-07T13:12:19-06:00
---

# `unique` command line arguments

---

_Issue opened by @suhr on 2019-03-29 17:05_

This feature request is inspired by [this discussion](https://github.com/tox-rs/tox-node/issues/53).

Long story short: at this moment, tox-node uses the `config` subcommand to run a node with the configuration from a configuration file. The reason for this is because tox-node can be also run without a configuration file, with all required options passed as command line arguments. The usage of subcommand allows to avoid the situation when both configuration file and command line arguments are provides simultaneously.

The problem is, I believe that this is in no way a valid usage of subcommands. The purpose of subcommands is to provide *actions*, but `config` is not an action. So instead of `config` subcommand there should be the `--config` command line option.

At this moment, clap-rs provides no ergonomic way to specify an option that excludes all other options. This issue proposes adding a method for specifying such options.


---

_Comment by @kbknapp on 2019-04-05 02:42_

There is quite a bit of discussion about the handling of configuration files. This is one of the main goals v3 is trying to solve.

I'm not clear on if you're currently handling the configuration files manually? If so, I would recommend the follow instead of a `unique`:

*note:* This mainly works if the number of required by default arguments is relatively low

* Create a `--mode=MODE` option, which only accepts two values `config|cli`, and defaults to `cli`
* In your required arguments use `Arg::required_if` to only be required if `--mode=cli`
* In your user-code, if `--mode=config` you can ignore CLI passed options (or allow CLI passed options to override the config options you've manually determined).

This is assuming I understood the problem correctly. While I'm not opposed to a `unique` option, I think there are better ways to handle the validation you're searching for. (Part of which will only become a thing once v3 is released though).

---

_Label `T: RFC / question` added by @kbknapp on 2019-04-05 02:42_

---

_Comment by @CreepySkeleton on 2020-07-01 02:53_

> At this moment, clap-rs provides no ergonomic way to specify an option that excludes all other options

I believe this is solved by `Arg::exclusive`. 

---

_Closed by @CreepySkeleton on 2020-07-01 02:53_

---
