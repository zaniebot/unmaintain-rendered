---
number: 3166
title: Stablize Rust-Native Completion Engine  Tracking Issue
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - E-medium
  - A-completion
  - E-help-wanted
  - C-tracking-issue
assignees: []
created_at: 2021-12-13T17:55:55Z
updated_at: 2025-03-27T15:17:17Z
url: https://github.com/clap-rs/clap/issues/3166
synced_at: 2026-01-10T01:27:35Z
---

# Stablize Rust-Native Completion Engine  Tracking Issue

---

_Issue opened by @epage on 2021-12-13 17:55_

Maintainer's notes:

Remaining work for feature parity
- [x] Communicate with bash (#3656) 
- [x] Complete path `ValueHint`s (#3656)
- [x] Complete possible values (#3656)
- [x] Complete short flags (#3656)
- [x] Complete long flags (#3656)
- [x] Complete subcommands (#3656)
- [ ] Communicate with zsh (#3916)
- [ ] Communicate with fish (#3917)
- [ ] Communicate with Powershell (#3918)
- [ ] Communicate with Elvish (#3919)
- [ ] Flag values (#3920)
- [x] Multiple values (#3921)
- [x] Hidden (#3951)
- [ ] Handling of space (#5587)
- [ ] Fix handling of `OsString` to preserve non-UTF8 paths
- [x] Document that `stdout` should not be used before getting to completion processing in case the completions are requested (#5643)
- [ ] [FIGNORE](https://www.gnu.org/software/bash/manual/bash.html#index-FIGNORE) support?
- [x] Fulfill #2488 for these new completions.  Ideally, we have the registration script sourced on-demand in the docs so that it is "auto-updating" with the users tool
- [x] #3930
- [ ] Verify each shell's behavior with quoting to see if we need to add quoting to what we output
- [ ] #5729
- [x] #5730
- [ ] #5668

Non-blocking work
- Smarter selection of what to show (#5058)
- Delimited values (#3922)
- `is_require_equal_set` support (#3923)
- `last` support
- `trailing_var_arg` support
- #5284 (doesn't appear supported by static completions)
- Removing options based on conflicts (self and others)
- Any App or Arg setting not currently handled by the static completions
  - Priority to value delimiter and requires equals
- Complete more `ValueHint`s
- Figure out where this will live as the dependency requires are dramatically different than the static completions and this shouldn't be behind a feature flag for forever
- Custom help messages (#5063)
- Completing multiple path elements at once (#5279)
- nushell support (#5840)

Design considerations
- Make it easy to use for the natively supported shells but allow other shells to be supported

Prior art
- https://github.com/pacak/bpaf
- https://pypi.org/project/argcomplete
- https://github.com/rsteube/carapace
- https://github.com/microsoft/inshellisense
- https://github.com/sigoden/argc-completions
- https://click.palletsprojects.com/en/8.1.x/shell-completion/

---

#3022 was a tipping point for me in realizing that maybe our current approach to completions doesn't work.  We effectively have to implement a mostly-untested parser within each shell.  Examples of other problems that seem to stem from this:
- https://github.com/clap-rs/clap/issues/2729
- https://github.com/clap-rs/clap/issues/2750
- https://github.com/clap-rs/clap/issues/2145
- https://github.com/clap-rs/clap/issues/1822
- https://github.com/clap-rs/clap/issues/1764

If we take the approach of [argcomplete](https://pypi.org/project/argcomplete/) where we do the parsing in our core code, rather than in each completion script, this will help us share parsing logic between shell, share some or all parsing logic with clap itself, and make a subset of the logic more testable.
  
We also need to decide whether to morph the existing parser into supporting this or create a custom parser (since the needs are pretty special).  If we do a custom parser, we should probably do https://github.com/clap-rs/clap/issues/2915 first so we can reuse lexing logic between clap and the completion generation.  I've also been considering https://github.com/clap-rs/clap/issues/2912 which would allow reusing the completion logic with any CLI parser.

---

_Label `C-enhancement` added by @epage on 2021-12-13 17:55_

---

_Label `A-completion` added by @epage on 2021-12-13 17:55_

---

_Label `S-blocked` added by @epage on 2021-12-13 17:55_

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2021-12-13 17:56_

---

_Referenced in [clap-rs/clap#2778](../../clap-rs/clap/issues/2778.md) on 2021-12-13 18:05_

---

_Referenced in [epi052/feroxbuster#112](../../epi052/feroxbuster/issues/112.md) on 2022-01-10 02:23_

---

_Referenced in [clap-rs/clap#3635](../../clap-rs/clap/pulls/3635.md) on 2022-04-15 17:59_

---

_Comment by @epage on 2022-04-15 22:07_

Bash's expectations

Inputs for `-F` and `-C`:
- COMP_LINE: The current command line. This variable is available only in shell functions and external commands invoked by the programmable completion facilities 
- COMP_POINT: The index of the current cursor position relative to the beginning of the current command. If the current cursor position is at the end of the current command, the value of this variable is equal to ${#COMP_LINE}. This variable is available only in shell functions and external commands invoked by the programmable completion facilities
- COMP_KEY: The key (or final key of a key sequence) used to invoke the current completion function. 
- COMP_TYPE:   Set to an integer value corresponding to the type of completion attempted that caused a completion function to be called: TAB, for normal completion, ‘?’, for listing completions after successive tabs, ‘!’, for listing alternatives on partial word completion, ‘@’, to list completions if the word is not unmodified, or ‘%’, for menu completion. This variable is available only in shell functions and external commands invoked by the programmable completion facilities 
- 1: name of the command whose arguments are being completed
- 2: word being completed
- 3: word preceding the word being completed on the current command line

Inputs for `-F`:
- COMP_WORDS, COMP_CWORD when used with `-F`

Output for `-C`:
- print a list of completions, one per line, to the standard output. Backslash may be used to escape a newline, if necessary. 

Output for `-F`:
- It must put the possible completions in the COMPREPLY array variable, one per array element. 

See
- https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion.html#Programmable-Completion
- https://www.gnu.org/software/bash/manual/html_node/A-Programmable-Completion-Example.html#A-Programmable-Completion-Example
- https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html#Programmable-Completion-Builtins
- https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html

---

_Comment by @epage on 2022-04-16 01:39_

argcomplete emulates bash's interface in fish by
```
    set -x COMP_LINE (commandline -p)
    set -x COMP_POINT (string length (commandline -cp))
    set -x COMP_TYPE
```
and then just registers that function

https://github.com/kislyuk/argcomplete/blob/develop/argcomplete/shell_integration.py#L60

---

_Comment by @epage on 2022-04-16 01:47_

Value hints are supported on
- zsh
- fish

Tooltips are supported on
- zsh
- powershell
- elvish
- fish

We should make sure we don't lose these features as part of this transition, ie we shouldn't drop to the lowest common denominator.

---

_Referenced in [rust-lang/cargo#6645](../../rust-lang/cargo/issues/6645.md) on 2022-04-21 15:31_

---

_Comment by @epage on 2022-04-22 15:13_

For Powershell
```Powershell
Register-ArgumentCompleter
        -CommandName <String[]>
        -ScriptBlock <ScriptBlock>
        [-Native]
        [<CommonParameters>]
```
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/register-argumentcompleter?view=powershell-7.2

The block receives
> When you specify the Native parameter, the script block must take the following parameters in the specified order. The names of the parameters aren't important because PowerShell passes in the values by position.
>
> - $wordToComplete (Position 0) - This parameter is set to value the user has provided before they pressed Tab. Your script block should use this value to determine tab completion values.
> - $commandAst (Position 1) - This parameter is set to the Abstract Syntax Tree (AST) for the current input line. For more information, see [Ast Class](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.language.ast).
> - $cursorPosition (Position 2) - This parameter is set to the position of the cursor when the user pressed Tab.

The block provides [CompletionResult](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.completionresult?view=powershellsdk-7.0.0)

> The CompletionResult object allows you to provide additional details to each returned value:
>
> - completionText (String) - The text to be used as the auto completion result. This is the value sent to the command.
> - listItemText (String) - The text to be displayed in a list, such as when the user presses Ctrl+Space. This is used for display only and is not passed to the command when selected.
> - resultType ([CompletionResultType](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.completionresulttype)) - The type of completion result.
> - toolTip (String) - The text for the tooltip with details to be displayed about the object. This is visible when the user selects an item after pressing Ctrl+Space.

So it seems like Powershell can fit within rust-driven completions and provide the full feature set.

---

_Comment by @epage on 2022-04-22 21:50_

I'm starting small and prototyping for just bash support.  Looking at argcomplete, it seems they had to use their [own shlex implementation](https://github.com/kislyuk/argcomplete/blob/efe11f30290fbd298aa5e6505556bb0ab1596ea0/argcomplete/my_shlex.py).  Hoping we can avoid that.  The first step is https://github.com/comex/rust-shlex/issues/12.

---

_Referenced in [clap-rs/clap#3656](../../clap-rs/clap/pulls/3656.md) on 2022-04-22 21:52_

---

_Label `S-blocked` removed by @epage on 2022-04-27 21:35_

---

_Label `E-medium` added by @epage on 2022-04-27 21:35_

---

_Referenced in [hiro-o918/awsctx#49](../../hiro-o918/awsctx/issues/49.md) on 2022-05-04 02:06_

---

_Referenced in [clap-rs/clap#3710](../../clap-rs/clap/pulls/3710.md) on 2022-05-11 20:30_

---

_Label `C-tracking-issue` added by @epage on 2022-06-16 21:00_

---

_Renamed from "Leverage clap's parser for completions" to "Stablize Rust-Native Completion Engine  Trakcing Issue" by @epage on 2022-06-16 21:00_

---

_Renamed from "Stablize Rust-Native Completion Engine  Trakcing Issue" to "Stablize Rust-Native Completion Engine  Tracking Issue" by @epage on 2022-06-16 21:05_

---

_Label `E-help-wanted` added by @epage on 2022-06-16 21:09_

---

_Label `:money_with_wings: $20` added by @epage on 2022-06-16 21:09_

---

_Label `:money_with_wings: $20` removed by @epage on 2022-06-16 21:10_

---

_Comment by @happenslol on 2022-06-26 21:37_

Hi, I'm interested in helping with this this I'm developing a few personal tools using clap that would hugely benefit from dynamic completions. Is there any good issues that nobody is working on you could point me towards?

---

_Comment by @epage on 2022-06-28 01:12_

@happenslol Thanks!

Anything unchecked in the "Remaining work" section is up for grabs; just post here that you are getting started on it.  The highest priority is the support for each shell as that will help gauge the feasibility and provide feedback on the API.  The rest is flushing out parsing implementation.

I should split those out into individual issues to make it easier for people to coordinate on those (and to add bounties) but I probably won't have time for another week or so.

---

_Comment by @happenslol on 2022-07-02 23:03_

Alright, had a busy week, so I'm only getting back around to this now.

I've looked at the work done in #3656, and it looks like the next steps to add zsh and fish support would be to pull some of the functionality out of `clap_complete::dynamic::bash`, and adapt it to the other shells to provide the same functionality. Am I correct in assuming that?

I'd look at `argcomplete` for the other implementations here, since the current one also seems to be coming from there.

---

_Comment by @epage on 2022-07-03 03:46_

@happenslol Yes, that is correct.  I'd like to focus on shell support initially as that is where the most unknowns exist

---

_Comment by @happenslol on 2022-07-10 18:55_

Started work [here](https://github.com/happenslol/clap/tree/implement-dynamic-zsh-completion/clap_complete/src/dynamic). I'm looking at [cobra](https://github.com/spf13/cobra) for zsh completions since `argcomplete` basically takes the easy way out and tells people to enable bash completion support for zsh :sweat_smile: 

---

_Comment by @epage on 2022-07-11 12:50_

Oh if cobra is doing the same type of dynamic completions as argcomplete, that is great!  That'll serve as a much better example!

---

_Comment by @happenslol on 2022-07-11 18:13_

Yeah, the cobra code is very well documented and easy to adapt.

One thing I was wondering about is the way options like `space`/`no-space` are passed to the completion command. cobra deals with this in a pretty involved way by encoding all options (compile-time and run-time) into a bitfield, and then parsing that bitfield in their completion script. It wouldn't be too hard to adapt this, but I feel like there isn't a clear definition of what options we even want to offer and the completion output would have to be changed, so that's probably better for another PR.

For a start, I would try to keep the features that are implemented for bash.

Should I open a new issue for zsh communication for questions like these, or is there some chat/forum where we could discuss them?


---

_Referenced in [clap-rs/clap#3915](../../clap-rs/clap/issues/3915.md) on 2022-07-13 13:54_

---

_Referenced in [clap-rs/clap#3916](../../clap-rs/clap/issues/3916.md) on 2022-07-13 14:15_

---

_Referenced in [clap-rs/clap#3917](../../clap-rs/clap/issues/3917.md) on 2022-07-13 14:16_

---

_Referenced in [clap-rs/clap#3918](../../clap-rs/clap/issues/3918.md) on 2022-07-13 14:16_

---

_Referenced in [clap-rs/clap#3919](../../clap-rs/clap/issues/3919.md) on 2022-07-13 14:17_

---

_Referenced in [clap-rs/clap#3920](../../clap-rs/clap/issues/3920.md) on 2022-07-13 14:18_

---

_Referenced in [clap-rs/clap#3921](../../clap-rs/clap/issues/3921.md) on 2022-07-13 14:19_

---

_Referenced in [clap-rs/clap#3922](../../clap-rs/clap/issues/3922.md) on 2022-07-13 14:19_

---

_Referenced in [clap-rs/clap#3923](../../clap-rs/clap/issues/3923.md) on 2022-07-13 14:20_

---

_Comment by @epage on 2022-07-13 14:21_

> Yeah, the cobra code is very well documented and easy to adapt.
> 
> One thing I was wondering about is the way options like `space`/`no-space` are passed to the completion command. cobra deals with this in a pretty involved way by encoding all options (compile-time and run-time) into a bitfield, and then parsing that bitfield in their completion script. It wouldn't be too hard to adapt this, but I feel like there isn't a clear definition of what options we even want to offer and the completion output would have to be changed, so that's probably better for another PR.
> 
> For a start, I would try to keep the features that are implemented for bash.
> 
> Should I open a new issue for zsh communication for questions like these, or is there some chat/forum where we could discuss them?

I created https://github.com/clap-rs/clap/issues/3916.  We can also talk on rust-lang zulip on the WG-CLI stream

---

_Referenced in [clap-rs/clap#3951](../../clap-rs/clap/issues/3951.md) on 2022-07-19 14:17_

---

_Referenced in [fish-shell/fish-shell#9167](../../fish-shell/fish-shell/pulls/9167.md) on 2022-08-29 08:20_

---

_Referenced in [rust-lang/cargo#11052](../../rust-lang/cargo/issues/11052.md) on 2022-09-23 20:04_

---

_Referenced in [clap-rs/clap#4265](../../clap-rs/clap/issues/4265.md) on 2022-09-26 22:02_

---

_Referenced in [araujobsd/ifwifi#5](../../araujobsd/ifwifi/issues/5.md) on 2022-11-06 21:30_

---

_Comment by @blaggacao on 2022-11-07 13:00_

I want to apport a data point:

- https://github.com/rsteube/carapace

Implements cobra completion with advanced things like a fs cache for dynamic completion querys (e.g. with provenience from remote APIs) for an x amount of seconds.

Imagine a aws CLI wrapper that interacts with your custom infrastructure and things like it.

---

_Referenced in [garden-rs/garden#10](../../garden-rs/garden/issues/10.md) on 2023-01-08 04:33_

---

_Referenced in [clap-rs/clap#4612](../../clap-rs/clap/pulls/4612.md) on 2023-01-10 21:07_

---

_Referenced in [vrc-get/vrc-get#35](../../vrc-get/vrc-get/issues/35.md) on 2023-02-10 14:02_

---

_Referenced in [astral-sh/rye#97](../../astral-sh/rye/pulls/97.md) on 2023-05-03 20:37_

---

_Referenced in [clap-rs/clap#4916](../../clap-rs/clap/issues/4916.md) on 2023-05-18 15:49_

---

_Referenced in [carapace-sh/carapace-bridge#125](../../carapace-sh/carapace-bridge/pulls/125.md) on 2023-07-19 12:12_

---

_Referenced in [LucasPickering/env-select#6](../../LucasPickering/env-select/issues/6.md) on 2023-07-20 22:23_

---

_Referenced in [clap-rs/clap#4996](../../clap-rs/clap/issues/4996.md) on 2023-07-21 20:22_

---

_Referenced in [Neptune-Crypto/neptune-core#44](../../Neptune-Crypto/neptune-core/pulls/44.md) on 2023-09-19 22:39_

---

_Referenced in [clap-rs/clap#5279](../../clap-rs/clap/issues/5279.md) on 2024-01-02 19:58_

---

_Referenced in [clap-rs/clap#5284](../../clap-rs/clap/issues/5284.md) on 2024-01-04 19:03_

---

_Referenced in [clap-rs/clap#5283](../../clap-rs/clap/pulls/5283.md) on 2024-01-05 03:37_

---

_Referenced in [clap-rs/clap#5329](../../clap-rs/clap/issues/5329.md) on 2024-01-24 16:13_

---

_Referenced in [clap-rs/clap#5379](../../clap-rs/clap/pulls/5379.md) on 2024-02-28 22:09_

---

_Referenced in [clap-rs/clap#5412](../../clap-rs/clap/pulls/5412.md) on 2024-03-21 17:57_

---

_Referenced in [clap-rs/clap#5424](../../clap-rs/clap/issues/5424.md) on 2024-03-25 18:20_

---

_Referenced in [clap-rs/clap#5430](../../clap-rs/clap/issues/5430.md) on 2024-03-26 16:29_

---

_Referenced in [clap-rs/clap#5427](../../clap-rs/clap/issues/5427.md) on 2024-03-26 16:33_

---

_Referenced in [crate-ci/typos#954](../../crate-ci/typos/issues/954.md) on 2024-04-02 21:58_

---

_Referenced in [moonrepo/moon#1455](../../moonrepo/moon/issues/1455.md) on 2024-05-01 22:54_

---

_Referenced in [zellij-org/zellij#3324](../../zellij-org/zellij/issues/3324.md) on 2024-05-04 16:06_

---

_Referenced in [clap-rs/clap#3910](../../clap-rs/clap/issues/3910.md) on 2024-05-06 03:16_

---

_Referenced in [clap-rs/clap#5533](../../clap-rs/clap/pulls/5533.md) on 2024-06-14 08:22_

---

_Referenced in [clap-rs/clap#5555](../../clap-rs/clap/issues/5555.md) on 2024-06-27 01:09_

---

_Referenced in [clap-rs/clap#5562](../../clap-rs/clap/issues/5562.md) on 2024-07-05 16:02_

---

_Referenced in [clap-rs/clap#5545](../../clap-rs/clap/pulls/5545.md) on 2024-07-10 16:14_

---

_Referenced in [clap-rs/clap#5586](../../clap-rs/clap/pulls/5586.md) on 2024-07-17 14:53_

---

_Referenced in [clap-rs/clap#5594](../../clap-rs/clap/issues/5594.md) on 2024-07-24 05:50_

---

_Referenced in [clap-rs/clap#5605](../../clap-rs/clap/issues/5605.md) on 2024-07-29 13:28_

---

_Referenced in [clap-rs/clap#810](../../clap-rs/clap/issues/810.md) on 2024-08-10 00:31_

---

_Referenced in [clap-rs/clap#1335](../../clap-rs/clap/issues/1335.md) on 2024-08-10 00:34_

---

_Referenced in [clap-rs/clap#2316](../../clap-rs/clap/issues/2316.md) on 2024-08-10 00:36_

---

_Referenced in [clap-rs/clap#2488](../../clap-rs/clap/issues/2488.md) on 2024-08-10 00:38_

---

_Referenced in [clap-rs/clap#2729](../../clap-rs/clap/issues/2729.md) on 2024-08-10 00:39_

---

_Referenced in [clap-rs/clap#3553](../../clap-rs/clap/issues/3553.md) on 2024-08-10 00:41_

---

_Referenced in [clap-rs/clap#3644](../../clap-rs/clap/issues/3644.md) on 2024-08-10 00:45_

---

_Referenced in [clap-rs/clap#4652](../../clap-rs/clap/issues/4652.md) on 2024-08-10 00:45_

---

_Referenced in [clap-rs/clap#5242](../../clap-rs/clap/issues/5242.md) on 2024-08-10 00:48_

---

_Referenced in [clap-rs/clap#5504](../../clap-rs/clap/issues/5504.md) on 2024-08-10 00:49_

---

_Referenced in [clap-rs/clap#5654](../../clap-rs/clap/issues/5654.md) on 2024-08-10 02:09_

---

_Referenced in [clap-rs/clap#5655](../../clap-rs/clap/issues/5655.md) on 2024-08-10 02:09_

---

_Referenced in [clap-rs/clap#3930](../../clap-rs/clap/issues/3930.md) on 2024-08-10 02:43_

---

_Referenced in [tauri-apps/tauri#7213](../../tauri-apps/tauri/issues/7213.md) on 2024-08-10 09:31_

---

_Referenced in [clap-rs/clap#5672](../../clap-rs/clap/issues/5672.md) on 2024-08-12 17:17_

---

_Referenced in [clap-rs/clap#5678](../../clap-rs/clap/issues/5678.md) on 2024-08-16 08:12_

---

_Referenced in [rust-lang/cargo#14493](../../rust-lang/cargo/pulls/14493.md) on 2024-09-04 16:32_

---

_Referenced in [JuliaLang/juliaup#1036](../../JuliaLang/juliaup/pulls/1036.md) on 2024-09-04 18:36_

---

_Referenced in [pimalaya/himalaya#473](../../pimalaya/himalaya/issues/473.md) on 2024-09-10 08:21_

---

_Referenced in [casey/just#2406](../../casey/just/issues/2406.md) on 2024-10-02 05:12_

---

_Referenced in [autobib/autobib#67](../../autobib/autobib/issues/67.md) on 2024-10-07 23:28_

---

_Referenced in [clap-rs/clap#5771](../../clap-rs/clap/issues/5771.md) on 2024-10-09 14:48_

---

_Referenced in [clap-rs/clap#5784](../../clap-rs/clap/issues/5784.md) on 2024-10-21 12:10_

---

_Referenced in [nix-community/nh#162](../../nix-community/nh/issues/162.md) on 2024-10-24 07:07_

---

_Referenced in [carapace-sh/carapace-bin#2564](../../carapace-sh/carapace-bin/issues/2564.md) on 2024-10-24 08:34_

---

_Referenced in [rust-lang/blog.rust-lang.org#1419](../../rust-lang/blog.rust-lang.org/pulls/1419.md) on 2024-10-24 21:42_

---

_Referenced in [clap-rs/clap#5791](../../clap-rs/clap/pulls/5791.md) on 2024-10-28 20:20_

---

_Comment by @calebbourg on 2024-11-05 18:31_

Hello @epage I'd love to get involved with this. Where might be a good place to provide some value here (something on the easier side to start maybe)?

---

_Comment by @epage on 2024-11-05 18:44_

The areas we need the most help are
- Ensure Powershell works
- Identify any missing features between the static and dynamic completions and either note them in the shell specific issues or create new issues (if they apply more generally)
- Figure out #5729
- Testing the various shells and identifying incorrect behavior, particularly around quoting

---

_Comment by @daniel5151 on 2024-11-05 18:46_

Hey all, just wanted to drop a link to my own hand-rolled implementation of Rust-native argument completion for `clap`. The code is all MIT licensed. I wrote this to support a feature I wanted in $work project, and now that we went open-source, I can actually link to my effort on this issue.

https://github.com/microsoft/openvmm/blob/main/support/clap_dyn_complete/src/lib.rs

Note that I wrote this code a few years back, before the recent uptick in investments in resolving this issue, and may not reflect the current approach `clap_complete` is taking.

This implementation is not fully-featured by any means, but it does include support for several different shells (zsh, fish, powershell), various `clap` constructs, and support for dynamic runtime completions + async support (which $work project uses to dynamically suggest fields based on the return values from async RPC calls).

I'm quite interested in getting this sort of functionality working in `clap_complete`, but I unfortunately don't have time to do so myself. Folks should feel free to crib any/all functionality from this implementation, in case any of it is useful.

---

_Comment by @epage on 2024-11-05 18:58_

Some more quick ones
- Looking over https://github.com/clap-rs/clap/issues?q=is%3Aopen+is%3Aissue+label%3AA-completion+label%3AM-breaking-change and doing any unblocking work on the shell side of these so we can explore what we want the APIs to be 
- Exploring a lazier way of doing completions, see https://github.com/clap-rs/clap/issues/5668#issuecomment-2457938301

---

_Comment by @epage on 2024-11-05 19:11_

@daniel5151 looking over that, some differences in approach
- `clap_dyn_complete` completes using the index into the string rather than an index into an array of arguments.  This allows mid-word completions and I assume avoids `COMP_WORDBREAKS` problems (#3920) at the cost of needing to re-implement quoting.  I wouldn't be surprised if we need to switch to that but wanting to see how viable our current approach is
- `clap_dyn_complete` accepts completion information through the CLI.  This creates more overhead as you now need your completer to run the CLI parser and enough application startup to get there and it is more rigid for adapting to shell-specific needs or features
- `clap_dyn_complete` offers native async support.  That makes it a lot heavier weight but can offer better concurrency for longer operations
- `clap_dyn_complete` delay creates custom completers.  The use case isn't too clear to me (`complete` would just be called once, why not just do the expensive stuff there?)
- `clap_dyn_complete` builds on top of the existing parser to get context, relying on `ignore_errors`.  imo a lot of different parts of the parser shouldn't run (like overrides) and instead we should be parse only whats done.  I also don't trust `ignore_errors` enough to `unwrap` with it :).  So far, we don't provide context though there is an issue for it.

---

_Comment by @epage on 2024-11-05 19:12_

@daniel5151 any integrating of ideas from `clap_dyn_complete` will require someone to drive it, creating issues from anything "needed" from it for the `clap_complete` solution to include, being able to talk to the problems or use cases that lead to the differences.

---

_Comment by @daniel5151 on 2024-11-05 19:42_

@epage, glad to see you took a look through the code!

A few additional thoughts, in response to your analysis.

> * `clap_dyn_complete` completes using the index into the string rather than an index into an array of arguments.  This allows mid-word completions and I assume avoids `COMP_WORDBREAKS` problems ([Support flags with values in native completions #3920](https://github.com/clap-rs/clap/issues/3920)) at the cost of needing to re-implement quoting.  I wouldn't be surprised if we need to switch to that but wanting to see how viable our current approach is

This was quite important to me, as the key completion I wanted to support was akin to navigating a virtual directory listing (i.e: completing `--remote-path foo/bar/baz`, and I wanted to be able to support completions after any intermediate `/`.

But you're right - I certainly recall wrestling with nuances around quoting and shell arguments. Not sure I ever solved it cleanly.

> * `clap_dyn_complete` offers native async support.  That makes it a lot heavier weight but can offer better concurrency for longer operations

IIRC, async support was purely a nice-to-have. I'm sure some more judicious use of block_on could've worked just as well (at least, for my use-case).

> * `clap_dyn_complete` delay creates custom completers.  The use case isn't too clear to me (`complete` would just be called once, why not just do the expensive stuff there?)

I believe part of the rationale here was that I wanted to plumb-through some shared context into different completion handlers without needing to use a global static.

> * `clap_dyn_complete` builds on top of the existing parser to get context, relying on `ignore_errors`.  imo a lot of different parts of the parser shouldn't run (like overrides) and instead we should be parse only whats done.  I also don't trust `ignore_errors` enough to `unwrap` with it :).  So far, we don't provide context though there is an issue for it.

Given that I basically wrote this implementation "for fun" at $work, I wasn't super keen getting bogged down in forking / digging through `clap` internals to get things up and running. I did the best I could with the API surface that was already available.

And of course - the only performance consideration I had in mind when writing `clap_dyn_complete` was "does it run well enough for the CLI in our project", which I'm sure wouldn't scale well to sorts of massive CLIs that exist out in the wild.

* * *

But yeah, ultimately, I only wrote `clap_dyn_complete` because there was a brief lull at $work, and I thought it'd be a fun little QOL feature to hack on. I'm sure many of my design choices wouldn't translate to a robust, "productized" version of dynamic argument completions.

My goal here was to simply call-out some prior art in this space, and have `clap_dyn_complete` serve as yet another reference point for any folks actively involved in driving this issue forwards from the `clap` side.

---

_Comment by @shannmu on 2024-11-06 05:51_

 @daniel5151  Thank you for sharing. Regarding the issue of allowing completion in the middle of words and with `COMP_WORDBREAKS`, the current `clap_complete` doesn't handle it very well. I’ll take a look at your approach.

I’ve tried using bash built-in functions like `__init_completions` in scripts to mask certain characters, and it successfully entered the common complete logic. However, it seems that bash’s post-processing of completions has some special handling. For example, when completing `--flag=`, it turns the `--flag=bar` completion into `--flag=--flag=bar`.

@epage , I have an important deadline on December 2nd, so during this period, I might only push some of the previous PRs. However, I’ve been following the community’s progress on `clap_complete` and I will accelerate the effort in December.

And thanks for your involvement, @calebbourg . I’ve contributed to the `clap_complete` project as well. Feel free to reach out to me via email or zulip https://rust-lang.zulipchat.com/#narrow/channel/421156-gsoc/topic/Project.3A.20Move.20cargo.20shell.20completions.20to.20Rust if you have any questions.

---

_Referenced in [astral-sh/uv#3249](../../astral-sh/uv/issues/3249.md) on 2024-11-12 20:20_

---

_Referenced in [AndydeCleyre/zpy#32](../../AndydeCleyre/zpy/issues/32.md) on 2024-11-12 20:21_

---

_Referenced in [picodata/pike#14](../../picodata/pike/issues/14.md) on 2024-12-18 16:08_

---

_Referenced in [Anomalocaridid/handlr-regex#89](../../Anomalocaridid/handlr-regex/issues/89.md) on 2024-12-20 00:36_

---

_Referenced in [sharkdp/bat#3170](../../sharkdp/bat/issues/3170.md) on 2025-01-08 17:34_

---

_Referenced in [ctron/oidc-cli#5](../../ctron/oidc-cli/pulls/5.md) on 2025-01-23 12:26_

---

_Comment by @titaniumtraveler on 2025-01-28 11:58_

Should this issue maybe be pinned?

I tend to reference it every now and then and always have to search it from scratch, which feels unnecessary. (Especially for such a central issue!)

Of cause I could have a bookmark (which I now have made), but I often jump between two different machines and don't always sync bookmarks between them.

---

_Referenced in [ouch-org/ouch#693](../../ouch-org/ouch/issues/693.md) on 2025-01-28 11:59_

---

_Comment by @epage on 2025-01-28 14:27_

We could potentially do that.  What is the reason you keep coming back to this issue?  I just want to check if there is something else missing for us to improve.

---

_Comment by @titaniumtraveler on 2025-01-28 16:40_

> What is the reason you keep coming back to this issue?

Nothing specific really. It is mostly that I'm excited for rust-native completion and think it will solve a lot of problems I encounter.
(And therefore reference it as potential future solution for some issues)

The example that lead to me searching it *this time* was the idea for letting https://github.com/ouch-org/ouch dynamically complete archive formats.

---

Also I definitely want to contribute to this at some point, but haven't really found the time to deep-dive into how the completion engine works and stuff 🙂

Native completion is a really cool idea for `clap` and I'm really excited for it!

---

_Referenced in [rust-lang/cargo#14520](../../rust-lang/cargo/issues/14520.md) on 2025-03-20 20:06_

---

_Comment by @nkitsaini on 2025-03-26 04:07_

I was looking into this for https://github.com/astral-sh/uv/issues/8432#issuecomment-2673610628.

It would be nice if we can use rust-native completion there. One of the blocker (apart from the ones already mentioned in issue) is the support for partial completions. i.e. `sr<TAB>` completes to `src/` (without space) instead of `src/ `. 

I think this should probably a blocker for release as most path completions rely on this, unless this is already supported and I missed it.

---

_Comment by @epage on 2025-03-26 14:01_

@nkitsaini #5587 is our issue for handling of trailing space.  It sounds like there is some shell-specific behavior there as I don't see that with bash.

Also, `SubcommandCandidates` only completes the initial external subcommand and doesn't complete arguments within the external subcommand.  We'll be exploring that as part of cargo if no one else gets too it sooner.  We need to be able to have at least somewhat functioning completions with aliases and external subcommands.

> I also came across a bug in PathCompleter where it doesn't respect the filter provided. So if you only want to complete files not folders using PathCompleter::file() it will still complete both. This one is easy fix though.

Unsure what this is referring to.  If you mean `PathCompleter::file().filter()`, then `filter` overides the filter applied in `file()`, rather than concatenates.  It might be good for us to change `filter` to being a new constructor.

If you mean `PathCompleter::file` returning directories, that is because we don't exhaustively enumerate every directory to see which may return a file, so we return them to further discover files.  We sort them alter and don't allow you to annotate them with further information in `map`.

---

_Comment by @nkitsaini on 2025-03-27 06:58_

> https://github.com/clap-rs/clap/issues/5587 is our issue for handling of trailing space. It sounds like there is some shell-specific behavior there as I don't see that with bash.

Nice. So once #5587  is resolved, the `SubcommandCandidate` should kind of work as follows?
```rs
#[clap(name = "cli", add = SubcommandCandidates::new(|| { vec![
    CompletionCandidate::new("git "), #  # binary, full completion
    CompletionCandidate::new("rg "), # binary, full completion
    CompletionCandidate::new("src/"), # Python file (partial completion)
    CompletionCandidate::new("src."), # Python modules (partial completion)
] }))]
struct Cli {
    #[arg(long)]
    input: Option<String>,
}
```

> Also, SubcommandCandidates only completes the initial external subcommand and doesn't complete arguments within the external subcommand. We'll be exploring that as part of cargo if no one else gets too it sooner. We need to be able to have at least somewhat functioning completions with aliases and external subcommands.

That is what I gathered. This would be amazing when supported!



> If you mean PathCompleter::file returning directories, that is because we don't exhaustively enumerate every directory to see which may return a file, so we return them to further discover files. We sort them alter and don't allow you to annotate them with further information in map.

Yeah I got confused there, my bad. I had added a filter `filter(|x| x.file_name().starts_with("s"))` and was seeing directories that didn't start with "s", I didn't notice the directory vs file distinction.






---

_Comment by @epage on 2025-03-27 15:17_

> Nice. So once https://github.com/clap-rs/clap/issues/5587 is resolved, the SubcommandCandidate should kind of work as follows?

Whether the trailing space is included in the completion candidate or not is something to be figured out in that issue and through the implementation.  However, we expect to support it somehow.

> Yeah I got confused there, my bad. I had added a filter filter(|x| x.file_name().starts_with("s")) and was seeing directories that didn't start with "s", I didn't notice the directory vs file distinction.

If there are ideas on how to better handle this distinction, we'd be open to it.

---

_Referenced in [HULKs/hulk#1829](../../HULKs/hulk/pulls/1829.md) on 2025-04-09 16:31_

---

_Referenced in [clap-rs/clap#6016](../../clap-rs/clap/issues/6016.md) on 2025-05-27 17:32_

---

_Referenced in [mattwparas/steel#410](../../mattwparas/steel/pulls/410.md) on 2025-06-05 22:22_

---

_Referenced in [prefix-dev/pixi#4029](../../prefix-dev/pixi/pulls/4029.md) on 2025-06-27 08:01_

---

_Referenced in [clap-rs/clap#6081](../../clap-rs/clap/pulls/6081.md) on 2025-08-04 21:50_

---

_Referenced in [j178/prek#380](../../j178/prek/pulls/380.md) on 2025-08-07 06:34_

---

_Referenced in [NLnetLabs/dnst#53](../../NLnetLabs/dnst/issues/53.md) on 2025-08-08 10:06_

---

_Referenced in [work-spaces/spaces#81](../../work-spaces/spaces/issues/81.md) on 2025-08-12 00:38_

---

_Referenced in [work-spaces/spaces#82](../../work-spaces/spaces/issues/82.md) on 2025-10-11 22:24_

---

_Referenced in [clap-rs/clap#6168](../../clap-rs/clap/issues/6168.md) on 2025-10-30 14:42_

---
