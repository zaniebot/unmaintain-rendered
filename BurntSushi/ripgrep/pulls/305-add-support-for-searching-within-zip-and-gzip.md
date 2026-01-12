```yaml
number: 305
title: Add support for searching within ZIP and gzip files
type: pull_request
state: closed
author: cesarb
labels: []
assignees: []
base: master
head: feature/uncompress
created_at: 2017-01-07T23:08:41Z
updated_at: 2017-03-13T00:40:11Z
url: https://github.com/BurntSushi/ripgrep/pull/305
synced_at: 2026-01-12T18:23:12Z
```

# Add support for searching within ZIP and gzip files

---

_@cesarb_

The ZIP format is the base for several formats, like OpenDocument,
Office Open XML, EPUB, and others. This commit adds support for the
common subset of the ZIP format used by these formats (only store or
deflate, no encryption, no patched data, no splitting or spanning).

Since the gzip format uses the same compression method (deflate), this
commit also adds support for gzip files.

Searching within nested files, for instance within a ZIP file full of
EPUB files, is also allowed.

The default is to not search within compressed files; a command line
parameter enables the file format detection. The maximum nesting depth
is controlled by another command line parameter.

Since the deflate code is not pure Rust, a Cargo feature is used to
allow compiling ripgrep without the compressed file support.

----

I wrote this code for my own use, so it's no problem if this pull request is not accepted; I can just keep using my own branch.

