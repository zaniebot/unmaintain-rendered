```yaml
number: 416
title: Carriage return handling on Windows
type: issue
state: closed
author: BatmanAoD
labels:
  - enhancement
  - libripgrep
assignees: []
created_at: 2017-03-21T17:17:43Z
updated_at: 2020-02-27T13:07:38Z
url: https://github.com/BurntSushi/ripgrep/issues/416
synced_at: 2026-01-12T16:13:21Z
```

# Carriage return handling on Windows

---

_@BatmanAoD_

On Windows, I noticed two surprising things about carriage returns (`\r`):

 * They aren't treated as word-boundaries (i.e. they won't cause the `\b` anchor to match). This means that `rg -w foo` fails to find `foo` at the end of a line in a file with Windows line-endings.
 * They aren't included in the end-of-line anchor `$`, which makes using the anchor more difficult.

I realize that this *could* be by design, since RipGrep may be using some standard definition of `\b` that explicitly does not include `\r`. That seems unintuitive, though, and I don't think I've seen that behavior in other regex engines running on Windows (e.g. in Python on Windows). 

---

_Comment by @BurntSushi on 2017-03-21 17:24_

> They aren't treated as word-boundaries (i.e. they won't cause the \b anchor to match). This means that rg -w foo fails to find foo at the end of a line in a file with Windows line-endings.

This doesn't seem right. Could you please provide a minimal reproducible example? e.g., This works fine for me:

```
$ echo -e 'foo\n' | rg -w foo
1:foo
$ echo -e 'foo\r\n' | rg -w foo
1:foo
```

> They aren't included in the end-of-line anchor `$`, which makes using the anchor more difficult.

I think GNU grep handles this by stripping `\r` characters and dealing with the fallout. This seems hard, or at best, very annoying.

The semantics of the regex engine aren't going to change, sorry.

---

_Label `question` added by @BurntSushi on 2017-03-21 17:24_

---

_Comment by @BatmanAoD on 2017-03-21 18:32_

Hm, you're right, I'm having trouble reproducing the `\b` issue with test files; I'll have to see if I can find out why I'm having issues with the file I was searching when I discovered the issue.

Regarding `$`, though, isn't the anchor essentially useless on Windows if `\r` causes a mismatch? `echo -e "foo\r\n" | <greptool> 'foo$'` seems to me like it should *always* match, regardless of the system. (I've checked that on Debian, GNU grep and ack both handle this case appropriately.)

Why not just let the anchor match when a character other than `\r` is followed by `\n` *or* `\r\n`? (I think you're right that this approach is not the one taken by grep, so I realize that there may be a good reason not to take this approach.)

I'm guessing that this behavior is due to the current behavior of rust-lang/regex ? If so, I can open an issue there, which seems like a more appropriate place (even though you're the primary author/contributor on that project as well).

---

_Comment by @BurntSushi on 2017-03-21 18:39_

The issue already exists: https://github.com/rust-lang/regex/issues/244

> Why not just let the anchor match when a character other than \r is followed by \n or \r\n?

If you know how to implement this efficiently in a DFA, then please teach me. :-)

---

_Comment by @BurntSushi on 2017-03-21 20:44_

Just to be clear, I think the state of affairs is, indeed, unfortunate. It's just hard to fix. I can't say that it will *never* get fixed, but I wouldn't expect something any time soon.

In the mean time, there are not-ideal workarounds. e.g., `echo -e 'foo\r\n' | rg 'foo\r?$'` works if you're in a bind.

---

_Comment by @BatmanAoD on 2017-03-21 20:51_

I was thinking about that--how opposed would you be to introducing an option (possibly via config-file) to automatically replace `$` in search patterns with `\r?$`, possibly only under certain conditions (e.g. only when running on Windows, or when the last two characters in the first candidate file are `\r\n`)? I think this would only cause unintended side-effects when paired with a flag (such as `-o`) that changes the output text depending on whether or not the `\r` is included in the match. 

