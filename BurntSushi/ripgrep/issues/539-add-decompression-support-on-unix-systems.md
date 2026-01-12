```yaml
number: 539
title: add decompression support on Unix systems
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-07-04T13:51:31Z
updated_at: 2019-04-17T18:10:04Z
url: https://github.com/BurntSushi/ripgrep/issues/539
synced_at: 2026-01-12T16:13:22Z
```

# add decompression support on Unix systems

---

_@BurntSushi_

@wavexx [Suggests farming out decompression to established stream decompressors](https://github.com/BurntSushi/ripgrep/issues/225#issuecomment-312873930). I am somewhat attracted to this path since it will cover a lot of use cases, and at least on the face of things, seems relatively easy to implement. If this turns out to be a much larger project than I anticipated, then the trade off may no longer make sense. The intention is to support decompression on all supported platforms, so we don't want to invest a bunch of work in a Unix-only option.

I'm not sure when I personally will have time to work on this, but as long as it's easy to maintain, I'm not intrinsically opposed to farming this out to another process. Note that the core search code can work on *any* arbitrary thing that implements [`io::Read`](https://doc.rust-lang.org/std/io/trait.Read.html), which means the glue code required for this should be pretty limited. There's already an example of this at play for encoding support, where any supported encoding is first transcoded to UTF-8 before being searched. The scope of that addition was pretty much limited to [implementing `io::Read`](https://github.com/BurntSushi/ripgrep/blob/master/src/decoder.rs) and doesn't require touching any of the core search code.

If someone else wanted to work on this, I'd be happy to mentor it. I haven't thought about it too much, but I think these are probably the high level steps one needs to take:

* Come up with a way of detecting when a certain type of decompressor is needed. It's probably sufficient to use the file extension as a first pass, although one should consider special cases like `.tar.gz` (probably by skipping them at first, but maybe eventually trying to read `tar` archives as well).
* Implement glue code that takes as input a command that executes stream decompression on a specific file path and outputs an implementation of `io::Read` that is suitable to pass to the `io::Read` searcher.
* Think about what a path forward looks like for replacing out-of-process decompression with in-process decompression. For example, if and when Rust gets a native gzip decompression library that is suitable for use (maybe it already is), then how could it be used to replace use of, say, the `gzip` command seamlessly? This will be important for eventually migrating to cross platform decompression support.
* Give some thought to Windows support. It doesn't actually seem like this is inherently Windows-incompatible, but may favor Unix systems by virtue of the tooling typically available in a Unix environment.
* Initially ignore any and all decompression on `stdin`. Callers can invoke their own decompressor in this case easily enough that we shouldn't do anything smart. (A future path might add a flag to force decompression of a specific format.)

---

_Label `enhancement` added by @BurntSushi on 2017-07-04 13:51_

---

_Label `help wanted` added by @BurntSushi on 2017-07-04 13:51_

---

_Comment by @phiresky on 2017-10-06 15:23_

> Note that the core search code can work on any arbitrary thing that implements io::Read, which means the glue code required for this should be pretty limited

If that's the case, it seems like it would be easy to implement an independent rust library which is able to transparently open compressed files as a io::Read using any method (streaming external tool via pipes, C FFI or native Rust library), without ripgrep itself actually caring about the method.

It still needs some thought though:
1. Which format maps to which decompressor. I think there is always only one correct solution to decompressing a given file so this should not need to be configured. Might be good to use [magic](https://github.com/robo9k/rust-magic) though to detect the file type for differing file endings (all the zip-derived formats like  jar, apk, docx, odt)
2. Buffering - gunzip et al. might buffer their output which looking at the man page cannot be configured
3. What about archives? io::Read only works cleanly when a single compressed stream maps to a single file

---

_Comment by @BurntSushi on 2017-10-06 15:27_

We should probably punt on archives for now.

---

_Comment by @tailhook on 2017-12-02 00:28_

What to do think about [anycat](https://crates.io/crates/anycat) crate for the task?

---

_Comment by @BurntSushi on 2017-12-02 00:36_

@tailhook No, I don't think so. That seems like a nifty little utility, but it doesn't pull its weight as a library dependency. Also, that's using C libraries, which I talked about in #225. This particular ticket is strictly for an implementation path that involves shelling out to existing utilities.

---

_Comment by @balajisivaraman on 2018-01-02 18:51_

I had a quick glance at this and had the following thought. The standard library provides a `Command` implementation that lets us spawn child processes from the currently running process. It also handily lets us pipe from the process' `stdout`. And the `stdout` already has the `Read` trait implemented for it.

By using the above, I was able to get a very trivial solution for this problem by just doing the following.

```
use std::process::Command;
use std::process::Stdio;
match Command::new("gunzip")
      .args(&["-c", "/home/balaji/Projects/rust/samples/test.gz"])
      .stdout(Stdio::piped())
      .spawn() {
         Ok(cmd) => self.search(printer,path, cmd.stdout.unwrap()), //Note: This is calling the worker's search method with the reader obtained from Pipe
         _ => Ok(0)
       }
```

I tested it out and it was able to spawn the process, do the unzipping and print the expected output to stdout.

@BurntSushi, My apologies if this looks like a very naive solution and not what you were looking for at all. The only reason I wanted to post that line of thought here is that I felt it achieved the intended purpose: Support decompression on unix systems at minimal cost to the maintainer for the time being.

Please let me know whether this is what you had in mind when you opened this ticket.

---

_Comment by @BurntSushi on 2018-01-02 18:58_

@balajisivaraman Woohoo! Yeah that is exactly what I had in mind. :) The trickier issues are thinking through the UX I think. I don't have time to rethink it through all right now but I can rattle off some general principles:

1. The absence of decompression commands should not cause ripgrep to fail. I do suspect a debug message would be appropriate.
2. If a decompression command runs but then fails, then ripgrep should probably emit an error message but other continue searching. (Similar to how it handles files that it doesn't have permission to read.)
3. What, if any, CLI flags do we expose related to this feature? Perhaps it's best to start by exposing none and simply recognize a hard-coded set of file extensions that we map to a particular kind of decompression command.
4. We likely need to be a touch smart about what we search. For example, it seems like we would want to search something like `titles.csv.gz` but perhaps not `ripgrep-0.7.1.tar.gz`, even though they both have `.gz` extensions. Perhaps anything with a `.tar.{foo}` extension is skipped by decompression? Not sure. (Keep in mind that ripgrep would currently ignore such things today because it's likely to detect it as a binary file very quickly. But in this case, it's not clear we want to do that after decompression since decompression itself could be quite costly.)

There are maybe other things I've missed. :)

---

_Comment by @balajisivaraman on 2018-01-02 19:10_

Yay! :smiley: 

Like you, I will also do some thinking on this and arrive at a cohesive list of things on how the UX should be by maybe tomorrow; adding to the ones you've already posted. Once that is finalised and agreed, I can actually take this up and start working on it, if that is OK with you. 👍 

---

_Comment by @BurntSushi on 2018-01-02 21:43_

@balajisivaraman Yeah that would be great! This would be a significant contribution. It's a really nice feature to have in ripgrep.

When thinking through the UX, keep in mind that "let's punt" is a totally valid answer. Keeping things simple and building on it later is a totally fine way to do things. The only downside here is that you need to be a little careful to leave yourself breathing room with respect to breaking changes. i.e., Adding a new flag for this and then removing that flag later is probably something we want to avoid (but adding no flags now and adding more flags later is OK!). (Not that I'm saying we should or shouldn't add flags for this, it's just an example.)

---

_Comment by @balajisivaraman on 2018-01-03 18:50_

Ok, I've thought about it a fair bit and here are my thoughts, partly summations of BurntSushi's:

- Going forward, when `rg` encounters any file ending with `.bz2`, `.gz`, `.xz` or `.lzma`, it will automatically use the corresponding decompressors (`bunzip2`, `gunzip`, `unxz` or `unlzma`) to perform the decompression as a child process. It will then use the `Read` instance obtained from that process' `stdout` to perform the search. (Note: One advantage we get by doing it this way is that it doesn't affect the existing UTF-16/32 transcoder, so if the archived file is encoded differently, we can still handle it.)

  - If the expected binary itself is not available, `rg` will gracefully exit and output a corresponding error message to the user. (Note: The binary is expected to be on the user's path and will *not* be fully qualified to a default location such as `/usr/bin` or `/usr/local/bin` for obvious reasons to account for varying install locations.)
  - If the executed command fails for any reason at all, then `rg` will fetch the error message from that process' `stderr`, display it, and continue searching the rest of the files.

- As of right now, `tar` archives will not be supported. If they are encountered, `rg` will fail gracefully letting the user know that it is currently unsupported, before doing any decompression. I feel this is better instead of continuing with the current method of skipping binary files. Users should probably be made aware of why their `.tar.gz` or `.tar.xz` files are not being processed by `rg`.

- I'm currently in favour of exposing a CLI flag similar to what `ag` does with its `-z`/`--search-zip` flag. My thought is based mostly on my use-case where I have many compressed log files in the same location as the currently being written to log file. I would want `rg` to skip the compressed files by default unless I specifically ask it to search through multiple bulky log `.gz` files. (Also the fact that searching archives will be slower is another reason we may want to go for a CLI argument. If users find that searches in some directories are suddenly slower because we're spawning a child process internally, they might be confused. By leaving it to the user's choice, we don't have this problem.)

- My idea is to have the detection and decompression of files in a separate module so that it'll be easier to change when Rust gets its own decompression libraries. (We have some WIPs already, with the most famous one being [flate](https://github.com/alexcrichton/flate2-rs/) which does pull in C bindings, but there's work being done on a [pure-Rust](https://crates.io/crates/miniz_oxide) lib.) With the actual decompression code currently planned for being somewhat lightweight and exposing a `Read` instance by default, we should have no issues replacing it with a library which also provides us with a `Read` instance.

- Also another thought I had is that the current line of implementation will not have any adverse impact on `rg`'s processing speed or parallel capabilities, outside of the performance loss we get from invoking the decompression in a sub-process, which is expected. Seeing as this feature will be implemented as part of the `worker` itself, which to my understanding is invoked by the parallel walker in case of multiple files. So we should be OK on this.

- **Windows Support**: This one's a bit of a challenge. I tried building `rg` with my trivial hack to see whether it can work on a Windows 10 machine I have.

    - Good news is that because we're using Rust's own `Command` and `Pipe` capabilities, they work out of the box.
    - The challenge is detection of binaries. If I simply use `bunzip` and I have the binary installed by "Git for Windows" on the Path, it still fails saying *It is not a valid Win32 application*. I tried building with Rust-Gnu as well to no avail. I have a feeling this could be made to work with some manouvering, as long as the binary itself is available on the user's path. I'll see what I can do.

I can't think of any more UX/code-maintencance/platform-support related issues, though I'm sure I will start discovering them as I begin working on the feature. If this sounds good to you, I can begin working on this right away. (I'll mostly put effort into this over nights and weekends.)

---

_Comment by @lnicola on 2018-01-03 19:04_

> The challenge is detection of binaries. If I simply use bunzip and I have the binary installed by "Git for Windows" on the Path, it still fails saying It is not a valid Win32 application. I tried building with Rust-Gnu as well to no avail. I have a feeling this could be made to work with some manouvering, as long as the binary itself is available on the user's path. I'll see what I can do.

I no longer have Windows to test this, but that sounds like a 32/64-bit mismatch. You should check Event Viewer for any errors in the Application section, or maybe try `depends`. It might be that you have a DLL twice in your `PATH` and `bunzip` is loading the wrong one.

In any case, this should probably be left for the user to handle.

---

_Comment by @okdana on 2018-01-03 19:42_

>Going forward, when rg encounters any file ending with .bz2, .gz, .xz or .lzma, it will automatically use the corresponding decompressors (bunzip2, gunzip, unxz or unlzma) to perform the decompression as a child process. 

Other compression-only formats to think about:

* Brotli (`.br`, `brotli -d`)
* Compress (`.Z`, `uncompress`)
* LZ4 (`.lz4`, `lz4cat` or `unlz4`)
* LZO (`.lzo`, `lzop -d`)

Of those, Compress and LZ4 are the most common i believe. `uncompress` is installed on most UNIX systems by default, and although `lz4` isn't it's used for compressing Linux kernel images, core dumps, &c. Doubt any of them are as commonly used for text as the others you mentioned tho

>As of right now, tar archives will not be supported. If they are encountered, rg will fail gracefully letting the user know that it is currently unsupported, before doing any decompression.

Only if they're supplied directly on the command line, i assume? Or in the debug messages maybe? It would be irksome if `rg` were to flood you with routine notices about not supporting files it encounters in the directory tree

>I'm currently in favour of exposing a CLI flag similar to what ag does with its -z/--search-zip flag.

I agree, i think it would make sense to have a flag that toggles compressed-file support as a whole on or off. That would surely be relevant no matter how the feature may be implemented in the future. (I suppose that users will still be able to ignore files with `-g`, but if there's going to be support for several different formats that might be tedious — and this ties in with the following)

---

I wonder also about how `rg` should handle files that *look* like they're compressed but aren't. e.g., if i incorporate the LZ4 library into a project maybe i'll have something like `README.lz4` in my repository — the `.lz4` here doesn't indicate compression, it's just a convention to append things to `README`. LZ4 is perhaps a bad example since `lz4cat` actually has the `-f` option to make it pass uncompressed files through as-is — most other decompression tools don't have anything like that though

I assume that scenario doesn't come up often, but a possibility to consider

---

_Comment by @balajisivaraman on 2018-01-06 08:37_

> Only if they're supplied directly on the command line, i assume? Or in the debug messages maybe? It would be irksome if rg were to flood you with routine notices about not supporting files it encounters in the directory tree

I was thinking the errors would be output when the user specifically asks to search in archive files using the new `-Z/--search-zip` flag. If we're not going the CLI flag route, then we definitely should do what you suggested and only output the error when specifically asked to search a `tar` file.

> I wonder also about how rg should handle files that look like they're compressed but aren't. e.g., if i incorporate the LZ4 library into a project maybe i'll have something like README.lz4 in my repository — the .lz4 here doesn't indicate compression, it's just a convention to append things to README. LZ4 is perhaps a bad example since lz4cat actually has the -f option to make it pass uncompressed files through as-is — most other decompression tools don't have anything like that though

One option to circumvent this issue is to use the [magic](https://github.com/robo9k/rust-magic) crate suggested earlier in this thread, which is the equivalent of running the `file` command on the file. That way we can safely assume that the file contains the type of data the extension says it contains.

> I no longer have Windows to test this, but that sounds like a 32/64-bit mismatch. You should check Event Viewer for any errors in the Application section, or maybe try depends. It might be that you have a DLL twice in your PATH and bunzip is loading the wrong one.

Thanks for the suggestion. I'll cleanup my Windows installation of Rust and the binaries that I have to recheck this.

---

_Comment by @balajisivaraman on 2018-01-07 17:00_

@BurntSushi, I've begun working on this on a [branch](https://github.com/balajisivaraman/ripgrep/tree/fix_539) in my fork, going with the simpler file extension based detection instead of the fancier `magic` based detection. Please let me know your thoughts on whether we're OK with `rg` having CLI flag for this as mentioned above or not, and I'll incorporate that into the code accordingly. Thanks!

---

_Comment by @BurntSushi on 2018-01-07 17:21_

> If the expected binary itself is not available, rg will gracefully exit and output a corresponding error message to the user. (Note: The binary is expected to be on the user's path and will not be fully qualified to a default location such as /usr/bin or /usr/local/bin for obvious reasons to account for varying install locations.)

(It occurs to be that by "gracefully exit," you mean, "continue searching." Apologies if I misunderstood! But I'll leave the text below as I wrote it before I realize the potential misunderstanding.)

This is definitely not OK. ripgrep must not stop working just because an external program isn't installed. If the binary isn't available, then there are two reasonable choices I can see:

1. Emit a debug message explaining why decompression didn't happen. Otherwise, continue searching.
2. Emit a warning message explaining why decompression didn't happen. Otherwise, continue searching.

My preference is (1). It's the most conservative change and is pretty consistent with ripgrep's general behavior of silently ignoring files that it thinks is irrelevant. In the future, we may of course elect to do (2).

> If the executed command fails for any reason at all, then rg will fetch the error message from that process' stderr, display it, and continue searching the rest of the files.

This is good. Notably, I don't think this should be a debug message, but rather, an actual warning emitting to stderr. This is in contrast to how a missing binary is handled.

> As of right now, tar archives will not be supported. If they are encountered, rg will fail gracefully letting the user know that it is currently unsupported, before doing any decompression. I feel this is better instead of continuing with the current method of skipping binary files. Users should probably be made aware of why their .tar.gz or .tar.xz files are not being processed by rg.

I would like to see this be a debug message. In particular, it's consistent with how we handle other files that are ignored.

> I'm currently in favour of exposing a CLI flag similar to what ag does with its -z/--search-zip flag. My thought is based mostly on my use-case where I have many compressed log files in the same location as the currently being written to log file. I would want rg to skip the compressed files by default unless I specifically ask it to search through multiple bulky log .gz files. (Also the fact that searching archives will be slower is another reason we may want to go for a CLI argument. If users find that searches in some directories are suddenly slower because we're spawning a child process internally, they might be confused. By leaving it to the user's choice, we don't have this problem.)

I agree with adding this flag, and by implication, making decompression opt-in.

> My idea is to have the detection and decompression of files in a separate module so that it'll be easier to change when Rust gets its own decompression libraries. (We have some WIPs already, with the most famous one being flate which does pull in C bindings, but there's work being done on a pure-Rust lib.) With the actual decompression code currently planned for being somewhat lightweight and exposing a Read instance by default, we should have no issues replacing it with a library which also provides us with a Read instance.

:+1:

> Also another thought I had is that the current line of implementation will not have any adverse impact on rg's processing speed or parallel capabilities, outside of the performance loss we get from invoking the decompression in a sub-process, which is expected. Seeing as this feature will be implemented as part of the worker itself, which to my understanding is invoked by the parallel walker in case of multiple files. So we should be OK on this.

There is definitely a potential performance impact here by shelling out to another process as opposed to using a library, but that is certainly part of the trade off of doing things this way. Otherwise, yeah, fully agreed. :+1:

> Windows Support: This one's a bit of a challenge. I tried building rg with my trivial hack to see whether it can work on a Windows 10 machine I have.

Aye. I think my recommendation here is to start with something that doesn't do anything platform specific. That is, even if the implementation is always incorrect on Windows, we should be covered by the aforementioned error handling.

---

@okdana 

> I wonder also about how rg should handle files that look like they're compressed but aren't. e.g., if i incorporate the LZ4 library into a project maybe i'll have something like README.lz4 in my repository — the .lz4 here doesn't indicate compression, it's just a convention to append things to README. LZ4 is perhaps a bad example since lz4cat actually has the -f option to make it pass uncompressed files through as-is — most other decompression tools don't have anything like that though

That is interesting scenario! I think I'd suggest ignoring it for now, and fixing it if and when somebody files a bug for it. Then we can try and fix it from there. I'm somewhat more encouraged to say this is OK since this decompression behavior will be opt-in. @balajisivaraman I'd probably avoid `magic` and such for now, but certainly, we may elect to use something like that in the future.

---

_Comment by @BurntSushi on 2018-01-07 17:23_

@balajisivaraman Thanks so much for the detailed specification that you wrote! Truly a great example to follow. :-)
  

---

_Comment by @balajisivaraman on 2018-01-08 16:49_

@BurntSushi, Yeah, I meant that `rg` will do what it's meant to do and exit normally. (I can see now that what I wrote was very confusing.) Thank you for the clarifications and suggestions. I'll incorporate them when I'm working on it. 👍 

---

_Comment by @Dieken on 2018-01-28 14:59_

Repeat comment in #225,  @BurntSushi @wavexx

You can set environment variable `GREP` to "rg", then use `zgrep/bzgrep/xzgrep -Hn` to uncompress and search on .gz/.bz2/.xz/.lzma/.lzo with `rg`,  I feel this is somewhat enough :-)

Notice `/usr/bin/bz*grep` and `/usr/bin/z*grep` on Mac OS X don't support environment variable `GREP`, BSD tools suck...  You need install `gzip` and `xz` with Homebrew.

---

_Comment by @wavexx on 2018-01-28 21:49_

On Sun, Jan 28 2018, Liu Yubao wrote:
> Repeat comment in #225, @BurntSushi @wavexx
>
> You can set environment variable GREP to "rg", then use
> zgrep/bzgrep/xzgrep -Hn to uncompress and search on
> .gz/.bz2/.xz/.lzma/.lzo with rg, I feel this is somewhat enough :-)

I don't generally know what sort of compressed files I'm dealing with.
But setting this aside, this bypasses the directory walking mechanism in
rg itself which has larger implications.


---

_Comment by @Dieken on 2018-01-29 05:19_

`xz*grep` from package `xz` (or `xz-utils`) supports xz/lzma/gzip/bzip2/lzop, I guess this is enough for most people.

Usually searching compressed files is only required to handle log files or some archive files, no need to support language types, gitignore etc,  the bottleneck in combination of `find + xargs -P + zgrep + rg` probably isn't `find`.  Even if you want languge types/gitignore etc, you can use `rg --files -t xxx` to replace `find`.

Certainly, it would be great if ripgrep can directly support compressed file and tar files,  my workaround is just *somewhat enough* before tons of work to port and optimize those C decompression libraries to Rust.

BTW, after so many years,  GNU grep doesn't support compressed files and tar files too, it's a UNIX philosophy to combine and reuse existed little utilities.


---

_Comment by @BurntSushi on 2018-01-29 12:20_

@Dieken Thanks for pointing out a clever work around. I'm sure some people will find it useful. Let's continue to push forward with #751 (which I still need to review), which is a nice intermediate step between "a hack using existing tools" and "port all the things to Rust."

> BTW, after so many years, GNU grep doesn't support compressed files and tar files too, it's a UNIX philosophy to combine and reuse existed little utilities.

Let's try to avoid this type of argumentation. Appealing to the UNIX philosophy kills debates instead of letting them thrive. Reality will dictate. The inherent nature of a tool that does recursive search is that it *must* violate the UNIX philosophy because it is combining multiple distinct tasks (directory walking and line oriented searching) into one. We create this coupling for UX and performance reasons, and there is no turning back now.

(The irony is that, aesthetically speaking, I am someone that very much appreciates the UNIX philosophy  but I simultaneously maintain a tool that unquestionably violates it. The key is that the UNIX philosophy is a *means to an end*, not an end itself.)

---

_Closed by @BurntSushi on 2018-01-30 14:13_

---

_Comment by @BatmanAoD on 2019-03-21 17:38_

It looks like the `decompress` module is pretty easy to extend; would you mind if I open a pull request incorporating `unzip` support?


---

_Comment by @BurntSushi on 2019-03-21 17:54_

@BatmanAoD I very much doubt it's as easy as you claim, since zips are _archives_ and not just decompression.

Please move discussion to https://github.com/BurntSushi/ripgrep/issues/1112 which is more similar to zip support than this ticket is.

---

_Comment by @BatmanAoD on 2019-03-21 19:48_

True. I hadn't realized `7z` and the like are not themselves archive formats.

Using `unzip` to pipe decompressed data into the search _is_ trivial, but as noted in #1112 the line number and file name info wouldn't necessarily be helpful. However, if #1112 were implemented, I would expect it to be useful in conjunction with `--search-zip`, so that e.g. `.tar.gz` files would be extracted and decompressed if both options are used, but `--search-zip` on its own would (continue to) only decompress the archive without extracting it.

So to me, at least, it seems reasonable to implement simple decompression without extraction, since it can be a useful feature on its own.

This should be fairly easy to let users implement for themselves using `--pre` (albeit awkward, since `unzip` needs arguments in order to pipe its output). But the same is true of the other decompression utilities, so I think it makes more sense to just include `unzip` without (yet) supporting extraction.

---

_Comment by @ssokolow on 2019-03-21 21:20_

> I hadn't realized 7z and the like are not themselves archive formats.

I don't know where you got the impression that `7z` isn't an archive format.

`7z` is an archive format built around the LZMA compression algorithm, just as `zip` is an archive format built around the DEFLATE compression algorithm. (And optionally others too.)

The `gzip`-like "LZMA a single file" formats are `.lzma`, `.xz`, and `.lz`. (`.lzma` is the precursor to `.xz` and `.lz` was created by someone who [identified major flaws in `.xz`](http://www.nongnu.org/lzip/xz_inadequate.html).)

---

_Comment by @BatmanAoD on 2019-03-23 00:33_

@ssokolow Simply because `7z` is already supported by this RipGrep feature, so I figured that if archive formats were verboten, then none of the existing formats were archive formats.

Andrew, does the fact that we already have an archive format being decompressed but not extracted by the `-z` flag change your opinion on whether adding `unzip` would be appropriate?

---

_Comment by @BatmanAoD on 2019-04-17 18:08_

@BurntSushi Since RipGrep already supports decompressing-but-not-extracting LZMA, would you object to the same behavior (at least for now) for `unzip`?

---

_Comment by @BurntSushi on 2019-04-17 18:10_

I'd rather not, sorry. Please use `--pre` (and perhaps `--pre-glob`) as a work-around for now.

---
