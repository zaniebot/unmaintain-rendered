```yaml
number: 269
title: Cannot search on cygwin when explicit path is given
type: issue
state: closed
author: knl
labels:
  - bug
  - question
  - wontfix
assignees: []
created_at: 2016-12-05T09:21:37Z
updated_at: 2024-03-22T17:45:33Z
url: https://github.com/BurntSushi/ripgrep/issues/269
synced_at: 2026-01-12T16:13:21Z
```

# Cannot search on cygwin when explicit path is given

---

_@knl_

Hi,

when I try to search by giving `ripgrep` a specific folder, I get an error:

```
knezn % rg test ~/work/analytics-grid                                                                                                                      
/cygdrive/c/Users/knezn/work/analytics-grid: The system cannot find the path specified. (os error 3)                                                                     
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.                                                 
                                                                                                                                                                         
knezn % rg test --debug ~/work/analytics-grid                                                                                                           
DEBUG:grep::search: regex ast:                                                                                                                                           
Literal {                                                                                                                                                                
    chars: [                                                                                                                                                             
        't',                                                                                                                                                             
        'e',                                                                                                                                                             
        's',                                                                                                                                                             
        't'                                                                                                                                                              
    ],                                                                                                                                                                   
    casei: false                                                                                                                                                         
}                                                                                                                                                                        
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }                                                   
/cygdrive/c/Users/knezn/work/analytics-grid: The system cannot find the path specified. (os error 3)                                                                     
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.                                                 
                                                                                                                                                                         
knezn % rg test --debug /cygdrive/c/Users/Knezn/work/analytics-grid                                                                                     
DEBUG:grep::search: regex ast:                                                                                                                                           
Literal {                                                                                                                                                                
    chars: [                                                                                                                                                             
        't',                                                                                                                                                             
        'e',                                                                                                                                                             
        's',                                                                                                                                                             
        't'                                                                                                                                                              
    ],                                                                                                                                                                   
    casei: false                                                                                                                                                         
}                                                                                                                                                                        
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }                                                   
/cygdrive/c/Users/Knezn/work/analytics-grid: The system cannot find the path specified. (os error 3)                                                                     
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.                                                 

knezn % rg test  /cygdrive/c/Users/knezn/work/analytics-grid
/cygdrive/c/Users/knezn/work/analytics-grid: The system cannot find the path specified. (os error 3)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.                
```

I'm on windows 7, running ripgrep 0.3.1 (expanded from [here](https://github.com/BurntSushi/ripgrep/releases/download/0.3.1/ripgrep-0.3.1-x86_64-pc-windows-gnu.zip)). My terminal is ConEmu, which runs zsh under cygwin 2.6.0-1.                                                                                                                                           
                                                                                                                                                                        
                                                                                                                                                                         

---

_Comment by @BurntSushi on 2016-12-05 12:03_

I guess I'll ask the obvious question: does `/cygdrive/c/Users/knezn/work/analytics-grid` actually exist? Can you `ls` that directory? Can you `grep` that directory?

If it does exist, I don't really know what the problem could be. I think you might have to do more debugging work to help me reproduce the problem.

---

_Label `question` added by @BurntSushi on 2016-12-05 12:03_

---

_Comment by @knl on 2016-12-05 12:12_

Oh, shoot, I forgot to add that part, sorry:

```
knezn % pwd
/cygdrive/c/Users/knezn/work/analytics-grid

knezn % ls /cygdrive/c/Users/knezn/work/analytics-grid
agent         compression             executors       module-tester  scripts      sigar-native  test-xml                application.jenkins.conf  README.md
agent-client  controller              load-test       node           shared       target        application.cloud.conf  local.conf                scalastyle-config.xml
bench         controller-http-client  mock-analytics  project        shared-etcd  test          application.conf        logback.xml               version.sbt
```

I'll gladly do any additional debugging if you could guide me, since I'm not experienced with rust & it's ecosystem.

