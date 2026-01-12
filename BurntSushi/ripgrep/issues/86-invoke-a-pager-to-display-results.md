```yaml
number: 86
title: invoke a pager to display results
type: issue
state: closed
author: c-cube
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2016-09-25T17:07:40Z
updated_at: 2025-10-24T18:39:07Z
url: https://github.com/BurntSushi/ripgrep/issues/86
synced_at: 2026-01-12T16:13:21Z
```

# invoke a pager to display results

---

_@c-cube_

Just tried the tool, it's great (as is the blostpost btw). I'm usually using `grep -r foo | less -R` for searching (with an alias to get colored output), since there might be several pages of results; `rg` coloring is nice but is lost if piped into a pager. Would it make sense to invoke a pager automatically if the tool detects that its output is not piped?


---

_Comment by @BurntSushi on 2016-09-25 17:26_

You might consider passing the `-p` flag to `rg`, which should cause it to retain its formatting.

I'm a little skeptical about actually calling a pager from within `rg`. That kind of seems like it's doing too much. (I will note though that `git grep` seems to do it, and it seems smart. It will invoke the pager automatically if there are lots of results.)


---

_Comment by @c-cube on 2016-09-25 17:28_

Indeed, I will probably alias `rg` to `rg -p`. Still, automatic paging is quite nice :-)


---

_Label `question` added by @BurntSushi on 2016-09-25 19:04_

---

_Comment by @ayoshi on 2016-09-25 19:05_

BTW, there is a nice way to invoke pager from rust console CLI: https://gitlab.com/imp/pager-rs it does pager instantiation in a quite clever way 


---

_Comment by @BurntSushi on 2016-09-25 19:12_

@ayoshi That is pretty slick. It won't work on Windows though. Doing it only on *nix is an option if we decide to proceed.


---

_Comment by @bluss on 2016-09-26 10:03_

git does it the same way I believe, makes sure that the pager is the parent process.


---

_Comment by @JohnVillalovos on 2016-09-26 17:01_

+1 from me on having a pager. I like how "git grep" will page the results. Something like that would be nice.

I can do:

`$ rg -p foo | less -r`

Mostly works. But it can leave stray colors. As in my prompt becomes green.


---

_Comment by @BurntSushi on 2016-09-26 17:04_

@JohnVillalovos If you use `less -R`, does that help with the stray colors?


---

_Comment by @JohnVillalovos on 2016-09-26 17:09_

@BurntSushi Thanks, that does work :)

More typing than I would like.  /me lazy  ;)  But it works!


---

_Comment by @BurntSushi on 2016-09-26 17:15_

@JohnVillalovos Yeah, I have `alias less="less -R"` in my `.bashrc`. :-)


---

_Comment by @bluss on 2016-09-26 18:46_

um I'm trying to make a shell script for that (better than alias, available in more contexts)

``` sh
#!/bin/sh

rg -p "$@" | less -R
```

(chmod +x that thing and call it as `rg test` to try). This just shows a blank terminal and seems to eat memory. **Warning**: If you reproduce, be sure to Ctrl-C it quick. Am I doing something wrong or what's happening?


---

_Comment by @bluss on 2016-09-26 18:49_

Oh.. oh.. :tired_face: 

any brown paper bags available?

_Hint: I made it spawn itself recursively._ sigh.


---

_Comment by @kugel- on 2016-09-27 08:19_

I also support this feature, missing it badly from ag and git grep.


---

_Comment by @nextlogic-ono on 2016-10-08 20:26_

@bluss Perhaps as a function instead?
Got this in .zshrc, works well.

```
function rg()
{
   /usr/local/bin/rg -p "$@" | more -R
}

```


---

_Comment by @bluss on 2016-10-08 22:57_

I use this in my bin directory, works well

``` bash
#!/bin/sh

~/.cargo/bin/rg -p "$@" | less -RFX
```


---

_Comment by @BurntSushi on 2016-10-10 23:48_

I like @bluss's approach here. I'm not strongly opposed to adding this into `ripgrep` itself, but it seems better to at least try and get by without doing so.


---

_Closed by @BurntSushi on 2016-10-10 23:48_

---

_Comment by @ssokolow on 2017-03-13 22:31_

