```yaml
number: 1019
title: "RFC: do you care which version of the Rust compiler ripgrep targets?"
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2018-08-20T12:13:06Z
updated_at: 2020-05-01T12:29:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1019
synced_at: 2026-01-12T16:13:22Z
```

# RFC: do you care which version of the Rust compiler ripgrep targets?

---

_@BurntSushi_

In this issue, I'm asking for feedback from any user, and I'm particularly interested in getting feedback from folks that maintain ripgrep in some package repository.

What I'd like to know is: do you care which version of the Rust compiler ripgrep targets, and if so, how does it impact you?

There are at least a couple different aspects to this question. For example:

* That we document a minimum Rust compiler version at all.
* That we support Rust compilers older than a few months.

My policy for this has been relatively ad hoc, but fairly conservative in the context of the Rust ecosystem.  I've mostly done this out of "good sense," in that it seems better for ripgrep to compile on more versions of the Rust compiler than not. However, what I'd like to know is whether it's worth doing this. For example, if ripgrep changed to "only guaranteed to compile on the latest stable release of the Rust compiler," how would that impact you?

What I'm trying to figure out here is what kind of room I have to maneuver. For example, sometime around the end of the year, Rust 2018 will be released, and ripgrep will eventually want to move to that. What I'd like to figure out is _when_ that kind of move would be acceptable, and in order to do that, I need to ask people how the minimum supported Rust version impacts them. So any kind of experience you can offer would be great!

cc @svenstaro @roblourens @dstcruz @joshtriplett @infinity0 @carlwgeorge @ignatenkobrain @cuviper @sylvestre

---

_Label `question` added by @BurntSushi on 2018-08-20 12:13_

---

_Renamed from "feedback wanted: do you care which version of the Rust compiler that ripgrep targets?" to "feedback wanted: do you care which version of the Rust compiler ripgrep targets?" by @BurntSushi on 2018-08-20 12:13_

---

_Comment by @igor-raits on 2018-08-20 12:19_

In Fedora we will always keep latest compiler once it gets release. So "stick to the latest and coolest release"

---

_Comment by @svenstaro on 2018-08-20 12:38_

In Arch we don't care about any compiler but the latest stable. :P

---

_Comment by @tshepang on 2018-08-20 13:18_

Not to speak for Debian, but my guess is that whichever version of rustc is in Testing should be one that is supported, or one in Unstable at worst. Stable does not feature in this decision since the version of ripgrep there will not change, essentially.

---

_Comment by @DoumanAsh on 2018-08-20 15:37_

As long as it targets latest stable I don't care

---

_Comment by @sylvestre on 2018-08-20 16:34_

For debian and Ubuntu, last stable is fine.

For now, I don't think we will backport rg to debian stable. 

---

_Comment by @federicomenaquintero on 2018-08-20 17:44_

For openSUSE, we pick up the latest compiler version.

For SLE (Suse Linux Enterprise), we'll freeze the compiler at the version that the long-term Firefox release will require.  I believe FF60 needs rustc 1.26.2, so that's what we'll use in SLE.

