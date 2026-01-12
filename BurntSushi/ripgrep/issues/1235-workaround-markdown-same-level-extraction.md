```yaml
number: 1235
title: "[WORKAROUND] Markdown same level extraction"
type: issue
state: closed
author: sergeevabc
labels:
  - question
assignees: []
created_at: 2019-04-04T08:40:56Z
updated_at: 2019-04-12T09:22:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1235
synced_at: 2026-01-12T16:13:23Z
```

# [WORKAROUND] Markdown same level extraction

---

_@sergeevabc_

ripgrep 0.10.0 (rev d6feeb7ff2) on Windows 7 x64.

Today we often use Markdown-formatted files, but data extraction is still a hassle. Imagine:
```
## Foreword
Lorem ipsum dolor sit amet.

### Summary
Lorem ipsum dolor sit amet.

### Images
- https://resource1.com/
- https://resource2.com/

#### Obsolete
- ~~https://resource3.com/~~

### Story1
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod...

### Story2
Tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim...
```

With ```grep -Poz "### Images\n(?:(?!### ).*\n?)*" input.md``` I can extract from ```### Images``` till the next heading of the same level (```### Story1``` in this case), i.e.
```
### Images
- https://resource1.com/
- https://resource2.com/

#### Obsolete
- ~~https://resource3.com/~~
```

However I’m confused how to accomplish the same trick with Ripgrep. Tried the following approaches:

```
$ rg --pcre2 "### Images\n(?:(?!### ).*\n?)*" input.md 
$ rg --pcre2 -o "### Images\n(?:(?!### ).*\n?)*" input.md 
$ rg --pcre2 -o -U "### Images\n(?:(?!### ).*\n?)*" input.md 
$ rg --pcre2 -o -U --multiline-dotall "### Images\n(?:(?!### ).*\n?)*" input.md 

(blank screen)
```
Perhaps you could tell what I’m missing here?


---

_Comment by @BurntSushi on 2019-04-04 11:14_

This works for me:

```
$ rg -PU '### Images\n(?:(?!### ).*\n?)*' input.md
```

The above is the most natural way to do it, given ripgrep's feature set. But the same trick with grep works with ripgrep too. You just need to use `--null-data` instead of `-z`.

```
$ rg -Po --null-data '### Images\n(?:(?!### ).*\n?)*' test.md
```

---

_Comment by @sergeevabc on 2019-04-04 11:42_

@BurntSushi, thank you for a quick reply, but so far I see no output as follows:
```
$ rg -PU '### Images\n(?:(?!### ).*\n?)*' input.md
Images\n(?:(?!###: The filename, directory name, or volume label syntax is incorrect. (os error 123)
).*\n?)*': The filename, directory name, or volume label syntax is incorrect. (os error 123)

$ rg -PU "### Images\n(?:(?!### ).*\n?)*" input.md
(blank screen)

$ rg -Po --null-data "### Images\n(?:(?!### ).*\n?)*" input.md
(blank screen)

$ rg --trace -PU "### Images\n(?:(?!### ).*\n?)*" input.md
DEBUG|globset|globset\src\lib.rs:434: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|grep-searcher\src\searcher\mod.rs:623: Some("input.md"): searching via memory map
TRACE|grep_searcher::searcher|grep-searcher\src\searcher\mod.rs:706: slice reader: needs transcoding, using generic reader
TRACE|grep_searcher::searcher|grep-searcher\src\searcher\mod.rs:674: generic reader: reading everything to heap for multiline
TRACE|grep_searcher::searcher|grep-searcher\src\searcher\mod.rs:676: generic reader: searching via multiline strategy
```

---

_Comment by @sergeevabc on 2019-04-04 12:00_

It seems to be somehow related to ```\n```, because when I change it to ```\s``` rigrep acts as expected. But why?

---

_Comment by @BurntSushi on 2019-04-04 12:08_

I don't know, but this doesn't look like a ripgrep issue. It's likely something to do with Windows or your console.

---

_Closed by @BurntSushi on 2019-04-04 12:08_

---

_Label `question` added by @BurntSushi on 2019-04-04 12:08_

---
