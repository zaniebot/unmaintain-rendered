```yaml
number: 6
title: add support for mercurial
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2016-09-16T22:08:24Z
updated_at: 2024-12-05T21:14:29Z
url: https://github.com/BurntSushi/ripgrep/issues/6
synced_at: 2026-01-12T16:13:21Z
```

# add support for mercurial

---

_@BurntSushi_

Mercurial is widely used enough that we should probably support it. Mercurial will actually be harder to do correctly than git, because an `.hgignore` file can support both regexes and globs. An `.hgignore` file can also specify _subincludes_, which include ignore patterns in a sub-directory (as opposed to `git`, which will read `.gitignore` files in sub-directories automatically).

Thankfully, `ripgrep` translates all globs to regexes, so we should be able to support Mercurial without too much trouble.

See: `hg help hgignore` and `hg help patterns`.


---

_Label `enhancement` added by @BurntSushi on 2016-09-16 22:08_

---

_Comment by @jFransham on 2016-11-29 10:04_

I'll see if I can do some work on this

---

_Label `help wanted` added by @BurntSushi on 2017-03-23 10:59_

---

_Comment by @BurntSushi on 2017-03-23 11:02_

If someone wanted to work on this, I'd be happy to mentor it. I think the code in this portion of ripgrep is isolated well enough that someone could truly focus on just the Mercurial ignore files and not too much else. I would be OK with this being done in multiple PRs. I would also be OK with partial support (e.g., "only `.hgignore` files with globs are supported") so long as there's a clear path to implementing complete support.

The first step would be to add a new `hgignore` submodule to the [`ignore` crate](https://docs.rs/ignore). It should roughly mirror the existing [`gitignore`](https://docs.rs/ignore/0.1.8/ignore/gitignore/index.html) submodule, although `hgignore` itself operates slightly differently than `gitignore`, and that could impact the API.

---

_Comment by @kalekseev on 2017-04-18 08:50_

@BurntSushi I would love to have the mercurial support in the ripgrep, yesterday I compiled a version that ignore globs from a projects .hgignore https://github.com/kalekseev/ripgrep. For now it's mostly copy-paste of gitignore code. I don't have any rust experience so not sure how far I can get with the task but I'm going to try to add support for hgignore specific features in the next couple of weeks and then create a PR.

---

_Comment by @BurntSushi on 2017-04-18 11:43_

@kalekseev Mercurial support would be great. Do note that both regexes and globs are supported, and I'd expect there to be plenty of subtle differences between gitignore and hgignore. So I think there needs to be lots of tests. If Mercurial has a test suite we can borrow from, that'd be great. :-)

---

_Comment by @theor on 2017-04-21 19:34_

apparently this is the whole hgignore test suite : https://www.mercurial-scm.org/repo/hg/file/tip/tests/test-hgignore.t

---

_Comment by @kalekseev on 2017-04-22 21:03_

@theor thanks that will help

---

_Comment by @tamird on 2017-12-15 18:38_

@BurntSushi I'd like to pick this up where #477 left off. Do you have any mentoring notes?

---

_Comment by @kalekseev on 2017-12-15 19:50_

@tamird you can check my branch it's not that hard to update it to requirements that @BurntSushi provided in #477.

As far as I can remember the tricky part was to add support for include/subinclude directives in .hgignore, a year ago ripgrep used some "hacks" to speedup file traversal and it was not easy to pair traversal logic with subinclude.

