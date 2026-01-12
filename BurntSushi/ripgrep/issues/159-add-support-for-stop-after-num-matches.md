```yaml
number: 159
title: Add support for stop after num matches
type: issue
state: closed
author: mengelbrecht
labels:
  - enhancement
assignees: []
created_at: 2016-10-09T08:41:59Z
updated_at: 2023-12-27T14:11:38Z
url: https://github.com/BurntSushi/ripgrep/issues/159
synced_at: 2026-01-12T16:13:21Z
```

# Add support for stop after num matches

---

_@mengelbrecht_

Add an option to stop reading a file after `num` matches have been found. This equals the `-m`, `--max-count` option from grep. 


---

_Comment by @BurntSushi on 2016-10-09 13:01_

I believe GNU grep has this, right?

I'm inclined to add it, but could you provide a use case for it? One slight
concern I have is its use in recursive directory search, since on a multi
core system, the output of search results isn't deterministic. However, I
can see it being useful as a way to avoid flooding your terminal.

For single file search, its usefulness is a bit clearer I think.

On Oct 9, 2016 4:42 AM, "Markus Engelbrecht" notifications@github.com
wrote:

> Add an option to stop reading a file after num matches have been found.
> This equals the -m, --max-count option from grep.
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/159, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34gtufVeUBufuNmREEbhedQaxI24uks5qyKjXgaJpZM4KR8jt
> .


---

_Comment by @mengelbrecht on 2016-10-09 14:00_

Yes GNU grep has this.

The use case is repeatedly filtering stdin (or a single file) with a match limit to look for a specific match. If the match is not found the user can refine the pattern until it is.
This technique can be applied to filter a list of files for a certain entry and pick it (e.g. [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) fuzzy file finding).


---

_Comment by @BurntSushi on 2016-10-09 14:08_

@mgee But even if you give a list of files, `ripgrep` will process them in parallel, which means the output won't be deterministic. That means the option only becomes useful in your use case if you pass `-j1` (which disables parallelism and therefore gets deterministic output).

I'm not necessarily saying that this is a good reason to not have this flag, but I am saying that it seems like bad UX.


---

_Label `enhancement` added by @BurntSushi on 2016-10-09 14:08_

---

_Comment by @mengelbrecht on 2016-10-09 14:31_

@BurntSushi true, this option only has good UX when used with stdin or a single file (my use case). Maybe a note in the documentation about non-determinism when `ripgrep` is given multiple files (or a path) would be sufficient?


---

_Comment by @mengelbrecht on 2016-10-09 15:28_

@BurntSushi I just realized that what I previously wrote could be misunderstood. With _a list of files_ I meant I have a single file which contains a list of filenames which I successively filter with a pattern. In each step the pattern can be refined to reduce the list of matched filenames until the desired filename is found.


---

_Comment by @BurntSushi on 2016-10-09 15:48_

@mgee Ah, I did misunderstand that. Thanks for clarifying.


---

_Closed by @BurntSushi on 2016-11-06 18:09_

---

_Comment by @mengelbrecht on 2016-11-06 19:04_

Thanks!


---

_Comment by @sergeevabc on 2019-03-25 12:46_

GNU’s Grep ```-m``` is quite an ambiguous switch. Manual says ```stop after NUM matches```, but since Grep is line-based tool (which slips away every now and then), it counts not actual pattern matches, but lines. Imagine this:
```
$ grep -m 4 -n -oP pattern
2:8  
2:11 
3:16           
4:28                      <--- you need 4 results only, stop after that
4:29 
4:30 
4:36 
```
Instead, you have to ```grep ... | head -4``` to get 4 results.

Now Ripgrep provides a clearer description (```limit the number of matching lines per file searched to NUM```), thanks for that! But it turns out that it cannot stop after actual pattern matches. So frustrating…

---

_Comment by @AlphaJack on 2023-07-26 21:33_

`ack -1 ...` is useful for recursive search, as it stops searching other files after a match is found. `grep` cannot reproduce this behavior, as `-m 1` does not prevent searching other files after a match is found.

---

_Comment by @Podbrushkin on 2023-12-27 11:50_

Not sure why this is closed as Completed, since RipGrep doesn't have option "to stop after num matches". 
I have a 5gb text file without linebreaks, I want to find a substring in it and get byte offset of its first occurrence. Both Grep and RipGrep can't help me with that.

`rg -b -o --no-line-number --line-buffered --fixed-strings -m 1 "000000" .\5gbunicode.txt` - This takes 20 seconds and outputs 3876278 lines, while I need only one. `--max-matches` would be very helpful. Or if there would be alternative `--line-buffered` which will buffer relative to actual output, not to file structure. I think to buffer lines relative to output is more reasonable, since buffering is about output, not input. This way at least it will be possible to limit output by pressing Ctrl+C once you see in terminal first matches occurring.

---

_Comment by @BurntSushi on 2023-12-27 14:11_

> Not sure why this is closed as Completed

It was closed as completed because a `-m/--max-count` flag was added. There was even a [commit](https://github.com/BurntSushi/ripgrep/commit/58aca2efb24801b43870acac5b40c59fbc9ef350) referenced that can click on above. I'm not sure how else to explain it.

> `rg -b -o --no-line-number --line-buffered --fixed-strings -m 1 "000000" .\5gbunicode.txt` - This takes 20 seconds and outputs 3876278 lines, while I need only one.

When reporting behavior that doesn't work like you expect, ***please*** include a reproduction. And please don't bump issues that are 7 years old. Please just take a moment to minimize it. Here, watch:

```
$ cat haystack
fooabcfooabc
fooabcfooabc
$ rg abc haystack
1:fooabcfooabc
2:fooabcfooabc
$ rg abc haystack -m1
1:fooabcfooabc
$ rg abc haystack -m1 -o
1:abc
1:abc
```

The above output should make it clear that the `-m1` flag is actually doing something. In the second example, only one line is printed, although that line does contain two matches. In the second example, two lines are printed, but only from the first matching line. Indeed, this is consistent with how the `-m/--max-count` flag is documented:

```
    -m NUM, --max-count=NUM
        Limit the number of matching lines per file searched to NUM.

        Note that 0 is a legal value but not likely to be useful. When used,
        ripgrep won't search anything.
```

That is, it is a limit on the number of matching lines and not the number of matches.

To me, this suggests your `5gbunicode.txt` file is one giant line.

> I want to find a substring in it and get byte offset of its first occurrence. Both Grep and RipGrep can't help me with that.

Of course they can. Watch:

```
$ rg abc haystack -m1 -o | head -n1
abc
```

Both ripgrep and grep will stop searching after printing the first match.

> Or if there would be alternative --line-buffered which will buffer relative to actual output, not to file structure. I think to buffer lines relative to output is more reasonable, since buffering is about output, not input. This way at least it will be possible to limit output by pressing Ctrl+C once you see in terminal first matches occurring.

I can't make heads or tails of what you're saying here. The default behavior is to choose the buffering strategy based on where ripgrep is printing (line buffering for tty and block buffering for a file). By passing `--line-buffered`, you are forcing line buffering and disabling ripgrep's automatic heuristic. It is unclear why you're doing that here and I don't understand what you're asking for. Please [open a Discussion question](https://github.com/BurntSushi/ripgrep/discussions) about this if you're still confused.

---