I just noticed the need for this and augmented my config-file wrapper to also pipe the output through `less -RFX` if it sees `-p` in the options.

https://github.com/ssokolow/profile/blob/master/home/.zshrc.d/rg

(Feel free to change that. I just needed something quick and didn't think I'd ever use `-p` for anything other than making the colors work when piped through `less`.)

I still think the need for the wrapper is a bit of a wart though, given that, in the abstract, such tools are competing on how convenient they can be and my wrapper feels fundamentally no different than "Oh? You want to ignore stuff in your `.gitignore`? Well, we offer a command-line flag to ignore things. You're free to write a wrapper." (Reading `--type-add` from a config file) or "Oh? You want colored output? Well, the colordiff people showed it can be done in a wrapper." (pager support)

---

_Comment by @jason-s on 2017-05-17 14:57_

`less` doesn't seem to ANSI terminal control sequences on Windows. Would be really nice to have a built-in pager. 

I guess what I don't understand behind the philosophy of trying to use external utilities for paging, is that ripgrep already takes care of some presentation issues through the use of ANSI coloring.

---

_Comment by @BurntSushi on 2017-05-17 14:59_

> Would be really nice to have a built-in pager.

This is a completely unreasonable request, so I'm just going to nip this one right in the bud: so long as I'm the maintainer, ripgrep will never ever never get a built-in pager.

---

_Comment by @ssokolow on 2017-05-17 15:02_

> less doesn't seem to ANSI terminal control sequences on Windows.

From what I remember, `cmd.exe` doesn't use ANSI sequences. Last time I looked at making one of my utilities do portable colorization, it seemed that `cmd.exe` expected you to do text colorization through a side-channel more similar to the "set pen attributes" APIs you find in things like Qt's QPainter.

(Which would mean that `less` would have to be color-aware enough to parse the input and convert it to an interleaved sequence of text blocks and "set color" API calls.)

---

_Comment by @jason-s on 2017-05-17 15:06_

>From what I remember, cmd.exe doesn't use ANSI sequences.

Well, rg itself uses colors just fine, so either it's using some other mechanism for Windows, or Windows does support ANSI terminal sequences.

I've tried updating `less` to the latest version I can find for Windows (v381 from UnxUpdates.zip in http://unxutils.sourceforge.net/ which does support `-R` and `-r`; my previous version only supported `-r`) but it still turns the ANSI sequences into visible gobbledygook.

I understand the need to keep things simple but it really impairs the usability of rg on Windows when there is a lot of output.

---

_Comment by @BurntSushi on 2017-05-17 15:15_

> I understand the need to keep things simple

Simplicity is only part of it. The other part of it is that you're asking me or someone else to build *an entire cross platform pager* into ripgrep, and then you're asking me to maintain that. It's an unreasonable request not only because it completely violates the principle of responsibility, but that's unreasonable because it would be an ungodly amount of really annoying work. On top of all of that, I don't even know that such a thing is possible in Windows.

---

On coloring... The standard way to color things in Windows is through the various console APIs. It's completely unlike ANSI escape sequences. Instead of putting special codes into the output text, you instead need to: 1) stop writing output, 2) ask the console to change the color to X, 3) write output again and 4) ask the console to change the color back to the setting before X.

With that said, Windows 10 has some support for ANSI escape sequences, but it needs to be explicitly enabled. Alternatively, the various MSYS2 terminals for cygwin support ANSI escape sequences.

> I understand the need to keep things simple but it really impairs the usability of rg on Windows when there is a lot of output.

I've spent a ton of time making ripgrep work well on Windows, but I have to draw the line somewhere. I don't know exactly where it is, but it certainly doesn't include a built-in pager.

---

_Comment by @jason-s on 2017-05-17 15:17_

Fair enough, that explains some of the complexity.

Maybe by the time I get a new laptop later this year and it's Windows 10 then this becomes a non-issue for me. I certainly don't defend Microsoft's decisions for brain-dead implementations of things that otherwise work very well in Unix-land.

Looks like 

    DOSKEY rgl=rg --color never $* ^| less

works for me in my crippled Windows 7 world....

---

_Comment by @BurntSushi on 2017-05-17 15:27_

