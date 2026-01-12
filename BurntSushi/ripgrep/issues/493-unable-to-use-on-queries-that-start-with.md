```yaml
number: 493
title: "Unable to use on queries that start with singlequote,' ,ascii char 39"
type: issue
state: closed
author: pindash
labels:
  - bug
assignees: []
created_at: 2017-05-28T10:46:14Z
updated_at: 2017-05-30T22:05:45Z
url: https://github.com/BurntSushi/ripgrep/issues/493
synced_at: 2026-01-12T16:13:22Z
```

# Unable to use on queries that start with singlequote,' ,ascii char 39

---

_@pindash_

Steps to reproduce:
rg -F " 're " -o -w input.txt
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /checkout/src/libcore/option.rs:323
note: Run with `RUST_BACKTRACE=1` for a backtrace.

It appears that the combination of asking rg to only return matches and word breaks causes the return value to to be empty. 

Running :
rg -F "'re" -o input.txt

Works well, and running with just -w flag also works. 

Also if any other char begins the query, then everything works as well.


---

_Comment by @BurntSushi on 2017-05-28 11:43_

Could you share `input.txt` please? (Or some equivalent that triggers the same bug.)

---

_Comment by @pindash on 2017-05-29 00:52_

Sure: 
here is input.txt:

```
nchoir octogenaries alumic peshwaship 're seminomata typhlectasis designator's gourdful
racked raisins albahaca windable speckledbill lidderon kolkhos steelify
antapodosis benedictine
```

--------------
Also I am not sure what the expected behaviour if a word can start with a singlequote but if you start with a space you still get this error.  
rg " 're" -wo input.txt
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /checkout/src/libcore/option.rs:323
note: Run with `RUST_BACKTRACE=1` for a backtrace.


---

_Label `bug` added by @BurntSushi on 2017-05-29 13:33_

---

_Closed by @BurntSushi on 2017-05-29 13:54_

---

_Comment by @pindash on 2017-05-30 03:38_

Wow, that was fast, thanks. 
btw, your blog post on the internals of ripgrep and others was amazing!
 I ended up using it as the basic reference point for my [blog post](https://iabdb.me/2017/05/29/how-the-mighty-have-fallen-or-how-i-learned-to-love-grep/).

---

_Comment by @BurntSushi on 2017-05-30 11:17_

@pindash No problem. That is an epic blog post! Nice work! There are definitely some details in there that are questionable though...

Firstly, I'm kind of curious to see so much focus put on memory maps. Did you happen to read through the sections on memory maps in my blog posts? They tend to provide a small speed boost on very large files, but that's about it.

Secondly, I'm impressed you were able to get so much mileage out of `rg -f`. I've never actually benchmarked ripgrep's `-f` flag against GNU grep's `-f` flag, so I don't think it's fair to say that I think it's faster. Namely, its current implementation is a quick hack. Note though that the regex engine may elect to use Aho-Corasick, so you can't really say whether it's being used or not. (Although, given the number of patterns, the regex engine is almost certainly avoiding all literal optimizations.) A better implementation would use Aho-Corasick explicitly inside ripgrep when the `-F` flag is used.

Thirdly, GNU grep doesn't actually use Aho-Corasick. It uses Commentz-Walter.

> Also, the newest grep had somehow increased it’s speed on non-ASCII characters. So some of the traditional speedups like setting up LC_ALL=C, which forces grep to use ASCII characters doesn’t actually increase the speed.

I don't think a blanket statement like this is accurate. Have you seen the benchmarks in my blog post? They might have fixed the performance problems when searching simple ASCII literals, but they certainly haven't fixed them when using actual regexes like `\w`. :-)

> Ripgrep then goes one step further and tries to optimize this by searching for a common literal among all those patterns so that it can skip using the regex engine which is much slower. If it finds a common literal string among the patterns it will submit it to it’s implementation of the teddy algorithm, which takes advantage of Intel’s more recent SIMD instructions (single instruction multiple data, AKA vectorized processing). The teddy algorithm submits the literal to be checked against 16 bytes at a time and if it finds a match it will go into the slower regex engine to verify. This implementation forces our pattern files to be smaller since a pattern-file that is too big will not have a common literal or will match too often.

Unfortunately, this analysis is wrong. The Teddy algorithm is only used when there is a small number of common literals identified. In a large list like the one you're using, it won't be used. (And even then, you didn't say how you install ripgrep, which means you might not even have SIMD turned on.)

> In fact, the pattern-files have a limit that will throw an error if they are too big because they overwhelm the regex engine.

That can be tweaked with `--dfa-size-limit` and `--regex-size-limit`.

> Because of this limitation, ripgrep has to search the entire file many more times than grep has to. 

I think this conclusion does turn out to be right though. :-) Or at least, part of the story.

