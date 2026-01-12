```yaml
number: 1488
title: make it easier to maintain fork with additional regex engines
type: issue
state: closed
author: pierreN
labels:
  - question
assignees: []
created_at: 2020-02-16T04:44:30Z
updated_at: 2020-03-21T00:26:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1488
synced_at: 2026-01-12T16:13:23Z
```

# make it easier to maintain fork with additional regex engines

---

_@pierreN_

I needed a CLI tool to parse a massive amount of regexps (and improve my rust at the same time) so I made a crate implementing the `Matcher` trait : https://git.sr.ht/~pierrenn/grep-hyperscan

From my (sporadic) tests, its starts to be useful when having at least 1000 regexps to parse more than 10GB of data. I had 4.5k on 150GB soo...  Since the data comes from disk reads, using `hyperscan` basically limits your speed to your disk speed.

Plus there is a limit to the size of the compiled expressions in `ripgrep`, so using `hyperscan` allows to bypass that.

Ideally it would be cool if it can be integrated in `ripgrep`. I prefered to open this issue before doing a PR to talk about it and gauge interest. Details are below.


## Implementation

It's just an implementation of `find_at` since `hyperscan` doesn't support groups (`new_captures` is implemented using `NoCaptures`).

I thought of 3 possible ways to implement `find_at`:
- `hyperscan` has a `HS_FLAG_SINGLEMATCH` which would be great for `find_at`. However this is incompatible with the flag `HS_FLAG_SOM_LEFTMOST` which is required to get the `from` of the `Match`.
- you can force hyperscan to stop scanning a haystack after the first match by returning not 0 in the callback provided to `hyperscan`. However, this means a call to hyperstack each time we have a match. An (outdated...) implementation of this idea is available in the branch `ideas/single_match`
- everytime we get a new haystack sent to `find_at`, scan it in one go with `hyperscan`, remember the matches into a `VecDeque`, and consume the deque at each new call. I guessed first that when we return `Ok(None)` from `find_at`, the next haystack will be a new one. However (and this is weird?) sometimes `find_at` get sent a new haystack while the "current" is not termined. From testing it seems to only be the same haystack with EOL added at the end, or the right-most part of the original haystack. Thus, we also start a new `hyperscan` run when we see a new haystack length. Avoiding the successive calls to `hyperscan` allows according to my (sporadic...) benchmarks to speed up the overall match by 10-20%. Ideally, it would be great if we could require `ripgrep` to only send once each haystack (or the minimal amount of data), but no idea how to do that.

## Things to do for integration

