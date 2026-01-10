---
number: 1232
title: Dynamic completion support
type: issue
state: closed
author: Manishearth
labels:
  - C-enhancement
  - A-completion
  - S-blocked
assignees: []
created_at: 2018-03-26T21:39:30Z
updated_at: 2024-08-10T00:35:03Z
url: https://github.com/clap-rs/clap/issues/1232
synced_at: 2026-01-10T01:26:45Z
---

# Dynamic completion support

---

_Issue opened by @Manishearth on 2018-03-26 21:39_

Maintainers notes:
- Assuming this is custom completion on top of clap-driven completion, this is blocked on #3166 
- Open question: does the completion function accept just the arg and current value or does it have access to `ArgMatches` for completing based on other data like in #1910

Like with `PossibleValue::help`, we should support [help text](https://github.com/clap-rs/clap/discussions/3622)

---
Currently clap does basic completion of arguments, however it can't be used to do things like the dynamic completion of `git checkout <branch>`.

It would be really neat if this could be handled by clap itself -- AFAICT nothing like this currently exists for any language and it would just be magical since you can then do everything in Rust code.

The rough idea is that the `Arg`/`Subcommand` builder can take a `.with_completion(|args, str|  ... )` closure. We pass the
closure the partial parsed list of other args, and the partial string being completed for this arg (may be empty). The closure returns a list of completions.

When you attempt to do something like `git branch -D foo<tab>`, the completion script will call `git --secret-completion-param 10 -- branch -D foo`, where 10 is the index in the arguments of where `<tab>` was pressed, and after the `--` we pass all the arguments. Clap parses these arguments except the partial one, and passes this down to the `with_completion` closure, which will return a list of completions which we print out (and feed back to bash or whatever).


A simpler version doesn't handle parsing all the other arguments, instead your closure just takes the partial string, so you just provide a closure operating on a string.

If this could be made to work neatly, it would be pretty great.

cc @killercup

---

_Comment by @kbknapp on 2018-03-30 01:22_

Sorry for the late reply, it's been a busy week!

This is related to #568 and something I very much want. The implementation you've outlined is pretty close to what we're thinking in #568 and probably almost exactly what will end up being implemented. I haven't worked out the final details yet as I've had other issues as as priority and the v3 release is a prerequisite for me to implement this (although I want this, or an initial implementation in the v3-alpha1 release).

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-03-30 01:23_

---

_Label `T: new feature` added by @kbknapp on 2018-04-01 03:34_

---

_Label `D: hard` added by @kbknapp on 2018-04-01 03:34_

---

_Label `P3: want to have` added by @kbknapp on 2018-04-01 03:34_

---

_Label `W: 3.x` added by @kbknapp on 2018-04-01 03:34_

---

_Label `C: completion gen` added by @kbknapp on 2018-04-01 03:34_

---

_Comment by @sportfloh on 2018-05-30 16:34_

hey, I was recently looking for such a feature and stumbled over clangs autocompletion feature (http://blog.llvm.org/2017/09/clang-bash-better-auto-completion-is.html).  Maybe their solution is somehow inspiring.
Can I help with this issue?

---

_Referenced in [clap-rs/clap#1269](../../clap-rs/clap/issues/1269.md) on 2018-06-05 01:22_

---

_Referenced in [matthias-t/workspace#17](../../matthias-t/workspace/issues/17.md) on 2018-07-31 13:23_

---

_Referenced in [rust-lang/rustup#2268](../../rust-lang/rustup/issues/2268.md) on 2020-03-29 08:08_

---

_Comment by @kinnison on 2020-03-29 08:09_

To reiterate a call for this, we'd *love* to have this in `rustup` so we could offer more localised completions (e.g. for `+foo` to complete over installed toolchains).

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-03-29 09:08_

---

_Label `P3: want to have` removed by @CreepySkeleton on 2020-03-29 09:08_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-03-29 09:08_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-09 07:19_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:19_

---

_Referenced in [clap-rs/clap#1880](../../clap-rs/clap/issues/1880.md) on 2020-04-29 15:38_

---

_Comment by @woodruffw on 2020-06-02 04:06_

I'd also like to register interest in this. It's not as conceptually clean, but an approach that injects the results of a subshell would be sufficient for many CLIs.

Borrowing the `with_completion` terminology above:

```rust
subcommand(
  App::new("dump")
  .arg(
    Arg::with_name("thing")
        .with_completion("foobar list")
  )
)
```

would go from something like this in the `bash` completions:

```bash
opts=" -h -V  --help --version  <thing> "
```

to this:

```bash
opts=" -h -V  --help --version  $(foobar list) "
```

(N.B. that this approach applies to `Arg`s and not the subcommands themselves).

---

_Referenced in [woodruffw/kbs2#32](../../woodruffw/kbs2/issues/32.md) on 2020-06-02 04:18_

---

_Comment by @CreepySkeleton on 2020-06-02 06:21_

I must admit I like the "special option to autocomplete" approach *very* much, but I've also came up with a number of tricky questions.

Let me summarize the prior discussion and outline the desired solution that _should_ work and satisfy everybody. 

We want this facility _somewhere_ in clap:
```rust
fn dynamic_completion<F>(hook: F) -> Variants 
where 
    F: FnOnce(Input) -> Variants;
```

Where to squeeze the `fn` in? I believe it should be a method in `App`, and this method should only affect the current subcommand by default. Developers are supposed to call this method for every subcommand separately. We may also provide a "global" variant.

The `hook` here is effectively a piece of developer-written code whose job is to look at the args that have already been supplied and provide the completion variants back to clap. 

How it works: presence of this kind of hook tells clap to generate a special `--autocomplete` option. The binary simply prints the completion variants, one per line (see example below).

How does clap supply the variants back to the shell so the later can use it? It doesn't. What it does is printing the values to stdin. This option is supposed to be called from inside the completions script, [like that](https://github.com/llvm-mirror/clang/blob/aa231e4be75ac4759c236b755c57876f76e3cf05/utils/bash-autocomplete.sh#L42). The script will handle the rest in a shell-specific way.

What does `Variants` look like and why not using simple `Vec<OsString>`? Because we need to leave the ability for devs to fall back to the default completion script, see also https://github.com/clap-rs/clap/pull/1793 to get what I mean by "default completion".

```rust
// we may want to extend the system in future
#[non_exhaustive]
enum Variants {
    /// Return this when the completion hook couldn't decide on the completion.
    /// This will fall back to the default completion system which should work sensibly.
    Fallback,
    
    /// Return when the hook did found some good options to complete.
    Success(Vec<OsString>) 
}
```

Some examples:
```sh
# What user does
cargo +<TAB>

# the call 
cargo --autocompletion 1 '+'

# the output (OK, pack the values and return early)
SUCSESS
nightly
stable
beta

# ATTENTION! There's a space between `+` and `<TAB> 
cargo + <TAB>

# the call
cargo --autocompletion 2 '+' 

# the output (Oops, let's just cross fingers and let the script handle the rest)
FALLBACK
```

The next question is: what is `Input`. Well, we need to 
1. Allow devs to specify the name of the long option 
2. Trim everything prior to the actual args for the current subcommand, if any
3. Tell user if there was a space between the last arg and TAB 

```rust
fn dynamic_completion<F>(name: &str, hook: F) -> Variants 
where 
    F: FnOnce(bool, &[OsString]) -> Variants;

.dynamic_completion(|has_space, args| { /* ... */ })

// alternatively, see |args, str| design in the first comment
```

Does everybody agree with this design? Any unresolved questions?

---

_Comment by @Manishearth on 2020-06-02 14:16_

Yeah, this is essentially what I proposed above, with the small tweak that I was using `--` as an argument separator instead of quoting the entire thing (I find quoting to just be very messy), and also I was havng the completion function live on the `Arg`. I do think having a completion function on the `Arg` (_and_ one that lives on the whole command) is good: this way it is very easy to write dynamic completion for _just one argument_, but you can also write it for the whole function if you want.

I'm less fond of the `has_space, args` approach because the user needs to reparse the args in this case. IMO we should parse the rest of the args and surface them to the user in a structured way, and then surface the partial string being tab-completed.



> Trim everything prior to the actual args for the current subcommand, if any

I think it should be all args except the one currently being tabbed. It should be okay to tab-complete in the middle of an arg list

---

_Comment by @CreepySkeleton on 2020-06-02 20:16_

>  instead of quoting the entire thing

No quoting the _entire_ string here (we simply don't know what the string was with most shells). We pass the args array as is, quoting each arg to guard against spaces. But using `--` is probably a good idea so, if user used `--` on his own, it would be accounted for.

```sh
# what we do
app arg1 --flag --opt1 val1 'val 2' -- something <TAB>

# what we get
app --autocomplete 7 -- '--flag' '--opt' 'val1' 'val 2' '--' 'something'
```

>  user needs to reparse the args in this case

User needs to reparse all args in any case. The "partial parsing" problem was reported as https://github.com/clap-rs/clap/issues/1880, and I think [this](https://github.com/clap-rs/clap/issues/1880#issuecomment-637779787) could work as a viable option. But anyway, the hook should work on top of bare strings and expect `partially_parsed_matches` to be incomplete and/or not quite correct. 

So, maybe:
```rust
// or maybe index instead of has_space?
.dynamic_completion(|has_space, partially_parsed_matches, args_as_os_strings| { /* ... */ })
```


---

_Comment by @kenoss on 2020-07-15 22:20_

A few weeks ago I saw completion approaches of `git`, `kubectl`, `pyenv` and `clap_generate` and I found that dynamic completion will be suitable for clap.

I agree with th CreepySkeleton's first observation.  But, I vote Manishearth's point.  I prefer the completion function live on the `Arg`.
I think forcing users to parse args themselves decrease clarity of clap.  We should provide structural way if we can.

I expect [normalization](https://github.com/clap-rs/clap/issues/1880#issuecomment-658980976) works for #1880 . 

Is there a case one have to parse raw string?  Let me classify the cases.

Simple cases.

```
# Candidates are options of `branch` subcommand.  `App("branch")` is responsible for completion.
$ git branch -<TAB>

# Candidates are short options of `branch` subcommand.  `App("branch")` is responsible.
$ git branch -a<TAB>

# Candidates are branches.  `Arg` with index of `App("branch")` is responsible.
$ git branch -a <TAB>

# No candidates.  `Arg` with short name `m` is responsible.
$ git commit -m <TAB>
```

More complex cases.
Note that [completion of git 4 uses variables](https://github.com/git/git/blob/master/contrib/completion/git-completion.bash#L3519):

- `cur`: current word, word arround <TAB>
- `prev`: previous word
- `words`: array of arguments
- `cword`: num of arguments

```
# `--strategy` and `--strategy-option`
$ git cherry-pick --strategy<TAB>

# Candidates are strategies.  `--strategy` is responsible.
$ git cherry-pick --strategy <TAB>
$ git cherry-pick --strategy=<TAB>
$ git cherry-pick --strategy <TAB> foo
$ git cherry-pick --strategy=<TAB> foo
```

The code is [here](https://github.com/git/git/blob/master/contrib/completion/git-completion.bash#L1061).
This uses `cur` and `prev`.

Clap version will be done easily because parser will remove ambiguity of ` ` and `=` and we get candidates by asking `Arg("strategy")`.

```
$ git fetch <TAB>
benches/        clap_derive/    etc/            ip6-localhost   localhost       src/            tests/          upstream
clap-perf/      clap_generate/  examples/       ip6-loopback    origin          target/         trochilidae

$ git fetch --mirror <TAB>
cow          destringify  master-orig  staging      trying       v2-master
debug        master       release      staging.tmp  v1-master    v3-dev

$ git fetch origin:
(nothing)

$ git fetch --mirror origin:
HEAD    debug   master
```

The code is [here](https://github.com/git/git/blob/master/contrib/completion/git-completion.bash#L987).
This uses subcommand's name and investigates its options.  Note that `git` has subcommands at most depth 1.

Consider the last case.  Clap version will be done by querying subcommand's name and options from `Arg`.

---

Seeing git's cases, I feel parsing by clap + `Arg.with_completion` + some query mechanism is sufficiently powerful.

---

_Comment by @kenoss on 2020-07-15 22:22_

Note that clap can automatically derive completion for short/long options in many cases, i.e., if one doesn't need special treatment.

---

_Referenced in [clap-rs/clap#1910](../../clap-rs/clap/issues/1910.md) on 2020-08-05 17:19_

---

_Referenced in [clap-rs/clap#2396](../../clap-rs/clap/issues/2396.md) on 2021-03-09 16:56_

---

_Comment by @PoignardAzur on 2021-03-19 19:11_

Is there progress on this issue? The approach suggested by @CreepySkeleton seems pretty good to me.

If the issue is stalled for need of a contributor, I'd be interested in spending a few days on it (especially if I can get some mentoring). I'd really like to improve cargo's autocompletion.

---

_Referenced in [rust-lang/cargo#9288](../../rust-lang/cargo/pulls/9288.md) on 2021-03-19 19:37_

---

_Comment by @pksunkara on 2021-03-19 19:48_

This is still in design phase. According to the current suggested design, this needs more foundational work regarding partial parsing (#1880).

---

_Referenced in [hyperledger-archives/grid#607](../../hyperledger-archives/grid/pulls/607.md) on 2021-03-26 15:10_

---

_Referenced in [pimalaya/himalaya#101](../../pimalaya/himalaya/issues/101.md) on 2021-05-03 22:35_

---

_Comment by @kolloch on 2021-05-13 19:18_

@CreepySkeleton I've been playing around with the code and I think we could get it to work to support dynamic completion of a simple flag value. I haven't looked at the shell completion logic but I am thinking in the direction of only calling `your_app --autocomplete 7 ...` if we are at a flag position with automatic expansion.

It would be super cool, to reduce all shell autocomplete logic to calling out to your prog with the `--autocomplete` argument but that would probably require a different parser or a lot more work.

Anyways, if we use the `--autocomplete` "API", we can step by step expand it to cover more use cases.

---

_Referenced in [clap-rs/clap#2488](../../clap-rs/clap/issues/2488.md) on 2021-07-06 13:44_

---

_Referenced in [clap-rs/clap#568](../../clap-rs/clap/issues/568.md) on 2021-07-22 14:37_

---

_Comment by @blaggacao on 2021-08-05 03:55_

how cobra does it:
- https://github.com/spf13/cobra/blob/master/bash_completionsV2.go
- https://github.com/spf13/cobra/blob/master/zsh_completions.go
- https://github.com/spf13/cobra/blob/master/shell_completions.go
- https://github.com/spf13/cobra/blob/master/powershell_completions.go
- https://github.com/spf13/cobra/blob/master/fish_completions.go

and here is the 'hidden' command:

https://github.com/spf13/cobra/blob/56060d19f88533a20f635116f64aff2065cf260a/completions.go#L148

looks like this is becoming a 'copy paste' task

---

_Comment by @nrdxp on 2021-08-06 17:10_

I would really appreciate this feature. Right now my app has a Struct describing what are basically inputs to all of my cli subcommands. It would be super great to use this information in completions. I was a bit surprised when I found out it may not be possible :disappointed: 

---

_Comment by @sergiimk on 2021-08-06 19:03_

As a long-time watcher of this issue thought I'd share my solution.

The CLI tool I'm working on deals with a lot of different kinds of entities that have long names (think `kubectl`), so dynamic completions were essential for usability.

The solution pieces are:
* Bash / Zsh completions [delegate all work](https://github.com/kamu-data/kamu-cli/blob/7be4742d764bf9bfc4f908e17037a7119bf20856/kamu-cli/src/commands/completions_command.rs#L3-L14) to `myapp complete -- <words> <position>` sub-command 
* The [`complete` command](https://github.com/kamu-data/kamu-cli/blob/7be4742d764bf9bfc4f908e17037a7119bf20856/kamu-cli/src/commands/complete_command.rs#L140) combines the input and a regular [`clap::App` definition](https://github.com/kamu-data/kamu-cli/blob/7be4742d764bf9bfc4f908e17037a7119bf20856/kamu-cli/src/cli_parser.rs#L72) to establish the context (which sub-command we're on, are we completing a positional or a named argument etc.)
* Using completions is as simple [as this](https://github.com/kamu-data/kamu-cli/blob/7be4742d764bf9bfc4f908e17037a7119bf20856/kamu-cli/src/cli_parser.rs#L140-L162)

It's not perfect (e.g. has troubles with multiple positional arguments of different kinds, and I struggled to make it work with `fish`) but satisfies 90% of what I need and can be extended.

For Rust apps that are very fast to start I really can't think of a good reason to generate completion scripts instead of delegating the entire work to the app itself in all cases. Dealing with completion differences between various shells is no fun.


---

_Removed from milestone `3.1` by @pksunkara on 2021-08-13 10:33_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Comment by @chisui on 2021-09-07 11:29_

Haskells [`optparse-applicative`](https://hackage.haskell.org/package/optparse-applicative-0.16.1.0#bash-zsh-and-fish-completions) has builtin completion support. Maybe a look at it's implementation is helpful.

---

_Referenced in [databendlabs/databend#2172](../../databendlabs/databend/issues/2172.md) on 2021-10-10 04:15_

---

_Referenced in [atomicdata-dev/atomic-server#188](../../atomicdata-dev/atomic-server/issues/188.md) on 2021-10-18 15:47_

---

_Comment by @woodruffw on 2021-10-21 23:33_

This isn't a catch-all solution, but: perhaps [`clap::ValueHint`](https://docs.rs/clap/3.0.0-beta.5/clap/enum.ValueHint.html) could be extended to include a variant that enables dynamism through a subprocess?

What I'm thinking of is something like `ValueHint::Process(String)`, where the variant value would be something like `"foo --bar"` and the shell completion generator would be responsible for emitting something like `$(foo --bar)` and correctly splitting and wrangling the resulting output into a completion set.

This would be a reasonable solution for many CLIs, and requires relatively little complexity on `clap`'s side (it's just another variant, and generators would be responsible for emitting it or a fallback appropriately.

Thoughts?


---

_Referenced in [clap-rs/clap#3022](../../clap-rs/clap/issues/3022.md) on 2021-11-14 01:52_

---

_Comment by @epage on 2021-11-15 14:31_

#3022 was a tipping point for me in realizing that maybe our current approach to completions doesn't work.  We effectively have to implement a mostly-untested parser within each shell.  Examples of other problems that seem to stem from this:
- https://github.com/clap-rs/clap/issues/2729
- https://github.com/clap-rs/clap/issues/2750
- https://github.com/clap-rs/clap/issues/2145
- https://github.com/clap-rs/clap/issues/1822
- https://github.com/clap-rs/clap/issues/1764

If we take the approach of [argcomplete](https://pypi.org/project/argcomplete/) where we do the parsing in our core code, rather than in each completion script, this will help us share parsing logic between shell, share some or all parsing logic with clap itself, and make a subset of the logic more testable.

In my mind, this is raising the priority of this issue.
  
This doesn't mean we'll work on it immediately though.  We need to first get clap3 out and some work that is spilling out of clap3.  We also need to decide whether to morph the existing parser into supporting this or create a custom parser (since the needs are pretty special).  If we do a custom parser, we should probably do https://github.com/clap-rs/clap/issues/2915 first so we can reuse lexing logic between clap and the completion generation.  I've also been considering https://github.com/clap-rs/clap/issues/2912 which would allow reusing the completion logic with any CLI parser.

---

_Referenced in [kube-rs/kopium#15](../../kube-rs/kopium/pulls/15.md) on 2021-11-28 19:04_

---

_Referenced in [kube-rs/kopium#6](../../kube-rs/kopium/issues/6.md) on 2021-11-28 19:20_

---

_Referenced in [epage/clapng#92](../../epage/clapng/issues/92.md) on 2021-12-06 17:33_

---

_Referenced in [epage/clapng#156](../../epage/clapng/issues/156.md) on 2021-12-06 20:15_

---

_Referenced in [epage/clapng#157](../../epage/clapng/issues/157.md) on 2021-12-06 20:15_

---

_Referenced in [epage/clapng#187](../../epage/clapng/issues/187.md) on 2021-12-06 21:14_

---

_Referenced in [epage/clapng#238](../../epage/clapng/issues/238.md) on 2021-12-06 22:24_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Label `P2: need to have` removed by @epage on 2021-12-09 16:57_

---

_Label `E-hard` removed by @epage on 2021-12-09 16:57_

---

_Label `S-blocked` added by @epage on 2021-12-09 16:57_

---

_Referenced in [tranzystorekk/pakr#1](../../tranzystorekk/pakr/issues/1.md) on 2021-12-13 18:11_

---

_Referenced in [leftwm/leftwm-theme#45](../../leftwm/leftwm-theme/issues/45.md) on 2022-01-10 07:18_

---

_Referenced in [foundry-rs/foundry#425](../../foundry-rs/foundry/pulls/425.md) on 2022-01-11 21:40_

---

_Referenced in [tealdeer-rs/tealdeer#256](../../tealdeer-rs/tealdeer/pulls/256.md) on 2022-01-15 22:36_

---

_Referenced in [It4innovations/hyperqueue#323](../../It4innovations/hyperqueue/issues/323.md) on 2022-01-18 07:57_

---

_Referenced in [casey/just#1084](../../casey/just/issues/1084.md) on 2022-02-04 00:49_

---

_Referenced in [planus-org/planus#89](../../planus-org/planus/pulls/89.md) on 2022-02-06 14:44_

---

_Referenced in [zellij-org/zellij#1169](../../zellij-org/zellij/issues/1169.md) on 2022-03-04 08:14_

---

_Referenced in [denoland/deno#14039](../../denoland/deno/issues/14039.md) on 2022-03-19 22:06_

---

_Referenced in [nextuponstream/todo#34](../../nextuponstream/todo/issues/34.md) on 2022-04-03 15:14_

---

_Referenced in [rust-lang/cargo#10472](../../rust-lang/cargo/pulls/10472.md) on 2022-04-06 15:11_

---

_Comment by @AndreasBackx on 2022-04-09 21:49_

Hey @epage, I noticed your brainstorm #3476 and relevant issues about the modularisation of Clap, and also your comment on rust-lang/cargo#10472 about perhaps making this your next Clap priority. I would love to help out with maybe an RFC or implementation, though maybe it's better to ask where a newcomer could be helping out with this issue? Let me know whether you already have any ideas and I (and others) could perhaps share some ideas as well, like already done above.

Additionally, are there any other communication channels that are more direct? The old Gitter chat does not seem to be active, there doesn't happen to be some chat channel elsewhere?

---

_Comment by @epage on 2022-04-09 22:05_

The first step is pulling out the lexer for reuse in the completions.  I'm working on that now.  Once thats in a good enough state, I can pull it out into a crate and we can create a proof of concept for this.  At that point, its filling out features which will be easy for anyone to contribute to.

---

_Referenced in [clap-rs/clap#3635](../../clap-rs/clap/pulls/3635.md) on 2022-04-15 17:59_

---

_Referenced in [rust-lang/cargo#6645](../../rust-lang/cargo/issues/6645.md) on 2022-04-21 15:31_

---

_Comment by @epage on 2022-04-27 21:21_

@AndreasBackx I've got the start for #3166 in #3656.  Once I have it merged, want to take over flushing it out so it sufficient for existing shell completions (cargo's handwritten or existing `clap_complete` users)?  Once we are at that point, we can look at resolving this issue.

> Additionally, are there any other communication channels that are more direct? The old Gitter chat does not seem to be active, there doesn't happen to be some chat channel elsewhere?

Sorry, forgot to respond to this one.   We do have a zulip instance that we've not really advertised past the maintainers yet.  I can also be reached on the rust-lang zulip strean "wg-cli".  I'm also on both rust related discord instances but I tend to miss notifications there.

EDIT: I've also gone ahead and joined "kbknapp/clap-rs" on gitter

---

_Referenced in [MatteoNardi/again#1](../../MatteoNardi/again/issues/1.md) on 2022-06-23 07:30_

---

_Referenced in [Anomalocaridid/handlr-regex#2](../../Anomalocaridid/handlr-regex/issues/2.md) on 2022-06-26 03:03_

---

_Referenced in [beatbrot/trackie#79](../../beatbrot/trackie/pulls/79.md) on 2022-07-12 12:27_

---

_Referenced in [stackabletech/stackablectl#54](../../stackabletech/stackablectl/pulls/54.md) on 2022-07-21 06:57_

---

_Referenced in [rust-lang/cargo#11052](../../rust-lang/cargo/issues/11052.md) on 2022-09-23 20:04_

---

_Referenced in [chshersh/tool-sync#133](../../chshersh/tool-sync/issues/133.md) on 2022-10-10 12:14_

---

_Referenced in [LucasPickering/env-select#6](../../LucasPickering/env-select/issues/6.md) on 2022-11-15 11:34_

---

_Referenced in [jj-vcs/jj#879](../../jj-vcs/jj/issues/879.md) on 2022-12-31 21:53_

---

_Referenced in [jj-vcs/jj#1217](../../jj-vcs/jj/issues/1217.md) on 2023-02-08 02:51_

---

_Referenced in [facebook/buck2#169](../../facebook/buck2/issues/169.md) on 2023-04-17 17:52_

---

_Referenced in [astral-sh/rye#213](../../astral-sh/rye/issues/213.md) on 2023-05-24 15:52_

---

_Referenced in [GitoxideLabs/gitoxide#877](../../GitoxideLabs/gitoxide/issues/877.md) on 2023-05-29 17:29_

---

_Referenced in [RaphGL/Tuckr#19](../../RaphGL/Tuckr/issues/19.md) on 2023-06-17 11:19_

---

_Referenced in [Neptune-Crypto/neptune-core#44](../../Neptune-Crypto/neptune-core/pulls/44.md) on 2023-09-19 22:39_

---

_Referenced in [clap-rs/clap#5157](../../clap-rs/clap/pulls/5157.md) on 2023-10-03 14:30_

---

_Referenced in [prefix-dev/pixi#383](../../prefix-dev/pixi/issues/383.md) on 2023-10-09 13:39_

---

_Referenced in [brocode/fw#226](../../brocode/fw/issues/226.md) on 2023-11-30 18:20_

---

_Referenced in [clap-rs/clap#5283](../../clap-rs/clap/pulls/5283.md) on 2024-01-05 03:37_

---

_Referenced in [kingwingfly/fav#1](../../kingwingfly/fav/issues/1.md) on 2024-02-05 19:19_

---

_Referenced in [tonarino/innernet#302](../../tonarino/innernet/issues/302.md) on 2024-02-25 16:22_

---

_Referenced in [prefix-dev/pixi#778](../../prefix-dev/pixi/issues/778.md) on 2024-02-29 10:39_

---

_Referenced in [clap-rs/clap#5424](../../clap-rs/clap/issues/5424.md) on 2024-03-25 15:12_

---

_Referenced in [clap-rs/clap#5427](../../clap-rs/clap/issues/5427.md) on 2024-03-26 16:33_

---

_Referenced in [clap-rs/clap#5449](../../clap-rs/clap/issues/5449.md) on 2024-04-09 03:39_

---

_Referenced in [pants721/skely#2](../../pants721/skely/issues/2.md) on 2024-04-13 16:38_

---

_Referenced in [casey/just#2113](../../casey/just/pulls/2113.md) on 2024-05-30 17:34_

---

_Referenced in [siketyan/ghr#382](../../siketyan/ghr/issues/382.md) on 2024-06-24 11:07_

---

_Referenced in [a-kenji/flake-edit#85](../../a-kenji/flake-edit/issues/85.md) on 2024-07-26 10:22_

---

_Referenced in [clap-rs/clap#5618](../../clap-rs/clap/pulls/5618.md) on 2024-08-02 15:02_

---

_Referenced in [clap-rs/clap#5621](../../clap-rs/clap/pulls/5621.md) on 2024-08-02 16:51_

---

_Referenced in [eclipse-ankaios/ankaios#348](../../eclipse-ankaios/ankaios/pulls/348.md) on 2024-08-09 12:45_

---

_Comment by @epage on 2024-08-10 00:33_

We have an initial version as [`clap_complete::dynamic::ArgValueCompleter`](https://docs.rs/clap_complete/latest/clap_complete/dynamic/struct.ArgValueCompleter.html)

For tracking stabilization, see #3166

---

_Closed by @epage on 2024-08-10 00:35_

---

_Referenced in [rust-lang/rustup#3156](../../rust-lang/rustup/issues/3156.md) on 2024-09-09 12:33_

---

_Referenced in [pimalaya/himalaya#473](../../pimalaya/himalaya/issues/473.md) on 2024-09-10 08:21_

---

_Referenced in [autobib/autobib#66](../../autobib/autobib/pulls/66.md) on 2024-10-04 17:05_

---

_Referenced in [wasmCloud/wasmCloud#3258](../../wasmCloud/wasmCloud/issues/3258.md) on 2024-10-21 08:41_

---

_Referenced in [nix-community/nh#162](../../nix-community/nh/issues/162.md) on 2024-10-24 07:07_

---

_Referenced in [GitoxideLabs/gitoxide#1655](../../GitoxideLabs/gitoxide/issues/1655.md) on 2024-11-06 10:22_

---

_Referenced in [work-spaces/spaces#82](../../work-spaces/spaces/issues/82.md) on 2025-10-11 22:23_

---
