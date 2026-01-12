```yaml
number: 608
title: Add --offset option to allow for pagination of large files
type: issue
state: closed
author: wcamarao
labels: []
assignees: []
created_at: 2017-09-21T14:58:44Z
updated_at: 2017-10-04T00:07:23Z
url: https://github.com/BurntSushi/ripgrep/issues/608
synced_at: 2026-01-12T16:13:22Z
```

# Add --offset option to allow for pagination of large files

---

_@wcamarao_

Motivation:

Say I want to grep a large file but there are too many results, and I want to display the results elsewhere (e.g. a web app).

I could start paginating with `rg --max-count 100 pattern 1gb.log` which will quickly return the first 100 lines, but I don't think there's an option in ripgrep to currently return the next 100 lines.

So, I'm doing `rg pattern 1gb.log |sed -n 101,200p` for the next 100 lines which is really slow because ripgrep will load all results to have sed only display the next 100 matching lines.

Suggestion:

Add `--offset` option so ripgrep can paginate through large results like piping `sed -n` but much faster as it won't load all matching lines into memory.

Note on naming:

Since ripgrep supports both `--max-count` and `--max-columns`, maybe the new flag could be named `--count-offset` for clarity that it's related to `--max-count`.

---

_Renamed from "Add --offset option to allow for "pagination" of large files" to "Add --offset option to allow for pagination of large files" by @wcamarao on 2017-09-21 15:28_

---

_Comment by @richarson on 2017-09-22 23:03_

How about doing this?:

```
rg pattern 1gb.log --max-count 100
rg pattern 1gb.log --max-count 200 | sed -n 101,200p
rg pattern 1gb.log --max-count 300 | sed -n 201,300p
rg pattern 1gb.log --max-count 400 | sed -n 301,400p
...
```
Or maybe even easier:
```
rg pattern 1gb.log --max-count 100
rg pattern 1gb.log --max-count 200 | tail -n 100
rg pattern 1gb.log --max-count 300 | tail -n 100
rg pattern 1gb.log --max-count 400 | tail -n 100
...
```

---

_Comment by @wcamarao on 2017-09-22 23:35_

Thanks. I thought about that too. OK for first requests, but it keeps growing memory usage linearly.

---

_Comment by @BurntSushi on 2017-09-22 23:56_

This isn't going to happen, and certainly isn't going to happen in a way that limits memory usage. Consider:

1. ripgrep makes no guarantees about order of results. You can ask ripgrep to sort the output, but this will force it to be single threaded.
2. Memory usage on search results isn't really something that I think is worth spending a lot of effort on. There is certainly an implicit assumption that search results are much smaller than the corpus being searched.