strace gives me this:
```
strace -f -- rg -v test ~/work/analytics-grid
--- Process 10740 created
--- Process 10740 loaded C:\Windows\System32\ntdll.dll at 0000000077830000
--- Process 10740 loaded C:\Windows\System32\kernel32.dll at 0000000077710000
--- Process 10740 loaded C:\Windows\System32\KernelBase.dll at 000007FEFD800000
--- Process 10740 loaded C:\Windows\System32\advapi32.dll at 000007FEFF970000
--- Process 10740 loaded C:\Windows\System32\msvcrt.dll at 000007FEFDC50000
--- Process 10740 loaded C:\Windows\System32\sechost.dll at 000007FEFF8D0000
--- Process 10740 loaded C:\Windows\System32\rpcrt4.dll at 000007FEFE5A0000
--- Process 10740 loaded C:\Windows\System32\shell32.dll at 000007FEFEB40000
--- Process 10740 loaded C:\Windows\System32\shlwapi.dll at 000007FEFF8F0000
--- Process 10740 loaded C:\Windows\System32\gdi32.dll at 000007FEFFA50000
--- Process 10740 loaded C:\Windows\System32\user32.dll at 0000000077610000
--- Process 10740 loaded C:\Windows\System32\lpk.dll at 000007FEFE6D0000
--- Process 10740 loaded C:\Windows\System32\usp10.dll at 000007FEFDCF0000
--- Process 10740 loaded C:\Windows\System32\userenv.dll at 000007FEFD7B0000
--- Process 10740 loaded C:\Windows\System32\profapi.dll at 000007FEFD570000
--- Process 10740 loaded C:\Windows\System32\ws2_32.dll at 000007FEFDE60000
--- Process 10740 loaded C:\Windows\System32\nsi.dll at 000007FEFD920000
--- Process 10740 loaded C:\Windows\System32\imm32.dll at 000007FEFDA40000
--- Process 10740 loaded C:\Windows\System32\msctf.dll at 000007FEFD930000
--- Process 10740 loaded C:\Program Files\ConEmu\ConEmu\ConEmuHk64.dll at 000000007E110000
--- Process 10740 loaded C:\Windows\System32\api-ms-win-core-synch-l1-2-0.dll at 000007FEF4CE0000
--- Process 10740 loaded C:\Windows\System32\cryptsp.dll at 000007FEFCDA0000
--- Process 10740 loaded C:\Windows\System32\rsaenh.dll at 000007FEFCAA0000
--- Process 10740 loaded C:\Windows\System32\cryptbase.dll at 000007FEFD3A0000
--- Process 10740 thread 6796 created
/cygdrive/c/Users/knezn/work/analytics-grid: The system cannot find the path specified. (os error 3)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
--- Process 10740 thread 6796 exited with status 0x1
--- Process 10740 exited with status 0x1
```

---

_Comment by @BurntSushi on 2016-12-05 12:17_

> I'll gladly do any additional debugging if you could guide me, since I'm not experienced with rust & it's ecosystem.

Unfortunately, I had no idea what to look for. This seems more related to an issue with cygwin than ripgrep/rust, but it's hard to say for sure. Certainly, I've used ripgrep extensively inside of cygwin, so I know it works, but I've never come across this error.

One thought it to look for symlinks. Do those occur anywhere in your path?

Does this error happen with *every* path you give to ripgrep? Or just particular ones? If so, try to identify the differences between paths that work and paths that don't.

---

_Comment by @knl on 2016-12-05 12:41_

So, the situation is as follows:

- `rg test ~/work/analytics-grid` - **fails**
- `rg test .` - works
- `rg test ../analytics-grid` - works
- `rg test ../../work/analytics-grid` - works
- `rg test /cygdrive/c/users/knezn/work/analytics-grid` - **fails**
- `cd .. && rg test ./analytics-grid` - works
- `rg test c:/users/knezn/work/analytics-grid` - **works**

These findings suggest that rg is not using cygwin to do path translation. In addition, when it works, for example, in the last test, one of the paths I get is like:

`c:/users/knezn/work/analytics-grid\test\src\test\scala\com\xxx\yyy\test\script\Test.scala`
(note mixed slashes)

There are no symlinks anywhere on path.

---

_Comment by @knl on 2016-12-05 12:49_

Addendum:

```
rg test `cygpath -w ~/work/analytics-grid`
```

this also works

---

_Comment by @BurntSushi on 2016-12-27 21:58_

> rg is not using cygwin to do path translation

What exactly does this mean?

---

_Comment by @knl on 2016-12-27 23:28_

