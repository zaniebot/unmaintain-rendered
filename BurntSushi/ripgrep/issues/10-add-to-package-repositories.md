```yaml
number: 10
title: add to package repositories
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-21T18:06:57Z
updated_at: 2022-08-02T19:10:11Z
url: https://github.com/BurntSushi/ripgrep/issues/10
synced_at: 2026-01-12T16:13:21Z
```

# add to package repositories

---

_@BurntSushi_

It'd be nice to at least get it into Ubuntu and homebrew. Sadly, I think either one of those will be quite difficult since Rust isn't packaged in either one of them.

The lowest hanging fruit is the Archlinux User Repository (AUR).

Fedora is getting Rust packaged soon, so that may be plausible.

Sadly, we may need to live with binaries distributed here for the time being.


---

_Label `enhancement` added by @BurntSushi on 2016-09-21 18:07_

---

_Renamed from "add ripgrep to package repositories" to "add to package repositories" by @BurntSushi on 2016-09-21 18:07_

---

_Comment by @BurntSushi on 2016-09-22 00:55_

I now have a [brew formula](https://github.com/BurntSushi/ripgrep/blob/master/pkg/brew/ripgrep.rb) and an [Archlinux `PKGBUILD`](https://github.com/BurntSushi/ripgrep/blob/master/pkg/archlinux/PKGBUILD). I also added [ripgrep to the Archlinux user repository](https://aur.archlinux.org/packages/ripgrep/).

Sadly, the brew formula isn't actually in Homebrew, and it downloads a binary, which is really bad form. I'd like to fix it, but I'm not quite sure how.


---

_Comment by @tekacs on 2016-09-23 18:30_

rust does appear to be packaged in Homebrew (http://braumeister.org/formula/rust) - just a heads-up.

The current approach uses a 'bottle' and so pulls binaries and should work just fine. It /does/ install cargo. It currently builds from source on the latest macOS Sierra, because the formula hasn't been updated to include a bottle for that yet.


---

_Comment by @BurntSushi on 2016-09-24 03:59_

@tekacs Oh, cool, somehow I missed that! I'll check that out, because I definitely want to get `ripgrep` into `brew` proper if that's possible.


---

_Comment by @mwean on 2016-09-24 07:49_

I tried to create a formula for it:

``` ruby
class Ripgrep < Formula
  desc "ripgrep combines the usability of The Silver Searcher with the raw speed of grep"
  homepage "https://github.com/BurntSushi/ripgrep"
  url "https://github.com/BurntSushi/ripgrep/archive/#{version}.tar.gz"
  version "0.1.17"
  sha256 "b558b6650bfa9cf0fd0fa58a8617cafc7c819ee25a26d15ca2ce39979dd18a18"

  depends_on "rust" => :build

  def install
    system "cargo", "build"
  end

  test do
    system bin/"rg", "--help"
  end
end
```

But couldn't get it to build. The first time I tried, it was stuck on `make` for over an hour before I killed it. Then I got this error:

```
Last 15 lines from /Users/Matt/Library/Logs/Homebrew/rust/02.make:
[ 22%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/SearchForAddressOfSpecialSymbol.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/Signals.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/TargetRegistry.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/ThreadLocal.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/Threading.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/TimeValue.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/Valgrind.cpp.o
[ 23%] Building CXX object lib/Support/CMakeFiles/LLVMSupport.dir/Watchdog.cpp.o
[ 23%] Linking CXX static library ../libLLVMSupport.a
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: ../libLLVMSupport.a(ThreadPool.cpp.o) has no symbols
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: ../libLLVMSupport.a(ThreadPool.cpp.o) has no symbols
[ 23%] Built target LLVMSupport
[ 23%] Built target obj.llvm-tblgen
make[1]: *** [all] Error 2
make: *** [/private/tmp/rust-20160924-69301-1t6mv0r/rustc-1.11.0/x86_64-apple-darwin/llvm/llvm-finished-building] Error 2

READ THIS: https://git.io/brew-troubleshooting
If reporting this issue please do so at (not Homebrew/brew):
  https://github.com/Homebrew/homebrew-core/issues

These open issues may also help:
rust: disable stable on 10.12 https://github.com/Homebrew/homebrew-core/pull/4962
```

I'm not familiar with Rust at all, but maybe some of this will help you.


---

_Comment by @est31 on 2016-09-24 13:22_

Rust is packaged for both [ubuntu](http://packages.ubuntu.com/search?keywords=rustc&searchon=names&suite=all&section=all) and [debian](https://packages.debian.org/search?keywords=rustc&searchon=names&exact=1&suite=all&section=all).


---

_Comment by @priyadarshan on 2016-09-24 15:14_

Ripgrep is such an excellent tool. It would be nice to have it in FreeBSD ports, too.

https://www.freebsd.org/doc/en_US.ISO8859-1/books/porters-handbook/porting-submitting.html

Do I understand correctly that, if one install rust:

http://www.freshports.org/lang/rust/
http://www.freshports.org/devel/cargo/

then `ripgrep` is easy to install with `cargo install ripgrep`?


---

_Comment by @mwean on 2016-09-24 21:56_

It looks like there's some problem installing rust on Sierra, so I won't be able to get the formula working. @BurntSushi can you try adding the formula a posted locally and trying to install?


---

_Comment by @Stebalien on 2016-09-25 01:22_

It looks like a trusted user just put ripgrep into Arch Linux's `[community]` repo.


---

_Comment by @BurntSushi on 2016-09-25 01:29_

Yup, many many thanks to @svenstaro!


---

_Comment by @patrickdepinguin on 2016-09-25 12:32_

Please consider a gentoo ebuild too.


---

_Comment by @passcod on 2016-09-25 22:23_

@BurntSushi could you mention that the Arch package is in `[community]`? It was confusing that it 404'd on the AUR and didn't install without refreshing package databases.


---

_Comment by @svenstaro on 2016-09-25 22:50_

@passcod: @BurntSushi just merged #92 which fixed that.


---

_Comment by @passcod on 2016-09-26 00:07_

Awesome and thanks again @svenstaro!


---

_Comment by @archon810 on 2016-09-26 05:10_

A vote to add to OpenSUSE repos here.


---

_Comment by @nihathrael on 2016-09-26 09:32_

> Rust is packaged for both ubuntu and debian.

At least for Ubuntu this repository currently doesn't build with the Rust/Cargo versions in the official repository (rust: 1.7.0, cargo: cargo 0.8.0 (built 2016-03-22))

Message:

```
> cargo build --release                                                                                                                                                                                                                                                                                     master [2b15832]
failed to parse manifest at `<path>/Cargo.toml`

Caused by:
  could not parse input as TOML
Cargo.toml:42:9 expected a key but found an empty string
Cargo.toml:42:9-42:10 expected `.`, but found `'`
```


---

_Comment by @est31 on 2016-09-26 09:49_

@nihathrael it works on rust 1.9.0, which will be the one that ubuntu 16.10 ships with in a few weeks.


---

_Comment by @BurntSushi on 2016-09-26 11:29_

@nihathrael Right, `ripgrep` requires Rust 1.9.


---

_Comment by @wezm on 2016-09-27 03:50_

@priyadarshan yes, that's correct.


---

_Comment by @hmeine on 2016-09-27 08:35_

I filed a request for MacPorts here: https://trac.macports.org/ticket/52416


---

_Comment by @sambrightman on 2016-09-28 07:12_

Isn't it _good_ form to download bottles (binaries) in Homebrew-land?


---

_Comment by @BurntSushi on 2016-09-28 09:31_

Go check out homebrew core. Aren't most formulas compiled from source?

On Sep 28, 2016 3:12 AM, "Sam Brightman" notifications@github.com wrote:

> Isn't it _good_ form to download bottles (binaries) in Homebrew-land?
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/10#issuecomment-250089650,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34rt2uQgRnUr3OUVjfJl2rW9v6Y4mks5quhNZgaJpZM4KDG1y
> .


---

_Comment by @sublee on 2016-09-28 11:37_

Is there any way to get a pre-built binary for Ubuntu?  I really want to use `ripgrep` for my all environments but I don't want to install Rust just for this reason.

Furthermore, I think Linuxbrew is not stable enough so I'm not using it.


---

_Comment by @BurntSushi on 2016-09-28 11:50_

@sublee The README contains links to [binaries that I distribute](https://github.com/BurntSushi/ripgrep/releases).


---

_Comment by @sublee on 2016-09-28 12:07_

@BurntSushi Oh, why didn't I find the link.  I was a fool.  Anyway, thank you for letting me know!


---

_Comment by @BurntSushi on 2016-09-28 12:08_

No worries! :-)


---

_Comment by @moshen on 2016-09-28 16:13_

Opened https://github.com/Homebrew/homebrew-core/pull/5268 , but it seems to be hung up on Sierra issues.


---

_Comment by @carlwgeorge on 2016-09-29 15:50_

I just [submitted](https://bugzilla.redhat.com/show_bug.cgi?id=1380442) this for Fedora and EPEL (to be used in RHEL and CentOS).


---

_Comment by @rolodato on 2016-09-29 21:48_

My humble vote for a Nix package here. Thanks!


---

_Comment by @gyscos on 2016-09-29 22:05_

Shouldn't binary packages be built with the `simd-accel` feature?


---

_Comment by @BurntSushi on 2016-09-29 22:41_

@gyscos They _should_, yes. But `simd-accel` is only available on nightly Rust. If your packaging story requires you to use the Rust that is already packaged, then it's probably a stable Rust, which means you can't use `simd-accel`.


---

_Comment by @BurntSushi on 2016-09-29 22:42_

FYI, The MacOS and Linux binaries that I distribute does have `simd-accel` enabled.


---

_Comment by @dstcruz on 2016-09-30 16:20_

Just submitted this for chocolatey (a windows package management tool). It is not available yet, as it is pending automated review. I'll work with them to make sure that it gets through.

https://chocolatey.org/packages/ripgrep/0.2.1

The repository for the chocolatey package is here:

https://github.com/dstcruz/chocolatey-mypackages/tree/master/packages/ripgrep

As per the instruction on the chocolatey's package page, you run the obvious command in windows:

`c:\> choco install ripgrep`


---

_Comment by @sambrightman on 2016-10-03 07:22_

> Go check out homebrew core. Aren't most formulas compiled from source?

In practice, no, I don't think so. Almost all provide bottles and I think it's considered good form to allow that (just for speed probably). However, you are right that they also all provide source compile, and not doing so is probably bad form.


---

_Comment by @priyadarshan on 2016-10-03 09:56_

I can confirm `ripgrep` is easily installable on FreeBSD 11-RELEASE with:

```
sudo pkg install -y rust
sudo pkg install -y cargo
cargo install ripgrep
[...]
Installing /home/priyadarshan/.cargo/bin/rg
warning: be sure to add `/home/priyadarshan/.cargo/bin` to your PATH to be able to run the installed binaries
```

Version:

```
rg --version
0.2.1
```

Perhaps it could be added to README, or wiki?


---

_Comment by @patrickdepinguin on 2016-10-03 15:02_

I submitted an ebuild to gentoo: https://bugs.gentoo.org/show_bug.cgi?id=596050
I'm not an accustomed ebuild writer, though, so I don't know how long it would take to get this applied.


---

_Comment by @moshen on 2016-10-03 15:58_

@sambrightman : [Homebrew prefer formula which build from source](https://github.com/Homebrew/brew/blob/master/docs/Acceptable-Formulae.md#we-dont-like-binary-formulae).  [The bottles/binary releases that they distribute are built by them on their CI infrastructure](https://github.com/Homebrew/brew/blob/master/docs/Bottles.md#bottle-creation).


---

_Comment by @nightpool on 2016-10-06 20:32_

Looks like this was merged into Homebrew proper—can someone update the readme?


---

_Comment by @chopfitzroy on 2016-10-15 11:27_

Just wanted to ask now that Ubuntu 16.10 is out are we looking at a pre-packaged build?


---

_Comment by @BurntSushi on 2016-10-15 13:34_

@nightpool README has been updated. :-) Also, Homebrew now has `ripgrep 0.2.3` as of today.

@CrashyBang I've never created an Ubuntu package before (and I don't use Ubuntu), so I'm not terribly familiar with how it works. Ubuntu would at least need to have Rust 1.9 and Cargo in its repos. If it has that, then `ripgrep` can be packaged.


---

_Comment by @carlwgeorge on 2016-10-16 02:44_

My [package review](https://bugzilla.redhat.com/show_bug.cgi?id=1380442) for Fedora and EPEL looks like it may be delayed for a bit.  Their build farm doesn't have outgoing internet access, so cargo fails with network resolution errors.

In the mean time, I went ahead and built RPMs for ripgrep in [this copr repository](https://copr.fedorainfracloud.org/coprs/carlgeorge/ripgrep/), so people can use it now.  I plan to maintain that repository until the package gets added to the main Fedora/EPEL repositories.


---

_Comment by @BurntSushi on 2016-10-16 02:53_

@carlwgeorge Awesome, thank you! A couple questions:
1. I think Cargo is supposed to be able to work without network access? There's a FAQ entry about it here: http://doc.crates.io/faq.html#how-can-cargo-work-offline --- Honestly, it looks like it might be a little hairy.
2. That `copr` repository is great news! Would you be willing to explain how folks could use it so I can put it in the README? (Please keep in mind that I'm not a user of RPMs, Fedora or EPEL, so I'm ill suited to do that myself.)


---

_Comment by @chopfitzroy on 2016-10-16 03:23_

Hey @BurntSushi okay I am an Ubuntu user so I will look into the above and see what I can do, will get back to you shortly.


---

_Comment by @BurntSushi on 2016-10-16 03:26_

@CrashyBang Awesome! Thanks so much!


---

_Comment by @carlwgeorge on 2016-10-16 09:38_

@BurntSushi 
1. Thanks for that FAQ entry, I didn't know about the `--frozen` flag.  Fedora is still sorting out what their Rust package guidelines will look like, so hopefully that tidbit will be useful in the discussion.  The dependencies are going to be a biggest thing to sort out.  Hopefully there is a way to package the source files of rust libraries into "-devel" packages, which can then be installed and referenced in other build roots without full internet access.
2. I went ahead and wrote the instructions up for you in #183.


---

_Comment by @BurntSushi on 2016-10-16 14:17_

> Hopefully there is a way to package the source files of rust libraries into "-devel" packages, which can then be installed and referenced in other build roots without full internet access.

Owch... That is not going to be a good time.

> 1. I went ahead and wrote the instructions up for you in #183.

Yay! Merged. :-)


---

_Comment by @mattiasb on 2016-11-18 10:51_

@dstcruz The chocolatey package has a missing dependency on VCRUNTIME140.dll it seems


---

_Comment by @BurntSushi on 2016-11-18 11:37_

@mattiasb I don't know what chocolatey is, but I'm not sure how to get in contact with its maintainer.


---

_Comment by @mattiasb on 2016-11-18 14:30_

@BurntSushi It's a package manager for windows. @dstcruz (from a few comments above) is the maintainer of the ripgrep package there. :)


---

_Comment by @dstcruz on 2016-11-18 15:16_

Hey @mattiasb! We can take this conversation to [the ripgrep chocolatey package repo](https://github.com/dstcruz/chocolatey-mypackages/tree/master/packages/ripgrep) if it turns out to be a packaging issue.  Right now all the package is doing is fetching and unzipping (the released zip files)[https://github.com/BurntSushi/ripgrep/releases].  According to [this stackoverflow post](http://stackoverflow.com/questions/30811668/php-7-missing-vcruntime140-dll), it would seem that due to me packaging the MSVC files, we might be missing that. Could you try downloading an testing the windows-gnu files from the releases link? If that works better I might switch to using those instead.

The decision to use the MSVC over the windows-gnu files was quite arbitrary. I don't know exactly what the difference is.  I'm curious to know if the windows-gnu files have other dependencies that we might need to take care of also.

Thanks!


---

_Comment by @BurntSushi on 2016-11-18 15:22_

@dstcruz I'm not a Windows expert, but my understanding is that MSVC should be preferred, but that the GNU version doesn't require any external dependencies. My understanding though is that MSVC does require  having the [Microsoft VC++ 2015 redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=48145) installed. I've been told that one can bundle this with the ripgrep executable though.

cc @retep998


---

_Comment by @dstcruz on 2016-11-18 18:41_

Ok, so it looks like in chocolatey I can have a dependency [on the VC++ 2015 redistributable package](https://chocolatey.org/packages/vcredist2015). Can someone comment on whether doing this is preferable to using the windows-gnu executable?

I'm not sure I understand what are the advantages of using either the MSVC or the gnu compiled versions.


---

_Comment by @retep998 on 2016-11-18 20:23_

Binaries linked using mingw come with bloat that needs to be stripped and even then aren't as small as binaries linked using msvc. Binaries linked using mingw come with dwarf debuginfo, so you can't ask the OS for backtraces, and libbacktrace is broken so you can't get backtraces that way either, meanwhile msvc backtraces work great when you have the PDB. The redistributable issue will soon be a non-issue due to https://github.com/rust-lang/rust/pull/37545 so msvc can statically link the CRT. So basically the only remaining point in MinGW's favor is that MinGW itself is open source.


---

_Comment by @dstcruz on 2016-11-18 20:51_

@retep998 thanks for the info. I'll keep the MSVC version then as the package, and include a dep to the vcredist2015 package.  Any way I can be alerted when [rust-lang/rust#37545](https://github.com/rust-lang/rust/pull/37545) becomes a reality and is used by ripgrep and I can stop making the vcredist2015 a dep? I read the release notes  for ripgrep, so if there is any blip about this there that would be enough.


---

_Comment by @dstcruz on 2016-11-18 21:34_

@mattiasb, I just pushed [ripgrep version 0.2.9.1](https://chocolatey.org/packages/ripgrep/0.2.9.1) to chocolatey. It adds a dependency on [vcredist2015](https://chocolatey.org/packages/vcredist2015). If you have a chance, could you please validate and see if this fixes your issue? Chocolatey might have a delay between the time I upload a package, and when it is available for public consumption. Please take that into account.

Thanks!


---

_Comment by @mattiasb on 2016-11-18 22:29_

@dstcruz great! Will test on monday when I'm back at work! (I don't run windows at home :))


---

_Comment by @BurntSushi on 2016-11-18 23:46_

@dstcruz When ripgrep makes use of rust-lang/rust#37545, I'll definitely put it in the changelog. :-)


---

_Comment by @ararslan on 2016-12-09 18:59_

@priyadarshan It would be great to have this directly available in `pkg`. Since it should work natively on FreeBSD as you've shown, I think it would be dead simple to register it with FreshPorts.

---

_Comment by @priyadarshan on 2016-12-10 18:20_

@ararslan FreshPorts is just a site that allows one to search and browse all ports. To [submit a new FreeBSD port](https://www.freebsd.org/doc/en_US.ISO8859-1/books/porters-handbook/quick-porting.html) is not trivial, although not too complicated.

---

_Comment by @pvalkone on 2016-12-11 13:03_

FYI, I just submitted a [PR](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=215212) for `ripgrep` on the FreeBSD Bugzilla.

---

_Comment by @jackwilsdon on 2017-01-25 14:36_

[`ripgrep` is available on Void Linux too](https://github.com/voidlinux/void-packages/tree/2dff594dd5ced35ae9ea75ccb52175030ef4718f/srcpkgs/ripgrep).

---

_Comment by @avindra on 2017-02-12 04:45_

It's not officially in opensuse but  installing via cargo works like magic 👍 

---

_Comment by @igor-raits on 2017-02-26 00:46_

[`ripgrep` is available on Fedora](https://lists.fedoraproject.org/archives/list/rust@lists.fedoraproject.org/thread/P2ZGCET4HRIQS7CJ7PQSQMLA3MQIRIDL/). Though not in official repos, but we're working on it.

---

_Comment by @MartinBonner on 2017-03-09 10:00_

So I'm running Ubuntu 14.04, and while it would be wonderful if I could just go `apt-get install ripgrep`, I've downloaded the tar.gz - but WHAT DO I DO NOW?  I'm guessing I need `rg` on my path, but what are `rg.1`, `rg.bash-completion`, `rg.fish`, `_rg`, and `_rg.ps1`?  (Legal stuff and the README is obvious).

Actually, should I raise this as a separate issue "Better install instructions for compiled linux binaries"?

---

_Comment by @ehiggs on 2017-03-09 10:36_

>  I'm guessing I need rg on my path

Yes. That's all you need to do. 

The other files are nice-to-haves if you want `rg` integrated into your shell. e.g. the `rg.bash-completion` is so you can do `rg --p<tab>` and see all the options that begin with `p`. If you use the fish shell, you use `rg.fish`. If you use zsh, you use `_rg`, and if you use powershell you use `_rg.ps1`.

`rg.1` is a manpage. You can copy that to wherever you keep your manpages. (you probably won't use `man rg` and will instead use `rg --help` or look up docs online so this is also not required.

>Actually, should I raise this as a separate issue "Better install instructions for compiled linux binaries"?

Upon completion of this ticket, the instructions will be to use the package manager. 

---

_Comment by @gyscos on 2017-03-09 23:51_

> Upon completion of this ticket, the instructions will be to use the package manager.

I feel like packaging instructions should still be kept around, maybe in an `INSTALL` file, for instance if new distributions want to be supported later on.

---

_Comment by @dvergeylen on 2017-03-24 14:15_

@MartinBonner,

hint: to know where to put the manpage file: `manpath` lists the possible folders.
Then simply copy `rg.1` file to one of them:
```sh
cp rg.1 /usr/local/share/man/man1/
```
(you might need privileges to copy)

---

_Comment by @mcepl on 2017-03-24 17:07_

> That copr repository is great news! Would you be willing to explain how folks could use it so I can put it in the README? (Please keep in mind that I'm not a user of RPMs, Fedora or EPEL, so I'm ill suited to do that myself.)

Just follow the instructions on https://copr.fedorainfracloud.org/coprs/carlgeorge/ripgrep/ (i.e., either run ``dnf copr enable carlgeorge/ripgrep`` if you have ``dnf``, or add provided ``.repo`` file to ``/etc/yum.repos.d``).

---

_Comment by @igor-raits on 2017-03-24 18:34_

@mcepl note that it's not preferred way for Fedora. We (Rust SIG in Fedora) have proper build of ripgrep in repos (see https://github.com/BurntSushi/ripgrep/issues/10#issuecomment-282523772)

---

_Comment by @mcepl on 2017-03-24 19:28_

@ignatenkobrain Except, you don’t:
```
https://fedorapeople.org/groups/rust/repos/7Workstation/x86_64/repodata/repomd.xml: [Errno 14] HTTPS Error 404 - Not Found
```
Not everybody has Fedora.

---

_Comment by @cuviper on 2017-03-24 19:32_

The main issue with that COPR as far as Fedora policy goes is that it's using a network-enabled build.  It hasn't solved the problem of packaging crate dependencies at all.  That's ok for a COPR, but it won't get into Fedora proper that way, nor EPEL.

---

_Comment by @carlwgeorge on 2017-03-24 21:39_

@ignatenkobrain @cuviper I understand that plenty of work has gone into this on your end, but it's frustrating that yall didn't work with me on [my original submission](https://bugzilla.redhat.com/show_bug.cgi?id=1380442).  Doing it yourself and closing my review request as WONTFIX is rather off-putting.  I'm also disappointed that yall choose to require `with` rich dependency support, as that will prevent building your ripgrep for EPEL7 (the main place I want it at).  At this point it looks like I'll have to continue to maintain my COPR, at least for EPEL7.

---

_Comment by @cuviper on 2017-03-24 21:46_

@carlwgeorge -- I thought @ignatenkobrain was polite in explaining that WONTFIX, so I'm sorry you feel otherwise.  The current packaging state we've come up with is largely automated, so collaboration requires working on the tools, not your particular specfile.  You are still quite welcome to get involved in the SIG!  And FWIW, I want a solution not using "with" too: https://pagure.io/fedora-rust/rust2rpm/issue/29

(edit: if you want to discuss this further, let's take it to #fedora-rust or rust@lists.fedoraproject.org)

---

_Comment by @BurntSushi on 2017-03-24 22:13_

I just wanted to say thanks to everyone who have been working so hard to get ripgrep properly packaged in the various Linux distros. I know it's frustrating and thankless work, but I'd like to say that I very much appreciate it!

---

_Comment by @x4121 on 2017-04-22 09:53_

Since we didn't hear back from @CrashyBang and 17.04 came out recently (which has repos for rustc 1.16 and cargo 0.17) I tried to build a package myself.
Seems like Launchpad forbids internet access during build and I'm not sure how to get the build dependencies to run cargo with `--frozen` (maybe something similar to cargo-vendor?).

If anyone has an idea on this, please let me know.

Other than that you can run
```
apt install cargo
cargo install ripgrep
```
on Ubuntu 17.04 (if you don't mind having 360mb "overhead" for cargo)

edit: I could of course provide *.deb binary-packages for Ubuntu.. but I for myself wouldn't trust binaries of random strangers on the internet ;)
edit2: Build steps and prebuild package are online https://github.com/x4121/ripgrep-ubuntu/releases

---

_Comment by @mcepl on 2017-04-24 09:58_

@carlwgeorge could we get 0.5.1 to your COPR, please?

---

_Comment by @BurntSushi on 2017-04-24 10:48_

@JoshTriplett do you have any ideas for @x4121?

---

_Comment by @carlwgeorge on 2017-04-24 14:09_

@mcepl Done.

---

_Comment by @joshtriplett on 2017-04-24 16:12_

@x4121 Depending on how far you want to go packaging it "correctly", you have two options.

Easy but not policy-compliant (note: cargo-vendor might help you with some of this): download and unpack all the .crate files for all the dependencies (recursively), as well as ripgrep itself, and unpack them all in subdirectories of a single directory that you use as the source. In each one, put a `.cargo-checksum.json` file, containing `{"package":"$SHA256","files":{}}` , where `$SHA256` is the sha256 checksum of the .crate file.  Create a directory containing a `.cargo/config` file, containing this:
```toml
[source.crates-io]
replace-with = "my-registry"

[source.my-registry]
directory = "/path/to/the/vendor/directory"
```
Then run `CARGO_HOME=/the/source/directory cargo install ripgrep --root some/installation/path` and copy the binary in place from that installation path.

If you want to build policy-compliant debs, then give debcargo a try.  `cargo install debcargo`.  You'll also need dh-cargo from Debian experimental.  You can use debcargo to build a deb for each package, then upload all those packages to your PPA in dependency order (to build using each other).

---

_Comment by @x4121 on 2017-04-26 12:47_

thanks @joshtriplett, will try to have a deeper look over the weekend. Having a debhelper module for cargo would of course be the ideal solution in the long run. Guess this will take a while until it's in Debian/Ubuntu though :(

---

_Comment by @ghost on 2017-04-26 12:58_

@x4121: https://wiki.debian.org/Teams/RustPackaging/Policy (from https://internals.rust-lang.org/t/debian-rust-packaging-policy-draft/4453/2)

---

_Comment by @x4121 on 2017-04-29 23:47_

Build with cargo-vendor was fairly easy/straightforward. Having the dependencies with the build files is IMHO 'prettier' than having to build all ~40 dependencies inside a PPA. Having everything (including debcargo) in the (non-experimental) Debian/Ubuntu-Sources would be nicer, of course.

I created a separate PPA for the package (so no accidental installation of other stuff):
https://launchpad.net/~x4121/+archive/ubuntu/ripgrep

If someone has the time/leisure and wants to put them into a 'more official' archive, go for it ;)

---

_Comment by @gubikmic on 2017-05-13 12:04_

I just ended up using `curl https://sh.rustup.rs -sSf | sh` and then `cargo install ripgrep`.
Not ideal, but doesn't take very long either.

---

_Comment by @pvalkone on 2017-06-21 18:27_

The `textproc/ripgrep` port was [committed](https://github.com/freebsd/freebsd-ports/commit/74797b1e4a74416dc31222662f3fa8b62b7bd142) to the FreeBSD ports tree earlier today. The package will probably be available in a few days. Thanks to @t6 for his work on the `USES=cargo` macro and help on getting the port submitted! 

---

_Comment by @valpackett on 2017-08-15 11:47_

Nice! Should be added to the README for FreeBSD: `pkg install ripgrep`

---

_Comment by @mika on 2017-08-16 09:15_

@x4121 @joshtriplett `dh-cargo` is in Debian testing/buster and unstable since June, are there any efforts in bringing ripgrep officially into Debian nowadays?

---

_Comment by @x4121 on 2017-08-16 09:47_

@mika I'm currently in full-time employment, but I will try to have a look at it. Haven't done packaging for Debian yet, though.

---

_Comment by @ptman on 2017-08-16 10:14_

If compiling produces a static binary then it should be really easy to make a package using [fpm](https://github.com/jordansissel/fpm)

---

_Comment by @mika on 2017-08-16 10:16_

@ptman thanks, but this isn't policy-compliant nor following/supported by official Debian procedures

---

_Comment by @BurntSushi on 2017-09-04 14:18_

It sounds like both Cargo and Rust are now packaged in Debian. What are the next steps?

---

_Comment by @x4121 on 2017-09-04 15:16_

The next step would be to have a look into `dh-cargo` as @mika suggested and see if this would get the build steps rid of my ugly cargo-vendor workaround. Sadly I didn't have a free weekend since the last post to have a look myself (if so. wants and can do this, I wouldn't be sad either :wink: )

---

_Comment by @Sjord on 2017-09-08 16:17_

I tried creating a package using debcargo, but I got caught up in a sort of dependency hell. Running `debcargo package ripgrep` creates a directory containing a `debian` directory. The control file specifies several build dependencies, such as `librust-bytecount-0.1+default-dev` and `librust-lazy-static-0.2+default-dev`. These in turn can be build using debcargo, and then you need the dependencies of those dependencies, and so on. Eventually I hit a problem where I did have a package, but it was the wrong kind. E.g. some package depends on librust-regex-0.2+default-dev but I only got librust-regex-0.2+simd-accel-dev_0.2.2-1_all.deb.

Besides debcargo there is also [cargo-deb](https://github.com/mmstick/cargo-deb).

---

_Comment by @joshtriplett on 2017-09-08 17:40_

@Sjord I've successfully built (local) packages for ripgrep using debcargo before. It shouldn't be possible to end up without a `librust-regex-0.2+default-dev` if you build regex using debcargo. Can you post the `debian/control` file that you get for regex?

---

_Comment by @scorpiodawg on 2017-10-30 18:38_

@MartinBonner @dvergeylen You can also run: `man -l rg.1` from within wherever you extracted the tarball if you don't have admin privileges.

---

_Comment by @dieggsy on 2017-10-30 19:27_

@scorpiodawg @dvergeylen @MartinBonner if you don't have admin privileges, it may be easier to set manpath in the appropriate shell init file (something like `export MANPATH="~/.local/share/man:MANPATH"`) and copy the file to there.

---

_Comment by @Calinou on 2017-11-02 18:26_

Just a heads up for Windows users: ripgrep is available in [Scoop](http://scoop.sh/). Just run this command after installing it:

```
scoop install ripgrep
```

No administrator privileges are required, and it's automatically made available in the `PATH`. This might be worth adding to the README 😄

---

_Comment by @niva-xx-zz on 2017-12-11 21:31_

Hi,
Under windows 10, msbuild 2017, is this the command which fit better to AMD64 CPU ?
cargo build --release --target x86_64-pc-windows-msvc

---

_Comment by @BurntSushi on 2017-12-11 22:57_

@niva-xx For unrelated issues, please open a new ticket. Please describe the problem you're trying to solve in as much detail as possible. Please also state the things you've tried, what happened and what you expected to happen.

---

_Comment by @gitaped on 2017-12-27 08:49_

@Sjord  @BurntSushi  On Debian 9 I was able to use [cargo-deb](https://github.com/mmstick/cargo-deb) to build a `ripgrep` `deb` package and successfully installed using `dpkg`.

---

_Comment by @BurntSushi on 2017-12-27 13:00_

@apeduru Awesome! Would it be possible for you to share the commands you needed to run to achieve that? Thanks!

---

_Comment by @gitaped on 2017-12-27 16:07_

Pulled from my history.

`sudo apt install liblzma-dev`
`cargo install cargo-deb`
`cd ~/src/ripgrep`
`cargo deb`
`sudo dpkg -i target/debian/ripgrep_0.7.1_amd64.deb`

Installed successfully.
```
$ dpkg -l | rg ripgrep
ii  ripgrep                               0.7.1                                       amd64        Line oriented search tool using Rust's regex library. Combines the raw
```

---

_Comment by @x4121 on 2017-12-27 17:39_

@apeduru as far as I understood @Sjord (and what was my problem as well), you can't really download stuff during build (as cargo does) when building "official" Debian/Ubuntu packages. AFAIK everything you need during build has to be provided as source or another "official" Debian/Ubuntu package. This makes builds very stable, secure and reproducible but is not really compatible with the ideas behind things like cargo. That's why @Sjord got dependencies like `librust-bytecount-0.1+default-dev` and why I built it for myself shipping all dependencies with the source code.
Would be very happy if I understood this wrong or someone finds a way around this :confused: 

---

_Comment by @gitaped on 2017-12-27 18:12_

@x4121 Yeah just scrolled up and saw your comment from earlier. Doesn't make sense to build packages manually.
Maybe for now, @BurntSushi can provide `deb` package releases here on Github in addition to the binaries? We can trust him right 😄 

---

_Comment by @carlwgeorge on 2017-12-27 23:05_

Fedora has the same policy as Debian; dependencies must either be in the package sources or available as another official package (BuildRequires).  The unofficial [copr](https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/) I maintain for Fedora, RHEL, and CentOS downloads the dependencies during the build for now.  Fedora policy now allows bundled dependencies, so if ripgrep were to vendor it's dependencies I could take advantage of that.  Is that something you would consider @BurntSushi?

References:
* https://github.com/alexcrichton/cargo-vendor
* http://doc.crates.io/source-replacement.html

---

_Comment by @BurntSushi on 2017-12-28 11:43_

> Maybe for now, @BurntSushi can provide deb package releases here on Github in addition to the binaries? We can trust him right 

Absolutely not. Sorry. We can't just add things to the release as if it doesn't cost anything.

> Fedora policy now allows bundled dependencies, so if ripgrep were to vendor it's dependencies I could take advantage of that. Is that something you would consider @BurntSushi?

I don't really understand what you're asking me to do. Are you asking me to run `cargo vendor` and then commit the results? What's the difference between whether I do that or someone else does that?

---

_Comment by @carlwgeorge on 2017-12-29 15:41_

The end goal would be to have a single tarball for users to download that they could build ripgrep from without downloading any additional dependencies.  Committing snapshots of your dependencies to this repository is probably the easiest way to achieve that goal.  It seems to be more common in the golang ecosystem (examples: [kubernetes](https://github.com/kubernetes/kubernetes/tree/master/vendor), [moby](https://github.com/moby/moby/tree/master/vendor), [buildah](https://github.com/projectatomic/buildah/tree/master/vendor), [caddy](https://github.com/mholt/caddy/tree/master/vendor)) than the rust ecosystem.

I don't know if `cargo vendor` is the best way to achieve this, it's just the first thing I came across while searching.

I suppose it doesn't matter who does the actual committing of the dependencies, I just wanted to discuss it with you first.

---

_Comment by @BurntSushi on 2017-12-29 15:55_

@carlwgeorge Oh I see, interesting. I like the idea of being able to download a source tarball that one can compile without using the Internet. Would it be acceptable to run `cargo vendor` in CI and produce a tarball from that instead of actually committing all of the deps into the repo?

---

_Comment by @carlwgeorge on 2017-12-29 16:03_

That sounds reasonable to me.  That would be the best of both worlds.  Cloning the repo would be kept smaller, but those desiring bundled dependencies could download the appropriate asset from the release instead of the automatic github tag tarballs.

---

_Comment by @BurntSushi on 2017-12-29 16:04_

@carlwgeorge Awesome! I will look into that. @joshtriplett What do you think of that idea? (Producing a source tarball that includes what it has today and also the results of running `cargo vendor`. Would that permit packaging ripgrep in Debian without, say, needing to package all of ripgrep's crates.io deps?)

---

_Comment by @joshtriplett on 2017-12-29 19:36_

@BurntSushi Not as an official package, no: https://www.debian.org/doc/debian-policy/#convenience-copies-of-code

It's rare for ftpmasters to make an exception and allow embedded copies of other libraries that are capable of being packaged separately.

---

_Comment by @BurntSushi on 2017-12-29 21:49_

Interesting, thanks for informing me of that! @carlwgeorge Does Fedora have a similar policy as the one that @joshtriplett linked for Debian?

---

_Comment by @carlwgeorge on 2017-12-29 23:25_

They did previously, but it was relaxed significantly.  It's more of a strong preference than a requirement now.

https://fedoraproject.org/wiki/Packaging:Guidelines#Bundling_and_Duplication_of_system_libraries

Context for the next paragraph: EPEL is a Fedora subproject that allows Fedora packagers to ship software for RHEL/CentOS that Red Hat doesn't include in the main distribution.

The [Fedora Rust SIG](https://fedoraproject.org/wiki/SIGs/Rust) has been working on the Rust-specific guidelines for Fedora, as well as packaging many popular libraries.  However, they have chosen to make heavy use of a recent RPM feature called "[with rich dependencies](https://github.com/rpm-software-management/rpm/pull/299)", to represent the dependency ranges in Rust.  That feature isn't available in RHEL7's version of RPM, so the work they are doing won't be able to be easily reused when building for RHEL7.  However, a tarball with bundled dependencies would circumvent the need for the dependency ranges, and allow an official EPEL build for RHEL7.  That's the type of scenario where using bundled dependencies is acceptable, and already regularly occurs for golang software in EPEL.

On a related note, [last month ripgrep was been officially added to Fedora Rawhide](https://lists.fedoraproject.org/archives/list/rust@lists.fedoraproject.org/thread/D7PYBU7JKGLRYR2HKRXBM6EGZZEDCK33/), meaning that it will be available from the default repos in Fedora 28 (due to be released in May 2018).

---

_Comment by @BurntSushi on 2017-12-30 13:48_

@carlwgeorge Interesting, thanks! It sounds like a source tarball with all of the source could still be useful then. If I'm feeling up to it, I'll see if I can do it for the next release. :) (Awesome news about Fedora btw!)

---

_Comment by @BurntSushi on 2018-02-18 15:34_

I've added [installation instructions for Ubuntu/Debian via the use of a binary Debian package](https://github.com/BurntSushi/ripgrep/commit/d6748a34457c3bd2eca8f9a731303cfadf8a292a).

---

_Comment by @carlwgeorge on 2018-05-22 13:33_

> Oh I see, interesting. I like the idea of being able to download a source tarball that one can compile without using the Internet. Would it be acceptable to run cargo vendor in CI and produce a tarball from that instead of actually committing all of the deps into the repo?

What is next to move this idea forward?

---

_Comment by @BurntSushi on 2018-05-22 13:41_

> What is next to move this idea forward?

I think someone needs to do the work to add a release artifact. Unfortunately, I don't have a good way of testing this, since PRs, AFAIK, can't create release artifacts. So if someone was going to work on this, they'd need to test it in their own setup.

Note that I cannot add complex release artifacts. Doing releases is extraordinarily painful already because something always seems to fail. Increasing the rate of failures will directly impact the frequency of releases.

---

_Comment by @BurntSushi on 2018-08-05 11:53_

[ripgrep is now in Debian Unstable](https://packages.debian.org/sid/ripgrep)! Thanks @infinity0!

Being ignorant of Debian's and Ubuntu's process here, does anyone know what the next steps are for a package in Debian Unstable making its way into Ubuntu's repositories?

---

_Comment by @Calinou on 2018-08-05 13:12_

> Being ignorant of Debian's and Ubuntu's process here, does anyone know what the next steps are for a package in Debian Unstable making its way into Ubuntu's repositories?

Based on the [Ubuntu 18.10 release schedule](https://wiki.ubuntu.com/CosmicCuttlefish/ReleaseSchedule), it *should* become available in the next Ubuntu release — the Debian Import Freeze is on 2018-08-23. However, it probably won't be backported to Ubuntu 18.04.

Be sure to check [packages.ubuntu.com/ripgrep](https://packages.ubuntu.com/search?keywords=ripgrep) in the future :slightly_smiling_face:

---

_Comment by @infinity0 on 2018-08-05 13:32_

There are some other hurdles as well. Currently the build-dependency tree of ripgrep contains packages that depend on other packages that were not yet uploaded. The reason is because when we package crate X for Debian, we generate packages for all of its features, X+featureY etc. However ripgrep does not require all of these features in order to build, which is why we were already able to package ripgrep for Debian Unstable.

However for the Debian Testing release process, it is not allowed to have a package X+featureY which depends on package Y that does not yet exist, because users cannot install it. Unfortunately this prevents packages X+featureY' (for all Y') from getting into Debian Testing, because they are all built from the same source package rust-X. So to have ripgrep in Debian Testing, we need to package some more rust packages until we have the full transitive closure of all features.

Presumably Ubuntu has a similar requirement.

For more details see the [Debian rust packaging policy](https://wiki.debian.org/Teams/RustPackaging/Policy) which explains the distinction between source vs binary packages, build-dependencies vs runtime-dependencies, etc.

---

_Comment by @BurntSushi on 2018-08-27 22:48_

@infinity0 How do we determine which version of Ubuntu will package ripgrep? I think I thought it might show up here: https://packages.ubuntu.com/search?keywords=ripgrep&searchon=names&suite=cosmic&section=all

---

_Comment by @okdana on 2018-08-27 23:24_

Here's the source package, it's in proposed:

https://launchpad.net/ubuntu/+source/rust-ripgrep

Cosmic's feature/import freeze was last week, so it should have just barely made it in, but judging by the logs i guess there are build-chain problems?

It's still possible to get it in as an exception up until beta freeze (end of September), here's the documentation for requesting that:

https://wiki.ubuntu.com/FreezeExceptionProcess#FeatureFreeze_for_new_packages

If there are like infrastructure issues or whatever it might be more complicated though, idk

(ETA: I guess those issues are probably the ones mentioned above)

---

_Comment by @infinity0 on 2018-08-28 03:32_

Despite it being "proposed" by an automatic system I'd imagine it will fail further checks down the line, for the same reasons I mentioned earlier. If you don't understand what I said I can explain it in more detail.

Nobody in Ubuntu is doing Rust packaging right now, it's all coming from Debian and is automatically imported. https://qa.debian.org/developer.php?login=pkg-rust-maintainers@alioth-lists.debian.net You'll notice all the rows that say "Excuse" (i.e. why that package is not in Debian Testing) are the same rows where the Ubuntu column has no corresponding package, or has an older version than the latest in Debian.

The "Excuses" pages list the specific reasons why certain packages aren't yet in Debian Testing, which is exactly what I said above but applied to specific packages.


---

_Comment by @BurntSushi on 2018-08-28 10:27_

@infinity0 Ah I see, thanks! That clarifies things!

---

_Comment by @infinity0 on 2018-09-08 04:07_

Blocked on https://github.com/tapeinosyne/hyphenation/issues/12

Everything else is straightforward

---

_Comment by @infinity0 on 2018-09-08 04:23_

Oh I forgot, another issue is https://github.com/hsivonen/simd/issues/25

This might affect ripgrep in the future because it's one of its dependencies. Right now we're compiling the default feature-set on Debian, which currently has no simd acceleration. If you want to add this to the default feature-set, the build will break unless you condition this for particular architectures.

I also notice for cargo-deb you're defining `features = ["pcre2"]` under `[package.metadata.deb]`. Since this is a `cargo-deb`-specific section I don't feel comfortable reading it, but if you want to add a "debian" feature-set I can have our tooling read and use that.


---

_Comment by @BurntSushi on 2018-09-08 13:53_

@infinity0 RE `hyphenation`: I've moved the issue up the dependency tree: https://github.com/mgeisler/textwrap/issues/148

RE simd: I don't think ripgrep will ever include it in its default feature set because it only works on nightly Rust. Is there an unstated blocker here, or is the current situation OK?

RE metadata: The only reason that the `[package.metadata.deb]` section exists is because that's what the `cargo deb` tool demands. So I think we should only add a `[package.metadata.debian]` section if your tooling demands it. In particular, I think "pcre2 is an optional dependency" is already expressed by the existing `[features]` section. Or is there something else I'm missing here?

---

_Comment by @joshtriplett on 2018-09-08 16:04_

> RE simd: I don't think ripgrep will ever include it in its default feature set because it only works on nightly Rust.

What about automatic support for the subset of simd that std includes directly now?

---

_Comment by @BurntSushi on 2018-09-08 16:10_

> What about automatic support for the subset of simd that std includes directly now?

That would be ideal, yes. But it's not up to me. It's up to the maintainer of encoding_rs.

---

_Comment by @infinity0 on 2018-09-09 17:34_

> RE simd: I don't think ripgrep will ever include it in its default feature set because it only works on nightly Rust. Is there an unstated blocker here, or is the current situation OK?

It just means it's harder for other people to take advantage of it. Not only do we have to compile with "--features simd-accel" but we have to condition this on the architectures this actually works on. This logic really should be part of the crates' Cargo.tomls themselves, package maintainers shouldn't be burdened with having to figure this out. Specifically encoding_rs/simd are the crates at issue here, but it affects ripgrep via dependencies.

> In particular, I think "pcre2 is an optional dependency" is already expressed by the existing [features] section. Or is there something else I'm missing here?

I thought `features [ "pcre2" ]` in `[package.metadata.deb]` meant "the Debian package should compile with `--features pcre2`". We're not doing that with debcargo at the moment, for any binary crate. My point was how to let crate developers specify which features they want us to build for the Debian package, if not the default feature set. I was suggesting just adding a "debian" feature for that purpose, rather adding that to a tool-specific section.


---

_Comment by @infinity0 on 2018-09-09 17:36_

BTW ripgrep has already entered Debian Testing due to a limitation in the release scripts

- https://alioth-lists.debian.net/pipermail/pkg-rust-maintainers/2018-September/002574.html
- https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=903211#10

Ubuntu may have stricter checks without these bugs though.


---

_Comment by @infinity0 on 2018-09-09 17:48_

> ripgrep has already entered Debian Testing

To clarify, I'm pretty sure this won't be allowed when Debian Testing actually gets released as Debian Buster, so we still need to e.g. find a solution to the hyphenation issue.

---

_Comment by @BurntSushi on 2018-09-09 21:18_

> e.g. find a solution to the hyphenation issue

Yeah it sounds like the textwrap maintainer is amenable to getting this straightened out, so I think we should be good here. Is there a timeline I should be aware of here?

> It just means it's harder for other people to take advantage of it. Not only do we have to compile with "--features simd-accel" but we have to condition this on the architectures this actually works on. This logic really should be part of the crates' Cargo.tomls themselves, package maintainers shouldn't be burdened with having to figure this out. Specifically encoding_rs/simd are the crates at issue here, but it affects ripgrep via dependencies.

My problem with this whole thing generating more work is that the `simd` crate was, and for the foreseeable future, will be an experimental nightly only thing. The Rust ecosystem is moving through a transition period in how we handle SIMD, so there are growing pains here. The thing you desire is exactly where the ecosystem should be headed towards. For example, the regex crate used by ripgrep will auto-configure the use of SIMD at runtime, so that nobody but that code needs to worry about platform support. Unfortunately, some of ripgrep's dependencies (`encoding_rs` and `bytecount`, namely) haven't moved to this new model yet, but AFAIK, that is the _goal_. So for the time being, SIMD support in Rust is split between runtime support (auto-configured) and compile time support (burden pushed to the person running `cargo build`). I think I agree with you that even compile time support should be handled better, but there's nothing on the roadmap for that AFAIK and nobody has even thought to work on it. Probably because runtime detection is so much nicer when possible.

In other words, if it were up to me, I'd probably forget about packaging optional nightly only Rust features. (For ripgrep, that includes both `simd-accel` and `avx-accel`.) In particular, since I imagine you're only packaging stable Rust, and stable Rust cannot build ripgrep with either the `simd-accel` or `avx-accel` features anyway, I don't think I can even conceptualize how those features would be used in the Debian package ecosystem.

> I thought features `[ "pcre2" ]` in `[package.metadata.deb]` meant "the Debian package should compile with --features pcre2". We're not doing that with debcargo at the moment, for any binary crate. My point was how to let crate developers specify which features they want us to build for the Debian package, if not the default feature set. I was suggesting just adding a "debian" feature for that purpose, rather adding that to a tool-specific section.

I see. So let me try to play this back to you... What you want to know is whether, when someone does, `apt-get install ripgrep`, whether that should automatically come with PCRE2 support or not? My feeling there is that *yes*, it should, because it's better for users. So I think what you're saying is that you'd like to see a signal for that? So is this all you would want?

```toml
[package.metadata.debian]
features = ["pcre2"]
```

Do you need/want any of the other metadata that's in the `package.metadata.deb` section?

I'm so sorry that I don't understand Debian's policy better. I'm sure that's part of the cause of what's making this difficult. I really really appreciate your time and patience with me as we figure this out. :-)

---

_Comment by @joshtriplett on 2018-09-09 23:20_

@BurntSushi Regarding SIMD: ideally the `encoding_rs` and `bytecount` crates will switch to the new SIMD model that works with stable Rust, and until then I don't think it makes sense for Debian to attempt to use those features. I'm assuming that once that happens, we won't need a `simd` feature at all, and SIMD will Just Work?

Regarding pcre2: I think @infinity0 was suggesting that you could just have a `debian` feature available, and make that feature depend on `pcre2`, then have `package.metadata.deb` use that same feature (or teach `cargo-deb` to use such a feature automatically). We could then teach debcargo to look for a `debian` feature and enable it by default if found. In general, we'd always prefer autodetection over configuration when possible. :)

(I'm also hoping one day we can integrate support for things like manpages and completions into cargo so you don't have to do something specific to `cargo-deb` to install them.)

---

_Comment by @BurntSushi on 2018-09-09 23:39_

> Regarding SIMD: ideally the encoding_rs and bytecount crates will switch to the new SIMD model that works with stable Rust, and until then I don't think it makes sense for Debian to attempt to use those features. I'm assuming that once that happens, we won't need a simd feature at all, and SIMD will Just Work?

That is my understanding, yes. This is already true for `regex`. `bytecount` is [in the process](https://github.com/llogiq/bytecount/pull/44) of moving to that model. I know less about `encoding_rs` though and what their specific plans are regarding runtime dispatch of optimized SIMD routines (cc @hsivonen). I know they want to moved to [the portable SIMD API, which isn't stable yet](https://github.com/rust-lang/rfcs/pull/2366), but I don't actually know what their plans are surrounding runtime dispatch.

w.r.t. to adding a Debian Cargo feature, that seems reasonable, yeah. @infinity0 Does that sound good?

---

_Comment by @infinity0 on 2018-09-10 03:30_

> Yeah it sounds like the textwrap maintainer is amenable to getting this straightened out, so I think we should be good here. Is there a timeline I should be aware of here?

4-5 months is the estimated time for the next Debian Stable judging by the previous few releases. But this deadline includes many potential other issues, including future issues not yet discovered, so getting this one out of the away ASAP would be best.

> [..] Probably because runtime detection is so much nicer when possible.

I see, thanks for explaining the background of SIMD in rust. I agree runtime detection is best, and this should hopefully include graceful fallbacks for architectures like ppc64 that have no SIMD at all. OK then for now I'll just ignore these optional simd features.

> In other words, if it were up to me, I'd probably forget about packaging optional nightly only Rust features. [..] I don't think I can even conceptualize how those features would be used in the Debian package ecosystem.

I just agreed to ignoring the simd-accel feature for now, but in general if absolutely necessary we can enable nightly features for specific crates that require it like the simd crate, simply by setting `RUSTC_BOOTSTRAP=1` during the build.

> w.r.t. to adding a Debian Cargo feature, that seems reasonable, yeah. @infinity0 Does that sound good?

That sounds good yes.

> Do you need/want any of the other metadata that's in the package.metadata.deb section?

No thanks it's OK, it's useful for us to read it as reference, but we have [other ways of doing that](https://salsa.debian.org/rust-team/debcargo-conf/tree/master/src/ripgrep/debian) that fits better with official Debian tooling.


---

_Comment by @igor-raits on 2018-09-10 06:11_

I would be slightly against having "debian" feature, because then we will end up with "fedora", "mageia", "opensuse" and thousand of others. it is really up to distribution to decide which features to use.

said that, IMO the most useful features should be in "default" feature.

---

_Comment by @joshtriplett on 2018-09-10 06:14_

Long-term, is there a plan to enable pcre2 in `default`? Or is it likely to always be non-default due to the additional library dependency?

---

_Comment by @BurntSushi on 2018-09-10 09:20_

It will always be non-default because of the additional library dependency.
I'd like to keep the default build pure Rust for now.

Having lots of distro specific features wouldn't be good, I agree.

On Mon, Sep 10, 2018, 02:14 Josh Triplett <notifications@github.com> wrote:

> Long-term, is there a plan to enable pcre2 in default? Or is it likely to
> always be non-default due to the additional library dependency?
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/10#issuecomment-419799555>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34jLIz0nuc9GFxEvN-BYQ_tjD4cmCks5uZgMugaJpZM4KDG1y>
> .
>


---

_Comment by @hsivonen on 2018-09-10 16:52_

> I know less about encoding_rs though and what their specific plans are regarding runtime dispatch of optimized SIMD routines (cc @hsivonen). I know they want to moved to the portable SIMD API, which isn't stable yet, but I don't actually know what their plans are surrounding runtime dispatch.

The plan is not to add run-time dispatch. encoding_rs relies heavily on inlining, and I don't want to add complexity to support hardware that's so old as to be desupported by major browsers.

Instead of turning off SIMD for all 32-bit x86 users, I hope that Debian decides, like Fedora, that supporting non-SSE2 x86 is no longer worthwhile.

The non-NEON case on ARMv7 is a bit less obsolete than the non-SSE2 case on x86. However, ARMv7 support in encoding_rs is primarily for Android where requiring NEON practically excludes only old Tegra devices. The situation for Linux distros isn't ideal.

> fallbacks for architectures like ppc64 that have no SIMD at all.

Do you mean the SIMD support in Rust isn't ready? Surely 64-bit POWER can be trusted to always have SIMD available. Even 32-bit PowerPC had it since G4. (I'm hoping to attempt SIMD-enabling encoding_rs on 32-bit PowerPC once the compiler and standard library support is there.)


---

_Comment by @BurntSushi on 2018-09-10 17:07_

> The plan is not to add run-time dispatch. encoding_rs relies heavily on inlining, and I don't want to add complexity to support hardware that's so old as to be desupported by major browsers.

That's unfortunate. That means the only way users of ripgrep will get encoding_rs's SIMD optimizations is by enabling compile time flags. (Which is the case today, but I figured Rust's support for runtime detection of CPU features would cause folks to use it so that >SSE2 optimizations can be enabled automatically on CPUs that support it.)

---

_Comment by @BurntSushi on 2018-09-10 17:23_

@hsivonen Ah, apologies! I retract my previous complaint. `encoding_rs` appears to only use `sse2` on Intel, so I guess there is no runtime dispatch required on the platform I care most about (x86_64). :-)

---

_Comment by @infinity0 on 2018-09-11 03:36_

> I would be slightly against having "debian" feature, because then we will end up with "fedora", "mageia", "opensuse" and thousand of others. it is really up to distribution to decide which features to use.

> It will always be non-default because of the additional library dependency. I'd like to keep the default build pure Rust for now.
> Having lots of distro specific features wouldn't be good, I agree.

It sounds like you're reserving the "default" feature to avoid installing extra stuff, how about a standard name like "default+packaged" for distros / package managers that can expect to package everything?

> Instead of turning off SIMD for all 32-bit x86 users, I hope that Debian decides, like Fedora, that supporting non-SSE2 x86 is no longer worthwhile.
>
> The non-NEON case on ARMv7 is a bit less obsolete than the non-SSE2 case on x86. However, ARMv7 support in encoding_rs is primarily for Android where requiring NEON practically excludes only old Tegra devices. The situation for Linux distros isn't ideal.

All of this is fine, my point is that this should be clearly documented in the rust-simd crate - so crates depending on rust-simd, such as encoding-rs and all of its dependents like ripgrep, can supply the correct `cfg` conditionals in their Cargo.toml. Or if they don't, distros like Debian have enough documentation to patch it in so that builds don't break on ppc64 etc. Currently I really have no idea, I have to guess by enabling `--feature simd+accel` on `ripgrep` and figure out whether build failures due to `encoding_rs`, on each of the 10 Debian architectures that have rust, are "my fault" or "your fault".

(edit: then some build failures are even invisible, e.g. some of our x86 autobuilders have sse2 and some don't, and then it's up to `cargo test` to actually exercise these instructions which we don't do by default due to the possibility of dev-dependency loops)

---

_Comment by @infinity0 on 2018-09-11 03:45_

> Do you mean the SIMD support in Rust isn't ready? Surely 64-bit POWER can be trusted to always have SIMD available. Even 32-bit PowerPC had it since G4.

To clarify this point, which I may not have made clear enough in my previous point - I think rust's own SIMD does work on ppc64 but some crates are using your crate `simd` as a dependency, which [doesn't build on ppc64](https://buildd.debian.org/status/fetch.php?pkg=rust-simd&arch=ppc64&ver=0.2.2-1&stamp=1536046860&raw=0). However neither `ripgrep` nor `encoding_rs` give any architecture conditionals in their `Cargo.toml`. If I wanted to experiment with enabling `simd+accel` for `ripgrep` in Debian, I have to figure out these `cfg` conditionals myself. But this information is not obvious and should be documented by `simd`. For example I had no idea it required sse on 32-bit x86 since the [build worked on Debian 32-bit x86](https://buildd.debian.org/status/fetch.php?pkg=rust-simd&arch=i386&ver=0.2.2-1&stamp=1535879876&raw=0), probably because that particular builder happened to support sse, and I generally don't investigate successful builds.


---

_Comment by @hsivonen on 2018-09-11 05:12_

> Ah, apologies! I retract my previous complaint. encoding_rs appears to only use sse2 on Intel, so I guess there is no runtime dispatch required on the platform I care most about (x86_64). :-)

Right. encoding_rs uses very basic 128-bit SIMD, so the kind of SIMD it needs is always present on x86_64 and aarch64.

The flag is there just because SIMD still requires nightly. Once `std::simd` graduates to the release channel, the plan is to not require any feature flag and to instead look at target features only, so in the future I expect SIMD to be enabled if targeting x86_64, i686, aarch64 or thumbv7neon/armv7neon and be disabled if targeting i586 or armv7/thumbv7. (i686 and i586 used in the Rust target sense and not in the GCC target sense.)

> some crates are using your crate simd as a dependency, which doesn't build on ppc64

Sorry about that. I added a note to the README of `simd` on trunk. The crate was that way when it was transferred to me and I've tried to keep changes to the minimum while waiting for `std::simd`. (This was already documented in the README for encoding_rs.)

---

_Comment by @sylvestre on 2018-09-14 06:38_

And ripgrep is now also available in Ubuntu https://launchpad.net/ubuntu/+source/rust-ripgrep starting from Cosmic! (18.10)



---

_Comment by @okdana on 2018-09-14 06:56_

btw, i didn't notice before because the file listing on Debian's package site doesn't work, but these packages are [missing](https://packages.ubuntu.com/cosmic/amd64/ripgrep/filelist) the zsh completion function (and the other non-bash ones, i guess). Should i file a bug for that?

ETA: Filed [bug # 908807](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=908807)

---

_Comment by @sylvestre on 2018-09-14 07:05_

Yes, please report that on the Debian bug tracking system


---

_Comment by @BurntSushi on 2018-09-14 13:28_

@sylvestre Woohoo! Awesome!!!

---

_Comment by @michalfita on 2018-12-13 21:01_

In Alpine Linux:
```bash
nikoleta:~# apk add ripgrep
ERROR: unsatisfiable constraints:
  ripgrep (missing):
    required by: world[ripgrep]
nikoleta:~#
```
Not present here: https://pkgs.alpinelinux.org/contents?file=ripgrep&path=&name=&branch=edge&repo=main&arch=x86_64

---

_Comment by @kpcyrd on 2018-12-13 21:48_

@michalfita this is blocked by https://github.com/alpinelinux/aports/pull/1250 which in turn would need better support for musl libc based systems by rust

---

_Comment by @citrus-it on 2019-01-23 15:03_

ripgrep is now available for OmniOS in the official extra repository - https://pkg.omniosce.org/r151028/extra/info/0/ooce%2Ftext%2Fripgrep%400.10.0%2C5.11-151028.0%3A20181208T100739Z

On OmniOS r151028 or above, it's as simple as `pkg install ripgrep`

---

_Comment by @BurntSushi on 2019-01-23 16:58_

@citrus-it A PR that adds it to the README would be great!

---

_Comment by @BurntSushi on 2019-01-27 18:12_

I think ripgrep is now in most or all of the major package repositories that I can personally think of, so I'm going to close this. :tada: 

PRs are of course welcome as ripgrep gets added to other package repositories.

---

_Closed by @BurntSushi on 2019-01-27 18:12_

---

_Comment by @lousyd on 2022-08-02 19:10_

@carlwgeorge , the last comment from you is [here](#issuecomment-390991521), discussing a ripgrep tarball with bundled dependencies in order to circumvent the "with rich dependencies" requirement. Did anything ever move forward on that?

---