I think the following tasks should be done for integration:
- [ ] add a way to switch regexp engines easily. For testing I added `-Y/--hyperscan` but this kind of clumsy... IMHO it would be best to have an option `--engine=` which accepts `default|pcre2|hyperscan` (and this would allow to easily add other engines such as [chimera](https://intel.github.io/hyperscan/dev-reference/chimera.html)) (default to default, and the `-e` shortcut is already taken soo.. ?)
- [ ] allow `-f` to read a text file OR an `hyperscan` database. Most of the running time spend by hyperscan is actually to compile the list of text regexp pattern to it's own format DB (see benchmarks below). Plus a lot of DB comes in the already compiled form. Plus sometimes you want to rerun the same regexps on different files...
- [ ] hence add a parameter to write the hyperscan compiled database to a file (after DB compilation/read and before the matching) (`-d/--hyper-write=filename`, disabled by default)
- [ ] add hyperscan specific option to allow empty buffers match, see `HS_FLAG_ALLOWEMPTY` in https://intel.github.io/hyperscan/dev-reference/api_constants.html#pattern-flags (`--hyper-allow-empty`, default false)
- [ ] add hyperscan specific option to force utf-8 mode for all regexp patterns, See `HS_FLAG_UTF8` in same URL (`--hyper-utf8`, default false)
- [ ] add hyperscan specific option to enable unicode property support for all regexp patterns. See `HS_FLAG_UCP` in same URL. (`--hyper-unicode-property`, default false)
- [ ] in the helper text for the options above + case sensitiveness + dotall + multiline + ... add that if they are used with hyperscan engine, they override the default behavior of each regexp
- [ ] suggests to use hyperscan when the regexp size exceeds the limit
- [ ] docs and testing...


Do you see something else ? Is there anything to change/which is not OK ? I'll edit the tasks accordingly.


## Benchmarking

I has to parse around ~150GB of HTML scraped webpage through ~4500 regexps, so that was my benchmark. The format for hyperscan regexp and default regexp is different, so I used 2 set of regexps for benchmarking. Plus there is a limit to the amount of regexps that the default engine can handle, and since my list of regexp is in the shape:

```
some.domain1.com/@[\w.\+\-]+
some.other.domain2.net/@[\w.\+\-]+
...
```

where I have a 4.5k list of web domains (this is to find possible fediverse accounts in a webpage). Using a basic list like that is too big for the default engine, so I used: `(some.domain1.com|some.other.domain2.net|...)/@[\w.\+\-]+`.

The default regexps are here : https://termbin.com/xdse , the hyperscan regexps are here : https://termbin.com/62ov

I used a subset of 15GB of data for testing. Parsing the regexps with the default engine, it takes around 8:20mn (best case). Using the hyperscan engine it takes less than 30 seconds to parse the files (that's basically the speed of my SSD) AND 5 minutes to compile the regexps. That's why we need a flag to deserialize/serialize regexps so using the hyperscan engine becomes easier.


----

Sorry for the (too!) long issue. The reason I opened this is:
- code reviews : I'm using this as an excuse to learn rust, so any comment on https://git.sr.ht/~pierrenn/grep-hyperscan is more than welcome
- gauge interest : do you think integrating this in ripgrep is a good idea ?
- instructions on how to proceed if interest : is the above todo list accurate ? what could be improved/changed/added/removed ?

---

_Comment by @BurntSushi on 2020-02-16 16:25_

Thanks very much for filing this issue. I'm glad you did before doing more work
on it.

So firstly, the regex-geek side of me finds this very exciting. I've always
wondered what it would look like to hook ripgrep up to Hyperscan. And indeed,
some of the APIs (like the `Matcher` trait) were influenced by the knowledge
that Hyperscan does internal iteration. However, it sounds like it's still not
quite fitting together correctly.

OK, so I'm just going to start by responding to a few points.

> Plus there is a limit to the size of the compiled expressions in ripgrep, so
> using hyperscan allows to bypass that.

While true, it can be trivially increased via `--regex-size-limit`. You'll also
want to increase `--dfa-size-limit`. Could you try that and see how ripgrep
fairs? I still expect Hyperscan to easily beat Rust's regex engine on a
benchmark like this, but numbers would still be interesting!

> hyperscan has a HS_FLAG_SINGLEMATCH which would be great for find_at. However
> this is incompatible with the flag HS_FLAG_SOM_LEFTMOST which is required to
> get the from of the Match.

Oh interesting. Is `HS_FLAG_SINGLEMATCH` new? I don't think Hyperscan had that
the last time I looked into it. In any case, it not working with SOM handling
is unfortunate.

> you can force hyperscan to stop scanning a haystack after the first match by
> returning not 0 in the callback provided to hyperscan. However, this means a
> call to hyperstack each time we have a match. An (outdated...) implementation
> of this idea is available in the branch ideas/single_match

Hmm. Did this work well? It seems like this should be the right way to go..?
But I might be missing something. (I've never actually used Hyperscan in
anger.)

> everytime we get a new haystack sent to find_at, scan it in one go with
> hyperscan, remember the matches into a VecDeque, and consume the deque at
> each new call.

This is a good idea.

> Ideally, it would be great if we could require ripgrep to only send once each
> haystack (or the minimal amount of data), but no idea how to do that.

Ah yeah, that sounds annoying. Unfortunately, this is correct and intended
behavior. A `Matcher` must be re-run, depending on the output settings, to
determine the offsets of each individual match. Otherwise, it can be quite a
bit cheaper to detect _whether_ a line matches at all without finding each
individual match. In general, a `Matcher` definitely cannot and should not
assume anything about the input. Certainly, checking the length is bound to be
incorrect in some cases. So I think this optimization might not be possible...
Unless you can some how store the matches in a way that makes them quick to
look up on subsequent calls to `find_at` without rescanning. But that seems
tricky. And it would still require knowing whether the previous `find_at` call
corresponds to the same `haystack` as the one you're given.

In general, the `Matcher` trait just wasn't designed to handle this sort of
optimization. I think the right path here would be to figure out a new API for
the `Matcher` trait. But this would likely make it quite a bit more complex, so
I'm not sure I want to go down that path...

> Do you see something else ? Is there anything to change/which is not OK ?
> I'll edit the tasks accordingly.

Your TODO list looks like a great start. There might be more things, e.g.,
supporting all of ripgrep's options, but nothing else is coming to mind.

> where I have a 4.5k list of web domains (this is to find
> possible fediverse accounts in a webpage). Using a basic
> list like that is too big for the default engine, so I used:
> `(some.domain1.com|some.other.domain2.net|...)/@[\w.\+\-]+`.

My guess is that Hyperscan doesn't have Unicode mode enabled by default, where
as Rust's regex engine does. In particular, `\w` and `.` are Unicode aware. You
can turn those things off with `(?-u)` at the beginning of each regex. (I
should add a `--no-unicode` flag for this.) That should also decrease the size
of each regex substantially, although you may still need to raise the regex and
DFA size limits as described above.

> code reviews : I'm using this as an excuse to learn rust, so any comment on
> https://git.sr.ht/~pierrenn/grep-hyperscan is more than welcome

Looks great! I'm impressed if this is your first Rust project. I don't see
anything that offends me.

> gauge interest : do you think integrating this in ripgrep is a good idea ?

Right, so... Unfortunately, I'm not sure it's a great idea. I think it would
be very cool, but the maintenance burden this will add is substantial.
Hyperscan is a beast to compile (last time I tried it), and just adding tests
for it to CI (which would be an unnegotiable requirement for me maintaining
it) would probably make CI times much longer. Nevermind keeping everything
updated and managing the integration. I'm afraid bundling two regex engines
with ripgrep proper is probably my limit. :-(

> instructions on how to proceed if interest : is the above todo list accurate
> ? what could be improved/changed/added/removed ?

I would really hate to see a world that didn't combine ripgrep with Hyperscan.
I think it's a great idea and I think your use case is very valid and
interesting. Unfortunately, I just can't personally justify taking it on. So I
think the only option that leaves is for someone to maintain a Hyperscan patch
for ripgrep. It _should_ be possible to arrange things such that the patch can
be cleanly applied to ripgrep going forwards, even as ripgrep evolves. I can't
really think of any other path forward here. It would be nice if there were a
way to add new regex engines to ripgrep without changing its source code, but
that seems quite tricky on its own.


---

_Label `question` added by @BurntSushi on 2020-02-16 16:25_

---

_Comment by @pierreN on 2020-02-16 18:19_

Thanks for the answer ! 

> While true, it can be trivially increased via --regex-size-limit. You'll also
> want to increase --dfa-size-limit. Could you try that and see how ripgrep
> fairs? I still expect Hyperscan to easily beat Rust's regex engine on a
> benchmark like this, but numbers would still be interesting!

Hah good idea, I missed that option. Increasing the size does allow the regexps to compile, but it just makes performances worst (at least 2 times worst, I stopped the process after a while).

> Oh interesting. Is HS_FLAG_SINGLEMATCH new? I don't think Hyperscan had that
> the last time I looked into it. In any case, it not working with SOM handling
> is unfortunate.

Yes this is unfortunate. Since the goal of hyperscan is mainly to detect if a payload is malicious or not it makes sense to not care about SOM... but not in our case.

> Certainly, checking the length is bound to be incorrect in some cases

Hah fair enough, I didn't see it that way. I'll rollback the crate to using callback return values then. At this speed the real threshold is more the disk read speed anyway so it won't make that huge of a difference!

> My guess is that Hyperscan doesn't have Unicode mode enabled by default, where
> as Rust's regex engine does. In particular, \w and . are Unicode aware.

 Yes, this is indeed better  thanks (like the old `LC_ALL=C` grep trick...). Doing that, default engine is down to 6mn40s.

> Unfortunately, I'm not sure it's a great idea

I'm disapointed by this answer ! But I understand you, maintaining free software is an ungrateful task. I could maintain a patchset but in the current state of thing it's going to be messy.. A good compromise would be to have the code infrastructure merged in the main project to handle tasks 1 and 2 (without the hyperscan bits) first and then have a maintenable patch.


---

_Comment by @BurntSushi on 2020-02-16 23:53_

> Hah good idea, I missed that option. Increasing the size does allow the regexps to compile, but it just makes performances worst (at least 2 times worst, I stopped the process after a while).

Interesting. As a sanity check, did you also set `--dfa-size-limit` to a crazy high value? Even if you did, your result is still quite plausible. Basically, the lazy DFA ends up overwhelmed and just spends a ton of time generating new states. In any case, it just goes to show much of an engineering marvel Hyperscan is.

> A good compromise would be to have the code infrastructure merged in the main project to handle tasks 1 and 2 (without the hyperscan bits) first and then have a maintenable patch.

Task 1 sounds good. I think the values should be `default`, `pcre2` and `auto-hybrid`. Task 2 seems Hyperscan specific?

For the patchset, I think it would be helpful to try to put as much as the Hyperscan bits in a separate file as possible, and as little as you can in `src/args.rg` and `src/app.rs`.

Anyway, good luck. I'm happy to accept PRs that will make maintaining a patchset easier, so long as they aren't too disruptive. I do really want to support your use case because I think it's awesome to integrate ripgrep and Hyperscan.

---

_Comment by @pierreN on 2020-02-22 06:19_

Sorry for the response delay.

> Interesting. As a sanity check, did you also set --dfa-size-limit to a crazy high value? Even if you did, your result is still quite plausible. Basically, the lazy DFA ends up overwhelmed and just spends a ton of time generating new states. In any case, it just goes to show much of an engineering marvel Hyperscan is.

This highly depends on the regexp used I guess. In my tests, I guess it's just more efficient to send to the default regexp engine `(domain1.com|donain2.com|...)@[\w.\+\-]+` than `(domain1.com@[\w.\+\-]+|domain2.com@[\w.\+\-]+|...)`

> Task 1 sounds good. I think the values should be default, pcre2 and auto-hybrid. Task 2 seems Hyperscan specific?

I'll send a MR for task 1 in a couple of days, thanks.

For task 2, what was worrying me was the `patterns: Vec<String>` in `args.rs` since a compiled hyperscan database is (roughly) a slice of `u8`. I started replacing it with `patterns: Vec<T>` but it ended up to make not much sense in the end (I didn't see at first all the pattern filling logic is in the `cli` crate..).

I guess I'll just do in the patch a hackish `from_utf8_unchecked` in `ArgMatches::patterns` instead. (so that I convert the slice of `u8` to a `String` and then check in the `RegexpMatcherBuilder` that if a hyperscan db is present, the len of the Vec is 1).

> For the patchset, I think it would be helpful to try to put as much as the Hyperscan bits in a separate file as possible, and as little as you can in src/args.rg and src/app.rs.

Yes, thanks indeed.

---

_Comment by @BurntSushi on 2020-02-22 12:24_

> I guess I'll just do in the patch a hackish from_utf8_unchecked in ArgMatches::patterns instead. 

Please do not do this. It's undefined behavior to create a `String` from invalid UTF-8.

I see what you mean though and how that could be annoying. I'm trying to think of an easy way to maintain a patch here. Hmmm... There is definitely a fairly strong assumption that `patterns` is actually a sequence of regex patterns and not something else. Maybe it could make sense to find some way to leave `patterns` alone and instead create a `dbs` that contains Hyperscan databases? At that point, all you'd have to do is find a way to populate each vec correctly, but it seems doable with some heuristics?

---

_Renamed from "Add hyperscan engine support" to "make it easier to maintain fork with additional regex engines" by @BurntSushi on 2020-03-15 13:24_

---

_Closed by @BurntSushi on 2020-03-15 13:39_

---

_Comment by @pierreN on 2020-03-15 13:52_

Thanks!

---

_Comment by @pierreN on 2020-03-21 00:26_

For future reference the patchset is here : https://git.sr.ht/~pierrenn/ripgrep

---