Overall, I think this is a relatively niche use case and I think the given work around is good enough (you'll want to tell ripgrep to sort the output though).

---

_Closed by @BurntSushi on 2017-09-22 23:56_

---

_Comment by @wcamarao on 2017-09-23 14:40_

I understand you don't find this a common use case, and I appreciate you explaining that and the technical challenge.

However I do have an idea on how to keep multithreading in different files while supporting offset across the board. So if you ever reconsider the idea, please reopen this issue to let me know.

Thanks

---

_Comment by @BurntSushi on 2017-09-23 15:28_

I'm always happy to entertain ideas, but I'd caution you that suggesting changes to how multithreaded search in ripgrep works should take into account how it works today.

---

_Comment by @wcamarao on 2017-09-23 16:19_

Glad to hear you're open to hearing more. Would you mind giving a brief explanation and pointing out the relevant part in the code? That may as well invalidate my idea :) but I'll be happy to dig further and think through it more if that happens. Thanks

---

_Comment by @BurntSushi on 2017-09-23 16:42_

I don't think a brief explanation is really possible, but I've explained it in other issues:

* https://github.com/BurntSushi/ripgrep/issues/152
* https://github.com/BurntSushi/ripgrep/issues/263

The actual parallelism is implemented in the `ignore` crate: https://github.com/BurntSushi/ripgrep/blob/214f2bef666efd686b1be250fc8e9f54a2cebb0f/ignore/src/walk.rs#L864-L1297

---

_Comment by @wcamarao on 2017-09-30 16:14_

I was hoping to find a variable in the parallelism implementation representing the --max-count flag, so we could add a new variable for the new --offset flag and pass it along to each thread.

So, when searching file contents in each thread/directory, it wouldn't need to search the whole file. Ripgrep already does that by using max count, correct? Idea is to introduce a new offset variable at that point, so it'll set two boundaries instead of one.

E.g. Currently, ripgrep sets one boundary (max count) and searches only the first N lines of each file in a directory. When passing the new offset flag, idea is to limit the search scope even further to a specific line range where you can define a starting point different from the first line of the file.

Does that make more sense to you now? I'm thinking maybe I didn't explain the idea clearly enough before, or maybe I'm still missing something about how this relates to directory walking in parallel.

Also, can you please point me to where max count is used for searching file contents?

Thanks

---

_Comment by @BurntSushi on 2017-10-02 10:42_

> Ripgrep already does that by using max count, correct?

Yes. I'd strongly recommend reading the search code. `max_count` in particular is used here: https://github.com/BurntSushi/ripgrep/blob/214f2bef666efd686b1be250fc8e9f54a2cebb0f/src/search_stream.rs#L132

> E.g. Currently, ripgrep sets one boundary (max count) and searches only the first N lines of each file in a directory. When passing the new offset flag, idea is to limit the search scope even further to a specific line range where you can define a starting point different from the first line of the file.

Max count specifically refers to the number of matches reported, not the number of lines searched.

---

_Comment by @BurntSushi on 2017-10-02 10:45_

@wcamarao One thing I may have misunderstood was that you probably want your proposed offset flag to be done on a per-file basis like how `--max-count` works. In that respect, me bringing up parallelism was a red herring.

I still believe you should be using standard command line tools as proposed to solve this problem. The memory usage argument is interesting, but that should only matter when a significant number of lines match in a significant file *and* you want to page through the entire thing. What is your use case for such a thing?

---

_Comment by @wcamarao on 2017-10-02 16:32_

Interesting, so max count is used to terminate the search earlier, and it only affects matches reported, not lines searched. Did you also mean it still searches the whole file, allocating memory for all lines searched, but not allocating more memory for matches reported within ripgrep? That could make sense to what I'm experiencing at the moment (more below).

Oh no problem. Thanks for digging this further with me and helping us both understand this better now. I really appreciate your continued support.

The case is a UI tool that given a root directory, it allows you to search file contents using ripgrep in the backend and reporting results in a "web app way" (meaning we *must* paginate through records found, otherwise pageload could be really bad). Most cases are like you described earlier, search results are much smaller than whole file, but sometimes that's not the case and that's what I'm trying to improve at the moment. For now I'm using only max count and it's lightning speed at first request, but it keeps losing performance and increasing memory usage on subsequent requests. That's why I was hoping it would be feasible to introduce something similar to `sed -n N,Np`.

I'm not aware of other command line tools I could use for this, so I'm considering moving the backend to something like lucene. I'm just amazed by how much I was able to achieve so far with all the speed and simplicity of ripgrep, so I'd like to keep it if possible.

---

_Comment by @BurntSushi on 2017-10-02 17:59_

> allocating memory for all lines searched

ripgrep's memory usage is proportional to the size of its *output*, not the size of the corpus it searches. (This is somewhat of a non-truth, in the sense that if you aren't searching with memory maps, then ripgrep requires at least one line to be resident in memory. So if your corpus is a single line---no matter how large---then ripgrep will store it all in memory.)

> For now I'm using only max count and it's lightning speed at first request, but it keeps losing performance and increasing memory usage on subsequent requests.

