```yaml
number: 696
title: IO speed vs grep or ag
type: issue
state: closed
author: alok
labels:
  - question
assignees: []
created_at: 2017-11-29T08:04:34Z
updated_at: 2019-08-16T11:43:58Z
url: https://github.com/BurntSushi/ripgrep/issues/696
synced_at: 2026-01-12T16:13:22Z
```

# IO speed vs grep or ag

---

_@alok_

I was reading this

https://github.com/junegunn/fzf.vim/issues/488#issuecomment-347714322

And the last few comments about how ag apparently can output IO 5 times faster than ripgrep stood out. Is that true? If so, are there any plans to speed it up, or is the case described in the issue outside of what you consider normal usage?

---

_Comment by @BurntSushi on 2017-11-29 10:25_

Please provide a test case independent of fzf.

---

_Comment by @BurntSushi on 2017-11-29 14:14_

My suspicion is that something else funky is going on here, and that "ripgrep has slow output" is probably neither true nor the cause.

On a simple search in a checkout of the Linux kernel, for example, it's not even close:

```
$ time rg . | wc -l
19100836

real    0m0.779s
user    0m7.396s
sys     0m1.002s
$ time ag . | wc -l
22093481

real    0m35.207s
user    0m35.783s
sys     0m3.725s
```

Note that a [similar case](http://blog.burntsushi.net/ripgrep/#everything) made an appearance in my benchmarks:

```
$ time rg '.*' | wc -l
22094380

real    0m1.080s
user    0m11.359s
sys     0m1.100s
$ time ag '.*' | wc -l
55940

real    0m2.857s
user    0m6.704s
sys     0m1.914s
```

In this case, `ag` doesn't even print every result (probably a bug) and it's still slower than ripgrep.

Note that the output of running `rg .` in the Linux root is about 1.3GB.

I've also tried comparing the timings when using the `--files` flag, and ripgrep is always as fast or faster than ag.

Bottom line: we need more data.

---

_Comment by @BurntSushi on 2017-11-29 14:24_

Colors don't appear to be the issue either:

```
$ time ag --color void > /tmp/results.ag

real    0m4.030s
user    0m5.284s
sys     0m1.966s
$ time rg --color=always void > /tmp/results.rg

real    0m0.154s
user    0m0.853s
sys     0m0.618s
```

---

_Comment by @BurntSushi on 2017-11-29 14:25_

If anything, what I'm discovering here is that ag's output is the one that is incredibly slow. If you run `ag` on a query with a lot of matches (e.g., `ag a`), then it runs obscenely slowly.

---

_Comment by @unphased on 2017-11-29 17:01_

I will look into this, thanks. I agree that these are curious results!

---

_Comment by @BurntSushi on 2017-11-29 17:09_

To confirm that `ag`'s printing is the slow part (not its searching), compare the timing of `ag -c . | wc -l` and `ag . | wc -l`

---

_Comment by @unphased on 2017-12-02 21:00_

I confirmed that rg is blazing fast when used for feeding FZF in vim, except when color=always. which sort of sucks because the way FZF works in vim does show colors, which is visually helpful. 

I'm actually unsure of the actual code involved here. FZF is a terminal application and it may be the case that running it in vim brings an entire terminal emulator/PTY layer within Vim into the mix.

It is no longer clear at all that the issue is on ripgrep's side. Looking at fzf.vim code, there appears to be a rather lot of stuff "geared toward" ag, not sure if it is responsible for the difference. 

---

_Comment by @BurntSushi on 2017-12-02 21:53_

@unphased Yeah basically in order for this to be a problem with ripgrep, I need to see a case that I can reproduce without fzf. I did test ripgrep and ag with colors (see above), but it didn't make any appreciable difference. Thanks for investigating! Please do let me know if you come up with anything!

---

_Comment by @unphased on 2017-12-02 22:10_

yeah I tried fzf a while ago but it tries to do things that are too smart (messing with tmux windows) when run directly from the shell. 

Lately I have found it tremendously powerful as a replacement for ctrlP in vim, and plus when you invoke it on a fresh system it very helpfully downloads the executable for you. So, I use fzf exclusively in vim now... I will revisit this another day when I really want colors inside fzf, but for the time being *nothing* beats non-colored rg's speed for feeding fzf

---

_Comment by @BurntSushi on 2018-01-31 02:26_

It is possible that #764 is the culprit here, although I am still perplexed at why I can't reproduce this in a terminal.

---

_Label `question` added by @BurntSushi on 2018-01-31 02:26_

---

_Comment by @wsdjeg on 2018-02-09 03:35_

I will create an example base on Vim's async API to confirm this issue.

when run rg or grep command, if I want only the first stdout data. grep is much faster than ag/rg etc.

---

_Comment by @unphased on 2018-02-09 05:48_

well, it looks like whatever dependency ripgrep uses causes the colors to be assembled in an almost worst case method where multiple color codes are emitted for every single character. It's a bit like immediate mode OpenGL geometry, it's just absurd overhead, despite technically producing correct results. It totally makes sense to me that we see this effect if the terminals you test on are significantly more efficient at handling the color state changes than vim's builtin terminal emulator is. 

Two problems afoot: 1) make ripgrep stop producing laughably inefficient ANSI color codes 2) make vim's terminal emulation less inefficient