> Maybe by the time I get a new laptop later this year and it's Windows 10 then this becomes a non-issue for me.

FWIW, I don't think ripgrep quite works yet with ANSI support on Windows 10. I think it really needs a champion to make it happen who knows more about how to make it work. You can force ripgrep to emit ANSI escapes with `--color ansi`---even on Windows---but I think you actually need to enable some config knob to make ANSI support work on Windows.

> `DOSKEY rgl=rg --color never $* ^| less`

Oh, I thought you wanted paging with coloring. Disabling coloring is certainly one way of getting rid of the  escapes. Although, I do find it interesting that you're seeing ANSI escapes at all in a Windows console. That shouldn't happen.

> I certainly don't defend Microsoft's decisions for brain-dead implementations of things that otherwise work very well in Unix-land.

I won't lie, sometimes I think this way too. But I try hard not to, and try extra hard not to actually say it because it tends to just ruffle everyone's feathers. And Unix has its fair share of strange things too. :-)

---

_Comment by @jason-s on 2017-05-17 15:37_

Well, I'd like paging with coloring if I can get it, but I need paging and if the way to do that is disabling coloring, then it works well enough for me.

> Although, I do find it interesting that you're seeing ANSI escapes at all in a Windows console. That shouldn't happen.

No idea. I just use `rg` without options, and I get the coloring, but if I pipe it through `less` then I see all the ANSI-looking gobbledygook like... oh wait a minute, it disappears when I pipe it through less. I only get it when I use `rg -p otherstuff | less`. Sigh. Never mind.

---

_Comment by @ssokolow on 2017-05-17 15:40_

> I certainly don't defend Microsoft's decisions for brain-dead implementations of things that otherwise work very well in Unix-land.

It's not always Microsoft's fault. For example, they chose `\` as a path separator when they updated DOS to support directories because they were already using CP/M-style `/` to denote command-line switches and I remember reading that the decision to follow CP/M on that front ultimately lay with IBM.

...and, the more layers of backward-compatibility abstraction you drop below, the more POSIX-compatible modern Windows tends to get. (I assume, purely out of practicality.)

For example, as far as I've been able to determine, DOS and Windows have accepted `/` as a path separator in APIs where it's not ambiguous since DOS 2.0 introduced directories. It's just the command-line parsing which requires paths to use `\` to avoid ambiguity.

...and kernels of the Windows NT lineage don't use drive letters internally... they're just an artifact of the Win32 API subsystem and it's possible to specify paths using an internal, singly-rooted syntax. (Though, admittedly, that *is* the one notable exception to the `/` compatibility. You have to use `\` in and after the escape prefix to bypass the Win32 path rules.)

(the NT kernel was written so that Win32, OS/2-compatibie, and POSIX-compliant APIs could be offered as pluggable subsystems and NTFS actually has a POSIX "personality" where, among other things, the only disallowed filename characters are `\0` and `/`. WSL is just the latest in a lineage of POSIX subsystem modules for the NT kernel.)

---

_Comment by @labianchin on 2017-09-24 15:53_

_Here how I solved this, if others are looking:_
I used to use `ag` before and defined an alias in `bashrc`/`zshrc` to "invoke ag with less":

```
alias agl="ag --pager='less -XFR'"
```

For `rg` I am now using:

```
rgl() {
  rg -p "$@" | less -XFR
}
```

---

_Comment by @ibotty on 2018-02-12 16:02_

I argue that it would be very convenient to  have it built-in. As @ssokolow said: it's about convenience.

Having said that: The following snippet (a bash function) will invoke `less` only when the output is a terminal. That way, there won't be strange escape sequences when redirecting stdout.

```bash
rg() {
    if [ -t 1 ]; then
        command rg -p "$@" | less -RFX
    else
        command rg "$@"
    fi
}
```

---

_Comment by @scorphus on 2018-05-07 08:37_

IMO, ripgrep should stick with searching and not involve any pager. The output can be easily paged as discussed above.

For those of you who use **Fish Shell**, here's the content of your ` ~/.config/fish/functions/rg.fish`:
```fish
function rg
  if isatty stdout
    command rg -p $argv | less -RMFXK
  else
    command rg $argv
  end
