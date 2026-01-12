```yaml
number: 1634
title: "expose optional match offsets in grep-searcher::SinkMatch"
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
  - question
  - libripgrep
assignees: []
created_at: 2020-07-03T13:28:16Z
updated_at: 2020-07-03T13:28:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1634
synced_at: 2026-01-12T16:13:23Z
```

# expose optional match offsets in grep-searcher::SinkMatch

---

_@BurntSushi_

Originally asked as a discussion question here: https://github.com/BurntSushi/ripgrep/discussions/1633

> I'm using the `grep` crate (AKA: libripgrep) to try and match exact byte portions of a slice. I've found that when my `Matcher` returns a match with a given range, this information is lost when it's sent over to the `Sink` as a `SinkMatch` struct.
> 
> I've created a small repository to reproduce the issue. Here's the entire code: https://github.com/acheronfail/grep-sink-example/blob/master/src/main.rs. See the [`README.md`](https://github.com/acheronfail/grep-sink-example) for the output of the program.
> ## What did I expect?
> 
> That the `Match` object which returned a range of `2..8` in the haystack would translate to a `SinkMatch` with a bytes portion that maps roughly to `HAYSTACK[2..8]`.
> ## What happens?
> 
> The `SinkMatch` struct's `bytes` field includes the entire line matched, and not the matched portion.
> 
> Am I using this incorrectly? Should the `SinkMatch` be giving me the matched bytes, or should I be doing this via the captures trait methods instead?



---

_Label `enhancement` added by @BurntSushi on 2020-07-03 13:28_

---

_Label `question` added by @BurntSushi on 2020-07-03 13:28_

---

_Label `libripgrep` added by @BurntSushi on 2020-07-03 13:28_

---

_Comment by @BurntSushi on 2020-07-03 13:28_

Re-posting my response from: https://github.com/BurntSushi/ripgrep/discussions/1633#discussioncomment-33547

-----

You aren't using it incorrectly and this is indeed expected behavior, although it is perhaps a design flaw. In particular, the `grep-searcher` and `grep-matcher` architecture generally assume two things:

1. Detecting whether a match exists in a particular line is cheaper (possibly substantially so) than finding the exact boundary of that match.
2. A match is generally infrequent compared to the size of the haystack.

This is why hamfisted APIs like [`Matcher::find_candidate_line`](https://docs.rs/grep-matcher/0.1.4/grep_matcher/trait.Matcher.html#method.find_candidate_line) exist. Indeed, in the most basic grep output format (no color), you never even need the match boundaries in the first place. All you need is the line that matched.

If, however, you're working on a problem in which you always find the precise match offsets up front, then the current architecture drops them on the floor, which would require you to re-run your search for each matching line. If matching lines are rare in your particular situation, then this shouldn't be a problem. If matching lines are frequent, then this could be a significant performance problem.

I do think the API is flexible enough where we could potentially fix this by enriching the `SinkMatch` structure, but it would require some thinking and some design work.

---
