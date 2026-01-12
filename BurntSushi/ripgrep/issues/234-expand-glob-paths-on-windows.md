```yaml
number: 234
title: Expand glob paths on Windows
type: issue
state: open
author: troplin
labels:
  - question
  - icebox
assignees: []
created_at: 2016-11-13T14:32:51Z
updated_at: 2025-05-30T18:52:00Z
url: https://github.com/BurntSushi/ripgrep/issues/234
synced_at: 2026-01-12T16:13:21Z
```

# Expand glob paths on Windows

---

_@troplin_

On Windows, glob expansion is not done by the shell (cmd.exe), but left to the individual program.
That means, that currently something like this doesn't work:
```text
>rg PATTERN *.txt
*.txt: The filename, directory name, or volume label syntax is incorrect. (os error 123)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```
It tries to open a file called `*.txt`, which obviously doesn't exist.

---

_Comment by @BurntSushi on 2016-11-13 14:40_

What do other command line tools like `grep` do?


---

_Label `question` added by @BurntSushi on 2016-11-13 14:40_

---

_Comment by @troplin on 2016-11-13 14:45_

`grep` does work as expected for me.
I installed it with cygwin though, so I'm not sure if this is a cygwin or grep feature.

But all native Windows tools I know do the expansion themselves, at least if it makes sense to expand.


---

_Comment by @troplin on 2016-11-13 14:48_

Usually you just use [FindFirstFile](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364418%28v=vs.85%29.aspx)/[FindNextFile](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364428%28v=vs.85%29.aspx) for this.
Not sure if it uses the exact same rules as unix glob.


---

_Comment by @BurntSushi on 2016-11-13 15:25_

_sigh_ Indeed, it looks like globbing is done as part of the command line program: https://cygwin.com/ml/cygwin/2009-12/msg01097.html

Other instances of the same problem:
- https://github.com/kripken/emscripten/issues/1675
- http://stackoverflow.com/questions/11557280/filename-globbing-windows-vs-unix

I think what this means is that I need to add a glob iterator to `globset`. Alternatively, we could use the existing iterator in the `glob` crate, but it doesn't support `{a,b}` syntax and gets some non-UTF-8 corner cases wrong.


---

_Comment by @BurntSushi on 2017-03-13 00:32_

> I think what this means is that I need to add a glob iterator to globset. Alternatively, we could use the existing iterator in the glob crate, but it doesn't support {a,b} syntax and gets some non-UTF-8 corner cases wrong.

The other other alternative is to use the standard Windows APIs for this. It seems like the kinds of globs it supports are not as nice as Unix-style globbing, but perhaps that's what Windows users expect, so it could be defensible to use that.

I don't anticipate working on this soon unfortunately.

---

_Label `icebox` added by @BurntSushi on 2017-03-13 00:33_

---

_Comment by @troplin on 2017-03-14 20:37_

I thinks using the Windows API is better than nothing and probably easier to implement.
I might give it a try if I find the time.

---

_Comment by @BurntSushi on 2017-03-14 20:53_

@troplin Thanks! If you do, I would hope to see the Windows logic put behind a separate crate. :-) (Which could live inside ripgrep, or could be yours to maintain.)

---

_Comment by @BurntSushi on 2018-02-02 14:13_

Question: should ripgrep support Unix-style globbing here, or should it use the standard `FindFirstFile`/`FindNextFile` APIs, presumably to be consistent with other Windows CLI tools?

cc @retep998 @roblourens

---

_Comment by @roblourens on 2018-02-02 16:59_

Do the windows APIs support anything besides `*` and `?`? It's not clear.

This doesn't affect vscode scenarios, but as a CLI user, I'd prefer more powerful patterns.	

---

_Comment by @sabi0 on 2018-09-26 14:05_

How about `-g <GLOB>` using Unix-style globs (as it does already) and just `<GLOB>` falling back to Windows API?

Basically there is already such behavior separation between ripgrep and shell globbing on Linux. And Windows CLI is just an "inferior shell" one might argue. So it would be natural for (emulated) "shell globbing" to work in the shell native way.

---

_Comment by @HerbM on 2018-10-28 21:52_

