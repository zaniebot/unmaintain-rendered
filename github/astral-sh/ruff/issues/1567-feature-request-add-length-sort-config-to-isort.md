---
number: 1567
title: "Feature request: Add Length-Sort config to isort"
type: issue
state: closed
author: einarwar
labels:
  - good first issue
  - isort
assignees: []
created_at: 2023-01-02T22:09:50Z
updated_at: 2023-11-28T06:00:39Z
url: https://github.com/astral-sh/ruff/issues/1567
synced_at: 2026-01-07T13:12:14-06:00
---

# Feature request: Add Length-Sort config to isort

---

_Issue opened by @einarwar on 2023-01-02 22:09_

Thanks for an awsome library. I am currently trying to replace the old flake8 and isort setup on our repo with ruff.

We can almost replace isort with ruff, but we are currently using the [Length-Sort](https://pycqa.github.io/isort/docs/configuration/options.html#length-sort) configuration from isort, which currently can't be found in ruff.


---

_Comment by @charliermarsh on 2023-01-02 22:55_

Love the Annoy-o-Tron.

---

_Label `isort` added by @charliermarsh on 2023-01-02 22:55_

---

_Label `good first issue` added by @charliermarsh on 2023-01-04 16:02_

---

_Referenced in [astral-sh/ruff#2082](../../astral-sh/ruff/pulls/2082.md) on 2023-01-22 05:45_

---

_Comment by @RobertCraigie on 2023-02-01 14:39_

We also need this to be able to migrate to ruff!

---

_Comment by @mihirsn on 2023-04-08 18:54_

Hi @charliermarsh 👋🏼 I've started learning Rust for a while but haven't contributed yet and want to start from somewhere. So if you allow can I pick this one ?

---

_Comment by @mihirsn on 2023-04-14 04:57_

Hi @charliermarsh, just wanted to know that if you have any idea for the implementation approach for this..if so could let me know I am thinking to pick this up. I saw the [comment](https://github.com/charliermarsh/ruff/pull/2082#issuecomment-1399410260), a bit confused by it.. so if you can elaborate it would be great.

---

_Comment by @vadim-su on 2023-06-01 15:45_

Hi there :wave:
I will try to implement this option, but I have a question about the already implemented method.
Do I understand correctly that `order_by_type` and `length-sort` are mutually exclusive options? If so, should I add an enum with the options?

---

_Comment by @warsaw on 2023-06-08 17:39_

While you're at supporting `length-sort`, please also support [`length-sort-straight`](https://pycqa.github.io/isort/docs/configuration/options.html#length-sort-straight).  That's probably the biggest isort feature I rely on that is missing from ruff.

---

_Comment by @warsaw on 2023-06-17 19:56_

> Do I understand correctly that order_by_type and length-sort are mutually exclusive options? If so, should I add an enum with the options?

I don't know the answer to that.  My guess is that they are not mutually exclusive since different isort options control them, but I don't know how they interact if both are set.

For `length-sort` and `length-sort-straight` my guess is that the latter is a subset of the former, but I'm not sure how they interact if say the former were true and the latter were false.

---

_Referenced in [astral-sh/ruff#6190](../../astral-sh/ruff/issues/6190.md) on 2023-07-31 09:46_

---

_Referenced in [python-validators/validators#289](../../python-validators/validators/pulls/289.md) on 2023-08-17 16:39_

---

_Referenced in [astral-sh/ruff#1904](../../astral-sh/ruff/issues/1904.md) on 2023-09-17 19:04_

---

_Comment by @bluthej on 2023-09-25 17:01_

Hey! I'm currently working on this one and I have a working implementation of the basic functionality. @suharnikov if you're still working on it as well I'll leave it for you of course :)

I do have one question though, I couldn't help but notice that there are a bunch of functions that take in a whole lot of `isort` settings (like `format_imports` in `rules/isort/mod.rs`), and I was wondering why they don't just take in a reference to an `isort::settings::Settings`? Is there a particular reason for that? I tried changing the function signatures and it seems to work alright that way too. 
Having all the settings passed around separately yields a fair amount of tedious work when you're trying to add a new setting because you have to chase down all the places that they need to be added and change all the function signatures.
But if there's a good reason that I just haven't figured out I'd love to know about it 🙂

---

_Comment by @charliermarsh on 2023-09-25 17:31_

@bluthej - I think it's probably okay to refactor those methods to accept `Settings`. If you're able to work on that, do you mind putting it up as a standalone PR, with _just_ that refactor, to make it easier to review and merge?

---

_Comment by @bluthej on 2023-09-25 19:13_

@charliermarsh absolutely! That's no problem at all :)

Then I'll start with the refactor and come back to this one when it's merged 😊

---

_Referenced in [astral-sh/ruff#7665](../../astral-sh/ruff/pulls/7665.md) on 2023-09-26 08:05_

---

_Referenced in [astral-sh/ruff#7738](../../astral-sh/ruff/issues/7738.md) on 2023-10-01 11:53_

---

_Referenced in [astral-sh/ruff#7963](../../astral-sh/ruff/pulls/7963.md) on 2023-10-15 11:07_

---

_Comment by @charliermarsh on 2023-10-30 04:38_

#7963 is merged, which should pave the way for this.

---

_Comment by @bluthej on 2023-11-02 07:00_

Hey, so I got back to working on this, and that led me to the following question.

When using length-sort, isort will count the dots of relative imports in the length instead of sorting by distance and then length of the module name itself (without the dots). Is that the behavior we want? Or do we want to first sort based on distance and then length of module name?

---

_Comment by @MichaReiser on 2023-11-02 07:25_

Note: We should clarify whether the `length` means number of characters or the unicode width. Or are unicode names not supported by python (whether they're discouraged is another question ;))

---

_Comment by @bluthej on 2023-11-02 17:52_

That's a fair point!

On my system (Python 12 on Arch Linux) I cannot seem to be able to import a module with a non-ASCII character, I get a module not found error. However importing members with non-ASCII characters works. Now isort uses the built-in `len`, which yields the number of characters, not the number of bytes or whatever.

I guess we can use `.chars().count()` to get the number of characters for members and `.len()` for modules (I assume the `len` method is faster because it doesn't iterate over the characters, it just accesses the length stored in the `str`/`String`). We might not sort modules properly if there are non-ASCII characters in some of them but as far as I can tell that means the input is not valid Python so it's probably worth the speedup. 

---

_Comment by @flying-sheep on 2023-11-03 10:22_

> Now isort uses the built-in `len`, which yields the number of characters

What’s a “character”? I think `len` does codepoints and not, say, grapheme clusters …

For a more accurate length, I think we’d have to iterate grapheme clusters and check [which ones are likely to render at double width](https://unicode.org/reports/tr11/). (Don’t let yourself be confused by “wide character” sometimes meaning storage size and sometimes literal width on screen)

E.g. check out how my terminal renders “平”, which of course is a valid Python identifier:

![image](https://github.com/astral-sh/ruff/assets/291575/c010a1c6-d7be-441e-91ab-a1315948b054)


---

_Comment by @bluthej on 2023-11-03 17:30_


> What’s a “character”? I think `len` does codepoints and not, say, grapheme clusters …

According to the [Python docs](https://docs.python.org/3/howto/unicode.html#the-string-type):

> Since Python 3.0, the language’s str type contains Unicode characters

and they add:

> The Unicode standard describes how characters are represented by code points

So you are correct, `len` does codepoints. The Rust equivalent would be `.chars().count()` right? 

> For a more accurate length, I think we’d have to iterate grapheme clusters

I wouldn't necessarily say "more accurate", but the argument can be made that it's more "natural" for humans. The downside is that it would make ruff behave differently from isort, so there's a bit of a tradeoff. 

> E.g. check out how my terminal renders “平”, which of course is a valid Python identifier:

Interesting! I'm going to have to figure out why my example didn't work :) 



---

_Comment by @charliermarsh on 2023-11-03 17:34_

I would prefer that we use Unicode width, which is what we use everywhere else (even though we use the term "length" -- we nearly renamed them all when we released the formatter but decided to defer that decision).

---

_Comment by @charliermarsh on 2023-11-03 17:35_

(You should be able to call `.width()` on any string using the `use unicode_width::UnicodeWidthChar` trait.)

---

_Comment by @flying-sheep on 2023-11-03 18:11_

That’s exactly the concept I was talking about btw :smiley: 

---

_Comment by @bluthej on 2023-11-04 10:32_

> (You should be able to call `.width()` on any string using the `use unicode_width::UnicodeWidthChar` trait.)

I think you meant the `unicode_width::UnicodeWidthStr` trait for strings :)
I switched to `.width()` and added some tests with non-ASCII characters.

What about relative imports? Should we follow isort and count the dots in the length?

---

_Referenced in [astral-sh/ruff#8841](../../astral-sh/ruff/pulls/8841.md) on 2023-11-26 17:47_

---

_Closed by @charliermarsh on 2023-11-28 06:00_

---