While I agree that a hypothetical `--offset` flag could reduce memory usage, I don't think it will be capable of reclaiming CPU time. You still need to find the first N matches that you want to skip over.

So all we're talking about here is memory usage. Are you really witnessing memory usage so high that it is producing problems for you? If so, then your users might be executing searches with a large amount of output, and no matter you do, that's going to cost you. Allowing users to paginate through that either requires more memory or more CPU without more sophisticated data structures (like making searches interruptible with some kind of saveable cursor).

> I'm not aware of other command line tools I could use for this, so I'm considering moving the backend to something like lucene.

Lucene is a completely different animal and solves a completely different problem that ripgrep does. ripgrep is basically "line oriented searcher, with some clever filtering." Lucene is "use the full weight of information retrieval to do relevance ranking." In fact, with Lucene, operations like "paginating through the entire result set" are discouraged and won't perform well, because it doesn't correspond to an action that a user typically performs on a large result set. (The one exception to this is if you're interested in creating training data, then one method of doing that is paying people to make exhaustive relevance judgments.)

So what's my point? My point is that if Lucene is the better tool for the problem you're trying to solve, then you should just use that.

----

With all of this said, I do think your feature request is interesting. I'm still not a fan of adding it, because every new feature needs to consider its interaction with other features. But if this feature were simple to implement and came with tests, I might be willing to maintain it. I personally probably won't add it any time soon.

---

_Comment by @wcamarao on 2017-10-02 21:06_

> I don't think it will be capable of reclaiming CPU time

Interesting. I thought it would, and that's my final goal. I also thought it was related to memory usage because I saw them growing proportionally, but no- I'm not too concerned about memory itself.

> Lucene is a completely different animal

I know, and for this case I don't need its most sophisticated features like relevance ranking you mentioned, phonetic searching and so on.

> because every new feature needs to consider its interaction with other features

I understand this very well and I appreciate it.

Changing topics slightly, I'd like to vote for #566 then. It could be used to show a total count alongside first results, giving the user an idea there's much more, so they'll probably want to refine the search instead of paging through. Combining this new count with the existing one, it could give a summary like "N results found in N files".

---

_Comment by @BurntSushi on 2017-10-03 12:30_

You can get a count of matched lines---which is a reasonable approximation of the number of occurrences---using the `--count` flag.

---

_Comment by @BurntSushi on 2017-10-03 12:33_

> I thought it would, and that's my final goal.

Think about it: if you say you want M matches after the first N matches, then ripgrep still needs to do the work to find the first N matches in order to know to skip them.

This isn't true of every method of pagination, of course. If you can save some state about where your last search left off *and* know that the corpus you're searching hasn't changed since that state was saved, then you could "pick up where you left off," which would keep CPU time and memory usage proportional to the output that is being displayed. But such a feature will most *definitely* never find its way into ripgrep.

---

_Comment by @wcamarao on 2017-10-03 20:14_

> ripgrep still needs to do the work to find the first N matches

That's slightly different than I thought. My idea is not to skip first N matches, it is to skip first N lines no matter how many matches are found within these lines.

Pagination state is kept on user side, which starts at offset 1 (first line) and reports up to N matching lines (max count). Say last matching line was 456, next request would be offset 457 and same max count (e.g. 100), meaning next 100 matching lines starting at line 457.

I understand this may be too specific for ripgrep, I'm mostly entertained by our discussion at this point.

---

_Comment by @BurntSushi on 2017-10-03 20:25_

@wcamarao oic. I didn't think of it that way since it seems so... not user friendly. :-) I can see how you could make use of it in a higher level UI though, sure.

---

_Comment by @wcamarao on 2017-10-04 00:07_

Exactly, after digging this further with you I discovered new things I didn't think before as well. Looking at the proposed interface now, I'd probably not have suggested this to be honest, so I appreciate you taking the time and exercising the idea.

---
