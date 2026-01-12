```yaml
number: 200
title: "ripgrep doesn't stop when its pipe is closed"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - libripgrep
assignees: []
created_at: 2016-10-29T12:58:03Z
updated_at: 2023-11-23T06:29:46Z
url: https://github.com/BurntSushi/ripgrep/issues/200
synced_at: 2026-01-12T16:13:21Z
```

# ripgrep doesn't stop when its pipe is closed

---

_@BurntSushi_

For example, the following two `rg` commands take the same amount of time, but the second one should be much shorter:

```
[andrew@Cheetah subtitles] ls -lh OpenSubtitles2016.raw.en 
-rw-r--r-- 1 andrew users 9.3G Sep 10 11:51 OpenSubtitles2016.raw.en
[andrew@Cheetah subtitles] time rg 'Sherlock Holmes' OpenSubtitles2016.raw.en | wc -l
5107

real    0m1.602s
user    0m1.250s
sys     0m0.350s
[andrew@Cheetah subtitles] time rg 'Sherlock Holmes' OpenSubtitles2016.raw.en | head -n1
You read Sherlock Holmes to deduce that?

real    0m1.626s
user    0m1.247s
sys     0m0.377s
```


---

_Label `bug` added by @BurntSushi on 2016-10-29 12:58_

---

_Comment by @BurntSushi on 2016-10-30 02:06_

This one isn't going to be fun to fix. This is what I get for ignoring any errors that occur when printing to `stdout`. (And I really should know better, I handle this correctly in `xsv`.) The issue is that a write to `stdout` in this case will fail with a pipe error, at which point, we should stop searching and quit.

The primary difficulty at present is bubbling up an error from the printing code all the way through the search code. Both the search/print code assume no errors can happen. We could just thread an error through everything.

My question for this in terms of UX is: do we treat all IO errors equally when writing output? Should we do something different if we see a pipe error versus, say, a permission error? Maybe any type of error causes ripgrep to stop whatever it's doing, but a pipe error indicates normal termination where as anything else results in a non-zero exit code and the error being printed to stderr.


---

_Comment by @BurntSushi on 2016-10-30 02:09_

I might elect to forgo fixing this until I factor the search code out into a separate crate. (Which will be a while. My guess is at least a month.)


---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Comment by @danr on 2017-02-08 09:57_

I upgraded yesterday from rg 0.3.2-1 to 0.4 and now these commands are essentially useless:

    rg --files | head
    rg --files | fzf

This is too bad, since I use rg+fzf in my workflow. 

---

_Comment by @BurntSushi on 2017-02-18 21:37_

@danr There was no regression. This bug was in `0.3.2` as well.

"useless" sounds like a bit of an exaggeration.

---

_Comment by @danr on 2017-02-27 09:37_

Sorry, I really did not mean to sound harsh. Thank you for your work on `ripgrep`!

---

_Comment by @kpp on 2017-03-31 19:45_

This is how grep acts:
```
$ strace grep Holmes subtitles/OpenSubtitles2016.raw.en 2>grep.log  | head -n 1

...
read(3, "s\nWell, Watson, what about him?\n"..., 32768) = 32768
write(1, "Roy Holmes.\nThen Simon Harrison "..., 4096) = 4096
read(3, "me, because he married against h"..., 32768) = 32768
...
read(3, "uch.\nWell, you built a real nice"..., 32768) = 32768
write(1, " a swamp.\nMr. Holmes, do not rea"..., 4096) = -1 EPIPE (Broken pipe)
--- SIGPIPE {si_signo=SIGPIPE, si_code=SI_USER, si_pid=11211, si_uid=1000} ---
+++ killed by SIGPIPE +++
```

This is how rg acts:
```
$ strace rg Holmes subtitles/OpenSubtitles2016.raw.en 2>rg.log  | head -n 1 

...
write(1, "Roy Holmes.\n", 12)           = 12
write(1, "Then Simon Harrison killing Barr"..., 80) = -1 EPIPE (Broken pipe)
--- SIGPIPE {si_signo=SIGPIPE, si_code=SI_USER, si_pid=11310, si_uid=1000} ---
write(1, "Then Simon Harrison killing Barr"..., 80) = -1 EPIPE (Broken pipe)
--- SIGPIPE {si_signo=SIGPIPE, si_code=SI_USER, si_pid=11310, si_uid=1000} ---
write(1, "Then Simon Harrison killing Barr"..., 80) = -1 EPIPE (Broken pipe)
--- SIGPIPE {si_signo=SIGPIPE, si_code=SI_USER, si_pid=11310, si_uid=1000} ---
...
```

