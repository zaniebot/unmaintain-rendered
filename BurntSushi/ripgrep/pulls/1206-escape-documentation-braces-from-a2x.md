```yaml
number: 1206
title: Escape documentation braces from a2x
type: pull_request
state: closed
author: ravron
labels: []
assignees: []
base: master
head: ra-man-page-multiline
created_at: 2019-02-28T03:02:38Z
updated_at: 2019-04-06T12:05:23Z
url: https://github.com/BurntSushi/ripgrep/pull/1206
synced_at: 2026-01-12T18:23:13Z
```

# Escape documentation braces from a2x

---

_@ravron_

`a2x` is used to convert these documentation strings into a man page. However, the AsciiDoc format reserves the `{identifier}` syntax for [attribute references](http://www.methods.co.nz/asciidoc/userguide.html#_document_attributes), and `a2x` interprets the unescaped substring `{any}` as such an attribute reference. The documentation at that link warns:

> If an attribute is not defined then the line containing the attribute reference is dropped. This property is used extensively in AsciiDoc configuration files to facilitate conditional markup generation.

Indeed, the generated `man` page is missing two lines (here, "lines" means lines of source code), which you can see in a release build like [this one](https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep-0.10.0-x86_64-apple-darwin.tar.gz), in the `doc/rg.1` file, resulting in this ill-formed paragraph:

> When multiline mode is enabled, ripgrep will lift the restriction that a match cannot include a line terminator. For example, when multiline mode is not codepoint other than \n. Similarly, the regex \n is explicitly forbidden, and if you try to use it, ripgrep will return an error. However, when multiline and regexes like \n are permitted.

The [FAQ says](http://methods.co.nz/asciidoc/faq.html#_how_can_i_escape_asciidoc_markup) the fix is to simply escape the unintentional markup with a backslash, which is what this PR does. I don't have the ripgrep build set up on my machine, so I have not verified this change. I would ask that a maintainer do so before merging.

---

_Comment by @okdana on 2019-02-28 03:11_

This came up before:

* https://github.com/BurntSushi/ripgrep/issues/1101#issuecomment-436092239
* https://github.com/BurntSushi/ripgrep/commit/b41e5963279d5a7c633514d4a6afcb480c6c6915#commitcomment-31192307

And i believe @BurntSushi fixed it with that commit

---

_Comment by @BurntSushi on 2019-04-06 12:05_

@ravron Thanks! As @okdana points out, I think this has already been fixed. In particular, one reason why I went my route instead of explicitly escaping the text in the help output is that the text in the help output is used in both the man page _and_ the `-h/--help` output. The extra escaping here would end up making the output of `--help` look weird. (We're bound to run into some trouble because of this dual use, and if we _had_ to deal with the extra escaping to make it work, then I'd accept that. This is all basically at the service of keeping one copy of the docs in a fairly simplistic way.)

---

_Closed by @BurntSushi on 2019-04-06 12:05_

---
