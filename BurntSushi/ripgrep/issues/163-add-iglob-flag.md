```yaml
number: 163
title: add --iglob flag
type: issue
state: closed
author: hefengrren
labels:
  - enhancement
  - help wanted
  - question
assignees: []
created_at: 2016-10-11T02:07:25Z
updated_at: 2017-10-22T02:04:17Z
url: https://github.com/BurntSushi/ripgrep/issues/163
synced_at: 2026-01-12T16:13:21Z
```

# add --iglob flag

---

_@hefengrren_

Take HTML for example: `ack --html` matches *.html, *.HTML, *.Html ...

But, `rg -thtml` only matches *.html.


---

_Comment by @BurntSushi on 2016-10-11 02:19_

Should `-g '*.html'` also be case insensitive? Should every glob be case insensitive?


---

_Label `question` added by @BurntSushi on 2016-10-11 02:19_

---

_Comment by @BurntSushi on 2016-10-11 02:20_

Do other tools that do globbing also match case insensitively?


---

_Comment by @BurntSushi on 2016-10-11 02:26_

Should we really be spending resources trying to match extensions like `hTmL`? That doesn't seem smart.

If there are common case-insensitive equivalents of certain extensions, then maybe they should just be explicitly added.


---

_Comment by @samlh on 2016-10-11 04:53_

> Should -g '*.html' also be case insensitive? Should every glob be case insensitive?

On windows, definitely.


---

_Comment by @hefengrren on 2016-10-11 09:07_

ack supports the selection (case sensitive or not) for globing: 

> ack2 **-g -i** filepattern | ack2 -x -w searchpattern

And the reason, why it is not done in one step: http://beyondgrep.com/ack-2.0/ .


---

_Comment by @BurntSushi on 2016-10-11 10:14_

I would like to see precedent established by other tools before moving
forward. Why is Windows special?

On Oct 11, 2016 5:07 AM, "hefengrren" notifications@github.com wrote:

> ack support the selection (case sensitive or not) for globing:
> 
> ack2 _-g -i_ filepattern | ack2 -x -w searchpattern
> 
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/163#issuecomment-252854530,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34mXvy1rRivBEEkYlx2QkEMnV327Oks5qy1HJgaJpZM4KTKwE
> .


---

_Comment by @BurntSushi on 2016-10-11 10:41_

For example, what do grep, find and git do?

On Oct 11, 2016 6:14 AM, "Andrew Gallant" jamslam@gmail.com wrote:

> I would like to see precedent established by other tools before moving
> forward. Why is Windows special?
> 
> On Oct 11, 2016 5:07 AM, "hefengrren" notifications@github.com wrote:
> 
> > ack support the selection (case sensitive or not) for globing:
> > 
> > ack2 _-g -i_ filepattern | ack2 -x -w searchpattern
> > 
> > —
> > You are receiving this because you commented.
> > Reply to this email directly, view it on GitHub
> > https://github.com/BurntSushi/ripgrep/issues/163#issuecomment-252854530,
> > or mute the thread
> > https://github.com/notifications/unsubscribe-auth/AAb34mXvy1rRivBEEkYlx2QkEMnV327Oks5qy1HJgaJpZM4KTKwE
> > .


---

_Comment by @hefengrren on 2016-10-11 16:13_

Windows is not special, OSX has the same "issue" for compatibility: https://www.quora.com/Why-does-OS-X-choose-to-have-a-case-insensitive-file-system-instead-of-a-case-sensitive-one .

Git has a config `core.ignoreCase`.


---

_Comment by @BurntSushi on 2016-10-11 16:39_

Folks, I am neither a Windows nor a Mac user. I don't have a whole lot of knowledge of how those platforms work. Please help me understand the problems and what other tools do.

Git's `core.ignoreCase` is a good start. That's something `ripgrep` could look for.

Note that enabling case insensitive globbing could result in a very large performance hit.


---

_Comment by @samlh on 2016-10-11 22:50_

Sorry for the terse reply earlier - it was just a gut reaction as a Windows user. The general rule on Windows is that all file operations are case insensitive, alas...

I've since tested git on Windows (with core.ignoreCase on), and .gitignore files are case insensitive there. I do think supporting this is important, even with a performance hit, as my fellow teammates add rules expecting that `**/Logs/*` will also match `**/logs/*` (from two different tools).

