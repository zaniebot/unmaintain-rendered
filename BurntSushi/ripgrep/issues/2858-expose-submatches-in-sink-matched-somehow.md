```yaml
number: 2858
title: "Expose submatches in `Sink::matched` somehow"
type: issue
state: closed
author: federicotdn
labels: []
assignees: []
created_at: 2024-07-21T20:41:03Z
updated_at: 2024-07-21T20:56:07Z
url: https://github.com/BurntSushi/ripgrep/issues/2858
synced_at: 2026-01-12T16:13:25Z
```

# Expose submatches in `Sink::matched` somehow

---

_@federicotdn_

Hi! I've been playing around with ripgrep the last days, using it as a library in Rust. I've implemented a `MySink` struct that implements the `Sink` trait. I've noticed however that the `SinkMatch`es provided to the sink via the `matched` method do not contain information about where specifically the match (or matches) was found in the line (I think these are referred to as 'submatches' in ripgrep).

I looked around in the rest of the codebase and I noticed that both `StandardSink` and `JSONSink` run some logic specific to extracting these submatches from the matching lines. In both cases, there's two functions being used: `find_iter_at_in_context` and `trim_line_terminator` (called by the first one). These two functions are not public, however, so if an implementor of `Sink` wanted to have the same handy behaviour as with the two mentioned `Sink`s, one would need to copy the code over. It is also not possible to access the internal state of these two `Sink`s in order to extract the submatch information (I think!).

I did notice though however the comment in `find_iter_at_in_context`, which makes me think that maybe exposing this function as part of the ripgrep library may not be something your are planning on doing. Curious to hear your thoughts! Maybe there's other better ways of exposing the submatch data.

---

_Comment by @BurntSushi on 2024-07-21 20:48_

It wouldn't be appropriate to expose submatches on `SinkMatch` because submatches aren't known at that point. It is very intentional that not all searches will look for submatches because they are expensive to compute. So doing it when you don't need to is not a good idea. Moreover, indeed, the location of the match itself isn't even provided because the `Searcher` might not even know it! All it might know is that there is a match somewhere. Finding the full match is also more expensive _and_ is not always needed, so it to would not be appropriate to expose in `SinkMatch` because it specifically isn't known.

The `find_iter_at_in_context` routine is _not_ in `grep-searcher`. It's in `grep-printer`. So it is not possible for it to be using any internal state that you can't get at.

There is a fair amount of logic in `grep-printer` used to hook up the output of a `Searcher` with the actual results emitted to end users. I don't have plans of exposing that logic. There are too many assumptions being made and it would make the crate abstractions even more complicated than they already are.

---

_Comment by @federicotdn on 2024-07-21 20:56_

That makes sense, thanks. The internal state I was referring to was the values already computed by `find_iter_at_in_context`, for `StandardSink` or `JSONSink`. But I see your point that the function itself operates on `Sink` state that is available to users of the library. I think I will take a look at how the function works internally and see if I can apply the same logic for my `Sink` implementation 👍🏻 

---

_Closed by @federicotdn on 2024-07-21 20:56_

---
