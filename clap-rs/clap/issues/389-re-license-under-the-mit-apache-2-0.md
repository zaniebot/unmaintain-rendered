```yaml
number: 389
title: Re-license under the MIT/Apache 2.0
type: issue
state: closed
author: kbknapp
labels:
  - A-docs
assignees: []
created_at: 2016-01-26T14:13:25Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/389
synced_at: 2026-01-12T16:14:09Z
```

# Re-license under the MIT/Apache 2.0

---

_@kbknapp_

# Note from @kbknapp

Although I'm not a huge fan of dual licensing, I do see a big benefit in following
the Rust trend here, and being compatible with that ecosystem.

Also please note this was **not** part of the mass auto-generated issues. I opened this issue because I think it's a good idea for this project. I _did_ however copy/paste the verbiage from the auto-generated issues.
- Kevin

---

TL;DR the Rust ecosystem is largely Apache-2.0. Being available under that
license is good for interoperation. The MIT license as an add-on can be nice
for GPLv2 projects to use your code.
# Why?

The MIT license requires reproducing countless copies of the same copyright
header with different names in the copyright field, for every MIT library in
use. The Apache license does not have this drawback. However, this is not the
primary motivation for me creating these issues. The Apache license also has
protections from patent trolls and an explicit contribution licensing clause.
However, the Apache license is incompatible with GPLv2. This is why Rust is
dual-licensed as MIT/Apache (the "primary" license being Apache, MIT only for
GPLv2 compat), and doing so would be wise for this project. This also makes
this crate suitable for inclusion and unrestricted sharing in the Rust
standard distribution and other projects using dual MIT/Apache, such as my
personal ulterior motive, [the Robigalia project](https://robigalia.org).

Some ask, "Does this really apply to binary redistributions? Does MIT really
require reproducing the whole thing?" I'm not a lawyer, and I can't give legal
advice, but some Google Android apps include open source attributions using
this interpretation. [Others also agree with
it](https://www.quora.com/Does-the-MIT-license-require-attribution-in-a-binary-only-distribution).
But, again, the copyright notice redistribution is not the primary motivation
for the dual-licensing. It's stronger protections to licensees and better
interoperation with the wider Rust ecosystem.
# How?

To do this, get explicit approval from each contributor of copyrightable work
(as not all contributions qualify for copyright, due to not being a "creative
work", e.g. a typo fix) and then add the following to your README:

```
## License

Licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any
additional terms or conditions.
```

and in your license headers, if you have them, use the following boilerplate
(based on that used in Rust):

```
// Copyright 2016 clap-rs Developers
//
// Licensed under the Apache License, Version 2.0, <LICENSE-APACHE or
// http://apache.org/licenses/LICENSE-2.0> or the MIT license <LICENSE-MIT or
// http://opensource.org/licenses/MIT>, at your option. This file may not be
// copied, modified, or distributed except according to those terms.
```

It's commonly asked whether license headers are required. I'm not comfortable
making an official recommendation either way, but the Apache license
recommends it in their appendix on how to use the license.

Be sure to add the relevant `LICENSE-{MIT,APACHE}` files. You can copy these
from the [Rust](https://github.com/rust-lang/rust) repo for a plain-text
version.

And don't forget to update the `license` metadata in your `Cargo.toml` to:

```
license = "MIT OR Apache-2.0"
```

I'll be going through projects which agree to be relicensed and have approval
by the necessary contributors and doing this changes, so feel free to leave
the heavy lifting to me!

# Contributor checkoff

All contributors with more than 100 lines of changes need to agree to relicensing for this to take pace. Please comment with :

```
I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.
```

Or, if you're a contributor, you can check the box in this repo next to your name. My scripts will pick this exact phrase up and check your checkbox, but I'll come through and manually review this issue later as well.

- [x] @kbknapp 
- [x] @Arnavion 
- [x] @afiune 
- [x] @alex-gulyas 
- [x] @AluisioASG  
- [x] @archer884 
- [x] @Bilalh 
- [x] @birkenfeld 
- [x] @bradurani 
- [x] @brennie  
- [x] @brianp 
- [x] @bluejekyll 
- [x] @Byron 
- [x] @cstorey 
- [x] @cite-reader
- [x] @crazymerlyn 
- [x] @davidszotten  
- [x] @dguo 
- [x] @grossws 
- [x] @Geogi
- [x] @gohyda  
- [x] @hgrecco 
- [x] @huonw 
- [x] @hoodie 
- [x] @idmit 
- [x] @iliekturtles 
- [ ] @james-darkfox  
- [x] @jespino 
- [x] @jimmycuadra 
- [x] @japaric 
- [x] @joshtriplett 
- [x] @Keats 
- [x] @little-dude 
- [x] @malbarbo 
- [x] @mgeisler 
- [x] @messense 
- [x] @N-006 
- [x] @nabijaczleweli 
- [x] @nagya11
- [ ] @nateozem 
- [x] @Nemo157  
- [x] @nelsonjchen 
- [x] @nicompte 
- [x] @nvzqz 
- [x] @ogham 
- [x] @panicbit 
- [x] @peppsac 
- [x] @psyomn 
- [x] @rnelson 
- [x] @rtaycher
- [ ] @segevfiner 
- [x] @SirVer  
- [x] @sru 
- [x] @SShrike 
- [x] @starkat99  
- [x] @swatteau 
- [ ] @tanakh 
- [x] @th4t 
- [x] @tormol   
- [x] @untitaker  
- [x] @Vinatorul 
- [ ] @wdv4758h 
- [x] @willmurphyscode 

Edit 2018-02-15: Updated list of contributors meeting requirements as of today. I also left names of those with less than 100 lines of changes that have already signed off.

---

_Label `T: RFC / question` added by @kbknapp on 2016-01-26 14:15_

---

_Label `C: docs` added by @kbknapp on 2016-01-26 14:15_

---

_Label `P3: want to have` added by @kbknapp on 2016-01-26 14:15_

---

_Comment by @grossws on 2016-01-26 14:32_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @untitaker on 2016-01-26 14:44_

I strongly doubt this has any effect at all. It's FUD IMO


---

_Comment by @psyomn on 2016-01-26 14:49_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

Cheers!


---

_Comment by @kbknapp on 2016-01-26 15:04_

@untitaker Perhaps. And to be honest it really doesn't matter to me either way, but if people would rather use Apache 2.0 vs MIT (and there appears to be quite a few in the Rust community) I don't have any objections to letting them.

Either way `clap` is entirely open source from my optic :stuck_out_tongue_winking_eye: 


---

_Comment by @archer884 on 2016-01-26 15:05_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @sru on 2016-01-26 15:12_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @dywedir on 2016-01-26 17:30_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @nelsonjchen on 2016-01-26 18:03_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @birkenfeld on 2016-01-26 18:05_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @rnelson on 2016-01-26 18:07_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @idmitrievsky on 2016-01-26 18:08_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @Keats on 2016-01-26 18:23_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @bradurani on 2016-01-26 18:43_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @Byron on 2016-01-26 19:36_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @alex-gulyas on 2016-01-26 19:53_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @jimmycuadra on 2016-01-26 22:43_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @untitaker on 2016-01-26 22:48_

I license past and future contributions under the dual MIT/Apache-2.0 license,
allowing licensees to chose either at their option.


---

_Comment by @huonw on 2016-01-26 23:14_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @messense on 2016-01-27 02:42_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @ogham on 2016-01-27 15:39_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @severen on 2016-01-28 08:22_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @Geogi on 2016-01-29 14:42_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Referenced in [clap-rs/clap#409](../../clap-rs/clap/pulls/409.md) on 2016-02-02 18:23_

---

_Comment by @noncrab on 2016-02-02 19:34_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Referenced in [clap-rs/clap#411](../../clap-rs/clap/pulls/411.md) on 2016-02-04 07:15_

---

_Comment by @cite-reader on 2016-02-04 19:05_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @japaric on 2016-02-24 16:40_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @vsupalov on 2016-03-08 13:44_

I license past and future contributions under the dual MIT/Apache-2.0
license, allowing licensees to chose either at their option.


---

_Comment by @hgrecco on 2016-04-18 00:09_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @panicbit on 2016-04-18 00:09_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @Bilalh on 2016-04-18 00:09_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @rtaycher on 2016-04-18 05:19_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @SirVer on 2016-04-18 08:47_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @tormol on 2016-10-04 18:06_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @nabijaczleweli on 2016-10-04 18:14_

Do I apply for posting the magic formula here as well? Here's my ["past contributions"](https://github.com/kbknapp/clap-rs/pulls?utf8=%E2%9C%93&q=author%3Anabijaczleweli%20is%3Apr), FTR


---

_Comment by @kbknapp on 2016-10-04 21:21_

@nabijaczleweli absolutely! I just haven't updated this list in a while. Doing so now :smile: 


---

_Comment by @nabijaczleweli on 2016-10-04 21:21_

Jolly good, then.

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @nabijaczleweli on 2016-10-04 21:34_

@kbknapp You might've updated the list but you fucked up indentation real bad :P


---

_Comment by @kbknapp on 2016-10-04 21:34_

Yup, it's fixed


---

_Comment by @nabijaczleweli on 2016-10-04 21:35_

Dope :+1:


---

_Comment by @jespino on 2016-10-04 21:38_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @afiune on 2016-10-04 21:41_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

Fantastic! 🤘🏻


---

_Comment by @joshtriplett on 2016-10-04 21:43_

For clap-rs:
I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @brianp on 2016-10-04 22:08_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @VoidStarKat on 2016-10-04 22:42_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @dguo on 2016-10-04 23:59_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @nvzqz on 2016-10-05 01:48_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @freshstrangemusic on 2016-10-05 01:49_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @nicompte on 2016-10-05 05:26_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @hoodie on 2016-10-05 07:29_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @iliekturtles on 2016-10-05 12:01_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @benceszigeti on 2016-10-05 12:03_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @AluisioASG on 2016-10-05 16:08_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @davidszotten on 2016-10-05 20:05_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @swatteau on 2016-10-07 15:54_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.


---

_Comment by @tormol on 2016-10-14 19:01_

Would it be possible to require that all contributions from now on are dual-licensed?
Then the list won't have to be updated with new contributors.


---

_Comment by @kbknapp on 2016-10-18 15:29_

@tormol I'm not a lawyer, so take with a grain of salt, but I don't think so. Only because future contributions may be based on old contibutions that haven't been dual licensed yet by the author.

Granted, I could go back through the list of contributors and see if any of the non-replies are from trivial contributions (i.e. spelling corrections), but I haven't done that yet.

Also, whenever we reach the 3.x milestone I _may_ re-licenses that as a "derivative" work of the 2.x branch using dual licenses for the 3.x since the MIT allows relicensing derivatives. Technically, I could probably do that with any "future version" but don't want to go that route unless it comes to it. Hence, the 3.x milestone seems to be reasonable.


---

_Referenced in [clap-rs/clap#738](../../clap-rs/clap/issues/738.md) on 2016-11-13 13:22_

---

_Referenced in [AlexPikalov/cdrs#84](../../AlexPikalov/cdrs/issues/84.md) on 2017-02-17 01:18_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:51_

---

_Comment by @little-dude on 2018-02-15 16:29_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

For some reason I missed this thread earlier, sorry.

---

_Comment by @kbknapp on 2018-02-15 17:36_

No worries! Thanks 👍 

I'm just trying to revive this so I can dual license v3. Granted I'm not a lawyer, and it's probably possible to dual license v3 *anyways* but I figured let's try and do it the easy way first.

---

_Comment by @mgeisler on 2018-02-15 18:44_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @Nemo157 on 2018-02-15 19:17_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @Arnavion on 2018-02-15 19:20_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @bluejekyll on 2018-02-15 20:23_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @crazymerlyn on 2018-02-16 03:08_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @peppsac on 2018-02-16 18:54_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @malbarbo on 2018-02-16 20:37_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Comment by @kbknapp on 2018-02-16 21:53_

Thanks for all the replies thus far! 👍 

---

_Comment by @willmurphyscode on 2018-02-18 10:03_

I license past and future contributions under the dual MIT/Apache-2.0 license, allowing licensees to chose either at their option.

---

_Closed by @kbknapp on 2018-07-23 18:26_

---
