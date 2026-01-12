```yaml
number: 2684
title: "complete/fish: Make fish completion more intelligent"
type: pull_request
state: merged
author: blyxxyz
labels: []
assignees: []
merged: true
base: master
head: fish-complete-context
created_at: 2023-12-11T17:50:46Z
updated_at: 2024-01-08T15:54:35Z
url: https://github.com/BurntSushi/ripgrep/pull/2684
synced_at: 2026-01-12T18:23:14Z
```

# complete/fish: Make fish completion more intelligent

---

_@blyxxyz_

- Stop using `-n __fish_use_subcommand`. This had the effect of ignoring options if a positional argument has already been given, but that's not how ripgrep works.

- Only suggest negation options if the option they're negating is passed (e.g., only complete `--no-pcre2` if `--pcre2` is present). The zsh completions already do this.

- Take into account whether an option takes an argument. If an option is not a switch then it won't suggest further options until the argument is given, e.g. `-C<tab>` won't suggest options but `-i<tab>` will.

- Suggest correct arguments for options. We already completed a fixed set of choices where available, but now we go further:

  - Filenames are only suggested for options that take filenames.

  - `--pre` and `--hostname-bin` suggest binaries from `$PATH`.

  - `-t`/`--type`/&c use `--type-list` for suggestions, like in zsh, with a preview of the glob patterns:
  ![image](https://github.com/BurntSushi/ripgrep/assets/15526524/8b3f8a58-0177-48cd-9f20-67a039ef24f2)

  - `--encoding` uses a hardcoded list extracted from the zsh completions. This has been refactored into a separate file, and the range globs (`{1..5}`) replaced by comma globs (`{1,2,3,4,5}`) since those work in both shells. I verified that this produces the same list as before in zsh, and the same list in fish (albeit in a different order).

---

_Comment by @BurntSushi on 2024-01-06 15:37_

Nice thank you! I made one small change to ensure that `include_str!("encodings.sh")` is only included once. I'm not sure if the compiler would automatically de-duplicate them, so I'd just rather do it myself. I also wrapped stuff to 80 columns.

I don't personally test these sorts of things (well, I do test zsh because I use it), so I'm just going to merge it as-is and we can always improve it as feedback comes in.

---

_Merged by @BurntSushi on 2024-01-06 15:39_

---

_Closed by @BurntSushi on 2024-01-06 15:39_

---

_Comment by @cstyles on 2024-01-07 20:45_

> Only suggest negation options if the option they're negating is passed (e.g., only complete `--no-pcre2` if `--pcre2` is present). The zsh completions already do this.

I think the intent here is good but unfortunately completions aren't "config-file-aware" which is kinda annoying. For example, I have `--max-columns-preview` in my `.ripgreprc` file and if I want to disable it for a particular invocation of ripgrep, now I have to type `--no-max-columns-preview` out each time.

---

_Branch deleted on 2024-01-07 20:56_

---

_Comment by @blyxxyz on 2024-01-07 21:04_

The zsh completions have the same behavior, so I figured it'd be OK, but maybe I should have given it more thought.

I think it's fixable by reimplementing `__fish_contains_opt`. I'll look into that.

---

_Comment by @BurntSushi on 2024-01-07 21:25_

@okdana Do you have thoughts? Ideally we would keep the behavior roughly the same between zsh and fish if possible. (You might have five the zsh completions before config file support landed?)

---

_Comment by @BurntSushi on 2024-01-07 21:26_

@cstyles In the future, concerns like this should be in new issues. As it is, this concern will be very difficult to track.

---

_Comment by @okdana on 2024-01-08 13:54_

> Only suggest negation options if the option they're negating is passed (e.g., only complete --no-pcre2 if --pcre2 is present). The zsh completions already do this.

I didn't really look at the code, and i'm not familiar with fish conventions, but this description is actually the *opposite* of how the zsh function behaves

In general with zsh completion you assume that if the user's already given a flag on the command line, they probably don't want to see other flags whose effects are exclusive with it, whether that's direct negation like `--pcre2`/`--no-pcre2`, multi-state settings like `--case-sensitive`/`--ignore-case`/`--smart-case`, etc.

This assumption is built into how `_arguments` specs are defined, so most functions that use `_arguments` work that way. And i think that's usually reasonable, though it does become annoying particularly with aliases, which zsh by default expands and interprets as if they were typed out by the user

As far as i know there isn't a standard way of bypassing that behaviour when it's not desired

> @okdana Do you have thoughts? Ideally we would keep the behavior roughly the same between zsh and fish if possible. (You might have five the zsh completions before config file support landed?)

As mentioned, if the above description is accurate, this is actually not how the zsh function works, so there's already a disconnect

The point about completion not being config-file-aware *does* apply to the zsh completion, but since the function can only use that information to *take away* options it would otherwise offer, i don't think it's worth trying to account for it (and afaik 99% of functions don't). Users will just sometimes see possibilities where they wouldn't if the options were typed directly on the command line

I do wish i could fix the alias problem i mentioned, because it causes similar misbehaviour. e.g. if the user has `alias rg='rg --pcre2'`, then it will not complete `--no-pcre2`, or vice versa — unless they work around it with the option `complete_aliases` and/or with manual `compdef`s, but those are arguably even more annoying. I'm not sure it's possible, though

Anyway, i would suggest that each function try to follow the conventions expected by its respective shell's users, whatever that may be in fish's case

---

_Comment by @blyxxyz on 2024-01-08 14:50_

Thanks, I see now. I conflated that conditional handling with this feature:
```zsh
  # ripgrep has many options which negate the effect of a more common one — for
  # example, `--no-column` to negate `--column`, and `--messages` to negate
  # `--no-messages`. There are so many of these, and they're so infrequently
  # used, that some users will probably find it irritating if they're completed
  # indiscriminately, so let's not do that unless either the current prefix
  # matches one of those negation options or the user has the `complete-all`
  # style set. Note that this prefix check has to be updated manually to account
  # for all of the potential negation options listed below!
  if
    # We also want to list all of these options during testing
    [[ $_RG_COMPLETE_LIST_ARGS == (1|t*|y*) ]] ||
    # (--[imnp]* => --ignore*, --messages, --no-*, --pcre2-unicode)
    [[ $PREFIX$SUFFIX == --[imnp]* ]] ||
    zstyle -t ":completion:${curcontext}:" complete-all
  then
    no=
  fi
```
zsh's behavior is indeed very different from what I implemented.

Fish completions commonly omit negation flags entirely. At least that's what the completions for curl and HTTPie do. Making them conditional seems better than that, and once it's config-aware I don't see significant downsides.

Hiding them unless the user has entered a prefix (like the above) is possible but definitely unconventional for fish completions.

---

_Comment by @BurntSushi on 2024-01-08 15:47_

Aye okay, I agree that it makes sense to do the conventional thing for each shell. That makes the most sense to me in terms of what users are going to expect.

I defer to Fish users to implement what is conventional though.

---

_Comment by @okdana on 2024-01-08 15:54_

> Making them conditional seems better than that, and once it's config-aware I don't see significant downsides.

Parsing the config file and inserting its contents into `argv` is really trivial (at least it would be in the zsh function), but keep in mind that actually getting the config path can only work some of the time. e.g. if you do (assume the fish equivalents here)

```
% export RIPGREP_CONFIG_PATH=/path/to/config
% rg -<TAB>
```

then it's no problem, the completion function has access to that value, but if you do

```
% RIPGREP_CONFIG_PATH=$mypath/config rg -<TAB>
```

it won't work reliably because your completion function can't just unilaterally execute the user's variable expansions and command substitutions, and maybe can't even see that assignment in the first place (zsh functions can't afaik)

In zsh, this possibility is basically ignored. But it's not that common to use environment variables to suppress completion possibilities in zsh functions. And the ripgrep FAQ does explicitly mention that second construction as a way to pass the config-file path, so i thought i should mention

You would also need to pre-check the command line for `--no-config` ofc

> Hiding them unless the user has entered a prefix (like the above) is possible but definitely unconventional for fish completions.

Yeah, it's unconventional for zsh as well. I just kind of made it up tbh. Would be fine from a convention perspective to change or get rid of it, if someone wanted

---