end
```
For those wondering, `less` options explained:
* `-R`: support color output (ANSI color sequences)
* `-M`: show line number information in the bottom of screen (current/pages X%)
* `-F`: automatically quit `less` if the entire example fits on the first page
* `-X`: do not use init/deinit strings; in other words: do not clear the screen
* `-K`: exit `less` in response to Ctrl-C (`^C`)

---

_Comment by @cemeyer on 2018-09-30 19:25_

The arguments presented against fixing this issue essentially boil down to simplicity of implementation, vs user convenience.

On the other hand, it seems to me. like @ayoshi's pager-rs suggestion would be extremely simple for the implementation.  I agree that implementing an entire pager in ripgrep is excessive.  But forking out to something like `less`, on platforms where it exists, is not unreasonable (IMO).

Fixing this seems beneficial for ripgrep users.  I think making every single user that wants paging reimplement `git` or `ag` paging in shell has a definite cost, and is a barrier for adoption vs something like `ag`.  `ag` has substantially similar performance and at least a history of substantially better ergonomics.  `rg` can do better than it does today :-).

For zsh users looking to work around this issue, here's what I've got in my `.zprofile` (it is essentially identical to @ibotty's bash function; the long options to `less` are just `-XFR`):

```
export RIPGREP_CONFIG_PATH="$HOME/.ripgreprc"
function rg(){
    # If outputting (fd 1 = stdout) directly to a terminal, page automatically:
    if [ -t 1 ]; then
        command rg --pretty "$@" \
            | less --no-init --quit-if-one-screen --RAW-CONTROL-CHARS
    else
        command rg "$@"
    fi
}
```

---

_Comment by @BurntSushi on 2018-09-30 20:37_

> ag has substantially similar performance and at least a history of substantially better ergonomics.

Neither of these things are obviously true to me.

The work around to this issue is simple enough that I'm fine with the status quo for now.

Complexity of implementation vs user convenience is a valid trade off to make, because we live in the real world where maintenance effort is a limited finite resource. So simply pointing to the existence of that trade off is not an effective way to convince me something.

Something that I would consider to be a constructive contribution here would be a deep dive on how other widely deployed cross platform software implements automatic paging functionality.


---

_Comment by @cemeyer on 2018-09-30 20:49_

> > ag has substantially similar performance and at least a history of substantially better ergonomics.
>
> Neither of these things are obviously true to me.

With all respect, as the primary author you're going to have certain biases and implicit blinders.  The tool is written primarily for your own itch, so by definition it meets your needs exactly.  Your preferences are not necessarily the same as everyone else (and evidence shows they are not).

> Complexity of implementation vs user convenience is a valid trade off to make, because we live in the real world where maintenance effort is a limited finite resource.

I absolutely agree.  I didn't mean to suggest otherwise.  I was just summarizing the discussion.

> So simply pointing to the existence of that trade off is not an effective way to convince me something.

Right.  That isn't intended to be supporting evidence, just a summary of the pros/cons.  I think the feedback from multiple users that find the ag behavior ergonomic _should be_ somewhat persuasive, but again, it's ultimately your program.

> Something that I would consider to be a constructive contribution here would be a deep dive on how other widely deployed cross platform software implements automatic paging functionality.

That would be interesting, yes.

---

_Comment by @BurntSushi on 2018-09-30 21:02_

> With all respect, as the primary author you're going to have certain biases and implicit blinders. The tool is written primarily for your own itch, so by definition it meets your needs exactly. Your preferences are not necessarily the same as everyone else (and evidence shows they are not).

Most (or at least many) features found in ripgrep are things that I do not use at all, because I either do not like them or do not find them useful in my particular circumstances. Now that you know that I understand the fairly obvious fact that my preferences are not the same as everyone else's, and that moreover, I'm willing to accommodate other's preferences even when they aren't my own, will you now be willing to elaborate on your generalizations that "ag has **substantially** similar performance and at least a history of **substantially** better ergonomics." (Emphasis mine.)

> Right. That isn't intended to be supporting evidence, just a summary of the pros/cons. I think the feedback from multiple users that find the ag behavior ergonomic should be somewhat persuasive, but again, it's ultimately your program.

I understand the benefits of having a built in pager. Moreover, I understand that some users would benefit from such functionality. These are not the only things that factor into my decision, which I think I've elaborated on above. My decisions aren't immutable. If you've watched this issue tracker over the years, I've reversed my thinking on several features as more feedback comes in and more is learned about potential implementation paths and estimated maintenance burden.

---

_Comment by @cemeyer on 2018-09-30 21:23_

Yep, understood — I am certainly not trying to fault your intelligence :-).  Ultimately, I am just providing a "me too" on this one.  (I think the 'ag' comparison has turned into a distraction — it's my subjective opinion, but it isn't an argument that is really on-topic for this issue.  Let's not litigate it here.)

---

_Comment by @tudor on 2019-03-12 22:46_

It would be great if ripgrep used an external pager only if stdout is a tty. So the same command can display pretty (colorized, paged) results on screen, or output raw results when redirected to a file.

Today, I have to run `rg -p "$@" | less -R` (aliased as `rgl`) if I want to look at the output, and `rg` directly if I want to redirect output.

The Silver Searcher has a `--pager` command-line flag that is ignored when the output is not a tty. Obeying the `PAGER` environment variable would be nice too.

---

_Comment by @tudor on 2019-03-12 22:50_

Actually, this seems to be a good workaround:

```
rg() {
    if [[ -t 1 ]]; then
        /usr/bin/rg -p "$@" | less -RFX
    else
        /usr/bin/rg "$@"
    fi
}
```

---

_Comment by @dwijnand on 2019-03-13 07:03_

@ibotty's version in https://github.com/BurntSushi/ripgrep/issues/86#issuecomment-364968686 is even better as it avoids picking a path.

---

_Comment by @tudor on 2019-03-13 16:05_

@dwijnand I missed that when scrolling through this task. Thanks!

---

_Comment by @BurntSushi on 2020-10-01 14:22_

This was recently requested again in #1695, and I think my stance on this has softened. I think I was originally opposed to this because of Windows and my effort to make sure everything was as cross platform as possible. But I don't think this should hold back efforts to improve environments where pagers are common.

My current plan is to add a `--pager` flag that will cause ripgrep's results to show inside of a pager if stdout is a tty. I'll probably look at how `git grep` invokes a pager and copy that logic since it has always worked well for me in practice.

---

_Reopened by @BurntSushi on 2020-10-01 14:22_

---

_Comment by @BurntSushi on 2020-10-01 14:23_

(I'm not sure when I'll have time to work on this, but if someone else wanted to submit a PR, that would be great!)

---

_Label `question` removed by @BurntSushi on 2020-10-01 14:23_

---

_Label `enhancement` added by @BurntSushi on 2020-10-01 14:23_

---

_Label `help wanted` added by @BurntSushi on 2020-10-01 14:23_

---

_Comment by @scorphus on 2020-10-01 15:28_

Sure! I'll take this one! 🥨🍻

---

_Comment by @scorphus on 2020-10-01 15:39_

The crate [pager](https://docs.rs/pager/0.15.0/pager/) is really nice and fits well. I'm gonna use it. Any objections, @BurntSushi? BTW, any further tips are more than appreciated.

---

_Comment by @BurntSushi on 2020-10-01 15:54_

@scorphus I am skeptical. That crate hasn't been updated in 2 years. Here is what I would do:

1. Research how `git grep` spawns a pager. This is important because `git grep` is a widely used and popular tool. It is very likely that they've probably iterated towards an optimal solution. So let's benefit from their experience and practice by seeing if we can emulate their logic.
2. Scrutinize the `pager` crate to see if it matches `git grep`'s behavior. Also, carefully review `pager`'s internals to make sure it looks okay. I just did a brief scan and I can already see that its atty detection won't work on Windows, so that's probably a deal-breaker unless its API allows us to work around that.

The implementation of `pager` does contain unsafe, and normally I'd see that as a good reason to use a crate for it, because it becomes a rallying point for other people to join together and audit and fix bugs that arise. But if the crate isn't being maintained, then this stops being an advantage.

---

_Comment by @wonderfulspam on 2021-01-07 19:10_

The `pager` crate has recently been updated but it still doesn't seem like it quite cuts the mustard, IMO. 

> Something that I would consider to be a constructive contribution here would be a deep dive on how other widely deployed cross platform software implements automatic paging functionality.

This is a very shallow (and opinionated) analysis rather than a deep dive, but perhaps it may be of use:

The `git grep` [implementation](https://github.com/git/git/blob/3cf59784d42c4152a0b3de7bb7a75d0071e5f878/builtin/grep.c) is a little gnarly with the logic for deciding whether and how to spawn a pager residing at the bottom of a 300+ line function. I would not be surprised if there are more subtle bugs like [this one](https://github.com/git/git/commit/c2048f0b394fc5d5644956481b39f3d099cbe51c) lurking under the surface.

I would consider taking a look at `bat`'s [implementation](https://github.com/sharkdp/bat/blob/master/src/output.rs). `bat` may not be as widely used as `git grep` but it does happen to be cross-platform, is written in Rust and, anecdotally, has provided a flawless paging experience for me in the past two years or so that it's been my daily driver. At the very least, @sharkdp may be able to provide some valuable pointers on the pitfalls and gotchas. See, for example, https://github.com/sharkdp/bat/pull/1402, or https://github.com/sharkdp/bat/issues/887#issuecomment-616156939 in which the maintainer of `less` even piped in to confirm that the [bug in question existed in `less`](https://github.com/gwsw/less/commit/a737ff2d16e66f10d8e39225466b524b818c9fa2), not `bat`.

Interestingly, [`bat` is currently exploring adding a built-in pager](https://github.com/sharkdp/bat/issues/1053) which would, presumably, also be available as a library that might be of interest.

---

_Comment by @scorphus on 2021-01-07 20:13_

Yeah! I've been considering bat's implementation as the one to follow. I've also been using it flawlessly for quite some time.

TBH, I didn't look too much into anything else, particularly git's one, although it was suggested — that code frightened me, to say the least. I didn't entirely believe it could really help ☺️

Thank you for the the links, @wonderfulspam! 🙌 Espcially the last one, I wasn't aware of the idea around a built-in parser for bat, that might be something to consider!

---

_Comment by @sharkdp on 2021-01-08 20:05_

> I would consider taking a look at `bat`'s [implementation](https://github.com/sharkdp/bat/blob/master/src/output.rs). `bat` may not be as widely used as `git grep` but it does happen to be cross-platform, is written in Rust and, anecdotally, has provided a flawless paging experience for me in the past two years or so that it's been my daily driver. At the very least, @sharkdp may be able to provide some valuable pointers on the pitfalls and gotchas.

I think you already pointed out the most important things. I'll try to list a few points that come to my mind right now:

- Adding paging support to `bat` is something that turned out to be MUCH more painful than I imagined initially (take a look at the amount of issues linked in https://github.com/sharkdp/bat/issues/1053). If you take a look at our implementation, you can probably tell that it's not completely trivial to make this work consistently across platforms and `less` versions. The logic would be a lot simpler if ripgrep decides to only add a `--pager=<cmd>` option (instead of also adding `PAGER` handling, something like `RIPGREP_PAGER`, etc.).
- One of the most annoying things in `bat` is the interplay between the pager and `bat`s own line wrapping and line-number output (especially if the terminal is resized). I guess this wouldn't be a problem for ripgrep.
- Another pain point is colorized output in combination with a pager. `less` can pipe-through ANSI codes when called with the `-R` option, but we had a lot of weird things happen. Especially on Windows.
- Making this an opt-in feature in ripgrep is definitely a good idea. From experience, I can tell that unknowing users will otherwise assume that the `less` frontend is part of the application itself. If they are not familiar with a pager, that can be confusing (how do I quit?). Also, they will report all kinds of bugs in `less` against your repo (e.g.: scrolling does not work).
- Even if users opt in with a `--pager=<cmd>` option in their config file, it might be a good idea to have a `--paging=auto/never/always` option (similar to what the `--color=…` option does). For `bat`, we even added a short option `-P` to disable the pager because it's used so often. Imagine you are fine-tuning a search pattern and run multiple ripgrep searches in sequence. You probably don't always want to quit the pager in-between.
- The explanation in https://github.com/sharkdp/bat#using-a-different-pager might be of interest (how we call `less` and why).

> Interestingly, [`bat` is currently exploring adding a built-in pager](https://github.com/sharkdp/bat/issues/1053) which would, presumably, also be available as a library that might be of interest.

If we ever make this happen, it would be available as a library. Yes. However, most of the features that we would want in `bat` are probably not that useful for ripgrep.

---

_Comment by @sergeevabc on 2021-04-01 19:21_

Gosh, these stubborn developers… Guys, check out [vgrep][1].
Basically, it’s a mix of Ripgrep and Less (with truly working colors), which we’d like to see here.

[1]: https://github.com/vrothberg/vgrep/

---

_Comment by @God-damnit-all on 2021-11-15 18:10_

I would like to point out that if you're using PowerShell (regardless of platform), `Out-Host -Paging` seems to work just fine with ripgrep's `--color always` parameter. Its pager leaves much to be desired, though.

You can also set `$PSStyle.OutputRendering` to `ANSI` and then `less -R` should work fine, even with [less-Windows](https://github.com/jftuga/less-Windows).

ripgrep having its own pager would certainly be more convenient, though.

---

_Comment by @dandavison on 2023-03-15 10:08_

[delta](https://github.com/dandavison/delta) has special support for acting as a pager for ripgrep: see the [delta manual section](https://dandavison.github.io/delta/grep.html). In addition to paging, this brings two other features:
- syntax-highlighted code in the search hits
- search hits can be formatted as terminal hyperlinks, which can be configured to open your text editor at the appropriate line (e.g. see [open-in-editor](https://github.com/dandavison/open-in-editor/))

You use it by piping `rg --json` output to delta. Here's an example in the delta repo using a light background and the `GitHub` color theme:

```sh
rg --json 'handle' | delta
```

**EDIT** Please see update below: https://github.com/BurntSushi/ripgrep/issues/86#issuecomment-1632851762


---

_Comment by @BurntSushi on 2023-03-15 12:03_

Wow, that is pretty slick! Nice work. @dandavison I've added a link to that from ripgrep's README: https://github.com/BurntSushi/ripgrep/commit/595e7845b87c1b9e6cfd4f1c23b3910dca3e15f0

PRs are welcome if you'd like to make wording tweaks!

---

_Comment by @ryuheechul on 2023-03-30 23:54_

Thanks @dandavison's comment!
Now my alias has changed like below

```zsh
# from this
alias rg='rg -p -i'