Otherwise, find.exe from the git for windows distribution behaves as on Linux, unless you change the flag:

```
>"c:\Program Files\Git\usr\bin\find.exe" . -name *.CS

>"c:\Program Files\Git\usr\bin\find.exe" . -iname *.CS
./foo.cs
./src/bar.cs
```

Well, almost:

```
>"c:\Program Files\Git\usr\bin\find.exe" SrC -name *.cs
SrC/bar.cs
```

In the short-term, it's not a huge deal for me - there are only a couple of rules that need duplicating in my current .gitignore files. 


---

_Comment by @BurntSushi on 2016-10-11 22:53_

I suspect you're right that some kind of support for case insensitive matching is a good thing.


---

_Label `enhancement` added by @BurntSushi on 2016-10-11 22:53_

---

_Comment by @BurntSushi on 2016-10-11 22:54_

I think there's still some question on how we want to expose it. e.g., `--iglob`? Do we need a short flag for it?

Reading `core.ignoreCase` sounds like a good idea for managing case insensitivity in `gitignore` files. What about for `.ignore` files?


---

_Comment by @hefengrren on 2016-10-12 06:03_

I'd rather have support directly in file types: `--itype`, `--itype-not`.
But, glob also works.


---

_Comment by @ChrisJefferson on 2016-10-14 10:55_

Mac is effectively case insensitive. Most functions ignore case, and you can't have two filenames which differ only by case. Here's an example to give you the flavour. This means mac users usually don't care about case. Windows obeys the same kind of rules (you can't have `q` and `Q` directories, `mkdir q, cd Q` works).

```
sh-3.2$ pwd
/Users/caj/temp
sh-3.2$ mkdir q
sh-3.2$ cd q
sh-3.2$ pwd
/Users/caj/temp/q
sh-3.2$ cd ../Q
sh-3.2$ pwd
/Users/caj/temp/Q
sh-3.2$ cd ..
sh-3.2$ mkdir Q
mkdir: Q: File exists
sh-3.2$ mkdir q
mkdir: q: File exists
sh-3.2$ rmdir Q
sh-3.2$ cd q
sh: cd: q: No such file or directory
```


---

_Comment by @BurntSushi on 2016-10-14 11:33_

@ChrisJefferson Right, but what do tools that provide some sort of globbing do? If you type `q` and hit tab, does it complete to `Q`? If you typed `q*`, would it match `Q`?


---

_Comment by @ChrisJefferson on 2016-10-14 13:00_

On windows, `q*` matches `QQ`. On mac by default it doesn't but many people (including me) turn it on fairly quickly.


---

_Comment by @lilyball on 2016-10-24 22:33_

The Mac filesystem is case-insensitive, but most CLI tools that search filenames/directory contents (as opposed to passing the string through to the filesystem to e.g. open or chdir) are still case-sensitive by default.


---

_Comment by @lilyball on 2016-10-24 22:37_

That said, I do agree that filetype detection should always be case-insensitive, regardless of how globbing works.


---

_Comment by @thijsvandien on 2017-01-20 04:11_

