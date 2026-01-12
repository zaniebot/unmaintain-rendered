```yaml
number: 566
title: "Have a --count option that counts occurrences instead of matched lines:"
type: issue
state: closed
author: santagada
labels:
  - enhancement
assignees: []
created_at: 2017-07-27T09:43:45Z
updated_at: 2018-03-10T15:39:38Z
url: https://github.com/BurntSushi/ripgrep/issues/566
synced_at: 2026-01-12T16:13:22Z
```

# Have a --count option that counts occurrences instead of matched lines:

---

_@santagada_

Right now -c/--count counts line matches, it would be great to have an option to count matches and not lines matched. Looking at the example:

```bash
$ echo "foo foo" > example.txt
$ rg -c foo example.txt
1
$ rg -o foo example.txt
1:foo
1:foo
```

would be interesting to have a way to get the total matches (2) in this case.

---

_Comment by @BurntSushi on 2017-07-27 10:35_

Seems reasonable I guess. Maybe a `--count-all` flag?

---

_Label `enhancement` added by @BurntSushi on 2017-07-27 10:36_

---

_Comment by @santagada on 2017-07-27 14:18_

I personally would prefer a `--count-matches` to be clear on what is the difference between the two

---

_Comment by @okdana on 2017-07-27 15:10_

Would it make sense to just tie the behaviour to `-o`?

If i wasn't already familiar with `grep` i think that's how i would have expected it to work tbh. Because why else would someone want to use `-o` with `-c`?

---

_Comment by @BurntSushi on 2017-07-27 15:16_

Hmm. I guess I feel like `-o/--only-matching` is more of a flag for output control, but I agree it is a possibly reasonable path forward.

---

_Comment by @santagada on 2017-07-27 15:30_

-o is totally unrelated to counting. I just used it to make the example simpler. My current problem is searching inside binary files where we see many matches on each "line" because counting lines on binary files seems very wrong (but the material of another bug request).... but I have use for both behaviors even on text files. for me --count and --count-matches seems the way to go.

---

_Comment by @okdana on 2017-07-27 16:22_

>-o is totally unrelated to counting

Yeah, but it doesn't have to be. It literally tells `rg` to return 'only the matches' instead of whole lines. Why it doesn't change the behaviour of `-c` to *count* 'only the matches' instead of whole lines, i can't think of a super compelling reason, besides the fact that that's how `grep` happens to work. (And `grep -o` isn't part of POSIX — i think it was a GNU extension added in the early 2000s — so there's not really even a standard AFAIK.)

idk, i don't feel super strongly about it, so that's all i'll say. Just thought i'd mention that that's how my intuition would've told me it should behave.

---

_Comment by @balajisivaraman on 2018-02-15 18:34_

I had a look at this (while looking a the code for the `--stats` feature) and can send a PR for this. But first of all, do we still want this feature implemented?

Another thing to note about the behaviour of `grep` on my machine, with GNU Grep 3.1, `-o -c` doesn't behave as @okdana says. (`ag`, with version 2.1.0, prints no. of matches and not no. of lines with `-c`.)

```
$  echo "foo foo" > grep.txt
$ grep -c 'foo' grep.txt
1
$ grep -o -c 'foo' grep.txt
1
$ grep -o 'foo' grep.txt
foo
foo
$ ag -c 'foo' grep.txt
2
```

If we do want this, shall I put it behind `--count-matches` as suggested or as a combination of `-o -c`?

---

_Comment by @BurntSushi on 2018-02-15 18:43_

@balajisivaraman I don't know the right answer here. Part of me leans toward `--count-matches` so that it is explicit. If `grep -o -c` still reports a line count instead of match count, then there may be value in keeping ripgrep's current behavior there (which is the same as grep).

Also, I think @okdana was pointing out that `grep -o -c` does report number of lines matched instead of number of total matches, and that that is one possible reason why ripgrep might want to do the same (to be consistent with grep).

---

_Comment by @balajisivaraman on 2018-02-15 18:46_

Ah, that does make a lot of sense. My bad for misunderstanding. I'll begin work under the assumption that we want `--count-matches` for this. 👍 

---

_Comment by @okdana on 2018-02-15 20:21_

I was actually suggesting that `rg` do something different from everybody else and have `-o` modify the behaviour of `-c`, since it seems like an intuitive result of the combination to me (and using them together for any other reason is essentially pointless). I still like the idea, personally, but given how much emphasis there's been on matching `grep`'s behaviour lately i can see why the other approach would be chosen.

---

_Comment by @kbknapp on 2018-02-15 20:52_

I don't see why it can't be *both*.

* I like `--count-matches` because it's explicit and easy to search for in the manpages/help. It's also super intuitive.
* I like `-c -o` because it's short and makes conceptual sense  (`rg -co 'foo' test.txt` would be awesome) . When I look at the grep output, I actually stop and think, "Huh? Oh well, that's strange but I guess so." I'd have a hard time people relying on this behavior

Adding a blurb in the `--count-matches` help saying, `analogous to -c -o` is easy too.

---

_Comment by @BurntSushi on 2018-02-15 21:15_

@okdana Yeah I know, but I definitely feel a compelling attraction to match grep's behavior in subtle cases like this. But you and @kbknapp present strong arguments, and I like @kbknapp's suggestion for having both.

@balajisivaraman If you want to start with `--count-matches` and continue on your current trajectory, then that sounds great. That can be introduced in the next minor version release. Changing the behavior of `-c -o` will have to wait for the next semver bump, so it should be in a separate PR. :-)

---

_Comment by @balajisivaraman on 2018-02-16 18:05_

One question I have is how should `--count-matches` behave when `-v` is passed in. Once again, the default `ag` behaviour is befuddling to me. (Or maybe I'm missing something?)

```
$ echo -e "foo foo\nbar bar\nbaz baz" > grep.txt
$ ag -c 'foo' grep.txt
2
$ ag -c 'baz' grep.txt
2
$ ag -c 'bar' grep.txt
2
$ ag -v -c 'foo' grep.txt (If we're counting non-matching lines, shouldn't this be 2?)
1
$ ag -v -c 'bar' grep.txt (This returns nothing, but should again be 2.)
$ ag -v -c 'baz' grep.txt (Same as above.)
1
```

Do we want `rg` to invert matches in the same way it would if `-c` were passed in, or should we lean towards no interaction between `--count-matches` and `-v`? I'm not sure which way to go.

---

_Comment by @BurntSushi on 2018-02-16 18:09_

@balajisivaraman The `-v` flag basically causes any ripgrep option that deals with individual matches to not work because the `-v` is a line oriented "match" or "no match." I don't think we should copy ag here, but instead, `--count-matches -v` should probably behave the same as `-c -v`.

---

_Closed by @BurntSushi on 2018-03-10 15:39_

---