If you look at this failing example: `rg test /cygdrive/c/users/knezn/work/analytics-grid`, even though I can access the file from the command line, it seems that `rg` can't. That's why I said that it is not using the fact that it is running under cygwin (in the sense it is not using cygwin's capabilities of transforming paths).

Also, if you look at the last example:

    c:/users/knezn/work/analytics-grid\test\src\test\scala\com\xxx\yyy\test\script\Test.scala

Not that there are forward and backward slashed. `\` is only sensical on windows, `/` on both cygwin & windows. Yet, I'd expect that `rg` would use outputs that are sensical on the system it runs under, that is in this case cygwin, and only output paths with `/` separators. Current output doesn't help, as I can't copy/paste them in the command line.


---

_Comment by @BurntSushi on 2016-12-27 23:36_

With respect to file path printing, ripgrep is printing paths using the system separator. That cygwin changes it is cygwin's problem. There's no way for ripgrep to reasonably fix that automatically. With that said, this issue is covered in #275, and I'm willing to provide a fix where the path separator can be supplied as a cli argument. 

With that out of the way, what I still don't understand is what "path translation" means. Is there documentation on it? How is ripgrep supposed to do it? Why is this even an issue in the first place? I understand the examples and I can see the same pattern as you, but I'm looking for deeper understanding here so that we can fix your problem. :-)

---

_Comment by @knl on 2016-12-27 23:52_

Ok, I'm not an expert on either cygwin nor Windows, so can't say about documentation. My only understanding is that under cygwin, paths starting with `/cygdrive/c/something` (that is, looking like regular unix paths are not recognized by windows (rightly so). And it seems that ripgrep is not taking into account that it is running under cygwin, so passes whatever it gets to windows. I would expect it to honor the environment it is running under.

Could it be possible to add an option to invoke `cygpath -w <path>` for path arguments if [cygpath](https://cygwin.com/cygwin-ug-net/cygpath.html) exe is available and `<path>` was not found?

---

_Comment by @BurntSushi on 2016-12-28 15:07_

> And it seems that ripgrep is not taking into account that it is running under cygwin, so passes whatever it gets to windows. I would expect it to honor the environment it is running under.

The environment is Windows though. I can't just tell Rust's standard library that "hey, we're running on Windows, but actually, it's in cygwin, so you need to do a few things in a weird way."

> Could it be possible to add an option to invoke cygpath -w <path> for path arguments if cygpath exe is available and <path> was not found?

What do other tools do? Is it possible to write a wrapper shell script that does this for you?

---

_Comment by @gvansickle on 2017-01-02 16:29_

Hey @BurntSushi ,

> The environment is Windows though.

Not really.  The environment is Cygwin, which is a POSIX environment which happens to be running on Windows.  It's best treated as yet another Unix variant.

> What do other tools do?

The vast majority treat it as yet another Unix variant.  That's really the whole reason Cygwin exists at all.  So e.g., if you're at a Cygwin bash prompt and you type:
```bash
echo $HOME
```
... you don't get a Windows path, you get `/home/Gary` - which is actually `C:\cygwin64\home\Gary` via the Cygwin `mount` mechanism.  Any tools compiled against the Cygwin DLL are going to be getting and expecting POSIX-oid filesystem semantics, and for the most part don't need to care that they're running on Cygwin vs. say Linux or BSD or whatever.  There of course are exceptions; e.g. Cygwin has gone to great pains to make some things such as `ls 'C:\'` mostly do the right thing usually.  But in general Cygwin apps don't work well/at all with native Windows paths directly.

> I can't just tell Rust's standard library that "hey, we're running on Windows, but actually, it's in cygwin, so you need to do a few things in a weird way."

You shouldn't need to (but I don't know how Rust works here, you may not have a good option).  A "ripgrep for Windows" should be built such that it doesn't know what Cygwin is.  Likewise, a "ripgrep for Cygwin" would be built in the same way you'd build it for any other POSIX platform, and it wouldn't know what Windows is (other than what leaks through the Cygwin DLL layer).  The latter would require a "Rust for Cygwin" to exist, or at least a Rust which could cross-compile to Cygwin.

---

_Comment by @BurntSushi on 2017-01-02 16:59_

@gvansickle So what you're telling me is that I can compile a binary for Windows, and it will work in a native Windows console, and it will pretty much work in a cygwin shell, but actually, it won't completely work because file paths are weird? So this means that a Windows binary must be exclusively compiled for either cygwin or native Windows, and such a binary should *never* be used in the opposite environment?

FWIW, Rust doesn't have any option to compile against cygwin as far as I'm aware.

cc @retep998

---

_Comment by @retep998 on 2017-01-02 17:41_

