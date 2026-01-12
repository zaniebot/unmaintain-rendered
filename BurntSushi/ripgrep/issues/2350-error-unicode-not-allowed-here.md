```yaml
number: 2350
title: "error: Unicode not allowed here"
type: issue
state: closed
author: kenorb
labels:
  - question
assignees: []
created_at: 2022-11-11T19:48:47Z
updated_at: 2023-11-22T00:21:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2350
synced_at: 2026-01-12T16:13:24Z
```

# error: Unicode not allowed here

---

_@kenorb_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
```

#### How did you install ripgrep?

Apt.

#### What operating system are you using ripgrep on?

Ubuntu 22.04.1

#### Describe your bug.

I've got bunch of errors when parsing stream of data such as:

> /dev/fd/63:545: found invalid UTF-8 in pattern at byte offset

I've tried to use `--no-unicode` flat and ignoring certain invalid lines, but still got this error:

> error: Unicode not allowed here

#### What are the steps to reproduce the behavior?

It's very difficult for me to come up with some working scenario at this point.

But the list of invalid Unicode characters are like (based on the errors):

- `\x8D\x1A` (disable Unicode ...
- `\xC0` (disable Unicode mode ...
- `ťa` (a5c5?)

So I've tried to exclude invalid lines with `rg -ve line1 -e line2`, but ended up with:

> error: Unicode not allowed here

before printing very long multi-line string.

My syntax is like:

```
cat somefile | rg --no-unicode -vFf <(rg -NI --no-unicode '^[A-Fa-f0-9]{4,}' files/*.txt | rg --no-unicode -ve brokenline1 -e brokenline2)
```

The type of data are not binary, just bunch of lines with some unfinished Unicode characters as it seems.

I think the error happens in the inner instance in `rg -f <(rg ...)`.

Because with the `--debug` it prints:

```
regex parse error:
longline...ťa...钱包
```

Lots of spaces, but it's because rg trying to point where it's failing, as the line got 3mln characters.
But I think it's pointing to special `ť` character.

#### What is the actual behavior?

Despite putting `--no-unicode` in each instance of ripgrep, it still can't parse stream of lines.

#### What is the expected behavior?

What do you think ripgrep should have done?

I expect that `--no-unicode` (or some other similar flag) will ignore all kind of invalid UTF-16/UTF-8 characters.

---

_Comment by @BurntSushi on 2022-11-11 19:57_

`--no-unicode` is not about the haystack. It's about the regex. If you pass `--no-unicode`, then it disables "Unicode mode" in the regex. So things like `\pL` or even non-ASCII Unicode characters themselves become disallowed.

Given that _not_ using `--no-unicode` leads to decoding errors (note that those decoding errors are in the regex patterns themselves, not the haystack), and using `--no-unicode` leads to "Unicode is not allowed," then to me that implies you are trying to run regexes that want to _both_ match invalid UTF-8 in places and non-ASCII Unicode codepoints in others. You can do that, but you need to stop using `--no-unicode` and selectively enable or disable Unicode mode in the regex. For example, `(?-u:\xFF)\pL` will match `\xFF` (which is not valid UTF-8) followed by the UTF-8 encoding of any Unicode letter. Note that `\xFF\pL` matches the UTF-8 encoding of the Unicode codepoint `U+00FF` (which is not just the `\xFF` byte) followed by the UTF-8 encoding of any Unicode letter, and `(?-u)\xFF\pL` won't compile because `\pL` is not allowed when Unicode mode is disabled.

With all that said... the initial error you got:

> /dev/fd/63:545: found invalid UTF-8 in pattern at byte offset

Is _not_ resolved by `--no-unicode`. This error is telling you that you're trying to provide a regex pattern that is itself not UTF-8 encoded. Patterns _must_ be UTF-8 encoded, even if they themselves match invalid UTF-8. Notice, for example, that `(?-u:\xFF)` is UTF-8 encoded, yet, it matches the byte `\xFF` which is not UTF-8 encoded.

There is no way to compile patterns that are not UTF-8 encoded. This is an intentional design decision and it will never change. The only way to fix that error is to 1) decide on your intended match semantics and 2) convert non UTF-8 bytes to a regex with the intended match semantics. That might be as simple as hex escaping all bytes and disabling Unicode mode, for example. But it depends on what you're doing.

If none of this helps you, then I think the only path forward here is for you to provide a concrete reproduction. That is, give me commands and data I can run on my local system to reproduce the output you get, and then you tell me the output you want. If you give me that, I should be able to give you a path to that output (or confirm there is no path I know of). Please also minimize your commands to the extent possible. So, for example, nesting `rg` commands shouldn't be needed to exhibit your issue.

---

_Label `question` added by @BurntSushi on 2022-11-11 19:58_

---

_Comment by @BurntSushi on 2023-11-22 00:21_

Closing due to inactivity.

---

_Closed by @BurntSushi on 2023-11-22 00:21_

---