---

_Comment by @BurntSushi on 2017-03-31 19:47_

@kpp Right, this bug is understood, it's just a pain to fix. Please do not fix it. I would like to fix it personally when I refactor this code into a separate crate.

---

_Comment by @kpp on 2017-03-31 19:50_

`grep` is faster than `rg`! This issue is the proof! All hail `grep`! `grep`, `grep`, `grep`!

---

_Comment by @glandium on 2017-03-31 22:07_

Actually, what that strace says is that grep does an explicit "panic" of some sort (it raises SIGPIPE) when it gets a EPIPE error from write. It doesn't bubble the error up or anything, which is what your proposed fix needs refactoring for.

---

_Comment by @BurntSushi on 2017-03-31 22:14_

@glandium Right. Rust's standard library (IIRC) suppresses the `SIGPIPE` signal so that consumers need to explicitly handle it. The downside of that design is precisely that bugs like this occur, but the upside is that you get a bit more control. I was just being stupid when I wrote down the initial printer code. :-)

---

_Comment by @oconnor663 on 2017-08-24 19:56_

Could we just `libc::signal(libc::SIGPIPE, libc::SIG_DFL)` somewhere early in `main()`, if we wanted an easy workaround for the short term? Not sure what the Windows equivalent is though. Maybe the more portable thing would be to `std::process::exit` on write errors? Gross in library code, but anyway just until the error plumbing is there.

---

_Comment by @BurntSushi on 2017-08-27 19:01_

@oconnor663 Graciously submitted a PR implementing the `libc::signal` idea, which means this is fixed for now on Unix. I'm going to leave this open to track a proper fix.

---

_Comment by @junegunn on 2017-09-27 03:36_