@BurntSushi Compiling against cygwin means using cygwin's libc. Since Rust's libstd does not use libc for IO on Windows and works with win32 directly, this means that Rust compiled for Windows will always behave as a native Windows program, and trying to link it against cygwin's libraries will only end in misery. Rust simply does not support msys/cygwin and likely never will. Even if some cygwin fanatic somehow manages to port all of libstd to a new cygwin target, a significant portion of the cargo ecosystem still uses win32 directly and will break under cygwin.

Cygwin and msys both have their own funky file paths, effectively emulating a posix filesystem that starts at `/` and mounting directories and drives in there. Unfortunately this means native windows programs do not understand cygwin/msys paths. While it is not hard for a native Windows program to support cygwin/msys paths by running input through cygpath, knowing when to do so is rather difficult. `/foo/bar` clearly looks like a posix path, and yet it is also a valid Windows path, specifically a path relative to the root of the current drive. Also just detecting `~` and replacing it with `HOME` is far from ideal because `~` is also a valid directory name. 

So basically, cygwin/msys people seem to have a crazy insistence on using posix style paths on Windows with their own filesystem that native Windows programs don't understand. To support their strange needs you will have to detect when your program is being called from a cygwin/msys shell (maybe just provide a flag for this and let cygwin people set an alias to always set that flag) and run any input paths through cygpath.


tl;dr I hate cygwin.

---

_Comment by @BurntSushi on 2017-01-02 18:04_

@retep998 Thanks!

So it sounds like we have two realistic options here:

1. If `cygpath` is available, shell out to it for every path given to ripgrep. It looks like this will fail in some corner cases, and it will also impose a severe performance penalty when providing lots of paths to ripgrep.
2. Look at the source of `cygpath` and see if we can dynamically link to whatever cygwin library that provides path translation, and just use that. This should be much better for performance, but could still be susceptible to getting corner cases wrong.

My feeling is that we probably shouldn't worry about the corner cases that @retep998 mentioned, so long as we can be sure of whether we're running in a cygwin environment or not.

---

_Comment by @BurntSushi on 2017-01-29 14:38_

@forgottenswitch By default, ripgrep doesn't follow symlinks. You need to pass `-L` to follow symlinks, so having to do some extra processing there should be fine?

Also, in the time since I commented, I took a brief look at the implementation for `cygpath`. It is quite complex and has lots of corner cases. I suspect dynamically linking to `cygwin1.dll` will be easier.

---

_Comment by @BurntSushi on 2017-01-29 14:45_

