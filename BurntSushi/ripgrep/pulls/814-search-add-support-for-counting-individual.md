```yaml
number: 814
title: "search: add support for counting individual matches (Fixes #566)"
type: pull_request
state: closed
author: balajisivaraman
labels: []
assignees: []
base: master
head: fix_566
created_at: 2018-02-17T16:09:46Z
updated_at: 2018-03-10T16:08:16Z
url: https://github.com/BurntSushi/ripgrep/pull/814
synced_at: 2026-01-12T18:23:13Z
```

# search: add support for counting individual matches (Fixes #566)

---

_@balajisivaraman_

As discussed in #566, this PR adds a `--count-matches` flag that will count the individual matches instead of the matching lines.

- This does essentially the same thing `printer.matched` does for printing only matches, which is to use the Regex's `find_iter` method for getting the individual matches. We then increment a variable to keep track of the count, ignoring the actual match itself. Since this will always happen behind a flag, I hope this won't have any performance implications.
- I initially considered using the existing `match_count` variable in `search_stream` and `search_buffer` to keep track of individual matches if `--count-matches` were passed in. This would've been handy if we later wanted to use the individual match count for something like the `--stats` flag, as it would get returned all the way up to `main.rs`; we can then use this for our purposes.

  However, I decided against this because it conflicts with the existing `--max-count` argument, which terminates the search early if we've hit the max count of matching lines. As a result, `rg` still terminates for matching line count and not individual match count even after this change. I'm just throwing this out there for your consideration.

  Currently, I've renamed the existing `match_count` to `matching_line_count` to better reflect what it keeps track of. `match_count` is now an `Option<usize>` and used to keep track of individual match count.
- `-c/--count` and `--count-matches` will override each other.

---

_Review comment by @BurntSushi on `src/search_buffer.rs`:24 on 2018-02-20 13:11_

Could you name this `match_line_count` so that it's more consistent with `match_count`?

---

_Review comment by @BurntSushi on `src/search_buffer.rs`:155 on 2018-02-20 13:12_

When wrapping a conditional to multiple lines, could you move the brace down to the next line so that the division between the conditional and the conditional body is more clear?

---

_Review comment by @BurntSushi on `src/search_buffer.rs`:24 on 2018-02-20 13:15_

Also, in the future (no need to do this for this PR), it would be super helpful to put renames (and similarly uninteresting changes) in a separate commit. In this case, a commit that came before the principle change. The reason is that I don't really care about the mechanics of a name change so long as the compiler is happy, but it increases the noise in the diff among other changes that I do carefully want to look at.

It can be hard to do this though because it does require some forethought, so don't feel like I'm imposing a strong requirement! But if you think of it and it's not too much work, it definitely helps the review process.

---

_Review comment by @BurntSushi on `src/search_buffer.rs`:141 on 2018-02-20 13:17_

Would it be possible to move this call into `print_match` itself?

---

_Review comment by @BurntSushi on `src/search_stream.rs`:308 on 2018-02-20 13:20_

Same as for the mmap searcher. Can you move this into the `print_match` method?

---

_@BurntSushi requested changes on 2018-02-20 13:21_

@balajisivaraman This looks great to me! I left some comments but I think they are all pretty minor!

---

_@balajisivaraman reviewed on 2018-02-20 15:07_

---

_Review comment by @balajisivaraman on `src/search_buffer.rs`:24 on 2018-02-20 15:07_

Ah this does indeed make a lot of sense to me. I understand completely and will make the change in a separate commit for this PR also. No issues. :-) Good housekeeping is always welcome.

---

_Closed by @BurntSushi on 2018-03-10 15:39_

---

_Comment by @BurntSushi on 2018-03-10 15:40_

@balajisivaraman OK, I finally got a chance to merge this today in https://github.com/BurntSushi/ripgrep/commit/27fc9f2fd341bd3ed672f79d43bf983514188b96. All I did was fix up the conflicts with latest master and reformatted the commit message. (Commit messages should note the issues they close.) I also added an additional commit that causes `--count --only-matching` to behave the same as `--count-matches`. (The next release of ripgrep will be 0.9.0, so this is OK.)

---

_Branch deleted on 2018-03-10 16:08_

---
