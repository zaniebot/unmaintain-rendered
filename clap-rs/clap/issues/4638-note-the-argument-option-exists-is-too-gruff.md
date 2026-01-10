---
number: 4638
title: "`note: the argument '--option' exists` is too gruff"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2023-01-13T19:30:52Z
updated_at: 2023-07-17T14:12:49Z
url: https://github.com/clap-rs/clap/issues/4638
synced_at: 2026-01-10T01:27:59Z
---

# `note: the argument '--option' exists` is too gruff

---

_Issue opened by @epage on 2023-01-13 19:30_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

n/a

### Clap Version

4.1.0

### Minimal reproducible code

N/A

### Steps to reproduce the bug with the above code

`cmd --optio`

### Actual Behaviour

```
error: unexpected argument '--optio'

  note: argument '--option' exists

Usage: clap-test --option <opt>... [positional] [positional2] [positional3]...

For more information, try '--help'.
```

The suggestion is too gruff

### Expected Behaviour

Ideas for lightening things:
- Change `note` to `tip` or `help`
  - rustc uses `help`
  - I prefer `tip` as `help`  is used as a word elsewhere in these messages
- Call out that it is a "similar" argument, value, subcommand, etc
  - rustc does this

```
error: unexpected argument '--optio'

  tip: a similar argument, '--option', exists

Usage: clap-test --option <opt>... [positional] [positional2] [positional3]...

For more information, try '--help'.
```

### Additional Context

We changed from "did you mean" to "note" in #4385.

This is split out of a [reddit thread](https://www.reddit.com/r/rust/comments/10axc9u/clap_v41_a_rust_cli_argument_parser/j47gcpx/)

The problem is changing this is pervasive enough that it would be a minor incompatibility according to our new contributing guidelines.

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2023-01-13 19:30_

---

_Label `A-help` added by @epage on 2023-01-13 19:30_

---

_Added to milestone `4.x` by @epage on 2023-01-13 19:33_

---

_Comment by @Mehrbod2002 on 2023-01-29 16:10_

@epage 
Can I work on this ? 

---

_Comment by @epage on 2023-01-30 13:57_

@Mehrbod2002 the first step for this is going through the reddit thread, comparing the different options, and proposing a specific wording with justification for why that wording.

After that depends on where this falls on the line for our [compatibility expectations](https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md#compatibility-expectations).  If we decide this falls under "minor" then we need to wait longer before we can release 4.2.

---

_Comment by @epage on 2023-03-23 17:24_

As a reminder, the rustc error:
```
error[E0425]: cannot find value `i` in this scope
   --> src/builder/arg.rs:108:27
    |
108 |         Arg::default().id(i)
    |                           ^ help: a local variable with a similar name exists: `id`

For more information about this error, try `rustc --explain E0425`.
error: could not compile `clap` due to previous error
```

---

_Comment by @epage on 2023-03-23 17:26_

reddit comments

> It comes across as possible passive aggressive to me. The other changes to the message are good, though.

---

> unless the output is colored to highlight the diff it might be helpful to have 'similar' in there. Users might think this means --option exists but is not valid in this context (not realizing there is a typo, reading too fast)
> e.g. :
> an argument with a similar name exists: `--option`
which is even closer to your rustc example

> Not a fan. Too much filler before getting to the actual suggestion.
>
> I don't even think rustc diagnostics are a suitable comparison (I know the blog brought it up), as it has to show significantly more types of different errors and across multiple contexts.

> Less filler, which is good, but still has the suggestion at the end. "Similar" has a minor ambiguity about similar behaviour or meaning, rather than the name itself.
>
> The more concise your goal, the more challenging English becomes (various famous quotes about this).
>
> See what you think of my [other comment](https://www.reddit.com/r/rust/comments/10axc9u/clap_v41_a_rust_cli_argument_parser/j48fmnz/) I posted after that one. I started off undecided, but had strongly convinced myself by the time I reached my conclusion. :)

---

> I think that replacing "note" with "help" would make it quite a bit friendlier.

> All three are fine, though I prefer tip: >> note: > help:.
>
> -   note: has a slight implication that it is part of the reason / explanation, like an addendum, rather than a suggestion / action / next step.
> -   help:, to me, has a burned-in ambiguity of "here is some help for you" vs "help me!". :)
> -   hint: wasn't mentioned, but implies that clap already knows that this suggestion is the correct action (like a gentler form try:). rustc can get away with these being a compiler with far more context, but clap cannot.

---

> The exists part sounds a little odd, but also exists doesn't work either as the also can imply they both exist, rather than instead of. I've seen possible candidate used sometimes, but that's long and formal. The argument part is not needed, as the error has already said it is an argument, but then it would look really short. 
>
> How about:
>
> `tip: '--option' has a similar name`
>
> Bonus Points: Re-wording / re-ordering the error so that the bad argument name is at the front means that it also aligns with the argument name in the tip makes typos much easier to see due to that alignment. Example:

    error: '--optiom' argument unexpected or not valid here

      tip: '--option' has a similar name  

    Usage: clap-test --option <opt>... [positional]...

---

_Comment by @epage on 2023-03-23 17:32_

Errors from other ecosystems
```console
$ grep --gfdgfg
grep: unrecognized option '--gfdgfg'
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
```
```console
$ python3 --jkhjk
unknown option --jkhjk
usage: python3 [option] ... [-c cmd | -m mod | file | -] [arg] ...
Try `python -h' for more information.
```
```console
$ podman --jkhhjk
Error: unknown flag: --jkhhjk
```
```console
 git --lkjjkhk
unknown option: --lkjjkhk
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           [--super-prefix=<path>] [--config-env=<name>=<envvar>]
           <command> [<args>]
```

---

_Comment by @epage on 2023-03-23 17:42_

Quick thoughts
- Do we use ~~"unrecognized"~~, ~~"unknown"~~, "unexpected"?
  - Unexpected works best with this also being used with positionals
- Do we use "tip", ~~"note", "help", "hint"~~
  - ~~help~~ while used by rustc, within argument parsing "help" draws attention for other meanings (`--help`)
  - ~~`hint`~~ sounds too opinionated (we know the answer), that we are stringing them along
  - I prefer `note:` but there seems to be enough interest in this changing that my next preference is towards `tip`
- How much we favor a nicer message or easier comparison of the unexpected / suggested
- ~~"argument", "flag", or "option"?~~ the error exisits for any type of argument
  - In older versions of clap, "flag" strictly meant it didn't take a value (a FLAGS section in help) and "option" strictly meant it did
  - Flag and option are briefer and adds an extra bit of context

---

_Referenced in [clap-rs/clap#4797](../../clap-rs/clap/pulls/4797.md) on 2023-03-28 02:29_

---

_Referenced in [clap-rs/clap#4798](../../clap-rs/clap/pulls/4798.md) on 2023-03-28 03:19_

---

_Referenced in [rust-lang/cargo#11905](../../rust-lang/cargo/pulls/11905.md) on 2023-04-12 13:24_

---

_Comment by @epage on 2023-07-17 14:12_

As this has been addressed for a while and there hasn't been any further concerns raised, I'm going to consider this resolved

---

_Closed by @epage on 2023-07-17 14:12_

---
