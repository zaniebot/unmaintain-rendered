```yaml
number: 1
title: add support for other text encodings
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - question
assignees: []
created_at: 2016-09-09T16:20:33Z
updated_at: 2017-03-24T11:48:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1
synced_at: 2026-01-12T16:13:21Z
```

# add support for other text encodings

---

_@BurntSushi_

Right now, ripgrep only supports reading UTF-8 encoded text (even if some of it is invalid). In my estimation, this is probably good enough for the vast majority of use cases.

However, it may be useful to search other encodings. I don't think I'd be willing to, say, modify the regex engine itself to support other encodings, but if it were easy to do transcoding on the fly, then I think it wouldn't add too much complexity. The [`encoding_rs`](https://github.com/hsivonen/encoding_rs) project in particular appears to support this type of text decoding.

Some thoughts/concerns:
- Transcoding would require using the incremental searcher as opposed to the memory map searcher. (Which is fine.)
- Transcoding requires picking a source encoding, and doing this seems non-trivial. You might imagine the CLI growing a new flag that specifies a text encoding, but what happens if you want to search directories containing files with many different types of text encodings? Do we need a more elaborate system? I'm inclined towards not, since I think the juice probably isn't worth the squeeze.


---

_Label `enhancement` added by @BurntSushi on 2016-09-09 16:20_

---

_Label `question` added by @BurntSushi on 2016-09-09 16:20_

---

_Comment by @nabijaczleweli on 2016-09-22 00:28_

TBH I'd just ignore (and/or die violently on) anything that doesn't map to UTF-8, which is the _de facto standard_ in the Rust world.
Trying to support non-utf8 encodings seems more trouble than it's worth, IMO


---

_Comment by @BurntSushi on 2016-09-22 00:43_