My guiding use case was to search within a huge (>4GiB) ZIP file full of EPUB files. Another use case enabled by this change is to search within a directory full of gzip-compressed log files (as created by logrotate; see #225 ).

It should be simple to add support for bzip2 (and with it ZIP version 4.6 and method 12) and others, but each one would need yet another C library. Since flate2 is a dependency of cargo, it should compile without issue for all relevant platforms, but the same might not be true of other compression libraries.

If you want this code to be unconditional instead of using a feature, it's no problem; I made it a feature just because going the other way (unconditional to a feature) would be more work than making it conditional from the start.

---

_Comment by @BurntSushi on 2017-01-07 23:45_

This looks great! The code looks great. I am especially in love with the fact that you did this without touching any of the search code. Unfortunately... I have some concerns:

1. I really want ripgrep to be as much Rust as possible. This is, however, a mostly ideological position. In practice, we probably aren't getting pure Rust versions of industrial strength decompressors for common formats any time soon. So, I might be able to cave on that one. However, I do want to retain the ability to compile a single static executable, and I would like for everything to work on Windows/Mac/Linux.  For the Windows GNU builds it looks like you might need something like [this](https://github.com/alexcrichton/gcc-rs/blob/master/appveyor.yml). For the MUSL builds, I think you'll need to add a musl package to get `musl-gcc`, although I don't know the exact package name.
2. I am *very* concerned about my ability to maintain this code. Specifically, I am concerned with `src/zip/mod.rs` the most (everything else looks like something I'd be OK with). In particular, I'm thinking about future bugs being filed against this feature and me needing to understand the intricacies of `for_each_entry` and how it can fail to fix said bugs.
3. I think CI needs to run tests with the new `uncompress` feature set.

Could you perhaps say more about (2)? I've never worked with these formats in detail before, so I'm pretty blind to their inner workings. How will it fail? Are their different/esoteric versions of this format that this code will fail to understand?

---

_Comment by @cesarb on 2017-01-08 01:16_

> Could you perhaps say more about (2)? I've never worked with these formats in detail before, so I'm pretty blind to their inner workings. How will it fail? Are their different/esoteric versions of this format that this code will fail to understand?

First, you need a copy of APPNOTE.TXT. You can find a good one at https://pkware.cachefly.net/webdocs/APPNOTE/APPNOTE-6.3.3.TXT or elsewhere. I wrote the whole code based on that, and it should be much easier to understand with that reference in hand.

A ZIP file conceptually has two parts: the files, each one preceded by a "local file header", and at the end a "central directory". The best way to read a ZIP file is to seek first to the very end of the file, where you'll find an "end of central directory record", which has a field pointing to the start of the central directory. Of course, that way won't work for me, since I won't have `Seek` in all cases.

The alternative way of reading the ZIP file, which doesn't need `Seek`, is to read each "local file header", which has the "compressed size" of the file data, and after that many bytes you should find either another "local file header", or one of the central directory entries. Of course, things are not so simple in practice.

The first gotcha is that the compressed file might be larger than 4GiB. To find the real compressed size in that case, you have to parse the ZIP64 extension header.

The second gotcha is when the ZIP file was created as a stream (so it couldn't `Seek` back to write the size after the compression). In that case, the compressed size can be found in a "data descriptor" _after_ the compressed data. The way out from that catch-22 is that the deflate stream has a "last block" marker, so you can inflate until it finishes, and the stream should then be at the start of the "data descriptor".

Of course, things are not so simple. Originally, the "data descriptor" had no magic number, but one was added later, so it might or might not have a magic number, and you have to account for both alternatives. And, just for extra fun, when the ZIP64 extension header is present in the "local file header", the "data descriptor" has a different format (u64 instead of u32 for the sizes).

----

So, a quick fly-over of the `zip::for_each_entry` function. The intent of that function is that, for each file within the ZIP file, it will call a closure, passing it the name of the file, and either a `Read` for its contents, or an explanation of why it can't be read (as an `Err`).

Each pass over the `loop` is a file within the ZIP file. First I check the signature. If it's the "local file header", I go on; if it's something which I believe indicates the start of the "central directory", I exit; otherwise, either the code got lost or the file is corrupted, and I return an error.

Then I read the header itself, which is a bunch of fixed-size fields followed by a pair of variable-size fields. The first one is the name (in which encoding is a question for later), and the second one contains the extension headers. I parse the extension headers (each has a tag and size, so I can skip over unknown ones); I do a lenient parse (accepting an early exit) since the ZIP64 header is usually the first one and it's the one I'm the most interested in. I also parse and validate the Info-ZIP Unicode path extension, if present.

Then I combine the header with what I found in the extensions: I get the sizes from the ZIP64 header, and convert the file name to UTF-8. If flag bit 11 is set, the file name is already in UTF-8; otherwise, if the Info-ZIP Unicode Path header is valid, I use it; otherwise, the file name should be in CP437 (see Appendix D).

Next I determine how the file is encoded. I understand the "version needed to extract" up to 4.5, I don't understand encrypted or patched files, and I understand only methods 0 (store) or 8 (deflate).

The final block of code is decoding the file. The easiest case is the second one, when flag bit 3 is unset: the compressed size is present. I create an `io::Take` to contain the data, pass it to the callback (inflating if needed), and drain whatever the callback didn't read so the stream points to the next "local file header".

The hard case is when the compressed size is not present, since I cannot create an `io::Take` over the whole compressed data. Instead, I create an `io::Take` just as a counter: its `.limit()` will let me figure out how many bytes I did read. I can only work with compressed data, and I drain within the compressed data to make sure it's at the end if the callback didn't eat it all.

Then I parse the "data descriptor", which might or might not have a magic number, and might be 32-bit or 64-bit, and check to see if the amount of data I consumed (tracked by the `Take`) matches the compressed size, which now I know. If it doesn't match, we got out of sync, so I have to abort.

----

There's plenty this code doesn't support. I focused on the subset used by standards like OpenDocument (see https://en.wikipedia.org/wiki/Zip_(file_format)#Standardization), which should be enough for most ZIP files you'll find in the wild. Most of what isn't supported is encryption (which makes no sense for ripgrep), advanced compression formats like bzip2 or LZMA (which most ZIP creators won't create by default, for compatibility reasons), old compression formats (which nobody uses since deflate is always better), or esoteric features like splitting a ZIP file into multiple floppy disks.

---

_Comment by @BurntSushi on 2017-01-08 02:47_

Thanks for that writeup! It was very useful.

I'd like to noodle on this for a bit. This is a big feature and I want to be 100% on it before merging. Once this becomes available, it will be very hard to back out of it.

---

_Comment by @cesarb on 2017-01-08 13:08_

> In practice, we probably aren't getting pure Rust versions of industrial strength decompressors for common formats any time soon.

If any compression format is going to get a good quality unsafe-free pure-Rust decompressor soon, it's deflate. It's that popular. I just found two pure-Rust decompressors, https://github.com/PistonDevelopers/inflate used by the `png` crate, and https://github.com/sile/libflate which is quite new.

In particular, this comment (https://github.com/PistonDevelopers/image-png/pull/46#issuecomment-270968040) mentions adding a feature to the `flate2` crate (which is the one I'm using) to expose its API on top of one of these pure Rust implementations, so needing C code for deflate should be a temporary situation.

---

_Comment by @BurntSushi on 2017-01-08 20:43_

@cesarb That's really cool. I care enough about using pure Rust that I will look into modifying this PR to use one of those libraries if you don't mind. :-) I'm not sure when I'll get to it though!

---

_Comment by @scottchiefbaker on 2017-01-12 21:58_

> If any compression format is going to get a good quality unsafe-free pure-Rust decompressor soon, it's deflate.

What does **unsafe-free** mean in this context?

---

_Comment by @BurntSushi on 2017-01-12 22:03_

> What does unsafe-free mean in this context?

It just means that the code doesn't explicitly use `unsafe` anywhere.

(I'm skeptical that can be done while being competitive in performance.)

---

_Comment by @cesarb on 2017-01-12 22:53_

> (I'm skeptical that can be done while being competitive in performance.)

It doesn't matter, you know someone is going to try, just for the bragging rights. Look, no ~~hands~~ unsafe!

---

_Comment by @tanriol on 2017-01-17 23:30_

I'd probably suggest that iterating over zip archive entries without seeking may be generally useful functionality (unzipping directly from a stream without having to write it out as a whole) that should be either a separate crate or maybe even integrated into the [zip](https://github.com/mvdnes/zip-rs) crate.

---

_Comment by @BurntSushi on 2017-03-13 00:40_

I realize it's been a while since I commented on this, but I've given this a lot of thought. As cool as this functionality as, I think I have to close it. My first and most important concern is that new features generally implies extra maintenance burden. This *particular* feature is something that I know will be a real headache to deal with. The issue is on two fronts:

1. This introduces C dependencies. While it seems doable to continue to produce static executables, it complicates the build process. This is *not* something I want to be debugging on Windows either.
2. Supporting compressed formats begs the addition of more formats, or worse, more esoteric versions of existing formats. (This is a little bit of a cop-out on my part, I admit.)

It's hard for me to say what adding this kind of feature would actually mean. I would probably like to see a separate crate that is well maintained for this purpose in a way that isolates ripgrep from the gory details. As it stands now, there's a bit too much in ripgrep proper in this PR. Preferably, this separate crate would be pure Rust.

None of this is really set in stone. I might be able to bend on this over time. But right now, I'd like to shelve the idea.

@cesarb Thanks so much for the work on this. There's no doubt this PR will be useful when ripgrep is ready for a feature like this.

---

_Closed by @BurntSushi on 2017-03-13 00:40_

---
