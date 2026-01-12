```yaml
number: 317
title: "consider permitting `.` to match invalid UTF-8 bytes"
type: issue
state: closed
author: BurntSushi
labels:
  - question
  - icebox
assignees: []
created_at: 2017-01-12T18:34:04Z
updated_at: 2018-08-19T15:06:26Z
url: https://github.com/BurntSushi/ripgrep/issues/317
synced_at: 2026-01-12T16:13:21Z
```

# consider permitting `.` to match invalid UTF-8 bytes

---

_@BurntSushi_

See this discussion on reddit: https://www.reddit.com/r/linux/comments/5nea8b/grep_r_lover_here_im_dumping_grep_r_for_ripgrep/dcbrvap/

The TL;DR is that I think this could be done by replacing occurrences of `.` with `(?:(?u:.)|(?-u:.))`.

---

_Label `question` added by @BurntSushi on 2017-01-12 18:34_

---

_Comment by @BurntSushi on 2017-01-12 18:34_

What are the downsides of a change like this? Performance should be unaffected. Does this change matching semantics in a way that is undesirable?

---

_Comment by @BurntSushi on 2017-01-12 18:44_

What about constructions like `[^a]`? Should that match "every Unicode scalar value except `a`"? Or  should it match "every byte except for `\x61`"? I think both are wrong, and instead, `[^a]` needs to be translated to `(?:(?u:[^a])|(?-u:[^a]))`, so that it matches either, but prefers Unicode scalar values.

---

_Comment by @BurntSushi on 2017-01-20 12:36_

I was thinking about this the other day and one tricky problem we'd face today is that the AST exposed by `regex-syntax` isn't really expressive enough to do this transformation in a way that's fine grained enough. For example, in the AST, there is no direct expression of `[^a]`, as it is simply translated directly to a character class containing every codepoint (or byte) sans `a`. Similarly, there's no direct expression of `(?-u:.)` or `(?u:.)`. Instead, it's translated directly to `AnyChar` or `AnyByte`. This information is important because while we might want to transform `.` to `(?:(?u:.)|(?-u:.))` we *wouldn't* want to transform `(?u:.)` since that would be quite surprising to the user.

To summarize, this issue is blocked on two things:

1. Introducing a more faithful AST of regex syntax in the `regex-syntax` crate. (This is a border-line rewrite of the parser, but there are more reasons to do this other than this issue, for example, better error reporting.)
2. Figuring out the UX of exactly when such a translation occurs.

I think this means that this particular issue isn't going to be resolved for quite some time.

---

_Label `icebox` added by @BurntSushi on 2017-01-20 12:36_

---

_Comment by @BurntSushi on 2018-08-19 14:10_

I'm going to close this, absent any feedback to the contrary. I'm still open to changing this, but I've honestly never needed or wanted this behavior, so this might be good to document as a "trick" in the FAQ or something.

---

_Closed by @BurntSushi on 2018-08-19 14:10_

---

_Comment by @ssokolow on 2018-08-19 14:27_

Sorry. Somehow, I thought I'd already responded to this.

Having at least an option for `.` to match invalid UTF-8 would be useful for matching within generated files containing filesystem paths, since POSIX paths are sequences of bytes and filenames can contain any byte except `/` or `\0`.

(A common source of invalid UTF-8 paths in my experience is mojibake on filesystems like ext2/3/4 which don't store any kind of in-band encoding metadata. I have some on my own filesystem, where files got written into a directory on a system using latin1 for the filesystem encoding, and then that disk got connected to a system using UTF-8 for the filesystem encoding and the directory tree was recursively copied off. I didn't even realize those particular filenames had been mojibake'd until they panicked serde_json when I was trying to use it with the ignore crate to generate a log of files omitted from an automated incremental backup so they could be manually restored from DVD+R or re-downloaded if necessary.)

---

_Comment by @BurntSushi on 2018-08-19 14:55_

@ssokolow Maybe I'm misunderstanding your comment, but this issue is really about changing what `.` means _by default_. You can of course always opt into "every byte is a character" by disabling Unicode. e.g., `(?-u:.)` will match the literal byte `\xFF` but `(?u:.)` (and also `.`, since Unicode is the default) will not.

---

_Comment by @ssokolow on 2018-08-19 14:59_

Ahh. Now that you mention that, I do remember that.

Clearly, I once again got so into what I was working on that I let myself get too tired to be posting without noticing. Sorry about that.

---

_Comment by @ssokolow on 2018-08-19 15:03_

...though I do agree that it should be mentioned in the FAQ.

In my experience, expecting POSIX paths to be valid UTF-8 is sort of like writing C code... Lots of people do it and it seems to work until it breaks at the worst possible time because nobody tested with the right input.

In ripgrep's case, that'd be false negatives which could be pretty problematic, depending on why you're doing the search.

---
