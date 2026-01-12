```yaml
number: 182
title: "--color does not work under Emacs"
type: issue
state: closed
author: xuchunyang
labels:
  - bug
assignees: []
created_at: 2016-10-15T08:04:07Z
updated_at: 2017-01-14T08:13:33Z
url: https://github.com/BurntSushi/ripgrep/issues/182
synced_at: 2026-01-12T16:13:21Z
```

# --color does not work under Emacs

---

_@xuchunyang_

I'm using rg 0.2.1 and Emacs 25. It looks like `--color` doesn't have effect under Emacs, while other tools such as grep works.
- `M-! echo hello | grep --color=always hello` gives `[01;31m[Khello[m[K`
- `M-! echo hello | rg --color=always hello` gives `hello` (where is the escape code?)

see also https://github.com/emacs-helm/helm/issues/1624#issuecomment-253879188


---

_Comment by @BurntSushi on 2016-10-15 13:31_

Tracking down why colors aren't working has historically been a very challenging task because a lot of it is dependent on your environment. For example:

```
$ TERM=foobar rg --color=always PATTERN --debug
```

Colors no longer work. The `--debug` flag might show a lot of other junk, but in my case, I do see this:

```
DEBUG:rg::out: error loading terminfo for coloring: could not find a terminfo entry for this terminal
```

If I try this with GNU grep, colors still work, even if my `TERM` setting is nonsensical:

```
TERM=foobar grep --color=always PATTERN *
```

So this could be one possible explanation. There may be other problems. For example, if `TERM` is unset:

```
$ unset TERM
$ rg --color=always PATTERN --debug
```

In the debug output, I see:

```
DEBUG:rg::out: error loading terminfo for coloring: TERM environment variable unset, unable to detect a terminal
```

Once again, GNU grep still works. It seems GNU grep will fall back to some default settings that `ripgrep` doesn't do, and maybe we should do that. In the mean time, my strong suspicion is that something about your environment isn't configured correctly and GNU grep is bailing you out.

So, can you run `rg` with `--debug` and see if you can find the relevant error message? If not, you can just post the entire output. If that doesn't turn up anything, what is the value of `TERM` when `rg` is run? What about `TERMINFO`?

Thanks!


---

_Comment by @xuchunyang on 2016-10-15 13:43_

`M-! echo hello | rg --debug --color=always hello` returns

```
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'h',
        'e',
        'l',
        'l',
        'o'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(hello)], limit_size: 250, limit_class: 10 }
DEBUG:rg::out: environment doesn't support coloring
hello
```

`M-! echo $TERM` returns `dumb` and `TERMINFO` is empty, probably not set.


---

_Comment by @BurntSushi on 2016-10-15 14:06_

Having `TERMINFO` unset is OK. `ripgrep` will look in standard locations for your terminfo database.

Indeed, if I set `TERM=dumb`, then I also get the same error message as you.

So... It sounds like you need to change your `TERM` to a setting that supports colors. There's more info on the `dumb` setting [here](http://stackoverflow.com/questions/39001517/features-obligatory-for-term-dumb-terminal) and [here](http://emacs.stackexchange.com/questions/10137/what-does-term-dumb-in-eshell-mean). Running `infocmp dumb` shows that a `dumb` terminal has very limited capabilities. If Emacs does indeed support colors, then it sounds like you'll need to change your `TERM` environment variable to something else.


---

_Comment by @thierryvolpiatto on 2016-10-15 14:30_

Actually colors are working fine not only for GNU/grep, but also `ag`, `pt`, `ack-grep` and `git-grep`, so perhaps the question is: What is different in ripgrep than on e.g `ag` that make ripgrep failing with a DUMB terminal ?


---

_Comment by @BurntSushi on 2016-10-15 14:37_

At least GNU grep hard codes ANSI color escape sequences and AFAIK doesn't actually respect the `TERMINFO` database. e.g.,

```
static const char *selected_match_color = "01;31";  /* bold red */
```