@BurntSushi Hi, any plans for a patch release including the fix? Many users, myself included, would want to install ripgrep using Homebrew/Linuxbrew, and use it with a secondary filter like [fzf](https://github.com/junegunn/fzf).

---

_Comment by @BurntSushi on 2017-09-27 10:27_

@junegunn Sure, I can do that. Hopefully soon.

---

_Comment by @junegunn on 2017-10-23 02:37_

Confirmed fixed in 0.7.1 on macOS. Thanks.

---

_Comment by @dpnova on 2017-11-01 01:20_

@junegunn it's still an issue for me on 0.7.1 on ubuntu - did you use a binary release?

Using this command:

```
export FZF_DEFAULT_COMMAND='rg --files --hidden --follow --glob "!.git/*" --glob "!target/*"'
```

---

_Comment by @BurntSushi on 2017-11-01 01:27_

@dpnova Could you please provide a reproducible example on a corpus that is public without using fzf? I've tested this myself on Linux and it works fine.

---

_Comment by @dpnova on 2017-11-01 02:04_

Just confirming... to repro I should be able to run 

`rg --files --hidden --follow --glob "" --glob '!target/*'` in the same folder I'm starting vim (with the fzf.vim plugin)

---

_Comment by @dpnova on 2017-11-01 10:06_

FWIW I can't repro without fzf. My test case is simply running the command. I'm sure I'm missing something though.

---

_Comment by @pbogut on 2017-11-01 10:17_

Do you have any big file in your repo? I had this problem when there was like 2GB sql file in my folder. Once I've added this file to `.gitignore` it started to work without an issue.

---

_Comment by @BurntSushi on 2017-11-01 11:06_

@pbogut The `rg` command in question is using `--files`, which means it isn't searching files.

@dpnova I don't see how that is a complete test case. Could you **please** provide more details? This bug doesn't impact the correctness of ripgrep (so whether you're able to "run" an `rg` command or not is not relevant). Rather, you need to check whether `rg ... | head -n1` is noticeably faster than simply `rg ...`. For this to make any sense at all, the `rg ...` command needs to run over a directory tree that is somewhat larger, otherwise you're unlikely to see a difference anyway.

For example, if I run `rg --files | wc -l` in a checkout of the Chromium repository (`git://github.com/nwjs/chromium.src`), then it takes `0.320` seconds to complete. But if I run `rg --files | head -n1 | wc -l` in the same repository, then it takes `0.023` seconds to complete. Before this fix landed, the latter command would always take the same amount of time as the former command because ripgrep wouldn't quit when its output pipe closed.

People, please, I'm begging you. Don't simply stop at "it doesn't work." *Describe* what you observe in as much detail as possible so that other people can diagnose your problem. Please, understand that not everyone uses FZF, so saying, "here's this FZF config and it doesn't work" will not get us anywhere.

---

_Comment by @dpnova on 2017-11-01 21:39_

Sorry I wasn't clear enough that I was asking for help to repro, I didnt get a response, so I tried *something*. Now I have a concrete case, thanks.

I'm currently waiting for the chromium repo to clone (yay Australian broadband).

In my own repo where I'm seeing the issue in fzf.vim the two cases look like this:

```
( rg --files; )  0.01s user 0.01s system 129% cpu 0.017 total
( rg --files | head -n1; )  0.01s user 0.01s system 119% cpu 0.015 total

```



---

_Comment by @dpnova on 2017-11-01 21:49_


This is running it in my home directory (fairly new formatted machine).

```
( rg --files; )  0.10s user 0.16s system 116% cpu 0.222 total
( rg --files | head -n1; )  0.00s user 0.00s system 116% cpu 0.008 total
```

To me this says this specific github issue isn't the case I'm talking about, despite the fzf github referencing this one. I'll take my discussion back over there. Sorry for any confusion.

---

_Comment by @BurntSushi on 2017-11-02 11:54_

@dpnova I agree with your conclusion. :-) Thanks for sticking it out and confirming that this particular bug isn't it!

---

_Comment by @ghost on 2017-12-04 07:46_

@BurntSushi : Can you have a look at this issue that use rg with fzf?. I can't find a way to reproduce it without fzf.
https://github.com/junegunn/fzf.vim/issues/539

---

_Comment by @junegunn on 2017-12-04 08:32_

@tuyenpm9 If you read the thread, you can see that this issue is not related to your problem.

---

_Comment by @ghost on 2017-12-04 08:38_

Yes, sorry about that.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

_Comment by @bpstahlman on 2020-04-21 00:01_

Although the fix definitely seems to have helped, ripgrep can still take a _long_ time to notice SIGPIPE. I haven't looked at the source, but the behavior I'm seeing leads me to suspect that ripgrep doesn't notice the pipe has closed until the next time it attempts to write to it. This is problematic in a long-running search that has entered a phase in which matches are found infrequently (or not at all). I first noticed the problem with an `rg | fzf` pipeline on a large directory tree, but I can reproduce it with a simple shell script that just forwards its stdin to stdout after setting up a signal trap that allows me to tell it when to close the pipe. If I instruct the script to close when ripgrep is finding lots of matches, ripgrep terminates almost immediately, but if I wait until ripgrep is no longer finding matches (but hasn't yet finished the search), the pipeline continues to run (presumably until ripgrep finds another match or the search is complete). Obviously, the time during which the pipeline is effectively hung is highly dependent on the search parameters and the size of the directory tree being searched. Given that some very common use cases for ripgrep involve pipelines (e.g., `vim $(rg --files-with-matches foo | fzf)`), this seems like a significant issue. Does the Rust framework make it inordinately difficult to handle SIGPIPE asynchronously?

---

_Comment by @BurntSushi on 2020-04-21 00:49_

@bpstahlman It would help if you could provide a more concrete reproduction that I can try. With that said, the behavior you're describing makes sense and it's what I would expect to happen.

> but the behavior I'm seeing leads me to suspect that ripgrep doesn't notice the pipe has closed until the next time it attempts to write to it

That is correct and expected. That's when a pipe error occurs: https://github.com/BurntSushi/ripgrep/blob/73103df6d982da1a0dc775e9b7a03f5bb8cfd7fb/crates/core/main.rs#L95-L98

> this seems like a significant issue

AFAIK, you are the first one to report this as a significant problem.

> Does the Rust framework make it inordinately difficult to handle SIGPIPE asynchronously?

