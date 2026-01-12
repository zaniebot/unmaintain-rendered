```yaml
number: 1788
title: Allow more flexible configuration of the output steam (stdout vs. stderr)
type: issue
state: closed
author: tzakharko
labels:
  - C-enhancement
  - A-help
  - S-wont-fix
assignees: []
created_at: 2020-04-05T15:40:37Z
updated_at: 2022-09-22T01:20:35Z
url: https://github.com/clap-rs/clap/issues/1788
synced_at: 2026-01-12T16:14:11Z
```

# Allow more flexible configuration of the output steam (stdout vs. stderr)

---

_@tzakharko_

### Describe your use case

Some command line tools are used exclusively as components within larger configurations and their `stdout` might be  piped into other tools. `Clap` is currently hard-coded in `Error::use_stderr()` to print certain messages (e.g. the help block) to `stdout`, which might lead to surprising behavior. 

### Describe the solution you'd like

It would be nice to have an option in configuring the behavior of `Error:: use_stderr()`. maybe a configuration option in `App` that would override the default behavior, e.g.:

```rust
App::new(...).
  ...
  .setting(AppSettings::PrintToStderr)
  .get_matches()
```

### Alternatives, if applicable

I am currently intercepting all `clap` messages and printing them manually like this:

```rust
App::new(...).
  ...
 .get_matches_safe()
 .unwrap_or_else(|err| {
     eprintln!("{}", err.message);
     std::process::exit(1);
});
```

but it feels a bit hacky to me. 

---

_Label `T: new feature` added by @tzakharko on 2020-04-05 15:40_

---

_Label `T: new feature` removed by @pksunkara on 2020-04-05 16:42_

---

_Label `C: settings` added by @pksunkara on 2020-04-05 16:42_

---

_Label `T: new setting` added by @pksunkara on 2020-04-05 16:42_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-05 16:42_

---

_Comment by @Dylan-DPC-zz on 2020-04-05 22:31_

@tzakharko hi thanks for the issue, would you be willing to send us a pr that makes this change? thanks

---

_Comment by @tzakharko on 2020-04-06 08:00_

@Dylan-DPC I am afraid I am neither familiar enough with the Clap codebase nor with Rust to figure out the best way of doing it at the moment. I suppose one would hav etc decouple the output settings  from the `Error` struct, so it's probably not trivial. 

I have opened the issue mainly because @pksunkara asked me to do so in a discussion thread. I understand this might be lower priority. 

---

_Comment by @Dylan-DPC-zz on 2020-04-06 14:41_

@tzakharko that's fine we will mentor you if needed. It's a good way to learn rust :D

---

_Comment by @tzakharko on 2020-04-07 07:48_

@Dylan-DPC fair enough :) I will have a look, but I won't be able to allocate any time for this short term. I will put it on my agenda and let's keep the issue open for now. 

---

_Label `D: easy` added by @CreepySkeleton on 2020-06-30 09:59_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-06-30 09:59_

---

_Label `Z: good first issue` added by @CreepySkeleton on 2020-06-30 09:59_

---

_Comment by @olson-sean-k on 2021-05-20 21:40_

I imagine this would be much trickier to implement, but how do folks feel about allowing arbitrary `Write` targets for this? Instead of naming bespoke outputs, users could instead provide an `impl Write`. This would still support selecting `stdout` or `stderr` via `std::io::stdout` and `std::io::stderr`, which both emit `impl Write` types.

I've been working on [a CLI tool](https://github.com/olson-sean-k/nym) that incorporates paging by [conditionally targeting a paging child process or the configured terminal](https://github.com/olson-sean-k/nym/blob/ae5729d574ea6554cc39ab3bf2205a984bf3b813/nym-cli/src/terminal.rs#L210). [`bat`](https://github.com/sharkdp/bat) does something similar. AFAICT, it is not possible to use `clap`'s generated help text, options, and subcommands with such a paging mechanism and the only alternative would be to define help options and subcommands manually and use `App::print{_long}_help` by hand. For example, you'll notice that both `nym` and `bat` automatically page their output except for help text, which is written to `stdout` by `clap`.

---