Therefore I would recommend to start with looking into hgignore format, maybe it would be good idea to release first version with support of only limited subset of hgignore rules (it's better than nothing). I used my version with only home and root hgignore support without problems during that year.

---

_Comment by @BurntSushi on 2017-12-18 13:09_

@tamird I don't think I have any specific technical advice beyond what @kalekseev said, but I will say this: if we can find a small portion of `.hgignore` support to add, then it might be best to start there before launching into complete support. Consider, for example, the following addition that would still be useful:

1. If an `.hgignore` uses regexes, then silently ignore (with perhaps a debug log message).
2. If an `.hgignore` uses globs, then parse it as if it were `.gitignore`. (Perhaps it is possible to reuse `Gitignore` itself. I don't know.)
3. If an `.hgignore` contains subincludes, then silently ignore them (with perhaps a debug log message).

I think this basically lets us support some part of Mercurial without going the whole way, and I expect this to still be useful to some folks while making the initial contribution simpler. One thing we should be careful about is to not do anything that results in incorrect behavior. For example, we should not attempt to read regexes as if they were globs. That is, with respect to ignore matching, we can suffer false negatives (searching files that we shouldn't, which is the status quo) but we should avoid false positives (ignoring files that we should search).

---

_Comment by @kalekseev on 2017-12-18 13:18_

> If an .hgignore uses regexes, then silently ignore (with perhaps a debug log message).

@BurntSushi @tamird regexes already work in my branch though I just reuse it as rust regex without any transformation, we can skip and show warn message when rust regex can't be built from .hgignore regex.

---

_Comment by @BurntSushi on 2017-12-18 13:25_

@kalekseev That's fair. Re-reading my PR review feedback, it looked like there might be some complications there. If it's simpler than I think, then great, let's do it. But I just wanted to lay out the simplest possible path to making a contribution here. :-)

---

_Comment by @kalekseev on 2018-05-07 07:18_

If anyone subscribed to that issue then please test my current implementation with your mercurial projects, thanks.
```
$ git clone git@github.com:kalekseev/ripgrep.git
$ cd ripgrep
$ git checkout hgignore
$ cargo build --release
$  cp target/release/rg ~/.local/bin/rg # change to your PATH dir
```
Things that are not implemented (skipped by ripgrep):
 - Extended mercurial patterns: path, rootfilesin, listfile, listfile0, include, subinclude
 - python specific regexes (that are not valid rust regexes)


---

_Comment by @skrattaren on 2018-05-07 20:04_

@kalekseev 
I've tested with a couple of my own projects (simple glob `hgignore`s) and main Mercurial repo as well (specifically checking some of regex patterns).  Everything seem to work fine, thank you!

P.S. `ag` skips all the regexp patterns ;)
P.P.S. Mercurial project has jumped onto Rust bandwagon, too: https://www.mercurial-scm.org/repo/hg/file/tip/rust

---

_Comment by @BurntSushi on 2018-07-21 17:48_

I hate to do this since I think this feature request is valid and would be beneficial to folks, but after a lot of thought and reflection, I think I'm going to have to pass on this. At least for now. At the end of the day, this comes down to resources that I don't think I have. Let me elaborate.

I am quite sure that if ripgrep adds partial support for Mercurial and advertises it, bug reports will come rolling in expressing dissatisfaction with one of the myriad number of things that we don't support. In particular, I could see us maybe supporting Mercurial's glob syntax, but supporting its regex syntax is just not something I have the bandwidth for because it appears to be tightly coupled to Python's regex engine. That tight coupling is a serious problem for ripgrep, because the only way to actually implement it is to use Python's regex engine itself. From speaking to folks, it sounds like regexes are used often enough in `.hgignore` in the wild that it would be super annoying to ignore it. Given that ripgrep is never going to embed Python's regex engine, I don't see how there is any kind of reasonable path to providing full support for Mercurial.

*Even if* we could live with the above, there is a significant problem: I don't use Mercurial, so the part of ripgrep that supports Mercurial would get absolutely no dogfooding at all. This is the absolute worst situation to be in because in order to pull off partial support like this, someone needs to be all over the nitty gritty details of how ripgrep interacts with Mercurial. The only person I trust to do that is me, and I know I don't have the bandwidth for it.

Initially, I was a bit more gung ho on this feature because I didn't quite realize how hard it would be to get complete or near-complete support for Mercurial. So I was at least OK with _starting_ with partial support. But if complete support forever remains out of reach along with a never ending stream of wontfix bugs, then I don't think I can realistically stomach that.

It's hard for me to imagine a path in which Mercurial support lands in ripgrep (short of me using Mercurial full time, which definitely isn't happening in the immediate future), but I admit it's possible. I think it would necessarily require that someone else shoulder the maintenance burden of it in another repository, and then we would need to work out a low effort way of integrating it into the `ignore` crate. It is also possible that Mercurial support could punt on reading `hgignore` files directly and let Mercurial itself do the heavy lifting, but I don't know the best way to do that, nor do I know if the requisite performance loss would be acceptable.

So for now, I'm going to close this issue along with #733 to realistically communicate that Mercurial support probably isn't going to come to ripgrep any time soon.

---

_Closed by @BurntSushi on 2018-07-21 17:48_

---

_Comment by @durin42 on 2018-07-21 18:00_

It could work to ask mercurial for something like `hg files 'set: not ignored()'`. I work on hg, and if you want to spitball ideas on how we could make this work just let me know.

---

_Comment by @BurntSushi on 2018-07-21 18:15_

@durin42 Thanks for chiming in! It sounds like your input would be quite valuable, but it would probably be better to find a champion for this first such that it can be developed and maintained out-of-tree.

---

_Comment by @kalekseev on 2018-07-22 21:41_

@BurntSushi this is reasonable decision to make, I'm not the one who have time to support mercurial beyond my use cases either but I will try to update my fork from time to time so anyone can compile it.

---

_Comment by @skrattaren on 2018-07-24 20:28_

That's sad, I was really uplifted when @kalekseev submitted his PR.  I have even a couple of TODOs in my *dotfiles* to switch from `ag` when _ripgrep_ supports `hgignore`s.

I'd like to point out that the same `ag` supports `hgignore`s without dogfooding (I am reasonably sure about that).  Also the points about dogfooding and bug reports seems a bit contradictory to me: if you want to find out about problems with `hgignore`, then bug reports are good.  If you don't, not having to deal with incomplete feature yourself is good, too.

Of course, I don't mean that you have to or should support `hgignore` files because I (and a couple of dozen people too, at least) want that feature.  Far from it!  I appreciate your time, expertise and effort, but I'll try to persuade you to change your mind =)