both would be good to have, but either one should go a long way toward improving the perf.

---

_Comment by @BurntSushi on 2018-02-09 11:55_

> well, it looks like whatever dependency ripgrep uses causes the colors to be assembled in an almost worst case method where multiple color codes are emitted for every single character

@unphased This characterization is imprecise. ripgrep emits distinct color codes *for every match*. If every match is a single character, and every single character is a match (e.g., `rg .`), then it degrades to the very bad case of emitting color codes for every character printed to the terminal. The optimization that ripgrep lacks is that it doesn't detect _adjacent matches_ and merge the color codes.

Knowing this, can anyone reliably reproduce the issue without fzf? Failing that, could someone say something about what ripgrep commands `fzf` is running?

---

_Comment by @unphased on 2018-02-09 17:15_

It seems that searching for `.` is a bad way to use the tool in this situation. I use `-v '^$'`. Don't know if that also causes insane ANSI color spew also. I can confirm though that the performance indicates that it does still end up a huge amount slower than not using colors. I'll try to test this and see what the output looks like.

Yes I can accept the argument that ripgrep shouldnt be expected to "resolve" unnecessary color state changes in its output. that would probably be being *too* smart about things.


```
command! -bang FLines call fzf#vim#grep(
     \ "rg --color=never --line-number --no-heading --ignore-case --hidden --ignore-file ".glob("~/.vim/rg.gitignore")." -v '^$'",
     \ 1,
     \ {'options': '--reverse --prompt "FLines> "'})
```

^ from my vimrc