> Additionally, ripgrep will perform terribly if the original patternfile is not sorted. I suspected that ripgrep was benefitting from our sorted tokens file, and was able to find common literals, which was allowing it to be much faster than a random tokens file.

This sounds like a bug. Nice find. :-) I created #497.

> Also because ripgrep uses a nonnaive version of Boyer-Moore it is able to skip many more bytes.

I am starting to think that I shouldn't say that ripgrep (or the regex engine) actually uses Boyer-Moore. The reason why is because it doesn't actually use a shift table, which is kind of a hallmark of Boyer-Moore, and therefore, there's no "skipping." Instead, it's fast because it picks "rare" bytes to feed to `memchr`, which maximizes the amount of time search stays in a highly optimized SIMD routine.

With that said, this certainly has no effect on the commands you're running. Boyer-Moore is for single substring search, not multiple substring search.

> A disclaimer where I suggest using ripgrep until grep reintroduces mmap for large files. Old versions of grep have bugs that are not documented all that well.

Not sure why you're saying this. Are you actually seeing a huge difference between grep with memory maps and grep without? (Using the same version of grep!) You should be able to run `rg` with and without memory maps too, e.g., `rg --mmap` and `rg --no-mmap`.

> So if you know that everything is in ASCII and you know that the words in the description are space delimited nothing beats KDBQ.

I think because of those assumptions, you're fundamentally solving a different problem. :-) If you made those assumptions in a custom search tool, then I wonder what the results would be!



---

_Comment by @pindash on 2017-05-30 12:29_

[Not sure if this should be continued here]
@BurntSushi
Thank you, I will try and go through all of your points and fix any of my
errors and assumptions. It probably deserves its own post with a python
runable[you inspired me] so that tests are repeatable.

I will verify with empirical evidence, but I expected memory maps [and they
were, but again I need to verify] to be faster especially if many processes
are reading the same large file. Especially since though this file was
large it comfortably fits in the systems main memory and therefore the page
files.

Also, Boyer-Moore has a weakness if you have a wild char at the end of a
string where as your rare byte optimization will work regardless so long as
a rare byte gets chosen.

Also if ripgrep is not using teddy on patterns of 200-400 why does it
perform faster on a sorted pattern file, where the tokens all share a
common first letter for example?


 I think because of those assumptions,  you're fundamentally solving a
 different problem. :-)

I quite agree, the problem ripgrep is solving is really more general, and
technically an inverted file index would be the appropriate solution for
something like this. At the same time when doing a one off analysis, the
overhead of building the index is too high a penalty.

Your thoroughness and expertise are much appreciated.




On May 30, 2017 7:17 AM, "Andrew Gallant" <notifications@github.com> wrote:

@pindash <https://github.com/pindash> No problem. That is an epic blog
post! Nice work! There are definitely some details in there that are
questionable though...

Firstly, I'm kind of curious to see so much focus put on memory maps. Did
you happen to read through the sections on memory maps in my blog posts?
They tend to provide a small speed boost on very large files, but that's
about it.

Secondly, I'm impressed you were able to get so much mileage out of rg -f.
I've never actually benchmarked ripgrep's -f flag against GNU grep's -f
flag, so I don't think it's fair to say that I think it's faster. Namely,
its current implementation is a quick hack. Note though that the regex
engine may elect to use Aho-Corasick, so you can't really say whether it's
being used or not. (Although, given the number of patterns, the regex
engine is almost certainly avoiding all literal optimizations.) A better
implementation would use Aho-Corasick explicitly inside ripgrep when the -F
flag is used.

Thirdly, GNU grep doesn't actually use Aho-Corasick. It uses
Commentz-Walter.

Also, the newest grep had somehow increased it’s speed on non-ASCII
characters. So some of the traditional speedups like setting up LC_ALL=C,
which forces grep to use ASCII characters doesn’t actually increase the
speed.

I don't think a blanket statement like this is accurate. Have you seen the
benchmarks in my blog post? They might have fixed the performance problems
when searching simple ASCII literals, but they certainly haven't fixed them
when using actual regexes like \w. :-)

Ripgrep then goes one step further and tries to optimize this by searching
for a common literal among all those patterns so that it can skip using the
regex engine which is much slower. If it finds a common literal string
among the patterns it will submit it to it’s implementation of the teddy
algorithm, which takes advantage of Intel’s more recent SIMD instructions
(single instruction multiple data, AKA vectorized processing). The teddy
algorithm submits the literal to be checked against 16 bytes at a time and
if it finds a match it will go into the slower regex engine to verify. This
implementation forces our pattern files to be smaller since a pattern-file
that is too big will not have a common literal or will match too often.

Unfortunately, this analysis is wrong. The Teddy algorithm is only used
when there is a small number of common literals identified. In a large list
like the one you're using, it won't be used. (And even then, you didn't say
how you install ripgrep, which means you might not even have SIMD turned
on.)