`ripgrep` isn't failing as far as I can see, it's simply respecting what the environment is saying, while all of the other tools aren't.

What is the correct behavior here? If `TERM=dumb` is set and `--color=always` is set, then how does `ripgrep` know which `TERM` settings to use? Should it forcefully pick a `TERM` other than `dumb`? Which one?


---

_Comment by @BurntSushi on 2016-10-15 14:43_

@ticki You seem to know a thing or two about this sort of thing. Can you make sense of this? I notice in your `termion` library, you don't read from the `TERMINFO` database and hardcode escape sequences. Does this work correctly across all terminals? (I suppose you're in good company, since GNU grep does the same.) If so, what's the purpose of the `TERMINFO` database in the first place? For things more advanced than colors?


---

_Comment by @thierryvolpiatto on 2016-10-15 14:47_

Andrew Gallant notifications@github.com writes:

> At least GNU grep hard codes ANSI color escape sequences and AFAIK doesn't actually respect the TERMINFO database. e.g.,
> 
> static const char _selected_match_color = "01;31";  /_ bold red */
> 
> ripgrep isn't failing as far as I can see, it's simply respecting what the environment is saying, while all of the other tools aren't.
> 
> What is the correct behavior here? If TERM=dumb is set and
> --color=always is set, then how does ripgrep know which TERM settings
> to use? Should it forcefully pick a TERM other than dumb? Which one?

Ok, just found that if I set TERM to eterm-color colors with ripgrep are
working, so perhaps it is the one to use when TERM=dumb ?

BTW ripgrep is using a basic red color, how do we configure which color
to use ?
Does ripgrep obey to `GREP_COLORS` env var ? Seems no.
Does it have a config file where we can set this ?

## 

Thierry


---

_Comment by @BurntSushi on 2016-10-15 14:49_

> Ok, just found that if I set TERM to eterm-color colors with ripgrep are
> working, so perhaps it is the one to use when TERM=dumb ?

"it works for me" isn't really a strong reason to assume "it always works," unfortunately.

> BTW ripgrep is using a basic red color, how do we configure which color
> to use ?
> Does ripgrep obey to `GREP_COLORS` env var ? Seems no.
> Does it have a config file where we can set this ?

Colors aren't yet configurable. See: #51


---

_Comment by @BurntSushi on 2016-10-15 14:51_

From reading the source of `termion`, I wonder if it'd be better to stop using the `term` crate, switch to `termion` for POSIX compatible stuff and handroll Windows support. This would mean not supporting the `TERMINFO` database, and I'm not sure what the implications of that are. (But it would also mean that colors would work independently of a `TERMINFO` database.)


---

_Comment by @BurntSushi on 2016-10-15 14:54_

@thierryvolpiatto @xuchunyang I think the bottom line here is that while `ripgrep` is technically respecting the letter of the law, it doesn't actually match up with user expectations and that's bad. That means I agree we should do something here, but the right answer might take a bit of time to happen. For now, I'd request setting `TERM` to something that makes `ripgrep` work as a temporary work-around. Hopefully that's feasible to do.


---

_Label `bug` added by @BurntSushi on 2016-10-15 14:54_

---

_Comment by @thierryvolpiatto on 2016-10-15 15:06_

Andrew Gallant notifications@github.com writes:

> @thierryvolpiatto @xuchunyang I think the bottom line here is that
> while ripgrep is technically respecting the letter of the law, it
> doesn't actually match up with user expectations and that's bad. That
> means I agree we should do something here, but the right answer might
> take a bit of time to happen.

There is no hurry, thanks for all your explanations and sorry to not be
able to help on this, my knowledge about all this is limited. 

> For now, I'd request setting TERM to something that makes ripgrep work
> as a temporary work-around. Hopefully that's feasible to do.

Ok, we will use `TERM=eterm-color` as a workaround for now. 

Thanks again for your help on this.

## 

Thierry


---

_Comment by @ticki on 2016-10-16 12:05_

