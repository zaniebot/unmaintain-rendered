```yaml
number: 921
title: Unstable and non-obvious behavior with seems-binary files
type: issue
state: closed
author: torkve
labels:
  - duplicate
  - wontfix
assignees: []
created_at: 2018-05-15T11:14:08Z
updated_at: 2018-05-15T12:32:55Z
url: https://github.com/BurntSushi/ripgrep/issues/921
synced_at: 2026-01-12T16:13:22Z
```

# Unstable and non-obvious behavior with seems-binary files

---

_@torkve_

#### What version of ripgrep are you using?

ripgrep 0.8.1
+SIMD -AVX

#### How did you install ripgrep?

Installed from git, can't remember exact revision.

#### What operating system are you using ripgrep on?

Ubuntu 18.04 amd64

#### Describe your question, feature request, or bug.

I'm grepping a logfile, containing some binary characters. Depending on the file contents, ripgrep may exit as if nothing found.

#### If this is a bug, what are the steps to reproduce the behavior?

Link to the minimal example: https://pastebin.com/1f6X9gPr
The file is base64-encoded to prevent possible binary data corruption, so you need to decode it before testing. Binary characters are present in lines 74-75.

Depending of the way to run ripgrep result is different:
```
0 ➜ rg switch logile
76:xxxxxx.log:2018-05-15 09:47:39.969 [DEBG] [xxxxxx:xxx.xx.wrld.hxx.swm.xxxx]  [jb]{67854a41:1:xx}  [xxx:xxxxxxxxx]  [ou XXXXX       ]  switch desc to XXXXX
0 ➜ cat logfile | rg switch
1 ➜
```

If one removes any of the lines before 74 from the log, first way to run also fails with no output and exitcode 1.

#### If this is a bug, what is the actual behavior?

Debug runs.
Working:
```
0 ➜ rg --debug switch logfile 
DEBUG/rg::config/src/config.rs:42: /home/torkve/.config/ripgreprc: arguments loaded from config file: ["--iglob=!*.min.js", "-S"]
DEBUG/rg::args/src/args.rs:143: final argv: ["rg", "--iglob=!*.min.js", "-S", "--debug", "switch", "logfile"]
DEBUG/globset//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.3.0/src/lib.rs:396: glob converted to regex: Glob { glob: "**/*.min.js", re: "(?-u)(?i)^(?:/?|.*/).*\\.min\\.js$", opts: GlobOptions { case_insensitive: true, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('m'), Literal('i'), Literal('n'), Literal('.'), Literal('j'), Literal('s')]) }
DEBUG/globset//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.3.0/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/grep::search//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/search.rs:195: regex ast:
Literal {
    chars: [
        's',
        'w',
        'i',
        't',
        'c',
        'h'
    ],
    casei: true
}
DEBUG/grep::literals//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/literals.rs:79: required literals found: [Cut(SWIT), Cut(sWIT), Cut(ſWIT), Cut(SwIT), Cut(swIT), Cut(ſwIT), Cut(SWiT), Cut(sWiT), Cut(ſWiT), Cut(SwiT), Cut(swiT), Cut(ſwiT), Cut(SWIt), Cut(sWIt), Cut(ſWIt), Cut(SwIt), Cut(swIt), Cut(ſwIt), Cut(SWit), Cut(sWit), Cut(ſWit), Cut(Swit), Cut(swit), Cut(ſwit)]
DEBUG/rg::args/src/args.rs:409: will try to use memory maps
76:xxxxxx.log:2018-05-15 09:47:39.969 [DEBG] [xxxxxx:xxx.xx.wrld.hxx.swm.xxxx]  [jb]{67854a41:1:xx}  [xxx:xxxxxxxxx]  [ou XXXXX       ]  switch desc to XXXXX
0 ➜
```
Non-working:
```
0 ➜ cat logfile | rg --debug switch
DEBUG/rg::config/src/config.rs:42: /home/torkve/.config/ripgreprc: arguments loaded from config file: ["--iglob=!*.min.js", "-S"]
DEBUG/rg::args/src/args.rs:143: final argv: ["rg", "--iglob=!*.min.js", "-S", "--debug", "switch"]
DEBUG/globset//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.3.0/src/lib.rs:396: glob converted to regex: Glob { glob: "**/*.min.js", re: "(?-u)(?i)^(?:/?|.*/).*\\.min\\.js$", opts: GlobOptions { case_insensitive: true, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('m'), Literal('i'), Literal('n'), Literal('.'), Literal('j'), Literal('s')]) }
DEBUG/globset//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.3.0/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/grep::search//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/search.rs:195: regex ast:
Literal {
    chars: [
        's',
        'w',
        'i',
        't',
        'c',
        'h'
    ],
    casei: true
}
DEBUG/grep::literals//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/literals.rs:79: required literals found: [Cut(SWIT), Cut(sWIT), Cut(ſWIT), Cut(SwIT), Cut(swIT), Cut(ſwIT), Cut(SWiT), Cut(sWiT), Cut(ſWiT), Cut(SwiT), Cut(swiT), Cut(ſwiT), Cut(SWIt), Cut(sWIt), Cut(ſWIt), Cut(SwIt), Cut(swIt), Cut(ſwIt), Cut(SWit), Cut(sWit), Cut(ſWit), Cut(Swit), Cut(swit), Cut(ſwit)]
1 ➜
```
After first line removed:
```
0 ➜ rg --debug switch logfile 
DEBUG/rg::config/src/config.rs:42: /home/torkve/.config/ripgreprc: arguments loaded from config file: ["--iglob=!*.min.js", "-S"]
DEBUG/rg::args/src/args.rs:143: final argv: ["rg", "--iglob=!*.min.js", "-S", "--debug", "switch", "logfile"]
DEBUG/globset//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.3.0/src/lib.rs:396: glob converted to regex: Glob { glob: "**/*.min.js", re: "(?-u)(?i)^(?:/?|.*/).*\\.min\\.js$", opts: GlobOptions { case_insensitive: true, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('m'), Literal('i'), Literal('n'), Literal('.'), Literal('j'), Literal('s')]) }
DEBUG/globset//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.3.0/src/lib.rs:401: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/grep::search//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/search.rs:195: regex ast:
Literal {
    chars: [
        's',
        'w',
        'i',
        't',
        'c',
        'h'
    ],
    casei: true
}
DEBUG/grep::literals//home/torkve/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/literals.rs:79: required literals found: [Cut(SWIT), Cut(sWIT), Cut(ſWIT), Cut(SwIT), Cut(swIT), Cut(ſwIT), Cut(SWiT), Cut(sWiT), Cut(ſWiT), Cut(SwiT), Cut(swiT), Cut(ſwiT), Cut(SWIt), Cut(sWIt), Cut(ſWIt), Cut(SwIt), Cut(swIt), Cut(ſwIt), Cut(SWit), Cut(sWit), Cut(ſWit), Cut(Swit), Cut(swit), Cut(ſwit)]
DEBUG/rg::args/src/args.rs:409: will try to use memory maps
1 ➜
```

