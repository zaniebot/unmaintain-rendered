```yaml
number: 1060
title: "ripgrep doesn't seem to work with .tar.xz"
type: issue
state: closed
author: upsuper
labels: []
assignees: []
created_at: 2018-09-21T12:32:22Z
updated_at: 2018-09-21T12:51:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1060
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep doesn't seem to work with .tar.xz

---

_@upsuper_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Homebrew prebuilt binary.

#### What operating system are you using ripgrep on?

macOS 10.13.6

#### Describe your question, feature request, or bug.

I tried to use `rg -z` to search text in a directory filled with `.tar.xz` files, but it finds nothing. I tried to use `xz` to uncompress a file into `.tar` and it still doesn't find anything. If I use `tar` to extract all files in the package, then `rg` can find the text from several files in the package.

It doesn't work even when I explicitly put the file in the command line.

I can confirm that `xz` and `tar` are all in my PATH.

#### If this is a bug, what are the steps to reproduce the behavior?

Just `rg -z "some content"`, and it lists nothing.

#### If this is a bug, what is the actual behavior?

`rg -z 'David' --debug` gives:
```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(David)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "*.xcodeproj/xcuserdata", re: "(?-u)^[^/]*\\.xcodeproj/xcuserdata$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([ZeroOrMore, Literal('.'), Literal('x'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('p'), Literal('r'), Literal('o'), Literal('j'), Literal('/'), Literal('x'), Literal('c'), Literal('u'), Literal('s'), Literal('e'), Literal('r'), Literal('d'), Literal('a'), Literal('t'), Literal('a')]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 3 basenames, 0 extensions, 0 prefixes, 0 suffixes, 3 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
``` 

#### If this is a bug, what is the expected behavior?

It should be able to find the content in the tar.xz packages.

---

_Comment by @BurntSushi on 2018-09-21 12:38_

ripgrep doesn't open archive files. It searches them like any other file. Archives are generally detected as binary files pretty quickly, so if you just want to search the raw data of an archive, then you should use the `-a` flag.

---

_Closed by @BurntSushi on 2018-09-21 12:38_

---

_Comment by @BurntSushi on 2018-09-21 12:38_

(I plan to improve how ripgrep handles binary files, in particular, showing better log messages. But I haven't gotten around to it yet. I suspect that once that lands, the `--debug` log would have made it clear that ripgrep is searching your archive but is quitting early because it detects it as binary data.)

---

_Comment by @upsuper on 2018-09-21 12:41_

Hmm, okay, that works, thanks.

I guess it would be great if you can also go through archives like directories. `tar` crate should be pretty solid to use for this.

---

_Comment by @BurntSushi on 2018-09-21 12:45_

You could probably hack something together with ripgrep's `--pre` and `--pre-glob` flags. But yeah, otherwise there are no plans to add explicit archive support.

---

_Comment by @BurntSushi on 2018-09-21 12:51_

Actually, the `tar` crate does look pretty good. I hadn't looked carefully at it yet. It looks like it would be fairly easy to use.

---