(For librsvg, I'm trying to tie it to the same version that Firefox requires, so that they will both work in the enterprise distro.)

---

_Comment by @cuviper on 2018-08-20 17:54_

Fedora's `rustc` is continuously updated, as mentioned, and I update EPEL7 this way too.  For RHEL proper, we're currently only shipping Rust in the Developer Tools `rust-toolset`, rebased quarterly.

---

_Comment by @BurntSushi on 2018-08-20 18:03_

Thanks for the feedback so far! One of the things I'm interested in is figuring out the ramifications of tracking Rust stable instead of "Rust stable minus N" where N is somewhat loosely determined. I know some of you already mentioned that tracking Rust stable is fine, but for example, @federicomenaquintero, if ripgrep started tracking Rust stable, how would that impact you? Would you be able to just package last version of ripgrep that worked on the version of Rust that you're using?

---

_Comment by @joshtriplett on 2018-08-20 20:57_

Generally speaking, if rustc isn't getting updated then neither will ripgrep. So you don't need to worry too much about the scenario where ripgrep gets updated but the update can't get packaged because rustc doesn't get updated.

The one case where that might cause a problem: if you ever need to put out an urgent bugfix for ripgrep (e.g. a hypothetical security issue parsing untrusted ignore files) then many other considerations for minimal updates and working on older rustc versions would apply.

Apart from that, using new features from the latest stable Rust seems fine in ripgrep itself. (Some of the crates ripgrep uses may have longer-term compatibility requirements.)

---

_Comment by @BurntSushi on 2018-08-20 20:59_

> The one case where that might cause a problem: if you ever need to put out an urgent bugfix for ripgrep (e.g. a hypothetical security issue parsing untrusted ignore files) then many other considerations for minimal updates and working on older rustc versions would apply.

Yeah that's a good one. I feel like if something like that were ever to happen, I'd just cut a branch at whatever version of ripgrep needs the fix and publish a patch release there. I don't really have the infrastructure in place to do it, and I'm not sure if I have the bandwidth to do it either, but I'd be willing to entertain the occasional backport if it's strongly motivated.

---

_Comment by @cuviper on 2018-08-20 21:01_

> Generally speaking, if rustc isn't getting updated then neither will ripgrep.

That's true of the *packaged* ripgrep.  The remaining question is whether you want to support folks using their distro toolchain to `cargo install ripgrep` on their own.

---

_Comment by @BurntSushi on 2018-08-20 21:07_

> That's true of the packaged ripgrep. The remaining question is whether you want to support folks using their distro toolchain to `cargo install ripgrep` on their own.

Yeah, it's becoming clear to me that this will be the key thing that is lost if ripgrep were to switch to "latest stable only" policy. I'm not sure how highly to weight this use case. :-/ I do know that users appreciate being able to `cargo install ripgrep`, but I also feel like those with `cargo` installed are _probably_ writing Rust code, and if they're writing Rust code, they're probably not on an older Rust version or are even using `rustup`. But I'm of course waving my hands here!

---

_Comment by @joshtriplett on 2018-08-20 21:11_

@cuviper At one point, I worried a lot more about that, since packaging Rust software for Debian and other distros felt much further off. But I feel like, now that we have a much better story for how distributions can package software like ripgrep, and people *can* `apt install ripgrep` or equivalent in popular distributions (and other distributions have examples to follow to make that work), I'm a lot less worried about `cargo install ripgrep` building the latest ripgrep on old cargo/rustc. I *especially* don't think people should expect that to work on, say, Debian stable, or testing during a freeze.

I'd still like to see us add metadata to `Cargo.toml` for rustc/cargo dependencies so that we get an early, clear error message for that kind of thing. (And that would also help when generating build dependencies.) But that isn't ripgrep's problem.

---

_Comment by @cuviper on 2018-08-20 21:25_

I do expect that most C and C++ software should be able to build with the system GCC, even on LTS-ish distros.  (Although I admit el7's gcc-4.8.5 is now old enough to be annoying, nevermind el6, but that's why we have devtoolset.) In the long term, I expect that Rust will settle down in a similar way.  For `ripgrep` in particular, it appears to be settling as a standard packaged distro tool too. 😄 

> I'd still like to see us add metadata to Cargo.toml for rustc/cargo dependencies so that we get an early, clear error message for that kind of thing.

Big 👍 

---

_Comment by @joshtriplett on 2018-08-20 21:29_

@cuviper C projects tend to adopt new C features more conservatively, not least of which due to requirements for portability to other compilers; if you need to build on the C compiler from a random proprietary UNIX, you'll probably build on old GCC too. But I've definitely found that "modern C++" software often doesn't build with LTS C++ compilers.

---

_Comment by @Ameobea on 2018-08-20 22:26_

I'm usually nightly all the time, so as long as it doesn't break *every* new nightly release (looking at you clippy) I'm happy.

---

_Comment by @dcarosone on 2018-08-21 10:08_

Nightly on anything where rustup works, and/or pkgsrc on smartos + netbsd

---

_Comment by @BurntSushi on 2018-08-21 15:19_

All righty, based on the feedback I've got so far, I'm going to try switching to a **latest stable** policy for the next release. In detail, it will probably look like this:

* Every patch release will compile on the same version of the Rust compiler as the previous patch release.
* New minor version releases of ripgrep may require a Rust compiler as recent as the latest stable release.

I think this should satisfy most everyone who commented, and should also let us backport patches if that becomes necessary.

If this doesn't work out for some reason or another, we can revert to a more conservative policy, but the benefits of sticking the latest stable release are too big to not try this at all.

@roblourens - I don't think I've heard from you on this, but I think you build your own ripgrep and probably easily control the version of the Rust compiler you use.

I also don't think I heard from any folks using Homebrew, but IIRC, they track the latest stable version of Rust as well.

---

_Comment by @roblourens on 2018-08-21 16:56_

That's right, I usually just update to the latest stable each time I build it.

---

_Closed by @BurntSushi on 2018-08-22 03:05_

---

_Renamed from "feedback wanted: do you care which version of the Rust compiler ripgrep targets?" to "RFC: do you care which version of the Rust compiler ripgrep targets?" by @BurntSushi on 2020-05-01 12:29_

---
