```yaml
number: 544
title: Fixed width line numbers
type: issue
state: closed
author: PeterSP
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-07-07T14:38:38Z
updated_at: 2020-04-03T11:35:14Z
url: https://github.com/BurntSushi/ripgrep/issues/544
synced_at: 2026-01-12T16:13:22Z
```

# Fixed width line numbers

---

_@PeterSP_

It would be nice to be able to see the indentation level of searched text line up. One way to do this would be to have fixed width line numbers in search results. This could be accomplished through an option flag and left-padding with either spaces or zeroes when the flag is active.

As this is primarily of interest for matches within a file, only the matches within a file need be considered for how wide the line numbers should be. Alternatively, one could have a pre-determined width (i.e. 6) and overflow larger line numbers.

Implemented, this could look either like this:
```
$ rg needle --fixed-width-line-numbers
haystack.txt:
 12:    needle {
144:      needle {

silo.txt:
  16:  } needle {
 256:    } // needle 1
 512:    } // needle 2
1024:  } // needles

warehouse.txt:
   1234:  - needle ( 5 boxes )
1234567:  - needle ( 20 boxes ) 
```

Or like this:
```
$ rg needle --fixed-width-line-numbers
haystack.txt:
    12:    needle {
   144:      needle {

silo.txt:
    16:  } needle {
   256:    } // needle 1
   512:    } // needle 2
  1024:  } // needles

warehouse.txt:
  1234:  - needle (5 boxes)
1234567:  - needle (20 boxes)
```

---

_Comment by @BurntSushi on 2017-07-07 14:52_

I think this has been suggested before, but you can't determine the correct padding without reading the entire file first (which isn't going to happen), and not having the correct padding seems like a bummer to me.

---

_Label `question` added by @BurntSushi on 2017-07-07 14:52_

---

_Comment by @PeterSP on 2017-07-07 15:10_

Yes, for Solution 1 one would have to first find all the matches then figure out the padding, which would slow down results.

Solution 2 avoids that. One could even allow a width argument for people who know the line number limit they're going to see. That said, solution 2 isn't a complete solution.

---

_Comment by @okdana on 2017-07-07 16:27_

For a while (before it got cumbersome to use) i had a wrapper script for `ag` which passed the output to `sed` so it could zero-pad all of the line numbers. I just fixed it at 6 digits, which was suitable for 99% of the files i was looking at, though admittedly a bit noisy in the typical case of only 3- or 4-digit line counts.

So... if the feature *does* seem interesting, i think the solution with the best effort-to-functionality ratio would probably be to just have a `--line-number-width=6` or whatever that people could put in their aliases.

On a related note, i actually *dislike* seeing the indentation in the output. It generally doesn't convey anything useful to me when removed of its context, so my wrapper script also eliminated that. So instead of something like this:

```
% ag -w let globset/src/pathutil.rs
16:    let path = path.as_ref().as_os_str().as_bytes();
26:    let last_slash = memrchr(b'/', path).map(|i| i + 1).unwrap_or(0);
78:    let name = unsafe { os_str_as_u8_slice(name) };
146:                let got = file_name_ext(OsStr::new($file_name));
162:                let got = normalize_path(Cow::Owned($path.to_vec()));
```

It would give this:

```
% ag -w let globset/src/pathutil.rs
000016:let path = path.as_ref().as_os_str().as_bytes();
000026:let last_slash = memrchr(b'/', path).map(|i| i + 1).unwrap_or(0);
000078:let name = unsafe { os_str_as_u8_slice(name) };
000146:let got = file_name_ext(OsStr::new($file_name));
000162:let got = normalize_path(Cow::Owned($path.to_vec()));
```

That might be a peculiar preference tho

---

_Comment by @PeterSP on 2017-07-09 12:57_

I feel like I would prefer space-padding to zero-padding.

I will confess that I was struck by a desire to line up the line numbers because the introduced difference in indentation bothered me, moreso than because I wanted to see the level of indentation. That suggests I might prefer stripping or truncating the whitespace also.

I think that might be out of scope for this issue, however.

---

_Comment by @BurntSushi on 2017-10-22 01:21_

If someone wants to do the work to thread through a `--line-number-width` flag, then I think I would be OK maintaining it, but I'm unlikely to do this myself.

---

_Label `enhancement` added by @BurntSushi on 2017-10-22 01:22_

---

_Label `help wanted` added by @BurntSushi on 2017-10-22 01:22_

---

_Label `question` removed by @BurntSushi on 2017-10-22 01:22_

---

_Comment by @balajisivaraman on 2017-12-30 05:26_

If someone hasn't already, I'd be happy to try my hand at this as it looks like a good first issue for me.

Just to clarify, the way I understand it, the requirement is as follows:

- Users should be able to specify a `--line-number-width` flag and the printed line number should be left padded for that width with empty spaces.
- If the above flag is not specified, no padding will happen.
- Should we also add a `--line-number-padding-char` to allow users to specify the character to use for padding, defaulting to SPACE if none is specified?

I'll begin working on it right away.

---

_Comment by @okdana on 2017-12-30 06:05_

Perhaps it's a bit too clever/unnecessary, but maybe the argument to `--line-number-width` could work like integer format strings in `printf`: If leading zeroes are present in the number (`--line-number-width=06`, pad with zeroes; otherwise (`--line-number-width=6`) pad with spaces.

Not sure it's really important to be able to specify the padding character at all, but having a whole separate option for it seems a bit over-kill somehow.

The rest sounds right tho

---

_Comment by @BurntSushi on 2017-12-30 13:45_

I definitely don't want two flags for this. I like @okdana's idea to use printf style.

---

_Comment by @BurntSushi on 2017-12-30 13:46_

@balajisivaraman Otherwise, I think your plan sounds pretty good to me!

---

_Closed by @BurntSushi on 2018-01-01 14:00_

---

_Comment by @Gerrit-K on 2020-04-03 11:35_

For those who found this issue ranked first on google and wonder where this feature is: it was removed a couple of months later in #795 

---