---

_Comment by @BurntSushi on 2017-03-21 21:12_

ripgrep doesn't have a config file. I'd rather folks work around it for now. I'm not strongly against your heuristic, but we'd need to be careful that it doesn't introduce any additional weirdness.

---

_Comment by @BatmanAoD on 2017-03-22 17:29_

Makes sense. Thanks for considering this.

---

_Comment by @BatmanAoD on 2017-03-23 08:06_

Ah, I've figured out my issue with word-boundaries--for some reason I was expecting `\b` to zero-length match between `(` and `\r`. I found that `rg -w '<pattern>\('` wasn't finding anything whereas I *did* find what I was looking for without the `-w` flag. But of course left-paren isn't a word character.

/shame

---

_Comment by @BurntSushi on 2017-03-23 10:44_

@BatmanAoD No worries, word boundaries are hard. See also #389.

---

_Comment by @BatmanAoD on 2017-03-31 06:12_

I'm looking into modifying the regex crate to handle `\r` and `\r\n` as newlines, but I've discovered that in the current implementation, `^$` actually matches the newline at the end of a file, probably because the end-of-line flag is set whenever the iterator reaches the end of the file, regardless of the actual final bytes in the file--and of course `\n` causes the beginning-of-line flag to be set. (At least, I *think* that's why it's happening--I haven't actually done anything to verify that this explanation is correct.)

Would you consider this a bug in `regex`? If so, I believe it can probably be fixed by tweaking the end-of-file flag-setting logic. (An off-the-cuff suggestion: perhaps the start-of-line flag needs to be prohibited at end-of-file, regardless of what the last byte is. I'm not sure what the value is in having `^` match when there's nothing left to read in the file.)

---

_Comment by @BatmanAoD on 2017-03-31 06:57_

Somewhat related: I'm not sure I understand exactly what the "reverse" matching logic is for or how it works. Is it just what's described in rust-lang/regex#190? Does the engine construct a DFA designed (guaranteed?) to find the same matches when applied backwards that the "normal" DFA would find when applied forwards?

If so, then with regard to this particular issue, it might actually make my proposed fix infeasible (or at least more complicated than I'd like), unless I'm misunderstanding something. When running the DFA forward, `$` should anchor immediately before the current character-index if the character is `\n` and the previous character *isn't* `\r`, but in reverse, `$` should anchor after `\n` unless the *next* character to be observed (i.e. the one right before `\n` in the input file) isn't `\r`. Based on the precedent set by the word-boundary logic, the way to handle this multi-character anchor dependency is via a special flag in `StateFlags`. But the actual logic to assign the flags for the typical case is in `exec_byte`, which (as far as I can tell) is intentionally designed not to have any special behavior for the reverse match. Moreover, there's not really any way that I can see to look *ahead* in the search, which I think would be necessary in the reverse case.

Tangentially: I haven't taken even a cursory look at the code to answer this for myself, but how is ripgrep processing input before feeding it to the regex crate? I've previously assumed that the reason `ack` can't support multi-line matching is because it reads a single line at a time and applies the search pattern to each line individually, and ripgrep has the same restriction--but if ripgrep were doing the line-splitting, I'd have expected you to suggest modifying *that* logic somehow to resolve the `$` issue (or possibly to explain why that wouldn't work--though maybe that's what you meant when you mentioned that unilaterally stripping `\r` from input seems "hard, or at best, very annoying").

---

_Comment by @BurntSushi on 2017-03-31 09:26_

It sounds like you are understanding why this is hard to fix in the regex engine. I'd prefer if we discussed the regex engine on the regex repo.

> Tangentially: I haven't taken even a cursory look at the code to answer this for myself, but how is ripgrep processing input before feeding it to the regex crate?

http://blog.burntsushi.net/ripgrep/#mechanics

---

_Comment by @BurntSushi on 2017-04-08 00:21_

> I did test that this doesn't match Perl's behavior. And for a grep tool like ripgrep, it is simply wrong, because finding empty lines is sometimes useful, and a false positive at the end of every file is not helpful. That doesn't mean that the behavior of this crate needs to change, since ripgrep could simply work around the issue, but you did direct me this way when I mentioned it on the ripgrep issue tracker... 😕 Either way, I am strongly of the opinion that ripgrep's behavior should match grep's for this case.

Please motivate this with use cases and examples. Reasoning about this in the abstract will probably never convince me that anything needs to be changed. (This also seems orthogonal to `\r\n` handling, so it should probably get a new issue, please.)

---

_Comment by @BurntSushi on 2017-04-08 00:30_

@BatmanAoD I created #441

---

_Label `icebox` added by @BurntSushi on 2017-05-08 22:10_

---

_Comment by @BurntSushi on 2018-06-25 20:27_

@BatmanAoD I think I see how this might be fixed for most use cases, at least in ripgrep. It won't hit everything, but:

1. Take your idea of replacing `$` with `(?:\r?$)` (except done at the HIR level). Semantically, this should never turn something that was a match into a non-match, but could turn non-matches into a match (which is, of course, the intended effect).
2. When matches are reported, if the match ends with a `\r`, then trim it.

I think I might be able to roll this into libripgrep. In particular, I am also going to try and take a crack at #389, and the solutions have a similarish feel.

---

_Comment by @BatmanAoD on 2018-06-25 23:30_

Note, of course, that in order to be entirely correct, `\r` should only be trimmed if it was matched due to `$` rather than with `.`, a literal `\r`, or something else along those lines. I'm not sure how difficult that part is, or whether it really matters.

---

_Comment by @BurntSushi on 2018-06-25 23:52_

@BatmanAoD In line oriented mode, a `.` can never match a `\n`, and this is enforced. Similarly, you cannot search for a literal `\n` either (try `rg '\n'`), so we should probably do the same for `\r` which seems fine to me. (This will be an opt-in in a new crate I'm building as part of libripgrep.)

Getting this right in multiline mode (again, will be a new thing part of libripgrep, it's already implemented) is trickier since matching `\n` (or `\r`) is explicitly allowed. But I think we'll either need to choose to sacrifice correctness or raise an error if this special CRLF handling and multi line mode are enabled at the same time.

---

_Comment by @BatmanAoD on 2018-06-28 21:27_

Is libripgrep being developed in a separate repository? Perhaps I could try implementing that solution?

---

_Comment by @BurntSushi on 2018-06-28 22:28_

@BatmanAoD That's probably not a good idea. As far as my open source work goes, I basically don't like collaborating when code is in a nascent state. There is just waaaaay too much context in my brain. It would take days to unload, and everything could switch at a moment's notice as I'm developing. I think this blog packages my thoughts in a neat little bow: http://habitatchronicles.com/2004/04/you-cant-tell-people-anything/

With that said, I do occasionally push my progress to the ag/libripgrep branch: https://github.com/BurntSushi/ripgrep/tree/ag/libripgrep --- Beware though, I sometimes force push!

---

_Comment by @BurntSushi on 2018-06-28 22:57_

And I have made sure to incorporate CRLF stripping as well: https://github.com/BurntSushi/ripgrep/blob/4d6fba44d3b5786517225aa6dcb20a0035be82f9/grep-regex/src/strip.rs#L8-L10

---

_Comment by @BatmanAoD on 2018-06-29 20:32_

I believe I understand!

---

_Label `enhancement` added by @BurntSushi on 2018-08-16 00:15_

---

_Label `libripgrep` added by @BurntSushi on 2018-08-16 00:15_

---

_Label `icebox` removed by @BurntSushi on 2018-08-16 00:15_

---

_Label `question` removed by @BurntSushi on 2018-08-16 00:15_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-08-16 00:15_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

_Comment by @sergeevabc on 2020-02-27 11:34_

Being a Windows user with ripgrep 11.0.2 on board, I’m a bit confused about this CRLF thing.

```
printf "1931. Arrowsmith" | rg "(\d+)..(.*)" -r "$2 $1"
Arrowsmith 1931               <--- as expected, but printf is not a part of Windows 

echo 1931. Arrowsmith | rg "(\d+)..(.*)" -r "$2 $1"
 1931smith                    <--- weird misplacement

echo 1931. Arrowsmith | rg "(\d+)..(.*)" -r "$2 $1" --crlf
Arrowsmith  1931              <--- extra space in between
```

---

_Comment by @BurntSushi on 2020-02-27 12:30_

I cannot reproduce on Linux:

```
$ printf "1931. Arrowsmith" | rg '(\d+)..(.*)' -r '$2 $1'
Arrowsmith 1931

$ echo 1931. Arrowsmith | rg '(\d+)..(.*)' -r '$2 $1'
Arrowsmith 1931

$ echo 1931. Arrowsmith | rg '(\d+)..(.*)' -r '$2 $1' --crlf
Arrowsmith 1931
```

OK, so I'm assuming that `echo` on Windows probably emits a `\r\n` as a new line, so let's try that:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1'
 1931smith
```

It's quite likely here that `(.*)` is matching the `\r` so the replacement actually winds up being `Arrowsmith\r 1931`. We can confirm this by looking at the hex output:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1' | xxd
00000000: 4172 726f 7773 6d69 7468 0d20 3139 3331  Arrowsmith. 1931
00000010: 0a
```

You can see the `0d` in there, which corresponds to `\r`. The strange output is actually what one expects:

```
$ echo 'Arrowsmith\r 1931'
 1931smith
```

Because `\r` will move the cursor position back to the beginning of the line. Subsequent printing then overwrites characters that were already printed. e.g.,

```
$ echo 'Arrowsmith\r 1931X'
 1931Xmith

$ echo 'Arrowsmith\r 1931XX'
 1931XXith

$ echo 'Arrowsmith\r 1931XXX'
 1931XXXth
```

Adding the `--crlf` flag seemingly gets this right:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1' --crlf
Arrowsmith 1931
```

Confirming with `xxd` that there is a single space:

```
$ printf "1931. Arrowsmith\r\n" | rg '(\d+)..(.*)' -r '$2 $1' --crlf | xxd
00000000: 4172 726f 7773 6d69 7468 2031 3933 310d  Arrowsmith 1931.
00000010: 0a
```

Notice that there is only one `0x20` byte there. So I can't reproduce your issue, at least on Linux, and in theory, the above should be equivalent to what you're doing.

---

_Comment by @sergeevabc on 2020-02-27 12:56_

@BurntSushi, I guess, Linux ```echo``` is different from Windows ```echo```, that’s whyyou’re partly unable to reproduce the issue. That’s unfortunate for me, because the third example [shows][1] ```--crlf``` produces unexpected extra space in the middle. Also let me provide the output of ```xxd```.

```
echo 1931. Arrowsmith | rg "(\d+)..(.*)" -r "$2 $1" --crlf | xxd
00000000: 4172 726f 7773 6d69 7468 2020 3139 3331  Arrowsmith  1931
00000010: 0d0a                                     ..
                                                              ^ that weird extra space
```
[1]: https://github.com/BurntSushi/ripgrep/issues/416#issuecomment-591925122

---

_Comment by @BurntSushi on 2020-02-27 13:04_

It will have to wait until I have a chance to debug this on Windows. Or else someone else should feel free to debug it.

---

_Comment by @BurntSushi on 2020-02-27 13:07_

Also not sure why you're reporting bugs on closed tickets. This makes it impossible to track. I created #1500 for you. Please just do that next time.

---
