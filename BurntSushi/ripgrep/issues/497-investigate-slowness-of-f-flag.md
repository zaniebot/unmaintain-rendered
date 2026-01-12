```yaml
number: 497
title: investigate slowness of -f flag
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - question
  - icebox
assignees: []
created_at: 2017-05-30T11:09:51Z
updated_at: 2019-04-07T23:11:04Z
url: https://github.com/BurntSushi/ripgrep/issues/497
synced_at: 2026-01-12T16:13:22Z
```

# investigate slowness of -f flag

---

_@BurntSushi_

This [blog post](https://iabdb.me/2017/05/29/how-the-mighty-have-fallen-or-how-i-learned-to-love-grep/) highlights a couple interesting problems with the `-f` flag. Or more explicitly, when a large set of literal patterns are given.

The first issue is known. ripgrep should use Aho-Corasick explicitly when `-F` is provided with multiple patterns.

The second issue is more interesting, but it appears that the order of the literal patterns provided actually impacts performance quite a bit. I wonder if the literal detection inside the regex engine is hitting a bad case.

---

_Label `enhancement` added by @BurntSushi on 2017-05-30 11:09_

---

_Label `question` added by @BurntSushi on 2017-05-30 11:09_

---

_Comment by @BurntSushi on 2017-05-30 11:37_

> The first issue is known. ripgrep should use Aho-Corasick explicitly when -F is provided with multiple patterns.

To add more context to this: right now, we take all of the patterns and join them together and sprinkle a `|` between each pattern. This works fine for small lists, but for large lists, it's not so great. Not only does it have to go through the entire regex parsing/compiling machinery, but the automaton construction itself is going to be quite wasteful. Instead, ripgrep should explicitly construct an Aho-Corasick automaton when it knows all of the patterns are literals using the `aho-corasick` crate. While this is conceptually simple to do, it requires plumbing an `aho_corasick::AcAutomaton` (or perhaps `aho_corasick::FullAcAutomaton` for small sets of literals for faster searching) through all of the search code, which is current hard-coded to take a `Regex`. We could do this by either introducing an enum of possible strategies, or using generics. There's a lot of code that knows about a `Regex`, so I worry about the hit on compilation time if we force the compiler to monomorphize all of that. Using an enum requires a branch for every search, but this intuitively feels fine since the branch predictor should make that effectively free.

With all that said, it might be wise to tackle this in libripgrep rather than trying to force it into the existing infrastructure.

---

_Label `libripgrep` added by @BurntSushi on 2017-05-30 11:38_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-05-30 11:38_

---

_Comment by @pevalme on 2017-06-28 16:37_

I've found some interesting results related to the performance of `rg -f` on big sets of regexs.

For the experiments I'll describe I have used [a 100 MB English subtitles file](https://drive.google.com/file/d/0B3oOQ14-tellNzhlU0tKT2xFSW8/view) and a [big set of 580 regular expressions](https://drive.google.com/file/d/0B3oOQ14-tellOG5WcXBlRmFWY00/view).

Let's feed `rg` with the first 100 regular expressions of the file; with these 100 regexs plus regex "a" and with these 100 regex plus regex "\\.". In my machine (Intel(R) Core(TM) i5-5200U CPU @ 2.20GHz; 4 Cores; 8 GB RAM), I get the following results (time given in minutes:seconds):

| Command | 100 regex | 100 + "a" | 100 + "\\." |
| --- | :-: | :-: | :-: |
| ` rg -f file --dfa-size-limit=10M` | 4:17  | 0:08 | 1:53 |
| ` rg -f file --dfa-size-limit=50M` | 2:58 | 0:06 | 0:57 |
| ` rg -f file --dfa-size-limit=100M` | 0:52 | 0:06 | 0:20 |
| ` rg -f file --dfa-size-limit=200M` | 0:10 | 0:06 | 0:15 |
| ` rg -f file --dfa-size-limit=500M` | 0:08 | 0:06 | 0:14 |
| ` rg -f file --dfa-size-limit=1G` | 0:08 | 0:06 | 0:14 |

### Why "a" and "\\."?
Because each of these two regular expression matches most of the lines in the file (2319098 and 2207743 out of  3414821, respectively) while there is a great difference between them: **the letter 'a' uses to appear  within the first characters of any line while the dot uses to appear only at the end each line**.

### My guess
According to what I have read, `rg` is doing a lazy determinization of the NFA that represents the big regex (an OR of all patterns). From time to time, due to the space limit for storing the DFA, `rg` removes the least used parts of it, having to built them again when needed. 

By adding the regex "a", the NFA reaches a final state _fast_ (in terms of the number of symbols of the line read before reaching it) and the required DFA remains small, without reaching the 10M bound and, thus, it is built only once. 

The case in which the regex "\\." is added instead of "a" could also be explained this way. 
Since the dot uses to appear only at the end of each string, the NFA is much _less fast_. Thus, the advantage of adding this regex is not that big since most of the transitions required to check whether a line matches the NFA before adding it are still taking place. Basically, the DFA is bigger now since longer substrings must be matched against the NFA before deciding if the whole line matches or not.
Nevertheless, since lines in this example are generally short, the DFA remains smaller than in the _100 regex_ case.

### Conclusion
It seems that detecting all matches of a big OR regex in a single line works really fast. However, the algorithm deciding whether a line matches the NFA suffers from the DFA-memory bound. Besides, even when the memory is not a problem, a simple regex that matches _fast_ the line greatly improves the performance of `rg`.

---

_Comment by @BurntSushi on 2017-06-28 16:46_

@pevalme Great analysis! I think I agree with all of it. There's probably some general advice that should be mentioned in the docs of `-f` that says that if you're trying to match against a large set of regexes, then you probably want to increase the DFA size limit using `--dfa-size-limit`. We could also just use a very large default value, since it represents a *limit*, to reduce the number of cases where `--dfa-size-limit` is needed.

---

_Label `libripgrep` removed by @BurntSushi on 2018-08-19 14:08_

---

_Removed from milestone `libripgrep` by @BurntSushi on 2018-08-19 14:08_

---

_Label `icebox` added by @BurntSushi on 2019-01-27 18:12_

---

_Closed by @BurntSushi on 2019-04-07 23:11_

---