Interestingly, on Mac, `ls` and `find` are both case-sensitive by default. On Windows (through [Gow](https://github.com/bmatzelle/gow)), `find` is still case-sensitive but `ls` isn't.

As written in my linked duplicate issue, I believe that if at all feasible, `rg` should adhere to the rules of the filesystem that any particular file is on—a search may span multiple volumes, some of which case-sensitive and some of which not!—with potentially the option to force case-sensitivity everywhere.

Otherwise, I believe `iglob` is a good option, since I like the idea of mixing and matching with `glob`; for example `rg --iglob '*.txt' --glob '!*.txt' --files` to find all text files that do not have an all lowercase extension, though I'm not sure `rg` would or should be an ideal tool for that.

As for types, I'm not sure. On one hand I think symmetry is valuable and `--itype(-not)` would allow for a similar use case as above, where `--type(-not)` refers to canonical forms and `--itype(-not)` would match non-canonical forms too. That makes it a per-type issue, though, what the canonical forms should be (which we already have). Ruby's RAKE appears to look for any of `rakefile`, `Rakefile`, `rakefile.rb`, `Rakefile.rb`, but matches `RAKEFILE` too on case-insensitive filesystems, yet we only look for `Rakefile`. What about something more general like `*.RST`? I haven't seen any OS or other software yet that considers `*.abc` to be something totally different than `*.ABC` or `*.AbC` and anyone inventing a new extension would be stupid to distinguish it only by casing. However, in the end, what are types really? For now, it seems just named globs with nicer syntax (CMIIW). From that perspective, it would make more sense to have both `--iglob` and `--itype(-not)`.

---

_Comment by @BurntSushi on 2017-01-20 04:35_

How does one detect whether a file system is case sensitive or not? Does
this check need to be done for every file path?

On Jan 19, 2017 23:11, "Thijs van Dien" <notifications@github.com> wrote:

> Interestingly, on Mac, ls and find are both case-sensitive by default. On
> Windows (through Gow <https://github.com/bmatzelle/gow>), find is still
> case-sensitive but ls isn't.
>
> As written in my linked duplicate issue, I believe that if at all
> feasible, rg should adhere to the rules of the filesystem that any
> particular file is on—a search may span multiple volumes, some of which
> case-sensitive and some of which not!—with potentially the option to force
> case-sensitivity everywhere.
>
> Otherwise, I believe iglob is a good option, since I like the idea of
> mixing and matching with glob; for example rg --iglob '*.txt' --glob
> '!*.txt' --files to find all text files that do not have an all lowercase
> extension, though I'm not sure rg would or should be an ideal tool for
> that.
>
> As for types, I'm not sure. On one hand I think symmetry is valuable and
> --itype(-not) would allow for a similar use case as above, where
> --type(-not) refers to canonical forms and --itype(-not) would match
> non-canonical forms too. That makes it a per-type issue, though. It might
> be easy to agree on that *.RS is bad naming, but *.TXT or *.SQL not so
> much...
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/163#issuecomment-273973290>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34vmyamudLhJ1AZuTM_dMsYBgJ6zhks5rUDQKgaJpZM4KTKwE>
> .
>


---

_Comment by @thijsvandien on 2017-01-20 04:41_

I'm afraid so, if you don't want to run into edge cases (like possibly for symlinks). Otherwise at least per directory, because one can mount one volume within another. On Windows it would probably be [`GetVolumeInformationByHandleW`](https://msdn.microsoft.com/en-us/en-en/library/windows/desktop/aa964920(v=vs.85).aspx). I haven't looked into other platforms (yet).

---

_Comment by @lilyball on 2017-01-20 04:49_

@thijsvandien You're wrong (about Mac, I don't know about Windows). Any path you pass as an argument to both `ls` and `find` is treated case-insensitively (on a case-insensitive FS), because those tools pass the path to the filesystem. `find`'s `-name` and `-regex` predicates are case-sensitive, because it's doing the string comparing itself rather than the filesystem being queried (and this is why `find` has the `-iname` and `-iregex` flags). Similarly, shell globbing is case-sensitive.

As for ripgrep, I think that filetype detection should be case-insensitive by default, regardless of filesystem, because file extensions are generally considered to be case-insensitive (e.g. `README.TXT` is just as much a text file as `readme.txt` is). So there's really no need for `--itype` (though `--iglob` would be useful, as globbing should be case-sensitive).

---

_Comment by @thijsvandien on 2017-01-20 04:53_

I did mean the `-name` flag to `find`, because if it'd be about the path to search in, even `rg` would pass that path to the OS, I suppose.

---

_Comment by @thijsvandien on 2017-01-20 05:10_

`ack` is strange in this regards. On FreeBSD with a case-sensitive filesystem, with `--type=html` it matches all of `a.html`, `b.HTML` and `c.HtMl`, but with `--type=ruby` it matches `Rakefile` but doesn't match`RaKeFiLe`... On a Mac with a case-insensitive filesystem, it behaves the same. While `RaKeFiLe` and `Rakefile` should be considered equal there, it only matches if it is named as the latter.

---

_Comment by @BurntSushi on 2017-01-20 11:55_

> (though --iglob would be useful, as globbing should be case-sensitive)

Why? [Others in this thread](https://github.com/BurntSushi/ripgrep/issues/163#issuecomment-253792743) have shown that globbing itself is case insensitive on certain file systems.

There are clearly performance trade offs involved in this choice, which makes it even harder to choose the right path. Both "check every file path" and "globing/types are case insensitive" have the potential to regress performance, so I think some experiments need to be done before we can move forward.

Performance alone might not be enough to completely rule out whether we add case insensitivity to ripgrep, but it could certainly impact whether it's the default.

---

_Comment by @lilyball on 2017-01-20 22:43_

On unices, globbing is case-sensitive by default. Yes, Bash (and presumably other shells) has an option to make globbing case-insensitive, but you have to opt in to that behavior. I don't really care how it behaves on Windows, so I wouldn't have any issue if you did case-insensitive globbing there, although I would expect that it's more valuable to have consistent behavior.

Regarding Rakefile, since that's not an extension, I think it's reasonable to do a case-sensitive match against Rakefile and RAKEFILE (so e.g. RaKeFiLe doesn't match), although you may want to find out how `rake` itself behaves on a case-sensitive filesytem (e.g. will `rake` pick up a file called RaKeFiLe?). I'm only advocating for case-insensitive type comparisons for extensions, not for special-cased filenames.

---

_Label `icebox` added by @BurntSushi on 2017-04-09 13:13_

---

_Comment by @BurntSushi on 2017-05-08 22:49_

I think that, at the very least, we should be able to add a `--iglob` flag to kick things off here.The [`globset` crate already supports case insensitive matching](https://docs.rs/globset/0.1.4/globset/struct.GlobBuilder.html#method.case_insensitive), so I think the implementation path for this looks like this:

1. Add a new `case_insensitive` method to the [`GitignoreBuilder`](https://docs.rs/ignore/0.1.9/ignore/gitignore/struct.GitignoreBuilder.html) type. The `add_line` method should then pass on this setting to the glob it creates via [`GlobBuilder`](https://docs.rs/globset/0.1.4/globset/struct.GlobBuilder.html#method.case_insensitive).
2. The `--glob` flag uses [`Override`](https://docs.rs/ignore/0.1.9/ignore/overrides/struct.Override.html) instead of [`Gitignore`](https://docs.rs/ignore/0.1.9/ignore/gitignore/struct.Gitignore.html), so we'll need a `case_insensitive` method on [`OverrideBuilder`](https://docs.rs/ignore/0.1.9/ignore/overrides/struct.OverrideBuilder.html) too. That value should get pass to the corresponding method on `GitignoreBuilder` created in (1).
3. Do the plumbing to add a new `--iglob` flag. I suspect the existing `--glob` flag can be copied for the most part. When building the [`Override`](https://docs.rs/ignore/0.1.9/ignore/overrides/struct.Override.html) matcher, enable case insensitive matching using the method added in (2).
4. Write a couple unit tests for the new functionality on `Gitignore` and `Override`, and then add an integration test to `tests/tests.rs`.

---

_Label `help wanted` added by @BurntSushi on 2017-05-08 22:49_

---

_Label `icebox` removed by @BurntSushi on 2017-05-08 22:49_

---

_Renamed from "file type detection should be case insensitive" to "add --iglob flag" by @BurntSushi on 2017-05-08 22:49_

---

_Comment by @PeterSP on 2017-06-30 11:11_

I started looking into implementing `core.ignorecase` and found that I think it would be unreasonable to check for `.git/config` in every directory. Would it perhaps be reasonable to only check for `core.ignorecase` in the global git config, the way ripgrep currently checks for `excludesfile`?

---

_Comment by @BurntSushi on 2017-07-03 10:56_

@PeterSP It does certainly feel like we should respect `core.ignorecase`. I might be tempted to want to add support for that, even if it means checking every `.git/config`. I think we already do some stat calls looking for `.git`, so if we could piggy back off that by only looking for `.git/config` when `.git` exists, then I think it would be pretty cheap to do it. Note that it may be tricky to implement it that way, so I'd be fine starting without that optimization, so long as it doesn't have a noticeable impact on speed.

---

_Comment by @BurntSushi on 2017-10-22 02:04_

This was done in #531

---

_Closed by @BurntSushi on 2017-10-22 02:04_

---