@forgottenswitch It should already work? (If it doesn't, can you open a new issue since that seems like a distinct bug to me.)

---

_Comment by @forgottenswitch on 2017-01-30 07:53_

Sorry, I confused cygwin with mingw when previously posting about symlinks.
Fwiw, linking to the dll [does not work](https://github.com/BurntSushi/ripgrep/compare/master...forgottenswitch:cyg) for me.

---

_Comment by @BurntSushi on 2017-01-30 12:13_

Can you say more about what doesn't work?

On Jan 30, 2017 2:53 AM, "Mihail Konev" <notifications@github.com> wrote:

> Sorry, I confused cygwin with mingw when previously posting about symlinks.
> Fwiw, linking to the dll does not work
> <https://github.com/BurntSushi/ripgrep/compare/master...forgottenswitch:cyg>
> for me.
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/269#issuecomment-275998653>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34ixgWOrjrWz59z595_jemuDiRZIIks5rXZcJgaJpZM4LEAWt>
> .
>


---

_Comment by @forgottenswitch on 2017-01-30 17:15_

It works. (Didn't when `GetProcAddress` was called from `ArgMatches.paths()`, sorry again).

---

_Comment by @forgottenswitch on 2017-02-12 21:51_

While there was a confusion above, it doesn't affect any conclusion.
- MSYS works as-is, as it deep-copies the symlinks.
- Native symlinks are insufficient to emulate posix ones,
  by needing Admin rights or matching the type of target.
- Symlinks need to be read; Cygwin does not use reparse point tags.

The wrong point was that Cygwin "takes precautions" by leaving the paths in
command line arguments posix;
it rather does not care being just an emulation layer, not an interoperability one.

Segfault persisted after moving the `GetProcAddress`;
it was caused by not following the initialization procedure described in the dll documentation ;)
But running a hello-world mingw `cygpath` on a Cygwin symlink made it segfault.
Looked like the subset of IO utilites needed all to be ported to cygwin dll.

A pure-Rust solution seemed preferrable, especially since it means faster `readdir`.
Wondering if [it is suitable](https://github.com/forgottenswitch/cygwin_fs.rs). Also [walkdir integration](https://github.com/BurntSushi/walkdir/compare/master...forgottenswitch:cygwin_fs).
Ripgrep integration still looks unclear, as it uses `ignore`, which reuses `walkdir`,
yet does reading of directory entries by itself.

---

_Label `icebox` added by @BurntSushi on 2017-03-12 21:10_

---

_Comment by @gene-pavlovsky on 2017-06-04 13:05_

I'm also using Cygwin and would like to have a ripgrep for Cygwin.
One thing to add: `/cygdrive` is the default prefix for Windows drives (e.g. `/cygdrive/c` means `C:/`), but that can be changed, e.g. my `/etc/fstab`:
` none           /mnt             cygdrive  binary,posix=0       0 0`
So on my system it's `/mnt/c`, not `/cygdrive/c`.
Just a warning :)

---

_Comment by @BurntSushi on 2018-04-24 15:51_

Sadly, I think I am going to mark this issue as `wontfix`. I've looked into simple solutions, which roughly consist of the following:

1. Shelling out to the `cygpath` binary when reading file paths provided at the command line.
2. Finding a way to link to the cygwin libraries and calling the conversion routines in-process. This is basically the same as (1), but will likely be quite a bit faster.

But this presents problems. Fundamentally, path translation cannot really be done at the outer edges of the program without creating more bugs. For example, if we use cygpath to convert paths like `/cygdrive/c/Users/andrew/work` to `C:\Users\andrew\work`, then the paths printed will be in the Windows format rather than the Unix format as typed. We could in turn re-apply cygpath to all paths printed by ripgrep, but that will slow things down and seems likely to lead to weird bugs when paths don't roundtrip through `cygpath` in a way that end users expect.

It's also not clear what kind of impact this will have on the gitignore logic, which cares a lot about paths.

At the end of the day, I think fixing this bug requires a lot of attention to fussy details that I just personally don't have the bandwidth to manage. If I'm missing something, or if a larger contingent of ripgrep users is strongly vexed by this than I thought, then I'm potentially willing to continue investigating here. But as it stands now, I'd say this bug is unlikely to be fixed.

---

_Closed by @BurntSushi on 2018-04-24 15:51_

---

_Label `bug` added by @BurntSushi on 2018-04-24 15:52_

---

_Label `wontfix` added by @BurntSushi on 2018-04-24 15:52_

---

_Label `icebox` removed by @BurntSushi on 2018-04-24 15:52_

---

_Comment by @jibal on 2018-11-18 18:29_

[Update: It appears that much of this discussion may be moot because rustup supports GNU builds and can use the MSYS2 toolchain which, I believe, supports cygwin paths -- I haven't verified this. If so, then rg can be built from source using a MSYS2 version of Rust, resulting in rg supporting cygwin paths.]

> I've used ripgrep extensively inside of cygwin

> What exactly does [rg is not using cygwin to do path translation] mean?

These statements are not consistent ... just sayin'.

The problem here reflects in part the immaturity and insularity of Rust. It should be possible to build Rust to use the POSIX API on Windows ... other portable systems software like GNU and dlang do so. This would get `rg` most of the way there ... perhaps it would still need some `cfg` conditionals, but possibly not. But it's probably not time well spent to try to engineer `rg` to use cygwin directly rather than have Rust support it as a target. (Unless some enterprising soul makes a PR for cygwin support in `rg` ... this is open source, after all.)

In response to retep998's comments:

> Since Rust's libstd does not use libc for IO on Windows and works with win32 directly, this means that Rust compiled for Windows will always behave as a native Windows program, and trying to link it against cygwin's libraries will only end in misery.

This is frankly incoherent. There is a version of Rust's `libstd` that is targeted to POSIX and it could be used, probably with some tweaking, to support cygwin by linking with the cygwin dll. No misery would ensue from linking the win32 version of `libstd` with the cygwin dll, it would just be pointless.

> Rust simply does not support msys/cygwin and likely never will.

This is true if Rust never matures into an actual portable systems programming environment. Hopefully it will.

> Even if some cygwin fanatic somehow manages to port all of libstd to a new cygwin target

cygwin users are not "fanatics". cygwin makes it possible to run a huge amount of software written for linux and other POSIX environments on Windows. For many people, cygwin is a necessity. And it didn't take fanaticism to get that software to run on cygwin ... in most cases it does so out of the box. Likewise, Rust's `libstd` is *already* ported to POSIX, since it runs on linux and other POSIX systems. It would of course need some additional engineering for cygwin, but *every mature portable systems framework has undergone such engineering*.  That Rust hasn't is again a sign of immaturity and insularity.

> a significant portion of the cargo ecosystem still uses win32 directly

I'm not familiar with the internals of cargo, but this would be incredibly poor engineering if true. Cargo should be using Rust's `libstd`, which again supports POSIX targets. Even if cargo has system-specific code for some reason, the POSIX/linux variant should be suitable for cygwin. It would help to understand that  a) Cygwin is a POSIX subsystem, and b) Rust supports POSIX. (Also, one generally builds separate executables on Windows for cygwin and for win32, although for less complex programs the cygwin version is often sufficient.)

> Unfortunately this means native windows programs do not understand cygwin/msys paths.

What the heck is a "native windows program"? All of the programs that come with cygwin are native Windows executables. The D compiler that runs on Windows, and programs written in D that run on Windows, are native Windows programs. The authors of these programs who care about portability provide targeting to cygwin and/or msys ... when building for those targets, the relevant header files and dll's are used, together with a system-specific layer that uses either the POSIX or win32 API.

> While it is not hard for a native Windows program to support cygwin/msys paths by running input through cygpath

Execing `cygpath` as a program is silly ... that's for use in a shell command or script. Just link with the cygwin dll and call it as needed.

> knowing when to do so is rather difficult. /foo/bar clearly looks like a posix path, and yet it is also a valid Windows path, specifically a path relative to the root of the current drive.

This is irrelevant -- `/foo/bar` when using cygwin means starting at cygwin's root (its installation directory). cygwin provides a means to access Windows drives via POSIX paths (and the cygwin API can parse Windows paths that are URNs or start with a drive letter). Well-engineered "native Windows programs" can be built to use either cygwin, msys, or the win32 API. This is not the first time anyone has wished to target their software to cygwin ... all these problems have already been dealt with by other engineers.

> Also just detecting ~ and replacing it with HOME is far from ideal because ~ is also a valid directory name."

This too is irrelevant, even more so. `~` is a valid directory name on POSIX systems too -- surely you don't think it's parsed by the kernel. `~` is parsed by bash or other shells built to run on cygwin, just as it is on POSIX systems, and when programs that read paths from files need to interpret `~` themselves, *every POSIX version already does so* by replacing it with `getenv("HOME")`. All of these "rather difficult" decisions *have already been made*. And this one isn't rocket science ... you replace `~` when it's the first component of the path. It can be escaped with `./~` if you've made the mistake of creating directory called `~`. 

> So basically, cygwin/msys people seem to have a crazy insistence on using posix style paths on Windows with their own filesystem that native Windows programs don't understand.

It's not a "crazy insistence", it's a POSIX subsystem. Many of the vast number of POSIX programs that run under cygwin, such as the GNU suite, would fail miserably if they couldn't find things in, say, `/etc` or `/usr/bin`.

"tl;dr I hate cygwin."

First, this is a fact about you, but you're not the topic at hand, so it's irrelevant. Second, you don't *understand* cygwin.

---

_Comment by @retep998 on 2018-11-18 19:40_

Rust at the moment is completely unsuitable for developing cygwin applications. So any discussion of making ripgrep a cygwin application unfortunately will not go anywhere with Rust in its current state.

However, it is entirely possible for someone to add a new target to Rust for creating cygwin applications. It's just that it would be a rather large amount of work to do so, along with numerous debates over how to handle things like the `std::path` API on this cygwin target.

Once you do finally get `std` on board with cygwin, there will then be the mountain of work to update all the third party crates out there that ripgrep relies on in order to be able to handle this new world where it's sort of Windows but sort of not.

If you want to go ahead and add a cygwin target to Rust, by all means. But as it is, there's very little that ripgrep can do, and there's nothing that I can do to help you, because as you said I don't understand cygwin.

---

_Comment by @BurntSushi on 2018-11-18 21:52_

@jibal 

> These statements are not consistent ... just sayin'.

If you're going to start your comment by implying that I'm lying, and then try to shrug it off with "just sayin'," then I'm not going to bother spending my free time trying to engage with you.

This is your first and last warning. If you can't participate productively, then you aren't welcome on this issue tracker. If you'd like to advance the status quo, then focus on specific concrete statements that relate to Rust's standard library and ripgrep's specific technical stack. As it is, your comment looks more like a rant then someone trying to help move things along, and such things **are not welcome on this issue tracker.**

---

_Comment by @jibal on 2018-11-18 22:25_

I didn't imply that you're lying, or anything close, and certainly don't think you were; please see
https://en.wikipedia.org/wiki/Principle_of_charity

What I suggested is that your notion of what experience using cygwin consists of is limited ... I imagine to testing ripgrep; at least, that was probably the case a couple of years ago when you wrote that. People who use cygwin day in and day out usually know about /cygdrive and cygwin's mount and the other elements of cygwin path mapping, and know about going back and forth between Windows and cygwin paths and that programs like rg (or anything not linked with cygwin.dll) don't work with absolute cygwin paths ... these issues were right up front in the bug report: `/cygdrive/c/Users/knezn/work/analytics-grid` ... So certainly your experience was limited to a degree you hadn't realized at the time, and that's all I meant. I can see that you took it as a personal insult and I apologize for that ... it's certainly not what I intended. I should have omitted it ... it served no useful purpose. I see that now. Mea culpa.

And there was a lot of substantive information in my comment; it was not simply a rant.


---

_Comment by @BurntSushi on 2018-11-18 23:10_

@jibal I appreciate your apology, thanks. Let's try to avoid statements about people, and also try to resist the temptation to wax poetic about Rust's "maturity" in the systems programming space just because it doesn't satisfy a particular user need. Telling us that we "don't understand cygwin" isn't helpful. Instead, let's try to adopt a more supportive relationship and help each other fill the gaps. Certainly, I am not a Windows user. However, I do use cygwin whenever I do use Windows. That time is predominantly spent doing QA, but not just for ripgrep, but for anything that I intend to have run on Windows. Figuring out how path translation works and when it happens is certainly something that an end user comes to appreciate very quickly, but figuring out where exactly it happens in the stack of abstractions in the cygwin environment was not easy for me because I don't have a strong familiarity with how cygwin is built. My understanding has improved over time though.

Essentially, what @retep998 says is accurate. The "correct" way for ripgrep to support cygwin is for the Rust distribution to provide a cygwin target. Such a thing does not exist, and as far as I know, nobody has ever championed such an idea. The Rust ecosystem _does_ support GNU/MSYS2 builds, and to respond to your update at the top of your initial comment, ripgrep does actually [distribute Windows binaries built with the GNU toolchain on Windows](https://github.com/BurntSushi/ripgrep/releases/tag/0.10.0) (in addition to binaries built with the MSVC toolchain). However, this does not resolve the path translation problems. As I understand it, path translation is fundamentally a cygwin thing, and the MSYS2 toolchain is not coupled to cygwin. But hey---I could be wrong here and maybe there is an easy answer lurking, but I doubt it.

In terms of championing a new cygwin target, and assuming one would be lobbying for the Rust project to take over maintenance of it, I suspect they would need some pretty compelling motivation for doing so. While I could be wrong, my perspective is that such a target would be for supporting cygwin environments as an end to itself. Popping up a level, my understanding of why cygwin even exists in the first place is to provide a compatibility layer for traditional Unix programs to run on Windows with minimal or zero modifications to the source code. From [Wikipedia's article on Cygwin](https://en.wikipedia.org/wiki/Cygwin):

> Cygwin (/ˈsɪɡwɪn/ SIG-win[2]) is a POSIX-compatible environment that runs natively on Microsoft Windows. Its goal is to allow programs of Unix-like systems to be recompiled and run natively on Windows with minimal source code modifications by providing them with the same underlying POSIX API they would expect in those systems.

At _that_ level, cygwin isn't a particularly compelling target to add to the Rust project because the Rust standard library was designed to be compatible with Windows from the very beginning. That is, from the perspective of Rust programmers, POSIX APIs _tend to be_ implementation details of the standard library. That is, the standard library provides cross platform APIs that exist independently of POSIX. So the specific motivation that caused cygwin to exist in the first place doesn't necessarily exist in the Rust ecosystem. Of course, this isn't to detract from cygwin compatibility as an end unto itself; it would certainly be useful. However, the underlying motivation probably isn't as compelling as it is for traditional Unix programs written in C. I suspect this is the key underlying reason why cygwin support has not materialized as a first class target in the Rust ecosystem. It is possible someone could build it, but I don't know who would do it. As @retep998 mentions, your problems don't end at the standard library. The entire library ecosystem effectively treats `#[cfg(windows)]` as "this is where we use Windows API functions," which would effectively bypass any POSIX compatibility layer built into the standard library. That is, the Rust standard library does not completely cover the entire Windows API. It's up to the library ecosystem to provide less common facilities. So all of those library crates would need to be updated. Overall, it's a tractable amount of work, but I'd still say it's on the order of "many man months" *in addition to* lots of collaboration with folks and working toward consensus for how the cygwin target should be enabled. In other words, it's a project that requires significant amounts of both technical and social capital.

In light of the aforementioned status on a cygwin target, the other route we could explore would be specific mitigations in ripgrep for the most common cygwin problems, where path translation is undeniably at the top of that list. As I understand it---and this has been discussed in a few places on the issue tracker over the years---there are effectively three options for fixing this particular bug outside of adding a new cygwin target to Rust proper:

1. Implement cygwin's path translation ourselves. Their code is open source, and it should be possible to port. I do remember perusing this code a while back and it looked non-trivial, which is why I haven't attempted doing this port myself.
2. Link with cygwin's library. If I recall correctly, someone did actually try this, but I can't remember if they met with success or not. To be honest, I don't know what this involves, unless it really is as simple as it sounds. However, since I haven't tried it, I don't know.
3. Shell out to the `cygpath` executable for path translation.

A key problem/distinction between the above three approaches and how a POSIX compatibility layer would work is that the above is very explicit and the most logical place for it to happen is when the program starts. That is, ripgrep could do path translation on all the paths provided by the end user at the command line, and then use the result of translation throughout the rest of program execution while forgetting about the original path provided by the end user. The problem with this, as I understand it, is that it could cause problems with automatic filtering based on gitignore logic. In particular, last time I checked, I _think_ the output of path translation is an absolute path, and it would be difficult to square this with gitignore filtering. The corner cases seem gnarly here. An alternative approach would be to perform path translation lazily and only immediately before calling into Windows APIs. This is, AIUI, effectively how the POSIX compatibility layer would work. This would probably be ideal to be honest, but it requires cooperation from everyone and all dependencies, which may be hard to achieve in practice (it's a fairly similar problem to updating the ecosystem to support a hypothetical new cygwin target).

The bottom line here is that cygwin is a compatibility layer specifically designed for C programs written against the POSIX API. Reasonable assumptions were made, and getting all of the pieces to fit neatly in a completely different ecosystem that _doesn't_ treat POSIX as a first class API is tricky business. It has nothing to do with "maturity" or even "insularity." It's about different design decisions and priorities.

In principle, I am open to simple solutions to this problem that don't dramatically increase maintenance burden or introduce unreliable behavior that results in new streams of bugs.

---

_Comment by @jibal on 2018-11-18 23:29_

>The "correct" way for ripgrep to support cygwin is for the Rust distribution to provide a cygwin target.

Yes, that's what I said: "it's probably not time well spent to try to engineer rg to use cygwin directly rather than have Rust support it as a target."

Also, see my Update on that post ... when I get a chance I will do some experiments ... maybe I'll be able to contribute something concrete. Edit: Oops, I apologize again ... I missed that you wrote about this. But there's this from the MSYS2 page:

_MSYS2 tries to provide an environment for building native Windows software. MSYS2 provides a large collection of packages containing such software, and libraries for their development. As a large portion of the software uses GNU build tools which are tightly coupled to the unix world, this environment is also POSIX-compatible, and is in fact based on Cygwin._

Of course the devil is in the details.

> The entire library ecosystem effectively treats #[cfg(windows)] as "this is where we use Windows API functions," which would effectively bypass any POSIX compatibility layer built into the standard library.

A cygwin port would have to make cfg(windows) false ... programs that need to do Windows-specific things even on cygwin would have to test for either cfg(windows) or cfg(cygwin). (There are ways to handle this sort of thing by having categories with values rather than these booleans, but Rust didn't go that route.)

[I've omitted some disagreements with your post ... I will refrain from further comment until/unless I can make a real contribution] 


---

_Comment by @Kreijstal on 2024-03-22 17:31_

we need rust cygwin target

---

_Comment by @BurntSushi on 2024-03-22 17:45_

My best guess is that it's never going to happen. And this issue tracker isn't the place to ask for it.

---
