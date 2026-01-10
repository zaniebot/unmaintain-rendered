---
number: 4916
title: "Completions shouldn't suggest options unless the current token starts with a `-`"
type: issue
state: closed
author: jthulhu
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2023-05-18T15:07:53Z
updated_at: 2024-08-10T00:47:06Z
url: https://github.com/clap-rs/clap/issues/4916
synced_at: 2026-01-10T01:28:03Z
---

# Completions shouldn't suggest options unless the current token starts with a `-`

---

_Issue opened by @jthulhu on 2023-05-18 15:07_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

`4` in `Cargo.toml`, the actual version is `4.2.1`

### Describe your use case

I am trying to provide shell completions for my program. As it uses `clap`, I'd like to leverage the (very cool) `clap-generate`. Unfortunately, `clap-generate` currently has an undesirable which is (as far as I've found out) not configurable: it will generate *every* possible completions, which is usually not what I'd like it to do. For instance, options (eg. `--help`) shouldn't be shown to the user unless the user has already started typing a dash, in which case only the options should be shown.

In general, I'd like the shell completions to follow the behavior of `git`, which I think has some very good shell completions.

### Describe the solution you'd like

I'd like `clap-generate` to be more configurable. The following settings should be available: whether to hide options unless they're the option completion possible, and whether to sort completion alphabetically or by order in which the subcommands are defined (if applicable).
Optionally, allow `clap-generate` to be "smarter": allow for some "folded" options (ie. when trying to complete `git clone --`, it will propose, among other suggestions, `git clone --no-...`; when trying to complete `git clone --no-`, only then it will show all the options that start with `--no-`).
Optionally, allow `clap-generate` to hide short options (one letter options): git hides them, so I guess it's ok not to show them.

### Alternatives, if applicable

Do not make it configurable, just make the asked behavior the only available behavior.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @jthulhu on 2023-05-18 15:07_

---

_Comment by @epage on 2023-05-18 15:49_

So I'm not a completion expert and most of it is more community maintained (though #3166 would fix most of that) but I believe we are just registering completions with the shell and the shell is deciding what options to show (which would also change with #3166).

---

_Comment by @jthulhu on 2023-05-18 15:59_

#3166 looks most promising, so it'll probably solve this issue.

Besides, I think that `clap-generate` is indeed just passing every completion to the shell, but that's precisely what I am asking it not to do: it would fit my use-case better if `clap-generate` filtered some completions, because showing everything is a bit overwhelming, even with very few options.

(this hasn't much to do with this issue, but I don't know where else to say this: great job with this library, it's quite amazing!)

---

_Comment by @epage on 2023-05-18 16:19_

So instead you don't want some arguments completed?

In the new completion system, we were looking at having hidden arguments completed only if there was nothing better to complete but I assume you wouldn't want to generally hide these.

We have been working on a plugin system that would allow `ValueHint` to be exclusively defined in `clap_complete`.  We could use this to allow arguments to be hidden from completions as well

I would be concerned about usability of not completing things for people though.

---

_Comment by @jthulhu on 2023-05-18 16:22_

Indeed, I don't want them to be completed *unless* the current word the user is typing starts with `--`, in which case I'd want them to be completed. I want the same behavior as git: if I try to complete `git |` (where `|` is the point), I get all subcommands but no option, and if I try to complete `git --|` I get all options.

---

_Comment by @epage on 2023-05-18 17:32_

If you have concrete suggestions on how to do that today, I would be open to it.  If not, this is likely reserved for the dynamic completions.

I was curious and it looks like argcomplete make this configurable

        :param always_complete_options:
            Controls the autocompletion of option strings if an option string opening character (normally ``-``) has not
            been entered. If ``True`` (default), both short (``-x``) and long (``--x``) option strings will be
            suggested. If ``False``, no option strings will be suggested. If ``long``, long options and short options
            with no long variant will be suggested. If ``short``, short options and long options with no short variant
            will be suggested.

https://github.com/kislyuk/argcomplete/blob/develop/argcomplete/finders.py#L78

---

_Comment by @jthulhu on 2023-05-18 18:01_

I know how to do that for bash (I guess this would be similar for other shells?). First of all, you'd have to do some bookkeeping to distinguish between options and arguments/subcommands. Then, in the generated script, in each function responsible for generating the completions, there should be
```bash
if [[ "$cur" == -* ]]; then
    COMPREPLY=(`compgen -W '<here should go the options>' -- $cur)
    return
fi

if [[ $COMP_CWORD -eq <position> ]]; then
  COMPREPLY=(`compgen -W '<here should go the subcommands>' -- $cur)
  return
fi
```
as opposed to how it is currently generated
```bash
if [[ $cur == -* || $COMP_CWORD -eq <position> ]]; then
  COMPREPLY=(`compgen -W '<here go both options and subcommands>' -- $cur)
  return
fi
```
Unfortunately, I am not familiar with `clap` source code, so I can't provide more specific details on how to do the bookkeeping (that must already happen somewhere, since options and arguments are already distinguished in `clap`), nor do I realize exactly how much work does that entail.

---

_Label `A-completion` added by @epage on 2023-05-18 19:00_

---

_Comment by @epage on 2024-08-10 00:47_

As I believe this is resolved with the native completions, I'm going to close this.  If this was incorrect, let us know!  I would recommend creating specific issues for each of what is missing.

You can track stabilization at #3166

---

_Closed by @epage on 2024-08-10 00:47_

---