when the `--color=never` is taken out, the performance drops a lot, but only when seen in fzf in vim (I haven't tested fzf not in vim). I'll test it outside of fzf and see what is going on with the color codes.

---

_Comment by @BurntSushi on 2018-02-09 17:59_

@unphased `-v '^$'` avoids the color spew problem. In fact, any usage of `-v` would since there is no match for ripgrep to highlight.

---

_Comment by @unphased on 2018-02-09 18:35_

It still ends up being unexplainably slow compared to configured for no colors.

---

_Comment by @BurntSushi on 2018-02-09 18:56_

Hmmm that suggests #764 may not be the culprit. :-/

---

_Comment by @unphased on 2018-02-10 08:29_

Upon testing in Linux, i do not see much of a slowdown at all comparing color to non colored here. But on my 2017 MacBook Pro there is a ~5 or so factor speed difference. Which now I think i would prefer to get the color and live with it being slower. It indexes 4 million lines (my home directory) within a tolerable amount of time. I don't even understand how that is possible, it's like probably on the order of close to a gigabyte of data getting crunched.

Looking at htop, when this runs, with `color=always`, fzf is at 130% cpu, and rg is at about 25-40% cpu. When `color=never`, fzf is 138% cpu and rg is 130% cpu. This makes me blame fzf instead of vim. 

---

_Comment by @unphased on 2018-02-12 18:06_

OK more information. in a linux vm in a macbook (a 2015 model this time) the speed difference is also very present. 

I did some more tests... 

```
~/util ❯❯❯ time rg --color=always --no-heading --hidden -v '^$' > rg_test_color                                                                                           master ◼
rg --color=always --no-heading --hidden -v '^$' > rg_test_color  0.35s user 0.17s system 198% cpu 0.267 total
~/util ❯❯❯ rm rg_test_color
remove rg_test_color? y
~/util ❯❯❯ time rg --color=never --no-heading --hidden -v '^$' > rg_test
rg --color=never --no-heading --hidden -v '^$' > rg_test  0.25s user 0.17s system 182% cpu 0.230 total
```

as we expect it seems rg is only marginally slower at producing the output.

when i then take these saved outputs to feed to fzf like this: 

`cat rg_test_color | .vim/plugged/fzf/bin/fzf --ansi`

the additional slowdown is not very noticeable, certainly not ~5x slower. 

I'm going to look in the vim fzf plugin for its invocation, since it doesnt seem to be that slow using just `--ansi`.

---

_Comment by @unphased on 2018-02-12 18:40_

Well i've reproduced the same commands being sent into fzf that vim does..

```
rg --color=always --no-heading --hidden -v '^$' | ../.vim/plugged/fzf/bin/fzf '--ansi' '--prompt' 'Rg> ' '--multi' '--bind' 'alt-a:select-all,alt-d:deselect-all' '--color' 'hl:68,hl+:110' --reverse --prompt "FLines> " --expect=ctrl-v,ctrl-s,ctrl-t --height=29
```

when i take the ansi flag out (and feed it ansi colored rg output) it is just as fast as when non colored output is fed in (with or without the ansi flag). That indicates to me that the ansi color parsing itself is the bottleneck.

again this is more or less what we could have guessed but now i am more confident about it.

---

_Comment by @BurntSushi on 2018-02-12 21:42_

@unphased Excellent investigation! Does this explain why rg is slow but ag is fast when using fzf?

---

_Comment by @unphased on 2018-02-12 23:00_

That is the one thing this doesn’t explain yet and I have to assemble some output that is comparable with which to do that!

---

_Comment by @unphased on 2018-02-12 23:30_

OK heres another more methodical set of data, now, i dont have a way to time how long fzf takes to index the input, but i can eyeball it. working with around 50MB of input (the colors dont bloat the size of input much... 

```
-rw-r--r--     1 slu   staff  46974160 Feb 12 12:51 rg_test
-rw-r--r--     1 slu   staff  49710421 Feb 12 12:56 rg_test_color
```

comparing these 4

```
rg --color=never --no-heading --hidden -v '^$' | ../.vim/plugged/fzf/bin/fzf '--ansi' '--prompt' 'Rg> ' '--multi' '--bind' 'alt-a:select-all,alt-d:deselect-all' '--color' 'hl:68,hl+:110' --reverse --prompt "FLines> " --expect=ctrl-v,ctrl-s,ctrl-t --height=29
rg --color=always --no-heading --hidden -v '^$' | ../.vim/plugged/fzf/bin/fzf '--ansi' '--prompt' 'Rg> ' '--multi' '--bind' 'alt-a:select-all,alt-d:deselect-all' '--color' 'hl:68,hl+:110' --reverse --prompt "FLines> " --expect=ctrl-v,ctrl-s,ctrl-t --height=29
cat ../rg_test | ../.vim/plugged/fzf/bin/fzf '--ansi' '--prompt' 'Rg> ' '--multi' '--bind' 'alt-a:select-all,alt-d:deselect-all' '--color' 'hl:68,hl+:110' --reverse --prompt "FLines> " --expect=ctrl-v,ctrl-s,ctrl-t --height=29
cat ../rg_test_color | ../.vim/plugged/fzf/bin/fzf '--ansi' '--prompt' 'Rg> ' '--multi' '--bind' 'alt-a:select-all,alt-d:deselect-all' '--color' 'hl:68,hl+:110' --reverse --prompt "FLines> " --expect=ctrl-v,ctrl-s,ctrl-t --height=29
```

Which are respectively: 1. rg producing no colors piped to fzf 2. rg producing colors piped to fzf 3. (1) but cached to disk 4. (2) but cached to disk

The timing looks like:

1. 0.9 sec
2. 1.5 sec
3. 0.8 sec
4. 1.5 sec

From doing the same thing inside my ubuntu 14.04 vm on same machine:

1. 1.2 sec
2. 2.0 sec
3. 1.2 sec
4. 2.0 sec

Grain of salt with these times, probably not accurate to more than ~0.25 sec, i did not use a stopwatch.


Now I don't know how my factor of 5 changed to a factor of barely 2. There does seem to be a 'caching' aspect to this. running it a second and subsequent times, fzf indexes a good bit faster. 

---

_Comment by @unphased on 2018-02-12 23:45_

I did the same procedure with `ag` and it takes a good 2 seconds just to run itself, and its color output does produce more colors (the entire match is highlighted) and this does lead to (as expected) about as much extra time for fzf to handle it. Heres what `ag --color` looks like in comparison to `rg --color=always`:

![image](https://user-images.githubusercontent.com/1542910/36126044-bb866e5e-1024-11e8-8b82-0a84c99a1308.png)

I tested it through fzf inside vim and it is exactly as fast as running fzf independently of vim.

At this point it doesn't seem like there are any "issues" to speak of that are left... not using colors is going to make for a snappier result, but it won't be as pleasant to look at and sift through. Them's the breaks. 

---

_Comment by @BurntSushi on 2018-07-22 16:41_

Since this issue hasn't seen any activity for a while and @unphased's fastidious investigation didn't turn up anything obvious, I'm going to close this in the absence of additional data. I think #764 is still an issue though that I'd like to fix.

---

_Closed by @BurntSushi on 2018-07-22 16:41_

---

_Comment by @unphased on 2018-07-22 17:19_

fwiw, I have been using `rg --color=always`, using it a lot (started a new job recently, new codebase) and although it might take 5 seconds for fzf in vim to fully index the 2 million lines... It's fine! Works great! I'd be happy to take a stopwatch to it and run some more trials inside vs outside vim and so on, and with other codebases. It still amazes me that fzf can take that much input in stride.  But I won't really have much occasion to care about this performance without a 10x larger codebase to scan.

---

_Comment by @BurntSushi on 2018-07-22 17:23_

@unphased I've never used fzf before, so it's hard for me to know whether that's too slow or not. I guess it kind of depends on how other similar tools perform. e.g., Maybe compare `grep -r` with `rg -j1` or something.

---

_Comment by @Gee19 on 2019-08-16 09:17_

I still run into this issue, let me know if/how I can help debug any further.

---

_Comment by @BurntSushi on 2019-08-16 11:43_

@Gee19 With #764 fixed, I don't think there are any other obvious problems for this.

Could you please write a detailed report of what exactly is happening, preferably in enough detail such that others can reproduce it? Ideally, all inputs and corpora are open so that others can try the exact same procedure.

---
