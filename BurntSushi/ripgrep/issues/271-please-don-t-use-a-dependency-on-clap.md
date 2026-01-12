```yaml
number: 271
title: "Please don't use a ~ dependency on clap"
type: issue
state: closed
author: joshtriplett
labels:
  - question
assignees: []
created_at: 2016-12-06T18:21:07Z
updated_at: 2016-12-24T20:44:41Z
url: https://github.com/BurntSushi/ripgrep/issues/271
synced_at: 2026-01-12T16:13:21Z
```

# Please don't use a ~ dependency on clap

---

_@joshtriplett_

ripgrep depends on `clap = "~2.18.0"`, which prevents building it with clap 2.19 or newer.  Clap does not break backward compatibility in minor versions; it only introduces new APIs.  The README for clap does mention using ~ dependencies, but as discussed in https://github.com/kbknapp/clap-rs/issues/765 , that only applies if you need to work with even older versions of Rust than clap's backward-compatibility requirement, which means older than stable-2.  Unless ripgrep has a requirement to run on ancient versions of Rust, please consider using a "compatible" version instead, such as `clap = "2.18"`.

This change would make it much easier to package ripgrep for Linux distributions, by not requiring multiple versions of clap 2.x simultaneously.  (In a Linux distribution, the packaged version of a library will always work with the packaged versions of Rust and Cargo.)

---

_Comment by @BurntSushi on 2016-12-06 18:28_

I was literally the person who pushed for a policy here: https://github.com/kbknapp/clap-rs/issues/740

I think bumping the minimum Rust version required to build a crate is a semver breaking change. Therefore, I have no choice but to use `~2.x`.

Linux distributions are pretty well know for packaging X version of software and not updating it for a very very long time. By being conscientious of our minimum Rust version, we can continue compiling in those environments while still issuing updates.

---

_Comment by @kbknapp on 2016-12-06 18:30_

You beat me to it, I was just about to say the same :wink:

---

_Comment by @joshtriplett on 2016-12-06 19:02_

@BurntSushi I'm not arguing against the policy that for a library crate, bumping the Rust version means bumping the semver minor (or major) number.  I'm just asking if ripgrep actually does have a policy of running on versions of Rust older than stable-2 (the last three stable versions).  If it doesn't, then clap's own policy should suffice there.

Long-term, I think it would make sense to fix Cargo itself to allow declaring a dependency on the minimum version of Rust (and Cargo) required.  Once that becomes widely used, then packages trying to maintain such backward-compatibility could just declare the version of Rust that they need, and people running older versions of Rust and Cargo can run an appropriate older version of a crate that meets that requirement.  For instance, if clap 2.18 declared that it needed Rust 1.X, and clap 2.19 declared that it needed Rust 1.Y with Y>X, ripgrep could declare itself compatible with clap 2.18, and Cargo running with Rust 1.X would determine that it needed to use clap 2.18 rather than clap 2.19.

---

_Comment by @kbknapp on 2016-12-06 19:02_

Also, let me elaborate; the root of the issue (IMO) is package managers sticking with old versions of Rust. Asking the package managers to stay with more up to date version is probably a losing battle. So since the issue won't be fixed at *that* level, we're down to the next level of lib versioning in the Rust ecosystem.

Just so that everyone is aware, I agree bumping the minimum version of Rust is a *change* and even potentially breaking (*but* only in the technical sense, due to not being able to update the version of Rust). Where I disagree is pragmatically speaking. Every time you bump the major version you risk a fracture in the ecosystem. Also, if it turns out that the "breaking change" was merely a bump in the minor version of Rust, no big deal, right? I believe that also slightly conditions consumers to either not worry about major version bumps (i.e. `lib = "*"`) which is equally as catastrophic, or they risk being several major versions behind due to "rapid" major version bumps (i.e. being on 2.x when the current is 5.x due to a mix of *actual* breaking changes and minor items like minimum Rust version bumps) which could make updating to current tricky.

Another thing I personally believe this leads to is libs sticking to pre-1.0 to avoid this altogether. IMO semver should really be a four number system `major.minor.feature.patch` or something similar but that's a different topic entirely.

This lead to me picking the policy clap currently uses. If the minimum version of Rust gets bumped, the minor version is bumped. Is this perfect? No, not at all.

If the Rust ecosystem starts going with the updating the minimum version of Rust is a major version bump, then clap will do the same. I'd like to actually see this though, as it's easy to say but more difficult to actually *do*. With most major libs still pre-1.0 it's hard to say how this will play out (12 of the top 100 crates [by download] are >= 1.0).

----

Having said all this, I don't want it being confused - if claps policy is actively detrimental for it's consumers it's *absolutely* possible to change if it doesn't break existing code. I'm not married to a particular policy, other than serving the consumers.

@joshtriplett which package managers are you speaking about specifically? For my own curiosity.

---

_Comment by @BurntSushi on 2016-12-06 20:57_

@kbknapp To be clear, I don't think clap's policy is necessarily wrong either. It's a reasonable choice to make. I basically agree completely with your analysis. I think the *fracture* part is more significant for libraries like `libc` where types are shared across crates. Basically, whenever `libc` gets a semver bump, the entire ecosystem comes crashing down. IIRC alexcrichton has some ideas on how to make that process smoother, but I don't know how it will all work.