@nabijaczleweli `ripgrep` isn't just for Rust people though. There are plenty of non-English speaking countries where UTF-8 isn't quite _everywhere_. There's enough folks that care about it that there are at least a couple search tools that do implement searching on files that aren't UTF-8. [The Platinum Searcher](https://github.com/monochromegane/the_platinum_searcher) and [highway](https://github.com/tkengo/highway) come to mind. Of course, both `grep` and `git grep` support other encodings as well. I'm also told that Windows has lots of UTF-16 data too.

Anyway, it's totally possible that this may never happen. But it's also a somewhat feasible thing to support without sacrificing performance for the UTF-8 case (at the cost of some code complexity). It is honestly somewhat low on my list of things I'd like to do with `ripgrep`.


---

_Comment by @BurntSushi on 2016-09-22 00:46_

(To be clear, the limit to what I'd be willing to support are text encodings that can be feasibly _transcoded_ to UTF-8.)


---

_Comment by @alexandernst on 2016-09-23 15:52_

How do you plan to detect the encoding of the text that is being ripgrep-ed in order to correctly decode it (or transcode it to utf8)?


---

_Comment by @BurntSushi on 2016-09-23 16:59_

@alexandernst That is precisely one of the problems described in this issue. ;-)


---

_Comment by @lambda on 2016-09-24 04:32_

Just yesterday, I was given a dump of log files from a Windows server, and took a while to figure out why I couldn't find the string that someone else reported as being in one of the files, when I was using `grep -r` to look for it. Took me a bit to figure out that while most of the files were in some ASCII compatible encoding, a few of them were in UTF-16.

For a tool that does recursive grepping, a lot of times what you have is some big directory tree, and you don't really know what's in it, and so you're looking for some needle in that big haystack. Not knowing what's in it can include not knowing the encoding.

Right now, what `rg` does is skip UTF-16 files as binary, and not report anything about them, so you wouldn't notice if you missed files due to this issue.

I think you do want something that applies a heuristic to determine the encoding, though I'm not sure you want that by default.

I think the way I would go about supporting this is by reporting files that could be text but are not valid UTF-8; so, those files which it's searching as invalid UTF-8, and those which it's detecting as binary but which could be UTF-16, UTF-16BE, or UTF-16LE, or possibly other encodings. If you don't at least report something, you won't know that you're missing something due to the encoding issue, but I'm not sure you'd want to do any kind of automatic encoding detection by default.

Then you could add a flag that will do heuristic encoding detection, based on BOM for UTF-16, or [even/odd nulls hack](https://bugzilla.mozilla.org/show_bug.cgi?id=631751) for UTF-16 BE or LE, or maybe even [character frequency based detection](http://www-archive.mozilla.org/projects/intl/UniversalCharsetDetection.html) for other encodings.


---

_Comment by @BurntSushi on 2016-09-24 04:56_

@lambda I agree we need to do something, but we are definitely limited. Things like, "report if a file isn't valid UTF-8" are probably off the table, because that would be way too expensive. We're probably limited to scanning the first few hundred/thousand bytes or so. Right now, it's a simple `memchr` for the NUL byte to determine if it's a "binary" file or not, to give you an idea of the scale. Reporting the existence of every binary file seems a bit... extreme. Hmm. I'll noodle on it.


---

_Comment by @rr- on 2016-09-24 05:37_

To me the inability to `rg --binary "\xDE\xAD\xBE\xEF"` is an absolute deal breaker. I work with huge binary files a lot and I often want to search for raw byte strings. I don't want to use two tools at once (`rg` for text, `ag` for binary).

Also grepping for anything in Japanese companies, be it source code or text documents, almost always means you deal with Shift-JIS.

TBH I wouldn't expect `rg` to be able to detect encoding on its own, I'd be plenty happy with being able to manually guide it with `--binary`, `--encoding=sjis` etc.


---

_Comment by @BurntSushi on 2016-09-24 05:40_

@rr- You can do that, it's just not documented on `ripgrep` (it's buried inside the [regex engine documentation](https://doc.rust-lang.org/regex/regex/bytes/index.html#syntax), which obviously is not going to be discoverable to end users, I need to work on that). Try this instead:

```
rg '(?-u)\xDE\xAD\xBE\xEF'
```

If you did try this:

```
rg '\xDE\xAD\xBE\xEF'
```

Then each hex escape is treated as a Unicode codepoint with its corresponding UTF-8 encoding. But in the former case, we disable Unicode support and get raw byte support. Unicode can actually be selectively re-enabled, so if you wanted a Unicode aware `\w` in the middle of some bytes somewhere, this would work:

```
rg '(?-u)\xDE\xAD(?u:\w+)\xBE\xEF'
```


---

_Comment by @BurntSushi on 2016-09-24 05:43_

But yes, I think everyone understands that only supporting UTF-8 (errm, ASCII compatible text encodings, technically) might be good enough for the vast majority of users, but is definitely going to be a deal breaker for many as well. This issue really doesn't need more motivation, it needs feasible ideas that can be implemented, and those need to take into account UX, performance and actual correctness. (Thanks @lambda for getting that started.)


---

_Comment by @rr- on 2016-09-24 05:52_

OK how about this: 
1. Expose `--encoding` which defaults to utf-8
2. Using `--encoding=utf16le test` internally encodes `test` as `t\0e\0s\0t\0`
3. `rg` treats the file as binary and searches for this literal pattern as a sequence of bytes

There are obviously going to be problems around UTF-8 ability to display ö as `LATIN SMALL LETTER O WITH DIAERESIS` and `COMBINING DIAERESIS` + `LATIN SMALL LETTER O` as well as [inconsistencies in Shift-JIS encoding of characters such as `REVERSE SOLIDUS`](http://svn.python.org/projects/python/tags/r24a1/Modules/cjkcodecs/README) but maybe it's good enough?


---

_Comment by @BurntSushi on 2016-09-24 05:58_

Well the huge problem with that is things like `\w` won't work. This is why I made the stipulation of a transcoding from `X` to `UTF-8` and _then_ matching. I mean, an explicit `--encoding` flag is basically an answer to this problem: it's exactly what I suggested way above. But it has downsides, like, what if your directory tree has some UTF-16 and some UTF-8?

Certainly, we can throw our hands in the air, require specific instruction of encoding by the end user and call it a day. But it doesn't really help getting at the unknown-unknowns that @lambda mentioned. Making unknown-unknowns discoverable is an important feature IMO. But perhaps it is orthogonal to text encoding support.


---

_Comment by @Nephyrin on 2016-09-24 21:15_

There's some similar discussion of UTF-16 for Ag @ ggreer/the_silver_searcher#914

Using byte-order-marks to detect UTF-other encodings may work.  At some point, if there's no meta-data to indicate encoding, a tool can only be so-magical.


---

_Comment by @puetzk on 2016-09-26 14:34_

As far as specifying encoding: .gitattributes supports

> _pattern_ encoding=_name_ 

to control its supporting tools, and you're already parsing git-style patterns for ignore. So that seems like a good way of supplying the encoding in complex cases...

That would make --encoding=_name_  cmdline switch equivalent to appending a "\* encoding=_name_" line (though you might also want a --default-encoding that "prepends" instead).

I'm not sure how best to combine this with recognizing BYTE ORDER MARK, though, and I agree that would also be very nice to have.


---

_Comment by @emk on 2016-10-08 02:14_

I'm not sure whether it would be useful or not, but I do maintain a [Rust wrapper](https://crates.io/crates/uchardet) around [uchardet](https://github.com/BYVoid/uchardet/tree/master), which detects the most common character encodings. It was extracted from Mozilla. In my experience it works quite well for identifying UTF-8 versus UTF-16 (either endianness) and some other common character encodings, but it gets a little dodgier as you go further afield.

Any such character detection library is obviously statistical in nature, and it will fail on some files.

My Rust wrapper includes a copy of the uchardet source code that hasn't been synced with upstream in a while, but I'd be happy to do so (it's just a git submodule resolved at build time), and to make any necessary feature improvements.


---

_Comment by @BurntSushi on 2016-10-18 15:48_

@emk Sorry for the late response. That's a neat idea! But I'd like to discourage using C dependencies beyond what we get with `libc`. In any case, it will be some time before I get to this, and I'd probably like to at least start without automatic detection. _After that_, depending on how much complexity it adds, I might be willing to expose it under an optional cargo feature.

(My hope is to move most of the search code out into a separate crate maintained in this repo. Once that's done, we can start talking about APIs.)


---

_Comment by @alejandro5042 on 2016-10-28 19:28_

Another vote for this: PowerShell on Windows unfortunately writes UTF-16 when piping to a file ([`Out-File`](https://technet.microsoft.com/en-us/library/hh849882.aspx?f=255&MSPPError=-2147217396)) [1]. That means a common case like this fails:

``` ps
"hi" > test.txt
rg hi
```

Matches nothing.

[1] In a classic case of inconsistency, it writes ASCII using [`Set-Content`](https://technet.microsoft.com/en-us/library/hh849828.aspx) 😢


---

_Comment by @emk on 2016-10-28 22:43_

The UTF-16LE encoding used on Windows (or rather, UCS-2LE, if that's still a thing) is an especially important use-case, and one of the easiest to detect. The typical steps [can be seen in this uchardet source file](https://cgit.freedesktop.org/uchardet/uchardet/tree/src/nsUniversalDetector.cpp):
1. Check for Unicode Byte Order Marks, which will normally work well for Windows files.
2. Check for alternating (mostly-)ASCII and 0 bytes, which will detect a large fraction of the remaining files.
3. Fall back to more sophisticated character set probers and heuristics to deal with harder cases and other encodings.

@Bobo1239 is [currently working to update](https://github.com/emk/rust-uchardet/pull/5) my Rust `uchardet` bindings to build against the latest `uchardet` and to support Windows. But it wouldn't take much to implement a pure-Rust solution that could detect UTF-8 and UTF-16.


---

_Comment by @emk on 2016-10-29 00:10_

@BurntSushi mentioned this issue elsewhere (#7) and wrote:

> The problem then comes with the output though. Do you output in the same encoding as the input? If so, that sounds pretty hairy. (Possibly even intractable.)

Is there any reason you couldn't just use Rust's standard output facilities? For a first version, detect at least UTF-16LE/BE, transcode to UTF-8 for searching, and allow Rust to output it the same way as existing strings? You pretty much need to choose a single output encoding in any case, and it might as well be whatever Rust generates for you.


---

_Comment by @BurntSushi on 2016-10-29 00:15_

@emk Rust's standard output facilities are encoding agnostic, but that's orthogonal. Here's my concern more explicitly stated:
1. With respect to **UX**: should the match output of ripgrep correspond to the original encoding or to the transcoded UTF-8?
2. If the answer to (1) is "the original encoding", how do you do it?


---

_Comment by @emk on 2016-10-29 00:45_

@BurntSushi Good questions, and tricky ones. :-)

As far as I can see, the fundamental problem with output in the original encoding is figuring out what to do if you find matches in files that use several different encodings. In that case, you pretty much need to choose a single encoding for output. This seems like a major force pushing towards keeping all output in UTF-8. I suppose the counter argument is that if the input from `stdin` is in UTF-16LE, then it's arguably nicer to use UTF-16LE for stdout as well. If you want to do that, the [encoding](https://crates.io/crates/encoding) crate can potentially do a fair bit of heavy lifting here, especially if you want to support a wide range of encodings.

I originally made Rust bindings for uchardet for use with [substudy](https://github.com/emk/substudy), which has to deal with quite a few encodings. In my experience, it's possible to detect and transcode the most common encodings automatically, and it works fairly well.

Now that I'm thinking about substudy, that provides an intesting data point: I have foreign-language subtitle files in multiple legacy encodings. In a perfectly ideal world, I would love ripgrep to correctly identify ISO Latin-1 or Windows-1252, but transcode everything to UTF-8 for matching and output, because my terminal is configured to display UTF-8, not Windows-1252. Similarly, if I write `“`, then I would want that to match smart quotes in both Windows-1252 and UTF-8 files. But this would be fairly ambitious.

Overall, it seems like UTF-16LE is the most important case to handle (for Windows support), and it's one of the easiest to implement (at least the basics).


---

_Comment by @BurntSushi on 2016-10-29 00:53_

> As far as I can see, the fundamental problem with output in the original encoding is figuring out what to do if you find matches in files that use several different encodings. In that case, you pretty much need to choose a single encoding for output. This seems like a major force pushing towards keeping all output in UTF-8. I suppose the counter argument is that if the input from stdin is in UTF-16LE, then it's arguably nicer to use UTF-16LE for stdout as well. If you want to do that, the encoding crate can potentially do a fair bit of heavy lifting here, especially if you want to support a wide range of encodings.

I'd also like to evaluate [`encoding_rs`](https://github.com/hsivonen/encoding_rs).

I don't disagree with anything you've said. Although I'd like to continue to emphasize that we should attack this problem without any C dependencies first. For example, one could imagine a solution to this problem that has no automatic detection at all. That seems like a decent place to start.

> Similarly, if I write `“`, then I would want that to match smart quotes in both Windows-1252 and UTF-8 files. But this would be fairly ambitious.

So long as `“` is input as UTF-8 in the regex (which _must_ be UTF-8) and transcoding is correct, then this should actually happen for free.


---

_Comment by @emk on 2016-10-29 01:19_

> I'd also like to evaluate encoding_rs.

Ah, I hadn't seen that yet! Thank you.

> I don't disagree with anything you've said. Although I'd like to continue to emphasize that we should attack this problem without any C dependencies first.

If it helps, I'm happy to provide a small Rust crate which provides a translation of the [essential bits of the main UTF-8 / UTF-16LE / UTF-16BE detector here](https://cgit.freedesktop.org/uchardet/uchardet/tree/src/nsUniversalDetector.cpp). This will at least handle most modern text files on Linux/Windows/Mac, which is probably 98% of what people actually want. The lack of UTF-16LE searching, in particular, is a big limitation on Windows.


---

_Comment by @BurntSushi on 2016-10-29 01:28_

@emk That sounds like a great next step after we have the ability to search transcoded data. :-)


---

_Comment by @emk on 2016-10-29 01:56_

Fair enough. :-)

Yes, I agree it would be good to implement the transliteration machinery before the detection code! If there's no auto-detection, what should the UI for selecting an input encoding look like?

As for automatic detection, looking at the source to the encoding detectors for the first time in a couple of years, the logic to detect the major encodings is surprisingly straightforward:
1. If it has a BOM, use the BOM to identify it as UTF-8, UTF-16 or UTF-32. This is the only code path that will ever identify a file as UTF-16 or UTF-32, interestingly enough.
2. If it contains only ASCII characters, identify it as ASCII.
3. If it contains ASCII plus 0xA0 (nbsp), supposedly a semi-common case, identify it as ISO Latin 1.
4. If none of the above match, run a [UTF-8 prober](https://cgit.freedesktop.org/uchardet/uchardet/tree/src/nsUTF8Prober.cpp). When working on substudy, I found this this was usually overkill, because UTF-8 has a very distinctive and strict structure, and you can usually identify it simply by asking: "Does this contain at least one UTF-8 character and does it parse as valid UTF-8?" If true, it's generally reasonable to assume UTF-8.

This will generally produce decent results for ASCII and the various UTF encodings. Given your "pure Rust" requirement, it's probably wisest not to try for anything more right away. All the necessary language models, etc., are available in uchardet, but there's a fair amount of fiddly code to handle all the weird encodings out there.


---

_Comment by @BurntSushi on 2016-10-29 02:01_

I don't know what the UI should look like, but we need one. I would
probably start by looking at iconv.

On Oct 28, 2016 9:56 PM, "Eric Kidd" notifications@github.com wrote:

> Fair enough. :-)
> 
> Yes, I agree it would be good to implement the transliteration machinery
> before the detection code! If there's no auto-detection, what should the UI
> for selecting an input encoding look like?
> 
> As for automatic detection, looking at the source to the encoding
> detectors for the first time in a couple of years, the logic to detect the
> major encodings is surprisingly straightforward:
> 1. If it has a BOM, use the BOM to identify it as UTF-8, UTF-16 or
>    UTF-32. This is the only code path that will ever identify a file as UTF-16
>    or UTF-32, interestingly enough.
> 2. If it contains only ASCII characters, identify it as ASCII.
> 3. If it contains ASCII plus 0xA0 (nbsp), supposedly a semi-common
>    case, identify it as ISO Latin 1.
> 4. If none of the above match, run a UTF-8 prober
>    https://cgit.freedesktop.org/uchardet/uchardet/tree/src/nsUTF8Prober.cpp.
>    When working on substudy, I found this this was usually overkill, because
>    UTF-8 has a very distinctive and strict structure, and you can usually
>    validate it simply by asking: "Does this contain at least one UTF-8
>    character and does it parse as valid UTF-8?" If true, it's generally
>    reasonable to assume UTF-8.
> 
> This will generally produce decent results for ASCII and the various UTF
> encodings. Given your "pure Rust" requirement, it's probably wisest not to
> try for anything more right away. All the necessary language models, etc.,
> are available in uchardet, but there's a fair amount of fiddly code to
> handle all the weird encodings out there.
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/1#issuecomment-257064221,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34n7FpwVKii93xQLcD-Me9EfNTg-Nks5q4qeygaJpZM4J5OAx
> .


---

_Closed by @BurntSushi on 2017-03-12 23:54_

---

_Comment by @ssokolow on 2017-03-14 03:52_

The [Whirlwind tour section of the README](https://github.com/BurntSushi/ripgrep#whirlwind-tour) still needs to be updated:

> One last thing before we get started: `ripgrep` assumes UTF-8 everywhere. It can still search files that are invalid UTF-8 (like, say, latin-1), but it will simply not work on UTF-16 encoded files or other more exotic encodings. [Support for other encodings may happen](https://github.com/BurntSushi/ripgrep/issues/1).

---

_Comment by @BurntSushi on 2017-03-14 12:31_

@ssokolow Fixed! Thanks.

---

_Comment by @blfpd on 2017-03-24 11:48_

@BurntSushi You might want to edit your anti-pitch on your [blog post](http://blog.burntsushi.net/ripgrep/#anti-pitch).

> If you need to search files with text encodings other than UTF-8 (like UTF-16), then ripgrep won’t work.

---

_Comment by @BurntSushi on 2017-03-24 11:48_

@batisteo Hmm right thanks, I always forget to update the blog.

---
