```yaml
number: 3120
title: Empty matches are reported as occurring inside UTF-8 characters
type: issue
state: closed
author: IsaacOscar
labels:
  - question
assignees: []
created_at: 2025-08-05T14:46:27Z
updated_at: 2025-09-23T01:16:49Z
url: https://github.com/BurntSushi/ripgrep/issues/3120
synced_at: 2026-01-12T16:13:25Z
```

# Empty matches are reported as occurring inside UTF-8 characters

---

_@IsaacOscar_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```

### How did you install ripgrep?

`zypper in rg`.
(the same issue also occurs when manually building from the master branch).

### What operating system are you using ripgrep on?

openSUSE Tumbleweed

### Describe your bug.

Basically empty regex's will match inside UTF-8 sequences, used together with the `--replace` option, you can then output invalid UTF-8, despite none of the input being invalid.

This issue is bassically the same as the one I reported here https://github.com/rust-lang/regex/discussions/1276 with the regex crate, which is apparently intended behaviour.

I am unsure if this behaviour makes sense however with ripgrep, as I always use it on valid UTF-8 files (unless I use `--text` of course).

### What are the steps to reproduce the behavior?

```
echo 😃 | rg '' --vimgrep
echo 😃 | rg '' -r '<$0>'
```
(Using the `--pcre` and/or `--encoding UTF-8` flags will give the same results).

### What is the actual behavior?

It prints
```
<stdin>:1:1:😃
<stdin>:1:2:😃
<stdin>:1:3:😃
<stdin>:1:4:😃
<stdin>:1:5:😃
<>�<>�<>�<>�<>
```
The above indicates that regex is matching at each of the 4 bytes that make up  '😃' (plus once again at the end).
The last line is invalid UTF-8, piping it through `hexdump -C` yields:
```
00000000  3c 3e f0 3c 3e 9f 3c 3e  98 3c 3e 83 3c 3e 0a     |<>.<>.<>.<>.<>.|
0000000f
```

### What is the expected behavior?

I expect it to print:

```
<stdin>:1:1:😃
<stdin>:1:5:😃
<>😃<>
```
I.e. match before and after the character (and not *inside* it).

The above can be obtained by changing the regex to `(?=.|$)`, which works as `.` can only match a valid UTF-8 sequence.

---

_Comment by @IsaacOscar on 2025-08-05 14:51_

I see this is similar to the previously closed bug https://github.com/BurntSushi/ripgrep/issues/937, but I can't reproduce the problems shown there as ripgrep isn't emitting colour changes for empty matches.

---

_Comment by @BurntSushi on 2025-08-05 14:59_

This is definitely intended behavior because ripgrep can't know whether the haystack is valid UTF-8 or not. So there is no notion of codepoint boundary.

I am _potentially_ open to changing something here. For example, perhaps UTF-8 mode in the regex engine can be enabled when `-E utf8` is provided, since in that case, there is a UTF-8 validity guarantee. But you haven't said why you care about this, so it isn't clear to me that it's worth doing anything here. In particular, regexes that match the empty string are pretty unusual.

---

_Label `question` added by @BurntSushi on 2025-08-05 14:59_

---

_Comment by @IsaacOscar on 2025-08-05 15:15_

Yes this is definitely a minor issue, I only found it when experimenting with regexes.
(I discovered a `\C` pattern for PCRE2 that explicitly matches individual bytes, and that definitely can be used to print invalid UTF-8, but I was very surprised that I could also do that without using PCRE2.

Personally I think  `-E utf8` should give you a warning message if your files isn't actually in UTF-8 (like you currently get when searching binary files without `--text`). This hasn't actually caused me any problems so far. (In fact this should probably be the default behaviour, I usually think of a binary file as a non-UTF-8 file, not a file with a null-byte).

Also I absolutely have had regexes match the empty string (usually because I'm using lookaround assertions), which are useful with `-r` to insert text in-between stuff (of course I could just use a non-empty matching regex and `$0`).


---

_Comment by @BurntSushi on 2025-09-23 01:16_

Thinking about this, I'm not really sure there is much to do here.

If someone wanted to submit a patch for enabling UTF-8 mode in `regex-automata` when `-E utf8` is provided, then I'd be open to considering that. But otherwise, I'm not really sure this is something that needs to be fixed.

> Personally I think -E utf8 should give you a warning message if your files isn't actually in UTF-8 (like you currently get when searching binary files without --text).

That's not what ripgrep does. Without `--text`, ripgrep just looks for a NUL byte. Which is actually valid UTF-8.

I want to be very clear here: by default, ripgrep does not do any UTF-8 validity checking. You can only opt into that with `-E utf8`, in which case, ripgrep will do transcoding and automatically substitute the Unicode replacement codepoint:

```
$ printf '\xFF' | rg '' | xxd
00000000: ff0a                                     ..
$ printf '\xFF' | rg -E utf8 '' | xxd
00000000: efbf bd0a                                ....
```

The above is behaving as intended. This is why we _could_ enable `regex-automata`'s UTF-8 mode when `-E utf8` is given, because it will _guarantee_ that the haystack is valid UTF-8.

But this is a very niche and subtle thing that I'm not sure is worth doing.

---

_Closed by @BurntSushi on 2025-09-23 01:16_

---