Could we have it as an *unsupported* feature, maybe?  So that one have to compile it with specific flag (`--enable-hgignore` or something)?  Maybe with some warning message in runtime?

Another idea is to print an error about unsupported regexes every time one is encountered.

Regardless, thanks to @BurntSushi for this tool and to @kalekseev for his effort in implementing it.

---

_Comment by @BurntSushi on 2018-07-24 21:04_

@krigstask Thanks for your kind words, and thanks for what appears to be your understanding, but my decision is pretty firm on this point. Someone else needs to champion this feature and shoulder the maintenance burden in a separate repository. That is, when someone invariably files a bug against Mercurial support in ripgrep, I need to be able to close it and redirect them to the Rust crate that is responsible for it.

When I initially wrote out this response to you, I replied to many of your points one-by-one. But that's just digging deeper into the weeds, and I don't have the energy to go there, especially with respect to some of the things you said. So I'll just speak more broadly. Namely, I'm very appreciative of @kalekseev's work on this, and it pains me to close a PR like that after so long. But saying "no" is, socially, one of the hardest things about maintaining a project. It's hard to know when to do it. But when I thought about it, I realized that the handling of ignore rules is one of the most complex parts of ripgrep and, consequently, something that attracts bugs. Once a PR gets merged, it is, ultimately, my responsibility to handle those bugs and maintain the feature (the only thing harder than saying No is removing a previously existing feature). I do not think that's a maintenance burden I can accept. Please understand that many of the maintenance costs are simply not visible to you at all. They range from social, to emotional, to technical.

---

_Comment by @durin42 on 2018-07-24 21:10_

OOC is there a plugin mechanism or something? Or is maintaining a fork the only path forward for now?