In fact, the pattern-files have a limit that will throw an error if they
are too big because they overwhelm the regex engine.

That can be tweaked with --dfa-size-limit and --regex-size-limit.

Because of this limitation, ripgrep has to search the entire file many more
times than grep has to.

I think this conclusion does turn out to be right though. :-) Or at least,
part of the story.

Additionally, ripgrep will perform terribly if the original patternfile is
not sorted. I suspected that ripgrep was benefitting from our sorted tokens
file, and was able to find common literals, which was allowing it to be
much faster than a random tokens file.

This sounds like a bug. Nice find. :-) I created #497
<https://github.com/BurntSushi/ripgrep/issues/497>.

Also because ripgrep uses a nonnaive version of Boyer-Moore it is able to
skip many more bytes.

I am starting to think that I shouldn't say that ripgrep (or the regex
engine) actually uses Boyer-Moore. The reason why is because it doesn't
actually use a shift table, which is kind of a hallmark of Boyer-Moore, and
therefore, there's no "skipping." Instead, it's fast because it picks
"rare" bytes to feed to memchr, which maximizes the amount of time search
stays in a highly optimized SIMD routine.

With that said, this certainly has no effect on the commands you're
running. Boyer-Moore is for single substring search, not multiple substring
search.

A disclaimer where I suggest using ripgrep until grep reintroduces mmap for
large files. Old versions of grep have bugs that are not documented all
that well.

Not sure why you're saying this. Are you actually seeing a huge difference
between grep with memory maps and grep without? (Using the same version of
grep!) You should be able to run rg with and without memory maps too, e.g., rg
--mmap and rg --no-mmap.

So if you know that everything is in ASCII and you know that the words in
the description are space delimited nothing beats KDBQ.

I think because of those assumptions, you're fundamentally solving a
different problem. :-) If you made those assumptions in a custom search
tool, then I wonder what the results would be!

—
You are receiving this because you were mentioned.

Reply to this email directly, view it on GitHub
<https://github.com/BurntSushi/ripgrep/issues/493#issuecomment-304847996>,
or mute the thread
<https://github.com/notifications/unsubscribe-auth/ACDWYfeI35oXkk05hEtmJpyh2ofGhXzzks5r-_rbgaJpZM4NonOJ>
.


---

_Comment by @BurntSushi on 2017-05-30 12:43_

> I will verify with empirical evidence, but I expected memory maps [and they
were, but again I need to verify] to be faster especially if many processes
are reading the same large file. Especially since though this file was
large it comfortably fits in the systems main memory and therefore the page
files.

Right. You might get a small win. I've never seen memory maps blow away a well written incremental solution (as can be found in GNU grep and ripgrep). Here's an example on a ~10GB file (`OpenSubtitles2016.raw.en`):

```
[andrew@Cheetah subtitles] time rg -c 'Sherlock Holmes' OpenSubtitles2016.raw.en --mmap
5107

real    0m1.498s
user    0m1.087s
sys     0m0.381s
[andrew@Cheetah subtitles] time rg -c 'Sherlock Holmes' OpenSubtitles2016.raw.en --no-mmap
5107

real    0m2.452s
user    0m1.028s
sys     0m1.382s
```

So... maybe that is a bit bigger than a "small" win. :-)

> Also, Boyer-Moore has a weakness if you have a wild char at the end of a
string where as your rare byte optimization will work regardless so long as
a rare byte gets chosen.

Right. This is kind of an artifact of the modern era, which is interesting. The issue is that most Boyer Moore implementations these days implement their skip loop with `memchr`, which in turn uses optimized SIMD routines. This makes the penalty for getting a substring with a common last byte far more severe than when the skip loop was a simple `while` loop.

> Also if ripgrep is not using teddy on patterns of 200-400 why does it
perform faster on a sorted pattern file, where the tokens all share a
common first letter for example?

I honestly don't know. But it surely has nothing to do with Teddy. Even if Teddy were running, the order of the literals shouldn't really matter. My current guess is that you're tripping over a performance bug in literal extraction. (Which is silly, because the number of literals is so high, we'll end up throwing away any extracted literals anyway.)

---

_Comment by @BurntSushi on 2017-05-30 22:05_

@pindash One other important thing to note: when I came up with the name "ripgrep," I was trying to think of short pertinent names that began with the letter R (R for Rust). I liked "rip" because of the connotation of "rip through your text." It wasn't until after I publicly mentioned the name that the alternative "rest in peace" meaning was made clear to me by others. I just hadn't realized it until someone actually said it. :-)

If I had realized that meaning before hand, I probably would have changed the name, since it's not particularly nice. On the other hand, the name "grep" isn't owned by any one group of people.

---
