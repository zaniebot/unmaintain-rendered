```yaml
number: 398
title: Add support for additional text encodings.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: utf16
created_at: 2017-03-09T01:33:29Z
updated_at: 2017-04-12T11:42:04Z
url: https://github.com/BurntSushi/ripgrep/pull/398
synced_at: 2026-01-12T18:23:13Z
```

# Add support for additional text encodings.

---

_@BurntSushi_

Add support for additional text encodings.

This includes, but is not limited to, UTF-16, latin-1, GBK, EUC-JP
and Shift_JIS. (Courtesy of the `encoding_rs` crate.)

Specifically, this feature enables ripgrep to search files that are
encoded in an encoding other than UTF-8. The list of available encodings
is tied directly to what the `encoding_rs` crate supports, which is in
turn tied to the Encoding Standard. The full list of available encodings
can be found here: https://encoding.spec.whatwg.org/#concept-encoding-get

This pull request also introduces the notion that text encodings can be
automatically detected on a best effort basis. Currently, the only
support for this is checking for a UTF-16 bom. In all other cases, a
text encoding of `auto` (the default) implies a UTF-8 or ASCII
compatible source encoding. When a text encoding is otherwise specified,
it is unconditionally used for all files searched.

Since ripgrep's regex engine is fundamentally built on top of UTF-8,
this feature works by transcoding the files to be searched from their
source encoding to UTF-8. This transcoding only happens when:

1. `auto` is specified and a non-UTF-8 encoding is detected.
2. A specific encoding is given by end users (including UTF-8).

When transcoding occurs, errors are handled by automatically inserting
the Unicode replacement character.

In all other cases, the source text is searched directly, which implies
an assumption that it is at least ASCII compatible, but where UTF-8 is
most useful. In this scenario, encoding errors are not detected.

This design may not be optimal in all cases, but it has some advantages:

1. In the happy path ("UTF-8 everywhere") remains happy. I have not been
   able to witness any performance regressions.
2. In the non-UTF-8 path, implementation complexity is kept relatively
   low. The cost here is transcoding itself. A potentially superior
   implementation might build decoding of any encoding into the regex
   engine itself. In particular, the fundamental problem with
   transcoding everything first is that literal optimizations are nearly
   negated.

Future work should entail improving the user experience. For example, we
might want to auto-detect more text encodings. A more elaborate UX
experience might permit end users to specify multiple text encodings,
although this seems hard to pull off in an ergonomic way.

Fixes #1

---

**Initial PR message:**

This is in-progress work on adding UTF-16 support to ripgrep.

This "works," but it needs to be cleaned up a bit before it can be merged:

* [x] Add tests where the input is UTF-16.
* [x] Add a flag that permits controlling this behavior. (We could elect to punt on this if the right UX choice isn't obvious.)
* [x] When reading anything but UTF-8, we need to disable the memory map searcher. It's not practical to transcode a memory mapped file in a way that isn't isomorphic to just using the normal incremental `io::Read` interface. (This should be done optimistically by "bailing" out of the memory map strategy if there's a BOM rather than pessimistically to avoid additional syscalls in the happy path.)
* [x] The API for `DecodeReader` needs to be tweaked so that its internal buffer can be reused. (Without this, the normal UTF-8 happy path causes ripgrep to be slower than grep!)

If you want to try this PR out (which would be most welcome), then because of the memory map issue mentioned above, you'll need to pass the `--no-mmap` flag to ripgrep.

To a first approximation, ripgrep is approximately 5x slower searching UTF-16 when compared to UTF-8. Interestingly, this applies both to searching a single ~1GB subtitle file and searching the Linux repo with all source files converted to UTF-16. At first I thought this seemed bad, but `iconv` itself isn't much faster---certainly within the same order of magnitude it seems.

Another exciting thing I learned is that GNU grep doesn't support searching UTF-16 at all. For some reason, I thought it did. So this is an even better feature than I previously thought!

---

_Assigned to @BurntSushi by @BurntSushi on 2017-03-09 01:33_

---

_Comment by @BurntSushi on 2017-03-09 01:33_

cc @roblourens

---

_Comment by @BurntSushi on 2017-03-11 16:33_

OK, I've updated the PR to take care of the internal buffer and memory map issue. I also refactored the transcoder to be more robust.

I'd like to punt on adding new flags for now, which I think means I just need to work on writing a few more tests and this should be good to go!

---

_Renamed from "Add UTF-16 support." to "Add support for additional text encodings." by @BurntSushi on 2017-03-12 18:10_

---

_Comment by @BurntSushi on 2017-03-12 18:25_

(Build failures are unrelated. There's a bug in nightly Cargo that's being fixed.)

---

_Comment by @BurntSushi on 2017-03-12 18:26_

I've updated the PR with tests. I also ended up adding support for all encodings supported by `encoding_rs`, which includes UTF-16, GBK, EUC-JP and Shift_JIS, among others. I added a new `-E/--encoding` flag that permits controlling which text encoding is used. The default value is `auto`, which basically means "assume UTF-8 unless we see a UTF-16 bom."

---

_Merged by @BurntSushi on 2017-03-12 23:54_

---

_Closed by @BurntSushi on 2017-03-12 23:54_

---

_Branch deleted on 2017-03-12 23:54_

---

_@hsivonen reviewed on 2017-04-12 09:57_

---

_Review comment by @hsivonen on `src/args.rs`:747 on 2017-04-12 09:57_

Considering that the `None` case results in error rather than continuing with some default value, it would make more sense to use the `for_label_no_replacement` variant here.

---

_Review comment by @BurntSushi on `src/args.rs`:747 on 2017-04-12 11:42_

TIL about the replacement encoding: https://encoding.spec.whatwg.org/#replacement

Thanks for the tip! :-)

---

_@BurntSushi reviewed on 2017-04-12 11:42_

---