ripgrep does not use any Rust "framework."

For more context, Rust by default ignores SIGPIPE: https://github.com/rust-lang/rust/issues/62569 (I make an appearance in that thread asking for something to be done, but there hasn't been any movement on that issue AFAIK.)

This means that pipe errors are only detected once an actual write occurs. At that point, the pipe error is reported "in band" instead of as a signal.

It is [trivial to stop ignoring SIGPIPE](https://github.com/BurntSushi/ripgrep/pull/586/files). This would make ripgrep behave like a "normal" C UNIX application. SIGPIPE gets sent to the process, and since there is no signal handler for it, the process (by default) will terminate immediately. This likely achieves the behavior you want.

ripgrep currently does not do that because it's not portable. I'm not a Windows expert, but AFAIK, there is no SIGPIPE on Windows. So ripgrep _has_ to deal with in-band pipe errors correctly anyway for compatibility with Windows. Dealing with these types of errors has been subtle and difficult to get right. For that reason, I don't really want to support _both_ in-band (synchronous) and out-of-band (asynchronous) ways of terminating on pipe errors. Because now I won't be dog-fooding the handling of in-band pipe errors on Unix.

---

_Comment by @bpstahlman on 2020-04-21 15:29_

I understand your reasoning and appreciate the detailed explanation. I suspect the reason for the lack of complaints regarding the status quo is that for small to medium-sized projects with suitable .gitignore files, the ripgrep search will often complete before the fzf user has finished making his file selection. And if fzf occasionally appears to hang after Enter is pressed, a user is unlikely to bother reporting unless it happens often or the delays are exceptionally long. I don't recall noticing the issue until I started using ripgrep on my home directory (which is fairly large and doesn't have a .gitignore).

As for the desire to avoid maintaining both in and out-of-band SIGPIPE handling logic... I wouldn't think it would be necessary to remove the "in-band" logic to add Linux-only "out-of-band" SIGPIPE handling. The "in-band" logic needed for Windows could still be tested in a Linux build compiled without the "out-of-band" logic, while the "out-of-band" logic would simply be compiled out of the Windows build. E.g.,

- **Windows Release:** in-band only
- **Linux Release:** out-of-band / in-band (optional)
- **Linux for Windows Test:** in-band only


---

_Comment by @BurntSushi on 2020-04-21 15:53_

> I wouldn't think it would be necessary to remove the "in-band" logic 

It isn't. The fact is that now two error paths need to be tested. That was my point.

---

_Comment by @bpstahlman on 2020-04-25 16:32_

@BurntSushi @junegunn 
> This means that pipe errors are only detected once an actual write occurs. At that point, the pipe error is reported "in band" instead of as a signal.

> It is trivial to stop ignoring SIGPIPE. This would make ripgrep behave like a "normal" C UNIX application. SIGPIPE gets sent to the process, and since there is no signal handler for it, the process (by default) will terminate immediately. This likely achieves the behavior you want.

Having looked into this a bit more, I'm not convinced that a change to SIGPIPE handling would have any impact on the behavior I'm seeing. IIUC, SIGPIPE isn't even generated until the writer process attempts to write to the closed pipe, which is too late. And I'm not even sure there's a way for a writer process to check for a closed pipe without attempting to write. I've tried using select(), fstat(), write() of 0 bytes, etc., and the only thing that gave any indication that stdout had closed was an attempt to write actual data to it. I would have expected there would be some way - even if it involved a relatively costly system call - for a writer process to check the status of a pipe without writing unwanted data if it happens to be open.

I'm not sure exactly how the fzf Vim plugin creates the `rg|fzf` pipeline, but I doubt it's possible for the plugin to force early termination, since it would have know way of knowing when the user's selection has been made. The fzf executable knows when the selection has been made, but may not have a clean way to kill the process at the write end of its pipeline. Ah well, perhaps the solution is to ensure that everything I want to search with `rg|fzf` has a good .ignore file...

---

_Comment by @Quaddroo on 2023-11-23 06:29_

I believe some people that find this issue (in relation to rg | fzf) will find their woes relieved by this issue:
https://github.com/junegunn/fzf/issues/2288
check the "process substitution" answer. Fixed what I wanted.

---
