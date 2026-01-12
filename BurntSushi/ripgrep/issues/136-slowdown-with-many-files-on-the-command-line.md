```yaml
number: 136
title: slowdown with many files on the command line, compared to no arguments
type: issue
state: closed
author: oconnor663
labels:
  - bug
assignees: []
created_at: 2016-09-29T21:10:03Z
updated_at: 2016-11-18T01:22:16Z
url: https://github.com/BurntSushi/ripgrep/issues/136
synced_at: 2026-01-12T18:23:11Z
```

# slowdown with many files on the command line, compared to no arguments

---

_@oconnor663_

Example using the https://github.com/keybase/client repo:

```
$ ls go/**/*.go | wc -l
1352

$ time rg blahblahblah go/       
rg blahblahblah go/  0.03s user 0.01s system 210% cpu 0.017 total

# <<< this is the slow one >>>
$ time rg blahblahblah go/**/*.go
rg blahblahblah go/**/*.go  0.14s user 0.01s system 104% cpu 0.137 total

$ time grep blahblahblah -r go/  
grep --color=auto blahblahblah -r go/  0.01s user 0.01s system 92% cpu 0.014 total

$ time grep blahblahblah go/**/*.go
grep --color=auto blahblahblah -r go/**/*.go  0.01s user 0.01s system 93% cpu 0.018 total
```

It looks like `rg` is slower when given a (long) list of files on the command line, than it is when it has to find those files itself. `grep` doesn't have this problem, so I don't think it's some confounder like my shell taking a long time to expand the glob.


---

_Comment by @BurntSushi on 2016-09-29 21:25_

I think this is a dupe. On mobile.

On Sep 29, 2016 5:10 PM, "oconnor663" notifications@github.com wrote:

> Example using the https://github.com/keybase/client repo:
> 
> $ ls go/*_/_.go | wc -l
> 1352
> 
> $ time rg blahblahblah go/
> rg blahblahblah go/  0.03s user 0.01s system 210% cpu 0.017 total
> 
> # <<< this is the slow one >>>
> 
> $ time rg blahblahblah go/**/*.go
> rg blahblahblah go/**/*.go  0.14s user 0.01s system 104% cpu 0.137 total
> 
> $ time grep blahblahblah -r go/
> grep --color=auto blahblahblah -r go/  0.01s user 0.01s system 92% cpu 0.014 total
> 
> $ time grep blahblahblah -r go/**/*.go
> grep --color=auto blahblahblah -r go/**/*.go  0.01s user 0.01s system 93% cpu 0.018 total
> 
> It looks like rg is slower when given a (long) list of files on the
> command line, than it is when it has to find those files itself. grep
> doesn't have this problem, so I don't think it's some confounder like my
> shell taking a long time to expand the glob.
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/136, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34ju30o2BwZj2r7Gzif2cpz5Ge9ZZks5qvCksgaJpZM4KKe9M
> .


---

_Comment by @BurntSushi on 2016-09-29 22:49_

Yeah, this is probably a dupe of #44. Your example is actually more embarrassing, because the positional arguments are _files_, not directories, which makes the reprocessing of parent ignore files even more strange.

I have some grander plans in mind on refactoring the ignore code, so fixing this will probably get lumped in with that. For now, I think we can just track #44.

Thanks for the report!


---

_Closed by @BurntSushi on 2016-09-29 22:49_

---

_Comment by @oconnor663 on 2016-09-30 01:10_

@BurntSushi I should've mentioned before, `--no-ignore` and `--no-ignore-parent` don't affect the slowdown here, unlike in #44. It might be a different cause?


---

_Comment by @BurntSushi on 2016-09-30 01:35_

Oh. Neat. I didn't check that. I'll look into it.

On Sep 29, 2016 21:10, "oconnor663" notifications@github.com wrote:

> @BurntSushi https://github.com/BurntSushi I should've mentioned before,
> --no-ignore and --no-ignore-parent _don't_ affect the slowdown here,
> unlike in #44 https://github.com/BurntSushi/ripgrep/issues/44. It might
> be a different cause?
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/136#issuecomment-250634253,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34pM7Jix-s-RuaECmFq1CPmPQhFCsks5qvGFtgaJpZM4KKe9M
> .


---

_Reopened by @BurntSushi on 2016-09-30 10:44_

---

_Comment by @BurntSushi on 2016-09-30 10:44_

You're right, something different is going on than what's in #44.


---

_Comment by @oconnor663 on 2016-09-30 14:53_

cc @patrickxb who actually found this.


---

_Label `bug` added by @BurntSushi on 2016-10-05 23:28_

---

_Comment by @BurntSushi on 2016-10-28 23:16_

Sadly (and embarrassingly), the blame for this can be placed squarely on my implementation of `docopt`. Virtually all of the time is spent in matching the argv string against the docopt matcher. My guess is that I could re-implement this to go faster, but I don't have the energy for a rewrite of docopt.

I think this means I should admit defeat switch to clap. :-) cc @kbknapp


---

_Comment by @kbknapp on 2016-10-29 15:25_

@BurntSushi if "admitting defeat" means having spent all your time writing some of the best parsers and libraries known to Rust (perhaps any language), and helping ensure the Rust ecosystem/users maintains or even exceeds current levels of quality and professionalism....well then I _aspire_ to one day "admit defeat" as well :wink:

As for this issue, I'd like to do a build using clap just to compare the results. I'll post the results in the next day or so once I have time to make the build and do the tests.


---

_Comment by @BurntSushi on 2016-10-29 15:29_

@kbknapp Haha aww shucks. FWIW, clap is unbelievably well maintained. I hadn't looked at it for a while, but when I checked it the other day I was completely blown away.

To give you more context: you probably won't notice much of a difference in standard usage. The problem appears specifically when there's a long list of positional file arguments. Docopt struggles with this because it uses a weird backtracking algorithm to match `argv` against its matcher. When `argv` is large, there's a _significant_ slow down. I doubt clap will even notice it at all.

Converting `ripgrep` over to clap seems like a largeish undertaking, so please don't kill yourself over it! (In the sense that there's a lot of options, not in the sense that Docopt weirdness is spread throughout ripgrep.) I do kind of want to do it myself as a forcing function to get more familiar with clap, but I won't stop you. :-) I will note that this is kind of high on my priority list because it's really easy to provoke this.


---

_Comment by @oconnor663 on 2016-10-29 15:55_

Woah. Maybe there's some hack we could stick on the front of the whole thing (either docopt, or ripgrep) to handle the specific case of a ton of positional args, that wouldn't need a complete rewrite? I've never looked at docopt internals though.


---

_Comment by @BurntSushi on 2016-10-29 16:04_

I'm sure there's probably something. I'm just not feeling up to digging around in Docopt internals. Everything about it is too complex. (Which is my failing.)


---

_Comment by @kbknapp on 2016-10-29 17:36_

If you'd like to do the converting that's cool with me, I'll just standby for any questions/suggestions! 

Instead of doing a full re-write I just a simple test (called `lof` for lots-o-files) to see if it would in fact make a difference:

```
$ find ~/.config -print | wc -l
4291

$ time lof-docopt ~/.config/**/*
0.52s user 0.11s system 99% cpu 0.629 total

$ time lof-clap ~/.config/**/*
0.05s user 0.02s system 97% cpu 0.075 total
```

I know it's not super scientific or perfect, but gives a rough estimate. Here's what I used:

Docopt:

``` rust
const USAGE: &'static str = "
Lots of Files

Usage:
  lof <files>...

";

#[derive(RustcDecodable)]
struct Args {
    arg_files: Vec<String>,
}

fn main() {
    let args: Args = Docopt::new(USAGE)
        .and_then(|d| d.decode())
        .unwrap_or_else(|e| e.exit());
}
```

And clap:

``` rust
fn main() {
    let m = App::new("lof")
        .arg(Arg::with_name("files")
            .multiple(true))
        .get_matches();
}
```

I also did version where I looped through and printed out the values both as `Strings` and `OsStrings`, but the results were basically no different.


---

_Comment by @BurntSushi on 2016-11-13 01:46_

OK, so I finally have ripgrep wired up to clap and I can confirm this issue in particular gets fixed. My test case with GNU grep:

```
[andrew@Cheetah linux] time sh ../cmd.grep
./include/linux/ide.h:  REQ_TYPE_ATA_PM_RESUME, /* resume request */
./include/linux/ide.h:   (rq)->cmd_type == REQ_TYPE_ATA_PM_RESUME)
./include/linux/ide.h: * State information carried for REQ_TYPE_ATA_PM_SUSPEND and REQ_TYPE_ATA_PM_RESUME

real    0m0.182s
user    0m0.120s
sys     0m0.043s
```

With current ripgrep master:

```
[andrew@Cheetah linux] time sh ../cmd.rg 
include/linux/ide.h
        REQ_TYPE_ATA_PM_RESUME, /* resume request */
         (rq)->cmd_type == REQ_TYPE_ATA_PM_RESUME)
 * State information carried for REQ_TYPE_ATA_PM_SUSPEND and REQ_TYPE_ATA_PM_RESUME

real    0m1.025s
user    0m0.783s
sys     0m0.240s
```

and finally with ripgrep wired up to clap:

```
[andrew@Cheetah linux] time sh ../cmd.rg 
include/linux/ide.h
        REQ_TYPE_ATA_PM_RESUME, /* resume request */
         (rq)->cmd_type == REQ_TYPE_ATA_PM_RESUME)
 * State information carried for REQ_TYPE_ATA_PM_SUSPEND and REQ_TYPE_ATA_PM_RESUME

real    0m0.106s
user    0m0.067s
sys     0m0.037s
```

Everything is run single-threaded on 3,458 files explicitly given on the command line. This test case was derived from the following `xargs` command in the `linux` repo (notice how gosh dang slow ripgrep _was_):

```
$ time find ./ -type f | xargs grep PM_RESUME | wc -l
16

real    0m1.170s
user    0m0.833s
sys     0m0.483s
$ time find ./ -type f | xargs rg-master -j1 PM_RESUME | wc -l
16

real    0m17.562s
user    0m13.960s
sys     0m3.753s
$ time find ./ -type f | xargs rg -j1 PM_RESUME | wc -l
16

real    0m1.136s
user    0m0.630s
sys     0m0.670s
```

Yay clap!


---

_Closed by @BurntSushi on 2016-11-18 01:22_

---