# to this function
rg ()
{
  env rg --json $@ | delta
}
```

And it works just like how `bat` works in terms of automatic paging!

---

_Comment by @wbolster on 2023-03-31 08:21_

> ```shell
> env rg --json $@ | delta
> ```

@ryuheechul, for correctness with all file names etc, you should be using this function body:

``` shell
command rg --json "$@" | delta
```

(you could also detect whether the output goes to a tty and avoid calling `delta` in that case, but the exact desired behaviour is subjective.)

---

_Comment by @ryuheechul on 2023-03-31 17:23_

@wbolster good points to think about and will be helpful as context when I encounter related issues :), thanks!

---

_Comment by @krishnakumarg1984 on 2023-05-08 10:37_

@BurntSushi I wonder if the cross-platform pager crate [`minus`](https://github.com/arijit79/minus) shall be of immediate help.  `minus` is written both as a library and a binary crate, with the library specifically intended for being used as a pager integration with other applications. This is exactly the use case we have here, and seems like a perfect fit for the problem we have.

For more info, please read the `minus` [README section on integration with applications](https://github.com/arijit79/minus#the-problem-with-traditional-pagers) as well as [this blog post](https://pijul.org/posts/2020-12-09-minus/) by its author detailing its design and purpose.

Hope that helps, and soon we won't hopefully need to check for & rely on the presence of an external tool `delta` for paged output from `ripgrep`.

---

_Comment by @BurntSushi on 2023-05-08 11:08_

@krishnakumarg1984 I do not think it's a good idea to depend on external crates like that for core functionality. As soon as I ship something that is based on what `minus` provides, I've essentially signed up to provide that functionality forever, regardless of whether `minus` continues to be maintained or not. It also has a very large dependency tree. A significant fraction of ripgrep's dependency tree. There's probably some overlap, but that's not something that inspires confidence from my perspective. Instead, it just increases the risk of having to maintain something else.

---

_Comment by @dandavison on 2023-07-12 16:26_

Quick update on delta's ripgrep support:

Delta now tries to exactly replicate ripgrep output format, so the idea is that piping `ripgrep --json` to delta should be a strictly additive improvement, bringing:
- paging
- syntax highlighting
- hyperlinks to open matching lines in an editor/IDE
- "n" and "N" keybindings to navigate between matches

The hyperlinks are particularly convenient if you're using an editor such as VSCode, Pycharm, or IntelliJ that install their own URL handlers for opening links. 

Docs: https://dandavison.github.io/delta/grep.html

Please open an issue in the delta repo if delta is failing to emulate the visual appearance of ripgrep output that you would like.

In the image below, the line numbers are [OSC8 terminal hyperlinks](https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda).

<table><tr><td><img width="600" alt="image" src="https://github.com/BurntSushi/ripgrep/assets/52205/5593f633-d279-43bd-b436-1d68a4376fa2"></td></tr></table>


---

_Comment by @ltrzesniewski on 2023-07-12 19:54_

@dandavison Nice work!

I couldn't find the URI format spec for the JetBrains IDEs, I hope you don't mind if I add the one you've found to #2483 (I'd like for ripgrep to support hyperlinks out of the box).

Did you by any chance find a list of the URI schemes used by the other JetBrains IDEs besides IntelliJ and PyCharm? If not, they should be rather easy to guess, but I can install them all just to make sure.

---

_Comment by @BurntSushi on 2023-11-26 19:08_

I think while I had said my stance on this softened a while back, I'm ultimately going to pass on this. I think this is a good example of scope creep, and that ripgrep just does not benefit much from having a `--pager` flag when users can invoke a pager themselves (or write an alias to do it for them).

---

_Closed by @BurntSushi on 2023-11-26 19:08_

---

_Label `help wanted` removed by @BurntSushi on 2023-11-26 19:09_

---

_Label `wontfix` added by @BurntSushi on 2023-11-26 19:09_

---

_Comment by @txtsd on 2024-11-09 13:14_

I just wanted to show everyone how beautiful `delta`'s output is! :purple_heart: 


![2024-11-09-18:40:57:44:582274252-640x1052_grim](https://github.com/user-attachments/assets/4ff6594e-58ea-4f7b-95f7-bce3497423f8)

I came here fully prepared to ask for native support for paging via delta, and then I found this issue. I wanted to show everyone what beauty sparked that compulsion.

---

_Comment by @gcflymoto on 2024-11-10 08:30_

Beautiful! What terminal are you using? Still see hyperlinks from latest delta broken in Konsole and kitty. 

---

_Comment by @txtsd on 2024-11-10 08:43_

> Beautiful! What terminal are you using? Still see hyperlinks from latest delta broken in Konsole and kitty. 

I use kitty! Hyperlinks worked when I tried them yesterday. 

---

_Comment by @UnsolvedCypher on 2025-01-15 14:59_

For fish users wanting to use Delta for your pager, you can add an abbreviation: `abbr -a --set-cursor -- rg 'rg % | delta'` . This will pipe your `rg` into `delta` but place your cursor before the pipe (where the % is) so you can type your query.

---

_Comment by @ciscohack on 2025-10-24 18:30_

Hello experts

if use this script how to hide line number i tried --no-line-number and -N but that doesn't work

#!/bin/sh

rg -p "$@" | less -R

---

_Comment by @ciscohack on 2025-10-24 18:31_

> BTW, there is a nice way to invoke pager from rust console CLI: https://gitlab.com/imp/pager-rs it does pager instantiation in a quite clever way

@ayoshi how to use it in install can you help me to know

---

_Comment by @BurntSushi on 2025-10-24 18:38_

@ciscohack `rg -p -N "$@" | less -R` should work just fine.

Please don't bump old issues to ask questions. Please ask questions via [discussions](https://github.com/BurntSushi/ripgrep/discussions/).

---

_Locked by @ghost on 2025-10-24 18:39_

---