(I just don't know - feel encouraged to link me to docs if that makes sense.)

---

_Comment by @BurntSushi on 2018-07-24 21:18_

There's no plugin mechanism. Handling ignore rules is a performance sensitive aspect of ripgrep's execution. Almost all of that work is handled by the [`ignore`](https://docs.rs/ignore) crate, which is maintained in this repository. @kalekseev's approach was to add a new `hgignore` sub-module along side the [`gitignore` sub-module](https://docs.rs/ignore/0.4.2/ignore/gitignore/index.html).

Forking ripgrep is itself one way of doing this. Another approach is to build the `hgignore` support in a crate that we then integrate into `ignore`. It might even start with @kalekseev's PR, if that was an approach you wanted to take. The key point is that bugs filed against ripgrep for hg support will get redirected to a hypothetical `hgignore` repository, which _must_ have an ongoing maintenance strategy.

---

_Comment by @durin42 on 2018-07-24 21:20_

The approach I'd be interested in taking would be implementing only as much as is easy, with a way to say "sorry, this .hgignore is too complicated" when people get, uh, creative. Is that hypothetically viable? I can probably even write the hgignore-parsing code, as long as it can bail out with an informative message when the going gets too tough.

(In general for performance reasons all .hgignores *should* be supportable by Rust's regular expression packages, but there will always be people that do creative things that are slow. Sometimes I'm one of them!)

---

_Comment by @BurntSushi on 2018-07-24 21:30_

@durin42 To adjust your expectations, I don't anticipate any part of this being easy. Maybe I'm wrong, but I'm also the one who has spent a lot of time working on ignore support, and it is _very_ subtle. Deceptively so. Perhaps the existing gitignore logic will help get you started. With that said, I also seem to recall that hgignore files are also handled differently than gitignore in terms of how they are interpreted in the directory tree? I can't remember, but that will be harder to support.

> "sorry, this .hgignore is too complicated" when people get, uh, creative. Is that hypothetically viable?

Hypothetically, yes, but we need to be careful. ripgrep's gitignore support is actually _slightly_ different than git's in how it handles `**` globs, and it produces an error message and people have filed bug reports to get it removed. My compromise was to add `--no-ignore-messages` that suppresses errors from parsing ignore files. And that was just a corner case. If we're emitting error messages frequently on hgignore files, people are going to get upset.

So the way I'd recommend doing this is to emit a debug message that is visible with the `--debug` flag. This is less visible, but this is also _the_ way to diagnose ignore-related problems in ripgrep today (and always has been). This can be done from any crate via the `debug!` macro (see how the `ignore` crate does it) after bringing in the `log` crate.

> (In general for performance reasons all .hgignores should be supportable by Rust's regular expression packages, but there will always be people that do creative things that are slow. Sometimes I'm one of them!)

It is definitely possible that many regexes found in hgignore will Just Work with Rust's regex engine, but there are plenty of differences too. The big one is the lack of look around support. And there's probably a list a mile long about syntactical differences. Whether any given one will actually bite folks is hard to say a priori. :-)

---

_Comment by @durin42 on 2018-07-24 21:33_

>It is definitely possible that many regexes found in hgignore will Just Work with Rust's regex engine, but there are plenty of differences too. The big one is the lack of look around support. And there's probably a list a mile long about syntactical differences. Whether any given one will actually bite folks is hard to say a priori. :-)

We've got support in-tree for using re2 instead of Python's built-in re package, and yeah, we have to fall back when people do the fancy stuff. It also turns out the fancy stuff is slow, so anyone concerned about performance probably doesn't ever try the fancy stuff. I think once ever have I had to talk someone through how to stop using fancy RE features so they can end up on the re2 happy path.

I'll take a look at implementing as much as can be implemented of hg's ignore logic as a rust crate at some point (with your `ignore` crate in mind as a client), and if I can make it work I'll give you a holler somehow.

Thanks for talking this out with me!

---

_Comment by @BurntSushi on 2018-07-24 21:37_

@durin42 That's interesting. I had no idea that Mercurial implicitly encouraged re2. That makes the prognosis here better than perhaps I thought. In that case, you're right, the syntax of re2 and Rust's regex engine is very very close. Still some corner cases, but nothing too major.

---

_Comment by @mk12 on 2024-12-05 19:31_

The reasoning for not supporting hgignore makes sense and I respect the decision. But I find it amusing that mercurial is itself using the Rust regex crate for hgignore, without seeming to be worried about regex engine differences: https://foss.heptapod.net/mercurial/mercurial-devel/-/blob/3f0cf7bb30869151af092735a066aaa01dc88723/rust/hg-core/src/matchers.rs#L768. This is in “rhg” developed in the main mercurial repo, which is a drop-in replacement for hg that makes many things faster (and falls back to Python for everything it can’t yet do).

---

_Comment by @BurntSushi on 2024-12-05 19:37_

@mk12 Does it detect unsupported regexes and fall back to a different engine?

---

_Comment by @mk12 on 2024-12-05 21:04_

I just tried it and it looks like if regex parsing fails (e.g. on look ahead giving the error message “error: look-around, including look-ahead and look-behind, is not supported”), then it will fall back to Python. So it would only be a problem if there are any regexes that parse correctly in Rust but have a different meaning in Python’s engine — I’m not sure if any exist.

Reading the thread more closely I see the problem is also the glob syntax etc. not just regex. Not saying it would be easy, but seems to me like one possible approach would be if mercurial devs split off the Rust hgignore parsing into a crate which ripgrep and others could use, so that you don’t have to maintain it at all.

---

_Comment by @BurntSushi on 2024-12-05 21:14_

Possible, but unlikely. It would need to be a high quality crate with a good maintenance story. Particularly since it would be providing user facing functionality.

I think it's more likely that this just never happens. Mercurial is even less popular now than it was when this issue was created.

---