@BurntSushi So TERMINFO is mostly for detecting compatibility, but is only really relevant for very old terminals. Termion implements escapes which are supported by virtually every VTE used today (most of it is straight out of the ANSI standards, some are ones introduced by xterm). For the Windows stuff: I'm not that familiar with the Windows API, but I'm more than willing to accept a PR adding Windows support to Termion (i.e. adding the winapi calls to the formatter, I guess).


---

_Comment by @BurntSushi on 2016-10-16 13:54_

@ticki Got ya. Given that every other tool in this space seems to not care about `TERMINFO`, it seems OK for `ripgrep` to do the same. With that in mind, I'd like to switch Unix handling to using `termion`, which I think should be straight-forward.

I would definitely recommend treading carefully with Windows. For Windows, you need to make _synchronous_ API calls to the console you're writing to, which means that you can't really implement Windows support in some arbitrary formatter that can write to anything. For `ripgrep`, this is particularly bad because we want to write matches to an in memory buffer and only actually print them later. There are a couple ways of doing this, including writing an ANSI interpreter, but I opted for something simpler but just as hacky: https://github.com/BurntSushi/ripgrep/blob/master/src/terminal_win.rs

What I'm saying is: as a future user of `termion`, I don't expect you to support Windows for this. I expect to be subjected to grueling pain. :-)


---

_Comment by @ticki on 2016-10-16 18:37_

@BurntSushi I agree with that. It would definitely be incredibly hacky to add windows support.


---

_Comment by @kaushalmodi on 2016-10-20 14:17_

I'm wondering if resolving this issue would also solve that other issue where ripgrep is inserting strange escape characters to the colored text. 


---

_Comment by @BurntSushi on 2016-10-20 14:22_

@kaushalmodi I bet it will, considering the new approach _should_ be isomorphic to what GNU grep does today, which doesn't suffer from the same issue (I think).


---

_Closed by @BurntSushi on 2016-11-20 20:01_

---

_Comment by @thierryvolpiatto on 2017-01-13 17:15_

Still not working in emacs with helm and from any emacs terminal (term or shell-mode, eshell) with rg version 0.3.2.  However it is working fine with 0.2.3 and the workaround we found:

`TERM=eterm-color rg --color=always --smart-case --no-heading --line-number %s %s %s`

Here the command line used with 0.3.2 (not highlighting):

`rg --color=always --smart-case --no-heading --line-number %s %s %s`

![capture d ecran_2017-01-13_18-03-37](https://cloud.githubusercontent.com/assets/1533939/21938213/acf88f62-d9ba-11e6-8044-0ae9d4239afa.png)

Same result with the workaround:

`TERM=eterm-color rg --color=always --smart-case --no-heading --line-number %s %s %s`

Now with rg version 0.2.3 and this command line (now matches are highlighted as expected):

`TERM=eterm-color rg --color=always --smart-case --no-heading --line-number %s %s %s`

![capture d ecran_2017-01-13_18-08-00](https://cloud.githubusercontent.com/assets/1533939/21938353/48aa1fd4-d9bb-11e6-872d-a8f94fa41bfc.png)


---

_Comment by @BurntSushi on 2017-01-13 18:12_

@thierryvolpiatto Thanks for taking the time to try and confirm the fix! Sorry it isn't working right for you. It's hard for me to understand your comment, but you're saying you can't get ripgrep `0.3.2` to output colors at all in emacs, regardless of any workaround? Can you try `--color ansi` instead of `--color always`? Also, what operation system are you on?

I honestly have no clue what's going on. If you have `--color always` on non-Windows platforms, then it should always emit ANSI escape sequences now. On Windows however, `--color always` will always attempt to use the native console API for colors, which might not work in emacs. Instead, `--color ansi` will force the issue for ANSI escapes. Alternatively, `--color auto` with `TERM=eterm-color` should also work.

---

_Comment by @thierryvolpiatto on 2017-01-13 18:56_


Thanks for looking at this.

Andrew Gallant <notifications@github.com> writes:

> @thierryvolpiatto Thanks for taking the time to try and confirm the
> fix! Sorry it isn't working right for you. It's hard for me to
> understand your comment, but you're saying you can't get ripgrep 0.3.2
> to output colors at all in emacs, regardless of any workaround?

Yes that's it, and 0.2.3 is working fine, I didn't try any version
between 0.2.3 and 0.3.2 though.

> Can you try --color ansi instead of --color always?

Still not working

> Also, what operation system are you on?

GNU/Linux

> Alternatively, --color auto with TERM=eterm-color should also work.

Same not working.

Let me know if I can do something to help debugging.

Thanks.

--
Thierry


---

_Comment by @BurntSushi on 2017-01-13 19:00_

@thierryvolpiatto Can you try to reproduce the issue outside of emacs? Basically, what you're reporting doesn't make any sense to me. :-)

---

_Comment by @thierryvolpiatto on 2017-01-13 19:10_


Andrew Gallant <notifications@github.com> writes:

> @thierryvolpiatto Can you try to reproduce the issue outside of emacs?

No, outside of emacs rg works perfectly.

> Basically, what you're reporting doesn't make any sense to me. :-)

