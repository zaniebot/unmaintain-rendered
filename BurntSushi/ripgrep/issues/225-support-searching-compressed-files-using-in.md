```yaml
number: 225
title: support searching compressed files using in-process decompression
type: issue
state: closed
author: danbst
labels:
  - question
  - icebox
assignees: []
created_at: 2016-11-08T18:15:04Z
updated_at: 2018-02-21T05:17:47Z
url: https://github.com/BurntSushi/ripgrep/issues/225
synced_at: 2026-01-12T16:13:21Z
```

# support searching compressed files using in-process decompression

---

_@danbst_

I'd like to use ripgrep for grepping log files, because it's faster then grep. But my logs are gzipped, and if I `zcat | rg` them I'll loose log filenames in output.

Also, would be great if bzip2 and xz decompressors will be supported too with automatic archive type detection.

---

_Label `question` added by @BurntSushi on 2016-11-09 22:19_

---

_Comment by @BurntSushi on 2016-11-09 22:34_

I'm torn on this one for both pragmatic and philosophical reasons.

Philosophically, this is making ripgrep do a lot more than just "open a file and search it." I don't actually believe that ripgrep should do everything, even though it already does a lot. Nevertheless, it's hard to argue with the usefulness of this feature.

Practically speaking, there are two primary issues as I see it:
1. If we wanted to implement this right now, we'd need to bring in C libraries. While there's no hard requirement against doing this (to my knowledge, as long as the three main platforms are supported), I would like to actively discourage it in the interest of keeping ripgrep pure Rust.
2. This requires some kind of UX design. How does ripgrep detect compressed files? When it finds them, does it always search them or does a user explicitly need to enable it? How are decompression options, if they exist, controlled? What is "automatic archive type detection"?

I think that if we were to do this, it is _at least_ blocked on some initial implementation of #1, since supporting additional text encodings has a lot of overlap with decompressing files before searching in terms of making the core search routines work with it.

With that said, I don't personally see myself working on this any time soon.


---

_Comment by @alejandro5042 on 2016-11-10 05:26_

_Brainstorming here..._

Instead of `rg` constantly playing catching up on every type of compressed file option, an alternative is to have a plugin system whereby the user can decide what `rg` does when it encounters a file. For example, `rg` can look up the extension in a configuration file (or the glob in a `.gitattributes`-type of file), execute the `preprocessor` step, runs the search on the results, then deletes it. Alternatively, it can simply stream the output of the command in memory and skip the file writes. Seems like quite a bit of work though....

Anyway, this could also be used to search Word, binary, XML, or other complicated formats; if for example you could turn the complicated format into plain-text format that `rg` can happily parse.


---

_Comment by @BurntSushi on 2016-11-10 11:30_

As the maintainer, I'm not particularly interested in the plugin path, sorry.


---

_Comment by @danbst on 2016-11-11 09:38_

Ok, I see. Also, I don't think that `ripgrep -z` will perform much better then zgrep, because decompression takes time and levels the ripgrep speed (unless Rust can magically do faster decompression).

Now, none of `ag`, `rg` and `zgrep` can replace each other - zgrep is faster then ag on compressed files and beats rg, ag can do searches in `gzip`, `zip`, `lzma` and `xz` (with autodetection), which none of zgrep and rg do, and rg is fastest when no compression is used and your regex feats declared expressivity.


---

_Comment by @BurntSushi on 2016-11-11 11:34_

> I don't think that ripgrep -z will perform much better then zgrep

That _might_ be true on single files (it really depends on what proportion of time is spent in decompression), but ripgrep can certainly get wins with parallelism.

> Now, none of ag, rg and zgrep can replace each other

Can we not use this as motivation for adding new features please? I ask this because it will _always_ be true. For example, if you need a POSIX compliant search tool, then neither ripgrep nor ag will suffice. If you need backreferences or lookaround, then ripgrep won't work but `grep -P` and and `ag` will. I don't think that will _ever_ change. ripgrep will never be everything to everyone and that's OK.

