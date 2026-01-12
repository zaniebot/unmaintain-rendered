```yaml
number: 2080
title: Allow changing message for generated help subcommand
type: issue
state: closed
author: casey
labels:
  - A-help
  - E-help-wanted
  - E-easy
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-08-17T08:17:35Z
updated_at: 2021-02-07T18:05:50Z
url: https://github.com/clap-rs/clap/issues/2080
synced_at: 2026-01-12T16:14:12Z
```

# Allow changing message for generated help subcommand

---

_@casey_

It would be nice if it were possible to change the help message for the Clap-provided `help` subcommand. For reference, the current text is "Prints this message or the help of the given subcommand(s)".

Forked from #1640.

---

_Label `T: new feature` added by @casey on 2020-08-17 08:17_

---

_Added to milestone `3.1` by @pksunkara on 2020-08-17 08:22_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-08-17 08:23_

---

_Label `C: help message` added by @pksunkara on 2020-08-17 08:23_

---

_Label `D: easy` added by @pksunkara on 2020-08-17 08:23_

---

_Label `help wanted` added by @pksunkara on 2020-08-17 08:23_

---

_Label `P3: want to have` added by @pksunkara on 2020-08-17 08:23_

---

_Label `Z: good first issue` added by @pksunkara on 2020-08-17 08:23_

---

_Label `P3: want to have` removed by @pksunkara on 2020-08-17 08:24_

---

_Label `P2: need to have` added by @pksunkara on 2020-08-17 08:24_

---

_Comment by @pksunkara on 2020-08-17 08:29_

To close this:

We need the following 2 api's added to `App` along with tests

* `global_help_about`
* `help_about`

---

_Comment by @CreepySkeleton on 2020-08-17 22:35_

A few remarks:

* The names must reflect the fact that the message is the thing being adjusted, not anything else.
* The names must mention "subcommand".

I propose `global_help_subcommand_message` and `help_subcommand_message`.

---

_Comment by @pksunkara on 2020-08-18 04:32_

This also refers to the `about` value of the `help` and `version` args. Not only subcommands, which is the reason for my original name proposal. We have been using `about` for the help message almost everywhere. We shouldn't refer it to as `message`. We have to be consistent.

---

_Comment by @hbina on 2020-08-22 00:53_

Are you referring to these texts?
```bash
hbina@hbinalapt:~/git/clap$ cargo run --example 01a_quick_example -- test --help
   Compiling clap v3.0.0-beta.1 (/home/hbina/git/clap)
    Finished dev [unoptimized + debuginfo] target(s) in 0.68s
     Running `target/debug/examples/01a_quick_example test --help`
01a_quick_example-test 
does testing things

USAGE:
    01a_quick_example <output> test [FLAGS]

FLAGS:
    -h, --help       Prints help information // <---- this??
    -l, --list       lists test values
    -V, --version    Prints version information // <--- and this?
```

Edit: Oh is this only relevant for clap 3.0+?

---

_Comment by @casey on 2020-08-22 01:27_

@hbina This is referring to the help subcommand text, not the help flag text. So the message that gets printed when you run `cmd help`, not `cmd --help`.

---

_Comment by @hbina on 2020-08-22 03:39_

I have submitted a PR that should resolve this but the tests are failing and I don't know why.
From a visual check they appear to be the same, but I might be missing some hidden newlines/carriage return.

---

_Referenced in [clap-rs/clap#2093](../../clap-rs/clap/pulls/2093.md) on 2020-08-22 03:39_

---

_Comment by @pksunkara on 2020-08-22 04:14_

Please rebases on master. We recently removed some extra trailing whitespace in help text.

---

_Comment by @CreepySkeleton on 2020-08-22 07:46_

Come to think of it, do we really need different methods for adjusting `--help` option message and `help` subcommand message? I'm almost 100% sure there's no case you'd want them to be different. 

So, maybe just make `help_message` affect the `help` subcommand?

---

_Comment by @casey on 2020-08-22 08:28_

@CreepySkeleton For my purposes, that would be ideal. The message I set for the `--help` option is appropriate for the `help` subcommand.

---

_Comment by @pksunkara on 2020-08-22 20:56_

Come on guys, that is why I proposed just one help_about in earlier comments.

---

_Comment by @CreepySkeleton on 2020-08-23 11:41_

@pksunkara Ugh, right. It just wasn't spelled out what that APIs actually do and I assumed wrong. 

I don't like the `help_about` name. I mean, imagine you aren't familiar with the project and trying to find the method that does the thing. `help_about` doesn't click anything in my brain 😕 

---

_Comment by @pksunkara on 2020-08-27 08:31_

Yeah, but we are calling the description of args/apps as `about` which is why I was proposing `_about`. Whatever we come up with should work with `version` too.

`help_SUFFIX`
`global_help_SUFFIX`
`version_SUFFIX`
`global_version_SUFFIX`

And we can't name it `help` because `help` is about the help arg/subcommand.

---

_Referenced in [clap-rs/clap#2143](../../clap-rs/clap/pulls/2143.md) on 2020-09-24 21:57_

---

_Closed by @bors[bot] on 2020-09-26 07:20_

---

_Comment by @pksunkara on 2021-02-07 18:05_

I have thought about this more and removed the added `help_about` in favor of doing `app.mut_arg("help", |a| a.about("help info"))`. Setting it once at the top level `App` should be enough.

---

_Referenced in [clap-rs/clap#2500](../../clap-rs/clap/issues/2500.md) on 2021-05-26 19:25_

---