Sorry if my explanations are confuse, let me know what you are not
understanding.

Thanks.

-- 
Thierry


---

_Comment by @BurntSushi on 2017-01-13 21:08_

@thierryvolpiatto Sorry, I think I understand what you're saying. What I meant was, I don't understand how what you're reporting is actually possible. If you say `--color ansi`, then ANSI escape sequences should be emitted *no questions asked*.

I think the only thing I can do at this point is try to reproducing it, but I've never used emacs before. (I'm a vim user.) How hard would it be to explain how to reproduce your issue to someone who has never used emacs before?

---

_Comment by @thierryvolpiatto on 2017-01-13 21:38_


Andrew Gallant <notifications@github.com> writes:

> @thierryvolpiatto Sorry, I think I understand what you're saying. What
> I meant was, I don't understand how what you're reporting is actually
> possible. If you say --color ansi, then ANSI escape sequences should
> be emitted no questions asked.
>
> I think the only thing I can do at this point is try to reproducing
> it, but I've never used emacs before. (I'm a vim user.) How hard would
> it be to explain how to reproduce your issue to someone who has never
> used emacs before?

It is easy, no need to install helm, you can use rg in an emacs terminal
as you would use it usually.

1) Install emacs preferably a recent version like emacs-24.5 or emacs-25.1.
Let me know if you have difficulties to install.

2) run from a terminal:

emacs -Q

3) Once emacs started hit M-x ansi-term RET RET (where M is Meta and RET is Return).

4) You have now a prompt, just use rg as usual.

5) To quit emacs hit C-x C-c (Ctrl-c Ctrl-c)

Hope that help.

Thanks.

--
Thierry


---

_Comment by @BurntSushi on 2017-01-13 23:24_

Interesting! That did the trick. Something is definitely a little weird, however, I am seeing that the match text is at least *bolded*, but it's not colored.

---

_Comment by @BurntSushi on 2017-01-13 23:24_

Errrmm, but I am using latest master which has some fixes related to that, so that might not be the case for `0.3.2`.

Anyway, I can reproduce the fundamental problem, so hopefully I can fix this. Thank you. :-)

---

_Comment by @BurntSushi on 2017-01-13 23:44_

Ah! I figured it out. It looks like the emacs terminal has very limited support for ANSI escape sequences, and doesn't actually work with constructs like `\x1B[38;5;4m` to set the text to blue (for example). Instead, it needs `\x1B[34m`. I think I just need to straighten this out and we should be good.

---

_Comment by @BurntSushi on 2017-01-14 00:03_

All right, this should finally be fixed in master. I hope to get a release out soon.

---

_Comment by @thierryvolpiatto on 2017-01-14 08:13_


Andrew Gallant <notifications@github.com> writes:

> All right, this should finally be fixed in master. I hope to get a release out soon.

I installed rust and builded rg from master, and I can confirm rg is now
outputing colors as expected :-).

Many thanks for your quick fix and for rg!

-- 
Thierry


---