With that said, it's certainly reasonable to expect some convergence of features. For example, I specifically built ripgrep so that folks could use it for both the "search a large repo of code" and "search very large files" use cases.

I think my initial comment on this issue still stands: this feature is a possibility, but has a few philosophical and practical problems with it.


---

_Comment by @wavexx on 2017-02-02 14:37_

I would expect at least basic unix stream compressors to be supported (.gz/.bz2/.xz). All editors decompress (and often recompress) those on the fly.

Debian ships share/doc/* files in compressed form when doing so results in space savings. I couldn't do a quick search there, and searching through docs is something I do often. It makes perfect sense to have documentation compressed.

Source is sometimes compressed too. Emacs compresses .el files by default on install as well. Ironically, using emacs ripgrep package I cannot search into an installed emacs lisp files ;)

I expect a performance hit when searching through compressed files, so I don't think that's a problem. My main concern is that I could miss a match because one of the files has been compressed. For text data files, this happens frequently, especially in repositories that need to float over the network.

---

_Comment by @BurntSushi on 2017-02-02 14:41_

@wavexx I don't think there's any question that this is a desirable feature. Thank you for sharing your use cases though, they're helpful.

---

_Comment by @moorereason on 2017-02-06 23:17_

I'm troubleshooting a cups server issue (for 3 days now), and I wish I could use a feature like this.  Some files within the cups installation are gzipped while others are not, and they're scattered all of the place.  So far I've been doing `find . -type f -name "*.gz" | xargs zgrep needle`, but I'm not finding what I expect to see and feel like I may be missing something.

Regarding the issues Andrew brought up earlier:  1) I'd rather not have ripgrep linked to C libs :-1: and 2) for the UX design, I'd just offer a `-z` option and have ripgrep work in "zip-only" mode.  That would be good enough for my use.

I'm a nobody, but my vote is to freeze this issue until pure-Rust compression libs are available and then re-evaluate this idea at that point.

---

_Label `icebox` added by @BurntSushi on 2017-03-13 01:29_

---

_Comment by @rik on 2017-03-21 14:21_

On macOS, zgrep is the 2.5.1 BSD version. [BSD grep is noticeably slower than GNU grep](http://jlebar.com/2012/11/28/GNU_grep_is_10x_faster_than_Mac_grep.html). I haven't found a way to easily install the GNU version so it would be nice to have a fast and convenient to grep directories of compressed log files.

---

_Comment by @BurntSushi on 2017-03-23 10:57_

@rik I think you can install the GNU tools through `brew`? (I'm not a Mac user though, so don't take my word for it. I think you wind up needing to use `ggrep` for GNU grep, for example.)

---

_Comment by @rik on 2017-03-23 11:33_

Sorry, I should have mentioned that I've looked into [Homebrew/homebrew-dupes](https://github.com/Homebrew/homebrew-dupes). Yes you can do that but that only installs `grep`, not `zgrep`.

---

_Comment by @Xenofex on 2017-04-20 04:52_

I use ag and sift for my everyday search, not rg just because it doesn't support gzip. This UX issue matters a lot when I do a lot of log search as a sysadmin. Generally log files are larger than code files. Now I have 3.6GB gzipped log files to search frequently, where the raw speed of a search tool really shines. As of my code, I don't really matter if it is ag, sift or rg, because every of them gives me the result fast enough.

As a search utility focusing on speed, I think log file search should be its target use case, where your effort devoted really helps.

Thanks!

---

_Comment by @wavexx on 2017-07-04 13:09_

To follow up on my previous comment, and to make a recommendation, it would be nice if ripgrep would just use readily available stream compressors directly as a co-process instead of embedding a decompression library. Although for small files the fork might incur in some penalty, it's unlikely ripgrep will ever be faster than pbzip2, with the advantage that you gain instant access to all common unix formats at once (you only need an extension/compressor map). You could always specialize z/gz later on to gain advantage for small scattered files.

---

_Comment by @lnicola on 2017-07-04 13:11_

@wavexx Please don't. Not everyone is on Linux etc.

---

_Comment by @wavexx on 2017-07-04 13:23_

On Tue, Jul 04 2017, Laurentiu Nicola wrote:
> @wavexx Please don't. Not everyone is on Linux etc.

My suggestion does not preclude an embedded library. But it does have a
massive advantage on unix systems. There are several variants of popular
stream compressors with varying tradeoffs of memory/speed.


---

_Comment by @BurntSushi on 2017-07-04 13:52_

@wavexx Thanks for the suggestion. I'm somewhat attracted to it. Since this ticket seems to be tracking general decompression support, I've created a more focused implementation specific ticket: #539. I'm not sure when I personally will be able to work on it, but I would be happy to mentor it. I think anyone with some Rust experience could probably do it!

---

_Comment by @ylluminarious on 2017-09-24 03:59_

@BurntSushi This ticket and #539 have been stagnant for a while. Earlier in this thread you mentioned:

> If we wanted to implement this right now, we'd need to bring in C libraries. While there's no hard requirement against doing this (to my knowledge, as long as the three main platforms are supported), I would like to actively discourage it in the interest of keeping ripgrep pure Rust.

Yet, given how long this feature has been stalled, should you not just bite the bullet and pull in the libraries?

This feature is a deal-breaker for myself, others that I know, and some in this very thread. It doesn't seem that keeping ripgrep "pure Rust" is worth the trouble at this point, esp. considering that this could have been implemented already.

Just my 2¢.

---

_Comment by @BurntSushi on 2017-09-24 04:24_

You aren't considering the maintenance burden of bringing in C code. I'm not inclined to change course at this time.

---

_Comment by @ylluminarious on 2017-09-24 04:50_

@BurntSushi But it would be a small amount, no? Does Rust not support an FFI to C to ease this sort of thing?

Also, as an aside, I believe that the "burden" of C is *very much* overstated these days. It's not as bad as most people make it sound...

---

_Comment by @lnicola on 2017-09-24 06:57_

@BurntSushi Maybe soon: https://github.com/alexcrichton/flate2-rs/issues/67

@ylluminarious 
> Also, as an aside, I believe that the "burden" of C is very much overstated these days. It's not as bad as most people make it sound...

You're probably right, but some people oppose to it on principle. Also, on Windows, it's still somewhat awkward at times (but those are bugs of course, and could/should be fixed).

---

_Comment by @jdanford on 2017-09-24 07:12_

@ylluminarious 
> But it would be a small amount, no?

You're probably right, so why don't you just submit a pull request that adds the necessary libraries and glue code?

---

_Comment by @BurntSushi on 2017-09-24 11:11_

I did not say it would be hard to implement. I said it would increase
*maintenance* costs. Right now, the build process is simple because
everything is in Rust.

I'm not opposed to this on principle. I'm on mobile so I don't have a link,
but someone has already tried to add support for this in a PR. There's some
discussion on that PR that I would encourage you to read.

On Sep 24, 2017 3:12 AM, "Jordan Danford" <notifications@github.com> wrote:

> @ylluminarious <https://github.com/ylluminarious>
>
> But it would be a small amount, no?
>
> You're probably right, so why don't you just submit a pull request that
> adds the necessary libraries and glue code?
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/225#issuecomment-331692068>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34s80eT9ArGfGN0c8wGrfkhwF3G4Bks5slgDkgaJpZM4KsvZm>
> .
>


---

_Comment by @BurntSushi on 2017-09-24 11:14_

@jdanford 

> You're probably right, so why don't you just submit a pull request that adds the necessary libraries and glue code?

Let us please try to keep things friendly here. I would rather someone not put in the work to add this unless it has a good chance of getting merged (unless they are explicitly okay with doing it regardless of the result).

---

_Comment by @BurntSushi on 2017-09-24 12:57_

I'd like to note that #539 exists as an implementation path, and the amount of work required to implement that is roughly on par with bringing in C libraries IMO.

See PR #305, which has more details.

---

_Comment by @ylluminarious on 2017-09-24 18:04_

@jdanford 

> why don't you just submit a pull request[...]?

As @BurntSushi mentioned, I do not want to put in time for such a feature unless it's likely to get accepted and merged.

---
@lnicola Thanks a lot for sharing https://github.com/alexcrichton/flate2-rs/issues/67 -- intriguing stuff with obvious usefulness here.

Also, yes, it is possible that things could be awkward on Windoze, but as you mention, hopefully that can be worked around / fixed.

---

@BurntSushi Thanks for adding that extra information on #305 and for your thoughts on #539. I had presumed that adding support via C would be easier at this point than via Rust, but that seems incorrect now.

---

_Comment by @jdanford on 2017-09-24 20:36_

Sorry for my snarky comment! It wasn't constructive, and it seems like everyone's on the same page now anyway.

---

_Comment by @ylluminarious on 2017-09-24 20:37_

@jdanford No offense taken -- it was a valid question.

---

_Comment by @Dieken on 2018-01-28 14:57_

You can set environment variable `GREP` to "rg", then use `zgrep/bzgrep/xzgrep -Hn` to uncompress and search on .gz/.bz2/.xz/.lzma/.lzo with `rg`,  I feel this is somewhat enough :-)

Notice `/usr/bin/bz*grep` and `/usr/bin/z*grep` on Mac OS X don't support environment variable `GREP`, BSD tools suck...  You need install `gzip` and `xz` with Homebrew.

---

_Comment by @BurntSushi on 2018-01-30 14:16_

For anyone watching this issue, the next release of ripgrep will have support for searching compressed files using the `-z/--search-zip` flag by shelling out to `gzip`/`xz`/`lzma`/`bzip2` for decompression. Thanks to @balajisivaraman for implementing this! (See #751 and #767.)

I am going to keep this issue open to track in-process decompression, since I suspect we will ultimately want to move to that. However, I don't expect that to happen any time soon.

---

_Renamed from "Support decompression on the fly" to "support searching compressed files using in-process decompression" by @BurntSushi on 2018-01-30 14:16_

---

_Comment by @Boscop on 2018-01-30 15:49_

Does it work on windows too when 7z is installed?

---

_Comment by @ylluminarious on 2018-01-30 16:06_

@BurntSushi Thanks for the news!

---

_Comment by @BurntSushi on 2018-01-30 16:08_

@Boscop I don't know. You need the `xz`, `gzip` and `bzip2` binaries. If they don't exist, then decompression doesn't happen. I tested this in a cygwin environment and it works. If a normal Windows environment requires additional binaries, then someone will need to put in the work to add that support.

---

_Comment by @ylluminarious on 2018-01-30 16:11_

@BurntSushi 
> You need the xz, gzip and bzip2 binaries.

We don't need *all* of those programs for *any* compressed file, do we? I assume you mean that we need a respective decompression program for a given file type.

---

_Comment by @BurntSushi on 2018-01-30 16:23_

@ylluminarious Yes.

---

_Comment by @Boscop on 2018-01-30 18:01_

7z.exe supports all of those.

---

_Comment by @albfan on 2018-02-20 17:31_

As maintainer make this configurable can give you lot of headaches, can resist to propose:

https://github.com/BurntSushi/ripgrep/blob/597bf04a56d43aa9c0eb0f8fbb90c9d51c53656c/src/decompressor.rs#L37
https://github.com/BurntSushi/ripgrep/blob/597bf04a56d43aa9c0eb0f8fbb90c9d51c53656c/src/decompressor.rs#L53

so you can config new files to decompress:

    --zip-command="jar:gzip -d -c"



---

_Comment by @BurntSushi on 2018-02-21 00:37_

[On second thought](https://github.com/BurntSushi/ripgrep/issues/225#issuecomment-361605938), I'm just going to close this issue, since there isn't much value in tracking it at this point. It will likely be a long time before in-process decompression happens.

---

_Closed by @BurntSushi on 2018-02-21 00:37_

---

_Comment by @ylluminarious on 2018-02-21 05:17_

@BurntSushi You should probably update your [Anti-Pitch](https://blog.burntsushi.net/ripgrep/#anti-pitch) for `ripgrep` on your blog, saying that search decompression is now possible through external utilities.

---