---

In an ideal world, everyone would always be able to update their Rust compiler to the latest. If there were no roadblocks to that process, then we probably wouldn't need to make too much of a fuss about this issue. In practice though, Linux distros and companies that need to approve all software end up falling *way* behind. How do we support those users? The only way I know to do it is to keep publishing releases of applications and libraries that work on those older versions. The only way to do *that* is to have a conscientious policy regarding the minimum Rust version required for building a crate (application *or* library).

ripgrep doesn't have a specific Rust version that it needs to support. Not yet, anyway. If ripgrep ever gets into the Ubuntu repos, then I expect there will be a lot of pressure to support whatever version of Rust that it bundles. If that time came, and I cave to that pressure, then I'll need to be darn sure that all of ripgrep's dependencies continue compiling on whatever version of Rust that Ubuntu uses.

With all that said, I could give up on that dream completely. (For example, what if Ubuntu packages Rust before we get SIMD?) What are the alternatives? Setting up my own ppa and going through all this works to package ripgrep? Blech. It isn't attractive.

@joshtriplett Anyway, sorry for the stream of consciousness thoughts here... If you have any other insights to add, I'd be really happy to hear them!

---

_Comment by @joshtriplett on 2016-12-06 20:58_

@kbknapp The policy makes perfect sense. I just think it's worth telling consumers of clap to distinguish between "I want any version compatible with the features I use, without any breaking API changes" (which should use a "compatible" version requirement), and "I need to support versions of Rust older than clap's "last three stable versions" policy" (which should use a ~ version requirement).  Consumers of clap for whom "last three stable versions" suffices can stick with "compatible" to make Linux distributors' lives easier.

This arose based on work to package Rust applications for Debian, which includes packaging clap and other libraries those applications depend on.  I couldn't build the latest ripgrep with the latest clap; I had to package an older version of clap. That would cause a problem if multiple applications had different dependencies on clap.  (Our packaging supports having multiple incompatible versions of a library packaged simultaneously, but not multiple compatible versions.)

It seems like the problem here is that applications like ripgrep have an excessively strong version constraint, not to declare what they work with but to declare what they want.  It almost seems like this would benefit from a distinction between "compatible with" and "preference"; ripgrep wants to *prefer* the oldest available clap that works, but it would work with newer clap, despite what the version requirement says.

---

_Comment by @joshtriplett on 2016-12-06 21:03_

@BurntSushi I would suggest that the problem is trying to support ripgrep from "cargo install ripgrep" on a distribution's old version of rust; I don't think you need to put a large amount of effort into supporting that case, *once we get a robust ecosystem for packaging Rust applications*.  The current situation arises because distributions have figured out how to package Rust and Cargo but not how to package applications.  (I'm working on fixing that right now, hence issues like these.)

Once distributions can package Rust applications, then they can package appropriate versions of Rust, dependencies, and ripgrep that all work together; that becomes their job to integrate.  Their "stable" distributions will have an older Rust and Cargo, but they'll also have an older ripgrep that works with those.  If they want to package a newer version of ripgrep, they can either package a newer version of Rust and Cargo to go with it, *or* they can do the necessary work to make it work on an older version (which will become almost trivially easy if Cargo supports declaring dependencies on versions of Rust and Cargo).  And if individual people want to install a newer ripgrep, they can install a newer Rust and Cargo too (rustup makes that easy).

---

_Comment by @BurntSushi on 2016-12-06 22:08_

> but they'll also have an older ripgrep that works with those

That is a pretty compelling point.

I'd like to noodle on this. Perhaps my current approach is just bad.

---

_Comment by @joshtriplett on 2016-12-06 22:11_

@BurntSushi I want to emphasize that I *don't* think your current solution is bad; I think it makes sense if you need to support distro-packaged Rust with latest-unpackaged ripgrep.  I just think that case itself arises as an artifact from not having a good story for Rust application packaging, and the problem goes away as part of fixing that.

---

_Comment by @BurntSushi on 2016-12-06 22:17_

@joshtriplett Gotya. I think the reason why I found your point compelling is that if a distro is going to insist on packaging an old version of Rust, then it seems reasonable to expect that they must also package an old version of ripgrep. For some reason, this notion just had never quite occurred to me.

But OK, good point, maybe it isn't bad, but maybe it is a bit too ambitious? :P

---

_Label `question` added by @BurntSushi on 2016-12-12 11:59_

---

_Closed by @BurntSushi on 2016-12-24 15:00_

---

_Comment by @BurntSushi on 2016-12-24 15:01_

I decided to remove the `~` dependency for now. I was personally swayed by @joshtriplett's arguments here. If this does wind up becoming a problem, we can revisit it.

---

_Comment by @joshtriplett on 2016-12-24 20:36_

Thank you!

---

_Comment by @kbknapp on 2016-12-24 20:40_

I don't plan on bumping the minimum version of rust prior to claps next major version bump, but if it ever does happen I don't mind pinging you directly before it happens :wink:

---

_Comment by @BurntSushi on 2016-12-24 20:44_

@kbknapp I am watching the clap repo. But otherwise, CI will at least catch it on my end.

---

_Comment by @BurntSushi on 2016-12-24 20:44_

Thanks though! :-)

---
