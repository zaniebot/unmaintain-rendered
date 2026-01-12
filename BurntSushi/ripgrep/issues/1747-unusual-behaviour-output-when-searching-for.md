```yaml
number: 1747
title: Unusual behaviour/output when searching for binary (raw byte) patterns with --byte-offset
type: issue
state: closed
author: shabble
labels:
  - invalid
assignees: []
created_at: 2020-11-27T19:07:24Z
updated_at: 2020-11-27T23:23:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1747
synced_at: 2026-01-12T16:13:24Z
```

# Unusual behaviour/output when searching for binary (raw byte) patterns with --byte-offset

---

_@shabble_

#### What version of ripgrep are you using?

ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

manual install into /bin from github release zipfile

#### What operating system are you using ripgrep on?

Ubuntu 20.04 LTS
5.4.0-52-generic #57-Ubuntu SMP Thu Oct 15 10:57:00 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

#### Describe your bug.

`--byte-offset` appears to corrupt or confuse the output reporting when searching binary files for specific byte sequence patterns

#### What are the steps to reproduce the behavior?

```
    $ echo -ne '\x01\x00\x07\x0a\x00\x05\x0a' | rg --encoding=none --text --only-matching  --multiline --multiline-dotall -e     '\x07\x0a\x00..' | xxd              
    00000000: 070a 0005 0a                             .....
```

```
    $ echo -ne '\x01\x00\x07\x0a\x00\x05\x0a' | rg --encoding=none --text --only-matching  --multiline --multiline-dotall -e '\x07\x0a\x00..' --byte-offset | xxd
    00000000: 323a 070a 323a 0005 0a                   2:..2:...
```

Running with `--debug` produces the exact same following line in both cases (separated here for readability, so it's not in  the hex dump):

    DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes


#### What is the actual behavior?

See above.

The second (with `--byte-offset`) repeats the offset again at the nul byte, and splits the match

#### What is the expected behavior?

With byte-offset, I would expect something like:

```
(unhexed):

2: <binary match content>

(as hex):

0000000: 323a 070a 0005 0a     2:.....
```

That is, there should be an ascii number for the byte offset at the beginning, followed by a colon, then the complete matching binary content of the pattern, possibly with the whole match terminated by a newline or other record separator.


---

_Comment by @BurntSushi on 2020-11-27 21:01_

Hmm this output looks correct to me. Remember, ripgrep is a _line oriented_ search tool. Look at what first sentence of the docs say for the `--byte-offset` flag, emphasis mine:

> Print the 0-based byte offset within the input file **before each line of output.**

For example:

```
$ echo -ne 'foo\nbar\nbaz\n' | rg --multiline --multiline-dotall '.*' --byte-offset
0:foo
4:bar
8:baz
```

And that's exactly what's happening here in your example. ripgrep isn't splitting the match at the NUL byte, it's splitting the match at the line terminator. Replace NUL bytes with, for example, `\x03` and you'll see the same behavior.

---

_Closed by @BurntSushi on 2020-11-27 21:01_

---

_Label `invalid` added by @BurntSushi on 2020-11-27 21:01_

---

_Comment by @shabble on 2020-11-27 22:09_

Thanks, I managed to overlook that in the help when I was trying to figure it out, but just after opening this I was starting to suspect it was intentional in that it mimics the `--line-numbers` behaviour for multi-line, so this is indeed as-intended.

My usage is probably fairly niche, but there seem to be very few tools that can search arbitrary byte patterns in binary data, so I was hoping rg would save the day :-)

I don't know nearly enough about the details of how things operate internally, but how plausible would it be to support changing the line/record separator to an arbitrary character, and potentially undefining it to treat the entire file as a single very long line/record?

I assume it would hurt performance, but would it necessarily be any worse than multi-line matching, or even text-based searching on actual very long single-line files like some JS minifiers spit out?

Regardless, thanks for the quick response and helpful explanation.

---

_Comment by @BurntSushi on 2020-11-27 22:44_

If there were a simple way to implement that feature, I'd be open to it. But I can't think of a way. Everything is oriented around lines, _even multi-line search_. Line numbers and what not are added in a line oriented way. Lines are a critical part of the architecture of a grep tool, because doing a search in a way that is _not_ line oriented is a very different architecture. It's either _stupidly_ simple or _stupidly_ hard, depending on what you're trying to do, the tools available and what your resource constraints are.

Now, it is possible to expose something that permits configuring the line terminator. Indeed, `--null-data` is a [special case of this that sets the line terminator to the NUL byte](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/core/args.rs#L681-L683). That option could in theory be exposed via a flag. But _unsetting_ the line terminator entirely is something that I'm not really sure how to do easily. And yeah, if it isn't easy to do, then I don't think it's worth doing for cases like this.

With that said, you might find that the `--json` output will give what you want. The `--json` output is more match oriented.

And finally, it seems to me like this output mode doesn't actually inhibit anything. It just might make interpreted the results a bit more complex, but no information is being removed.

---

_Comment by @shabble on 2020-11-27 23:23_

I think `--json` with some post-processing should do exactly what I need, thanks again for pointing it out.

Against your last point (and, on further thought, probably my original suggested "fixed" output format as well), I'm not sure it would be all that easy to separate the individual matches, as the matched portion could contain sequences that look like `Record_sep;Digit;Digit*;Colon` and depending on the pattern, might not have any fixed literals that you could use to decide if it's a new match or part of the previous one.

I started down the grep_{matcher,searcher,...} rust-docs rabbithole, but it's a bit over my head at the moment, so I'll stick with wrapping the json output for now.

---