_Referenced in [epage/clapng#149](../../epage/clapng/issues/149.md) on 2021-12-06 19:17_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:17_

---

_Label `A-help` added by @epage on 2021-12-08 20:17_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:17_

---

_Label `T: new setting` removed by @epage on 2021-12-08 20:37_

---

_Label `C-enhancement` added by @epage on 2021-12-08 20:37_

---

_Comment by @epage on 2021-12-09 21:22_

@tzakharko I believe clap is only reporting `--help` and `--version` to stdout.  Those are explicit user actions.  What would be the use case of passing those flags but still programmatically processing the output so you don't want it on stdout?

@olson-sean-k some quick thoughts on this
- We wouldn't be able to detect coloring support.  Thats ok, the user can do it and tell us what coloring to do (we would only be able to support ansi coloring)
- It'd be a bit annoying to pass the lifetimes everywhere if its part of the builder.  We're trying to reduce our lifetimes (#1041 )

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 21:22_

---

_Label `D: easy` removed by @epage on 2021-12-09 21:22_

---

_Label `E-easy` removed by @epage on 2021-12-09 21:22_

---

_Label `S-waiting-on-author` added by @epage on 2021-12-09 21:22_

---

_Comment by @epage on 2022-01-11 17:36_

I am leaning towards rejecting this until we have a use case that explains why output going to stdout for explicit user interactions is interfering with an applications behavior (especially since there are cases where people want their `-V` to be parseable).

This isn't to make a statement of whether we want this or not but because we are specifically looking at finding ways to trim the API of clap which also raises the bar for changes that expand the API.

---

_Closed by @epage on 2022-01-11 17:36_

---

_Label `S-waiting-on-author` removed by @epage on 2022-01-11 18:20_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:20_

---

_Comment by @TheOnlyMrCat on 2022-01-13 04:03_

An example use case is if a command is leveraging shell hooks to `cd` when certain subcommands are used. The shell hook is only capable of capturing `stdout`, due to limitations of the shell language, but the program otherwise tries to behave like a normal shell command with subcommands and flags.

---

_Comment by @epage on 2022-01-13 15:14_

I've not dealt with shell hooks to `cd`.  Could you elaborate more on the relationhip between the command and the hook and how a call to `--help` or `--version` wouldn't be processed correctly?

---

_Comment by @TheOnlyMrCat on 2022-01-13 22:39_

The shell hook is the main interface to the program, so the user would be expected to run `prog --help`.
The shell hook passes all arguments through to the program and captures the contents of `stdout`. (There is no way to swap `stdout` and `stderr`).

The automatic `--help` and `--version` flags print to `stdout`, which is incorrectly interpreted as a directory to attempt to change to, and as a result are not shown to the user in the intended way.

```sh
function prog() {
    result="$(prog_bin $@)" # Run `prog_bin` with the supplied arguments, capturing stdout but leaving stderr to the tty
    [[ -n $result ]] && cd $result # If anything was printed to stdout, `cd` to it.
}
```

---

_Comment by @tzakharko on 2022-01-14 12:31_

@epage Sorry for replying late and thanks for following this through. I have to be quite honest that I do not remember what was the actual problem that made me open this issue, it was two years ago and couldn't find any relevant notes. I think it had to do with the fact that some outputs did not pipe correctly when uses as part of a complex unix command sequence, but we are not currently using these tools so I guess that's all I can say. 

At any rate, I would be fine with this being closed, as it is not a critical issue for us at the moment. But I gather from the comments that some other folks are running into real-world issues, so maybe they will have a better motivating example. 

---

_Comment by @olson-sean-k on 2022-09-21 23:37_

It seems that version `4.0` of `clap` [is on the horizon](https://epage.github.io/blog/2022/09/clap4/), so I wanted to bump this again in hopes that some of the dust has settled since this issue was first opened.

> I am leaning towards rejecting this until we have a use case that explains why output going to stdout for explicit user interactions is interfering with an applications behavior

I believe the use case described in [my previous comment](https://github.com/clap-rs/clap/issues/1788#issuecomment-845496010) has precedent and falls into this category. It is not possible to (comprehensively) implement `cli --pager=target` if `clap` cannot be configured to write to (more) arbitrary targets. Redirecting standard I/O streams does not completely solve I/O routing for a CLI application.

> We wouldn't be able to detect coloring support. ... the user can do it and tell us what coloring to do

I feel that `clap` should probably allow users to configure styled (colored) output in all cases. Arbitrary outputs could interact with the styling APIs, with something like a `RenderTarget` that provides styling information as well as the `impl Write` target. 

Regarding styling, perhaps detection and styles could be feature gated. When the styling feature is enabled, `RenderTarget::default` could try to detect styling support and use it where available while simply disabling it when the feature is not enabled. In both of these cases the default `impl Write` target could be `std::io::stdout`.

Any thoughts on something like this post-`4.0`? Thanks for taking a look!

---

_Comment by @epage on 2022-09-22 01:20_

> I believe the use case described in https://github.com/clap-rs/clap/issues/1788#issuecomment-845496010 has precedent and falls into this category. It is not possible to (comprehensively) implement cli --pager=target if clap cannot be configured to write to (more) arbitrary targets. Redirecting standard I/O streams does not completely solve I/O routing for a CLI application.

Requiring clap to take on lifetimes again to pass around the streams would be a no-go.

For you printing everything yourself, my hope is that with the new styling API I plan to focus on for 4.x, we'll be able to provide you a `render_help` function that returns a type that will output ANSI escape codes (currently, we only allow access to output stripped of escape codes)

> Regarding styling, perhaps detection and styles could be feature gated.
> 
> Any thoughts on something like this post-4.0? Thanks for taking a look!

My priority is going to be on the issues currently targeting 4.x.  People are free to come up with a design for additional APIs but the further from my priorities, the less attention it will get.  There will also be a high bar for impact on all users.

---

_Referenced in [clap-rs/clap#4248](../../clap-rs/clap/pulls/4248.md) on 2022-09-22 14:57_

---

_Referenced in [clap-rs/clap#3289](../../clap-rs/clap/pulls/3289.md) on 2022-09-22 15:02_

---

_Referenced in [bitwarden/sdk-sm#190](../../bitwarden/sdk-sm/pulls/190.md) on 2023-08-25 19:07_

---