#### If this is a bug, what is the expected behavior?

I suppose that behavior should be at least stable, as it does, when the` --text` option is set.
If ripgrep at some considers file to be binary and stops further processing, may be it would be nice to have something like "Binary file matches" from grep, instead.

---

_Comment by @BurntSushi on 2018-05-15 11:26_

> I suppose that behavior should be at least stable, as it does, when the--text option is set.

The differences are likely attributable to the differences between the memory map searcher and the stream searcher (which uses a fixed size buffer). Namely, the latter will look for `NUL` bytes anywhere in the file, and this is done when the buffer is refilled. However, the memory map searcher will only look for `NUL` bytes in the beginning of a file, since I always assumed it would be pretty bad to scan the entire file first for a `NUL` byte.

It's not quite clear how to fix this. I can imagine a couple ways, but they come with significant trade offs in either performance or complexity. So this might be a `wontfix`.

> If ripgrep at some considers file to be binary and stops further processing, may be it would be nice to have something like "Binary file matches" from grep, instead.

I agree. That's tracked by #306. See also: #631 

---

_Label `question` added by @BurntSushi on 2018-05-15 11:26_

---

_Comment by @BurntSushi on 2018-05-15 11:27_

I suspect if you pass the `--no-mmap` flag to ripgrep, then you will be guaranteed to get consistent behavior here. The only cost is that you might miss out on some small performance improvements when searching very large files. But you should measure that yourself to see whether you're OK with it.

---

_Comment by @torkve on 2018-05-15 11:40_

Yes, with `--no-mmap` I really get consistent behavior: it always finds nothing :)
So it seems that there's no reason to turn this option on.

---

_Comment by @BurntSushi on 2018-05-15 12:01_

@torkve Sorry, I don't understand your last sentence. Can you fill in your pronouns? That ripgrep finds nothing is exactly the point, because ripgrep detected a binary file. This is standard behavior and matches what GNU grep does (sans #306). If you want to disable binary file detection, then use the `-a/--text` flag (like with GNU grep).

---

_Comment by @torkve on 2018-05-15 12:14_

My idea was that this in no way differs from the case when file is in text mode, but nothing found.
So the main problem is not in "ripgrep can sometimes accidentally find something", but in "if we are unaware that someone wrote NULL into log file, one can accidentally assume that they got no problems in logs, because ripgrep just exited with 1".
However it appears to belong to #306, besides inconsistency with mmap.

---

_Comment by @BurntSushi on 2018-05-15 12:28_

> but in "if we are unaware that someone wrote NULL into log file, one can accidentally assume that they got no problems in logs, because ripgrep just exited with 1".

Again, this is expected behavior and is *not* addressed by #306. If binary data is detected, then ripgrep will exit and ignore the file. The only thing #306 addresses is:

* If a match is found before exiting, then emit a "binary file matches" message.
* When a binary file is ignored (even if it matched), then emit a message to the debug log that it was ignored. This is only human visible when the `--debug` flag is present.

You are striking at the primary difference between grep and ripgrep. ripgrep does *automatic filtering* for you. Therefore, if you are worried about silently missing a match (whether it's a hidden file or a file containing binary data), then you need to either use a different tool or tell ripgrep to explicitly disable that filtering behavior. In this case, you can disable binary file detection with the `-a/--text` flag.

I'm closing this because the most that can be done here is #306. The mmap inconsistency is unfortunate, but I don't see that getting fixed.

---

_Closed by @BurntSushi on 2018-05-15 12:28_

---

_Label `duplicate` added by @BurntSushi on 2018-05-15 12:29_

---

_Label `wontfix` added by @BurntSushi on 2018-05-15 12:29_

---

_Label `question` removed by @BurntSushi on 2018-05-15 12:29_

---

_Comment by @BurntSushi on 2018-05-15 12:32_

To clarify:

* ripgrep's default behavior is most similar to GNU grep's `--binary-files=without-match`.
* ripgrep's `-a/--text` flag is the same as GNU grep's `-a/--text` flag, or, equivalently, `--binary-files=text`.
* ripgrep has no analog for `--binary-files=binary`, where once a binary file is detected, GNU grep will suppress all output from that file, but will otherwise continue searching until it has either found a match (and thereby emit "binary file matches") or reached the end of the file.

---