FYI: 
cmd.exe  does NOT support anything but * and ?  (specifically NOT [a-z] character classes).
PowerShell does support the character classes as part of "wildcards" (what it calls Globbing.

Current -g switch in RipGrep seems to work fine with ?, *, [class] 

It does NOT work without the -g (which is as designed but a bit unexpected for a Windows only user.)

Worth putting out a Windows specific warning when the last thing on the line is *.txt or similar and nothing to search, instead of the current warning:

    *.txt: The filename, directory name, or volume label syntax is incorrect. (os error 123)

Maybe add,  

    use '-g FilePattern' for globbing (wildcard file patterns)



---

_Comment by @dracan on 2020-02-06 19:17_

+1 for @HerbM's suggestion about mentioning the `-g` flag with the error message. This would have saved me time Googling how to grep against a file pattern, to eventually find this Github issue.

I've hit this issue before, but due to being in the middle of something, I've just moved over to searching in vscode instead of using RipGrep. I'm guessing that plenty of other Windows users have hit the same issue. Updating the error message would resolve this and make it immediately obvious how to do what the user was trying to do.

ps. Awesome tool btw! This is the only issue I've hit - other than that, it's amazing! Thanks for the great work!

---

_Comment by @BurntSushi on 2020-02-06 21:30_

@dracan Thanks for the feedback! Funny note though: VS Code's search uses ripgrep. :)

---

_Comment by @Taverius on 2020-02-13 08:11_

Definite +1 for yelling at the operator about -g, at the very least.

Counterpoint though:

> findstr PATTERN *.txt

Functions as one would expect with glob-expansion ... as does `ag`.

I know windows path expansions is a pain in the behind, but there's a case to be made for functional parity when even `findstr` manages it :D

---

_Comment by @BurntSushi on 2020-02-13 12:09_

Folks, +1 comments are not all that useful. Instead of basically just saying "me too", it would help if Windows users could answer questions I've asked. For example: https://github.com/BurntSushi/ripgrep/issues/234#issuecomment-362597070

---

_Comment by @sabi0 on 2020-02-13 12:13_

So I suppose you didn't like my proposal from https://github.com/BurntSushi/ripgrep/issues/234#issuecomment-424727971 ?

---

_Comment by @BurntSushi on 2020-02-13 12:17_

I have two answers to my question do far. Both of them are different. Yours is one of them. More input from others would be great. In particular, I would love to hear how existing cli utilities work. Do they use standard unix globbing? Or shell native globbing? And more importantly, do users actually like that?

---

_Comment by @sabi0 on 2020-02-13 12:50_

I believe most utilities use `FindFirstFile` / `FindNextFile`. For sure the standard ones like `findstr`.
But also e.g. `pcre2grep` and `git` (see https://github.com/git/git/blob/master/compat/win32/dirent.c).

IMO "consistency" is more important than "liking". So I would vote for using shell native globbing with `FindFirstFile` / `FindNextFile` on Windows.
However, as long as a special `-g` parameter is considered it would be fine IMO to make it behave consistently across all OSes (i.e. use Unix globbing).

---

_Comment by @Taverius on 2020-02-13 13:08_

`findstr` has limitations - it supports **file** globbing but not directory. `findstr /s PATTERN path/to/*.file` works but `findstr /s PATTERN path/*/*.file` doesn't.

That would be an acceptable minimum but I would strongly prefer unix parity here so that tools that rely on ripgrep don't need to split code paths for windows.

---

_Comment by @pfmoore on 2020-02-26 17:31_

> Question: should ripgrep support Unix-style globbing here, or should it use the standard FindFirstFile/FindNextFile APIs

As a Windows user, IMO just `*` and `?`. Reasons:

* They are standard for Windows tools. It's what the MS C runtime does when building argv, so command line users will be used to it.
* They are invalid filename characters, so you don't need to worry about escaping mechanisms (which is good, as backslash is unavailable due to it being the path separator).

I'd argue for globbing in all elements of the path, not just on the leaf element, and supporting `**` meaning "one or more levels of subdirectory", but these are nice to have rather than essential, and are not standard for the CRT, so it's entirely justifiable not to offer them.

Unix style globbing is powerful, and might be a bit more consistent cross-platform, but (a) you'd have to add an escaping mechanism (see above) and (b) not all Unix shells use the exactly same globbing syntax, so it's still not entirely portable. I dont think it's worth it, personally.

---

_Comment by @BurntSushi on 2020-02-26 17:39_

@pfmoore Thanks for the feedback! I think the two choices here are "support Windows-style globbing using the corresponding winapi calls" or "support Unix-style globbing." I don't think I'm willing to do some inbetween state.

Also, note that ripgrep already supports Unix-style globbing on Windows with the `-g/--glob` flag, along with the various gitignore support. This is why the underlying glob library [permits disabling backslash as an escape](https://docs.rs/globset/0.4.4/globset/struct.GlobBuilder.html#method.backslash_escape), which is indeed disabled on Windows by default. Moreover, globs already have an additional escaping mechanism built in. e.g., You can use `[*]` to refer to a literal `*`.

---

_Comment by @SandraEickel on 2020-08-27 12:25_

I have filed issue #1667 after browsing open issues which brought me here.

IMHO the best option would be for ripgrep to behave
- Windows-like when running from CMD or PowerShell
- UNIX-like when running from Git Bash (MINGW64) or CygWin
including input and output of path seperators, nor only globbing mechanisms.

So, this is no inbetween state - it is just checking for the type of environment (shell) and behaving accordingly.

There's no RUST for OS/2, but there we have the same situation, the normal OS/2 CMD (or 4OS2) and a UNIX-like environment with Dash (instead of Bash).

---

_Comment by @workingjubilee on 2023-07-04 21:59_

I believe ripgrep should not reimplement Unix globbing by default because it creates an incorrect expectation that a responsibility of the shell should be the responsibility of individual programs, demanding everyone repeat the work that the shell is very well capable of doing itself.

---

_Comment by @BurntSushi on 2023-07-04 22:04_

That's a fair point, but if end users are hitting this problem in a popular shell and there is no work-around for them, then it makes sense to me for ripgrep to expand the globs. AIUI, this is standard for Windows CLI tools? Although I'm not 100% sure on that. It might also help here to enumerate the shells that fail to expand globs. Is it just `cmd.exe` or is it PowerShell too?

---

_Comment by @pfmoore on 2023-07-04 22:31_

Powershell leaves it to the program as well. It’s the standard on windows that globbing is done by the individual program - in fact, the C runtime glob-expands argv automatically, so programs written in C get this behaviour without needing any explicit code for it.

---

_Comment by @workingjubilee on 2023-07-04 22:35_

Is the argv globbing behavior exposed as a user function to call, at least?

It's slightly disappointing to hear PowerShell is afflicted with this. Globbing may start with a string, yes, but morally it expands to generate a list, the kind of nicely-structured data PowerShell likes. e.g. note this deviation between "classic" shell globbing and POSIX-compliant globbing:
```quote
    Empty lists
       The nice and simple rule given above: "expand a wildcard pattern
       into the list of matching pathnames" was the original UNIX
       definition.  It allowed one to have patterns that expand into an
       empty list, as in

           xv -wait 0 *.gif *.jpg

       where perhaps no *.gif files are present (and this is not an
       error).  However, POSIX requires that a wildcard pattern is left
       unchanged when it is syntactically incorrect, or the list of
       matching pathnames is empty.  With bash one can force the
       classical behavior using this command:

           shopt -s nullglob

       (Similar problems occur elsewhere.  For example, where old
       scripts have

           rm `find . -name "*~"`

       new scripts require

           rm -f nosuchfile `find . -name "*~"`

       to avoid error messages from rm called with an empty argument
       list.)
```
—[glob(7) man page](https://man7.org/linux/man-pages/man7/glob.7.html)



---

_Comment by @BurntSushi on 2023-07-04 22:45_

@workingjubilee AIUI, you're supposed to use [`FindFirstFile`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-findfirstfilea?redirectedfrom=MSDN) and [`FindNextFile`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-findnextfilea?redirectedfrom=MSDN). The problem with those, IMO, is that the globbing syntax supported is extremely limited. I think it's basically just `?` and `*`. Better than nothing I suppose. There is discussion above talking about whether to use the Windows native functions or to just do our own globbing.

@pfmoore 

> in fact, the C runtime glob-expands argv automatically

Is this documented anywhere?

---

_Comment by @pfmoore on 2023-07-04 23:07_

> It's slightly disappointing to hear PowerShell is afflicted with this. Globbing may start with a string, yes, but morally it expands to generate a list, the kind of nicely-structured data PowerShell likes.

The thing is, it's not a matter of being "afflicted". It's a design choice - not all arguments are filenames, so why should the shell arbitrarily choose to treat them as such? But regardless of philosophy, it is a reality that Windows shells don't glob arguments, and it's extremely clumsy (probably impossible if you use `cmd.exe`) to manually glob in the shell so that a program sees a file list. So *not* globbing in the application is, whether you like it or not, a usability issue for Windows users.

> Is this documented anywhere?

I found [this](https://learn.microsoft.com/en-us/cpp/c-language/expanding-wildcard-arguments?view=msvc-170). I'm surprised it says it's not the default - maybe MSVC links that in by default and that document is about the C runtime not the compiler. Or maybe my memory is faulty, and it always needed the extra object file linking in. it's been quite a long while since I programmed in C in earnest. But all of that's very much for C, I don't know how or if it would relate to Rust.

---

_Comment by @workingjubilee on 2023-07-05 17:24_

> The thing is, it's not a matter of being "afflicted". It's a design choice - not all arguments are filenames, so why should the shell arbitrarily choose to treat them as such?

> So not globbing... is, whether you like it or not, a usability issue for Windows users.

Choose one!

---

_Comment by @pfmoore on 2023-07-05 17:59_

> Choose one!

Personally, I choose having the application do the globbing on operating systems where that's the norm...

---

_Comment by @workingjubilee on 2023-07-05 18:01_

Please go open an issue in the thousands of other application repos where this problem will remain unsolved by addressing it here, then.

---

_Comment by @BurntSushi on 2023-07-05 18:07_

Can we stop this tit-for-tat? This issue tracker is for ripgrep. I appreciate the argument that it would be better if this were solved in shell, but that is not a trump card IMO. It's just one consideration of many.

This issue is open because I generally think this is worth fixing on ripgrep's side, but I also simultaneously am not currently prioritizing it.

---

_Comment by @pfmoore on 2023-07-05 18:08_

Sorry, you're right.

---

_Comment by @kimiyoribaka on 2024-09-03 00:00_

Edit: Oh, Sorry, I just realized after posting that this is the wrong github project. I was trying to follow the discussions for emscripten, and I didn't know that github mentions could link to other projects.

---

_Comment by @pfmoore on 2025-05-30 16:19_

Is there any progress on this? I just hit it again, and came back here to see what the status was, but it looks like there's no change. Is there anything I can do to help (I'm a beginner at Rust, so I don't know how much coding help I can offer 🙁)?

FWIW, a workaround on Powershell is

```
rg needle (dir haystack/*.txt)
```

which isn't ideal, but at least it's better than pulling out my old copy of grep...

---

_Comment by @BurntSushi on 2025-05-30 16:25_

I don't think there's a simple solution to this. One of my long term projects is re-working the globbing/gitignore in ripgrep, but it's still pretty early days. It would include a directory iterator which we could leverage for this.

Probably the best work-around is what you have, or using ripgrep's `-g/--glob` flag.

---

_Comment by @pfmoore on 2025-05-30 16:50_

Thanks. I'm not sure I understand how `--glob` would work here? I did a quick check and got an error:

```
❯ rg ubuntu (dir .github/workflows/*)
C:\Work\Projects\editables\.github\workflows\pre-commit.yml
11:    runs-on: ubuntu-latest

C:\Work\Projects\editables\.github\workflows\publish.yml
8:    runs-on: ubuntu-latest
36:    runs-on: ubuntu-latest
58:    runs-on: ubuntu-latest
99:    runs-on: ubuntu-latest
❯ rg ubuntu -g .github/workflows/*
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

---

_Comment by @BurntSushi on 2025-05-30 17:01_

It can't work in all cases because `-g/--glob` is a filtering mechanism, where as supplying files as arguments explicitly tells ripgrep to search those and only those files. And that in turn overrides any of ripgrep's smart filtering that it's doing. So in your case, ripgrep is likely not descending into `.github` because it's a hidden directory. But if you tell ripgrep to disable all smart filtering, then it should act like you want. e.g.,

```
$ git remote -v
origin  git@github.com:BurntSushi/jiff (fetch)
origin  git@github.com:BurntSushi/jiff (push)
$ git rev-parse HEAD
8105228403c45402a5f553a80044d6a64891a8ab
$ rg CROSS
$ rg -g '.github/workflows/*' CROSS
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
$ rg -uuu -g '.github/workflows/*' CROSS
.github/workflows/ci.yml
401:      CROSS_VERSION: v0.2.5
426:        curl -LO "https://github.com/cross-rs/cross/releases/download/$CROSS_VERSION/cross-x86_64-unknown-linux-musl.tar.gz"
505:      CROSS_VERSION: v0.2.5
521:        curl -LO "https://github.com/cross-rs/cross/releases/download/$CROSS_VERSION/cross-x86_64-unknown-linux-musl.tar.gz"
```

For the specific case of including `.github` (or similar) directories in your searches, you might also consider adding a `.ignore` or `.rgignore` explicitly whitelisting it. [I do this in ripgrep's repo for example.](https://github.com/BurntSushi/ripgrep/blob/6dfaec03e830892e787686917509c17860456db1/.ignore#L1)

I acknowledge this is a non-ideal work-around.

---

_Comment by @pfmoore on 2025-05-30 18:52_

Ah! I'd forgotten `.github` is ignored by default. That explains it.

With the `--glob` and `(dir ...)` workarounds, that's sufficient until the long-term solution comes about. Thanks for your help.

---
