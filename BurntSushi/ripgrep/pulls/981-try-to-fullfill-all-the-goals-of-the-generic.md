```yaml
number: 981
title: Try to fullfill all the goals of the generic decoder program feature
type: pull_request
state: closed
author: c-blake
labels: []
assignees: []
base: master
head: master
created_at: 2018-07-13T14:30:59Z
updated_at: 2018-07-21T21:25:13Z
url: https://github.com/BurntSushi/ripgrep/pull/981
synced_at: 2026-01-12T18:23:13Z
```

# Try to fullfill all the goals of the generic decoder program feature

---

_@c-blake_

request: https://github.com/BurntSushi/ripgrep/issues/978

There are a a bunch of choices maybe I should mention in this PR.
I called it "-P,--preprocessor" to suggest its primary function.
I basically just imitated the way decompression was handled with
a new file ``src/preprocessor.rs`` instead of ``src/decompressor.rs``.

Currently, if both ``--search-zip`` and ``--preprocessor PROGRAM`` are
given,  the latter is used.  This seemed reasonable since preprocessing is more
general and probably involves more sophisticated users who can more easily
include whatever compression programs they want (or don't want) using whatever
dispatching algorithm they want.

For me, it compiles and runs fine on rust-1.27.1 both with no ``-P`` at all,
with ``-P program-using-only-stdin`` and ``-P program-using-argv1``.  The only
failing I see right now on Linux is that if you specify a bogus preprocessor
like ``rg -P /junk foo`` it does not error out at the very first file.

Oh, and while the shell script snippet formats fine in the --help output, the
auto-generated man page corrupts it into a 2- or 3-liner with some chars
dropped out.  Not sure what to do about that.  Could also just put that
material in GUIDE.md and drop it from the help.

Also, this is my very first significant stab at Rust work.  So, I may well
have done some things in an undesirable way.

---

_@okdana reviewed on 2018-07-13 15:55_

---

_Review comment by @okdana on `src/app.rs`:1467 on 2018-07-13 15:55_

Quoting `COMMAND FILE` doesn't seem right; place-holders aren't quoted anywhere else in the help text. Also you switch between `COMMAND FILE` and `COMMAND`; `COMMAND` seems simpler (or maybe `PROGRAM` if you're worried that's too vague). Also `deactivates` is misspelt

---

_Review comment by @okdana on `src/app.rs`:1488 on 2018-07-13 16:00_

Usually when one option overrides another (as you said this does with `-z`), you put that in the help text and then specify it here. You can see examples of that elsewhere in the file

I actually would have suggested making them *conflict*, because it's not intuitively obvious how they should behave together, but then i thought that a lot of people probably add `-z` to their aliases/configs, so that might be too inconvenient. Not sure

---

_@okdana reviewed on 2018-07-13 16:00_

---

_@okdana reviewed on 2018-07-13 16:09_

---

_Review comment by @okdana on `complete/_rg`:176 on 2018-07-13 16:09_

If you're going to put these together, i would suggest renaming the group and changing the comment, since it's no longer accurate. Maybe `(preprocessor-zip) # File-preprocessing options` or something, to match the way `pretty-vimgrep` is done

Also, the spec isn't right. It should be something like:

```
{-P+,--preprocessor=}'[specify preprocessor utility]:preprocessor utility:_command_names -e'
```

(`_command_names -e` will prefer `PATH` executables, but also do the right thing if you try to complete an actual file path)

Also, it probably should be placed *before* the other two options, since they were meant to be... semi-alphabetically ordered :/

---

_@c-blake reviewed on 2018-07-13 16:41_

---

_Review comment by @c-blake on `src/app.rs`:1467 on 2018-07-13 16:41_

Ok.  Dequoted & fixed spelling.  I use COMMAND FILE when there is also a FILE in the sentence context and COMMAND when it's just about the program.

---

_@okdana reviewed on 2018-07-13 16:42_

---

_Review comment by @okdana on `src/app.rs`:1467 on 2018-07-13 16:42_

Ah, i think i see what you mean

---

_Comment by @phiresky on 2018-07-13 16:50_

Just as an example of why this would be awesome: Together with [this caching pdftotext wrapper](https://gist.github.com/phiresky/5025490526ba70663ab3b8af6c40a8db) as a preprocessor this is able improve on pdfgrep by orders of magnitude:

On a semi-large directory of pdfs:


`time pdfgrep -r IP >/dev/null`

`3,20s user 0,08s system 99% cpu `**3,284 total**

`time rg --preprocessor pdftotext-cached.sh IP >/dev/null`

`0,12s user 0,04s system 374% cpu `**0,035 total**


That's almost a 9000% performance improvement.

(Even without caching it only takes 0.5s, not sure what the pdfgrep people are doing)

---

_Comment by @c-blake on 2018-07-13 17:23_

Yeah...Besides lecture slides I have a slew of papers I've collected over the years, as I expect many others have.  I use this all the time with a custom GNU grep patch I did that does basically the same thing.  Line numbers might be nice and all, but honestly I mostly use this with a specific enough pattern and ``--files-with-matches``, myself.  My use case is usually..."What was that paper or two that did xyz again?".

As mentioned in the feature request, applications are really bounded only by your imagination.  The searching through only the parts of context diffs with actual changes, conceivably even (trustworthy) foreign language translations if there was a good library/CLI tool for that, ``rg -P enFrancais`` or whatever.

Caching transformers surely can speed things up at the expense of disk space to maintain the cache.  Personally, my archives are small enough that I just re-decode on the fly.  The parallel operation of ``rg`` really helps with transformation time, actually.  Indeed, if one is willing to spend the space for a full cache, maintaining it with ``make`` or whatever, it will always be faster to run ``rg`` on that shadow file hierarchy and transform the filenames in the output (if you even need to).

 To get even as low as 70 us/file + 0.34 sec/GB on my box at home, I have to have a statically linked classifier that does its own pass-through for "uncoded data" _and_ trim my environment to just $PATH.  That may not sound like much overhead, but the microseconds and milliseconds of overhead pile up when you have 10s of thousands of files on fast NVMe storage or buffered in RAM and _most but not all_ of that is uncoded data.  The per-byte costs come from copies over the pipe buffer.  My email history is sort of like that.  Anyway, the real user time of those overheads is usually smaller/around the same as my time to enter a pattern and interpret the results.  A less careful management of the overheads can easily blow that up by 10X or more (some fancy dynlinked bash script dispatcher type stuff to dynlinked cat with a big environment, etc.) which pushes it into annoying territory (at least for me).

---

_Review comment by @okdana on `complete/_rg`:173 on 2018-07-13 17:32_

Missed the comment here

---

_@okdana reviewed on 2018-07-13 17:32_

---

_@c-blake reviewed on 2018-07-13 17:35_

---

_Review comment by @c-blake on `complete/_rg`:173 on 2018-07-13 17:35_

Oops.  Did you want that ``-E,--encoding`` option down in that input-decoding group, too?  Instead of "misc/other"?

---

_Review comment by @okdana on `complete/_rg`:173 on 2018-07-13 17:41_

No, these options were grouped together because they're completed exclusively of each other (that's what the `(...)` in the group name means). `-E` is independent and doesn't have any particular relationship with other options, so it can be dumped in with the miscellaneous stuff. I can see how the group name `input-decoding` might kind of imply that relationship, which is why i'd suggested `preprocessor-zip` or something more specific like that; doesn't matter too much tho

---

_@okdana reviewed on 2018-07-13 17:41_

---

_@c-blake reviewed on 2018-07-13 17:46_

---

_Review comment by @c-blake on `complete/_rg`:173 on 2018-07-13 17:46_

Ah.  Ok.  I see.  Well, if someone optimizes some other common case and adds another option like --search-pdf (just as an example) we have a general group name.  Doesn't matter to me unless someone complains.

---

_@BurntSushi approved on 2018-07-21 18:56_

This looks great! For your first Rust code, this ain't bad at all! :-)

I am going to clean this up with a variety of minor nits, but overall, your approach looks solid!

---

_Comment by @BurntSushi on 2018-07-21 19:08_

@c-blake Do you want to think of a different short option for this other than `-P`? I have something else in mind for use with `-P` so I'd rather not use it for this. I might even prefer not to allocate a short option for this at all, since it seems like a less common feature and short flag real estate is precious. Perhaps we could shorten the flag down to just `--pre`?

---

_Comment by @BurntSushi on 2018-07-21 20:19_

Some notes from working through the code and cleaning it up:

* `PreprocessorReader` does not attach the given file to stdin, which meant that preprocessor scripts couldn't actually read from stdin.
* The preprocessor isn't active when ripgrep is searching stdin. This seems OKish to me, but I've added it to the documentation.
* The preprocessor itself probably shouldn't be emitting log messages. Instead, it should return an error and the caller should decide how and when to print it. (In this case, that's the worker.) I see that this style was probably emulated from the decompressor implementation, but the decompressor should probably be updated in this vein as well. (Although, IIRC, the decompression stuff has slightly different constraints for when to emit error messages.)
* I've fixed up the ill-formatted script example for the man page.
* I've removed the `-P` short option and changed the long option to `--pre`.
* I've changed the representation of the preprocessor from `PathBuf` to `Option<PathBuf>` to better communicate that it is optional.
* I briefly investigated producing a better failure mode when the preprocessor command simply doesn't exist, but it looks like Rust's standard library doesn't expose its path resolution logic for commands, and I didn't want to try and re-litigate that. We could actually attempt to start the process, but then that would need to get added to the command interface, which is too deliciously simple to disturb IMO.

I've opened #989 with these changes.

---

_Closed by @BurntSushi on 2018-07-21 21:25_

---
