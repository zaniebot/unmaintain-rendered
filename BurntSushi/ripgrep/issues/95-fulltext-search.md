```yaml
number: 95
title: fulltext search
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-26T00:19:35Z
updated_at: 2021-02-01T22:52:15Z
url: https://github.com/BurntSushi/ripgrep/issues/95
synced_at: 2026-01-12T16:13:21Z
```

# fulltext search

---

_@BurntSushi_

One of the things I've wanted `ripgrep` to do from before I even started writing it was fulltext search. General fulltext search is hard, but I wonder how far we can get by focusing on fulltext search _for code_.

I know there are some tools out there already that aspire to do this:
- [CodeSearch](https://github.com/google/codesearch), which is based on Russ Cox's excellent [trigram index](https://swtch.com/~rsc/regexp/regexp4.html) article.
- [Hound](https://github.com/etsy/hound) (which is also based on Cox's write up).

I'd like to start collecting use cases for functionality like this. In particular, most of the technical problems are already solved. Obviously, we have `ripgrep` and we have a lighting fast [data structure for representing an inverted index](https://github.com/BurntSushi/fst) already (indeed, it's the same as what Lucene uses internally). I think the key problems remaining are figuring out the user interaction story. Some things off the top of my head:
- Does such a tool belong in `ripgrep`? Can use of an index be done seamlessly while still being a top notch general purpose search tool?
- What specific things does it do that make _code_ search in particular better? Do we need parsers for every language?
- How is an out-of-band index maintained? Does a user need to manually update? Is it a daemon that watches directories for file notification and update itself automatically? How do we know if an index is out-of-date?

Anyway, I'd like to start thinking about this. I don't know when I'll start on an implementation, but it'd be good to get ideas from other folks.


---

_Label `enhancement` added by @BurntSushi on 2016-09-26 00:19_

---

_Comment by @BurntSushi on 2016-09-26 23:01_

cc @comex I think you had some thoughts on this.


---

_Comment by @ghost on 2016-09-27 23:53_

cc @DivineDominion - you might have some thoughts on this, it relates to your comment https://github.com/seongjaelee/nvatom/issues/35#issuecomment-236913962


---

_Comment by @aidansteele on 2017-05-31 05:01_

I have about 50 GB of "code" in the form of Git repos with long histories across a corporate codebase. GitHub's search is abysmal in terms of functionality and ripgrep is the best thing since sliced bread, so I'm keen to wrap up ripgrep in a minimal web UI.

To begin with, I'll be happy to do something as crude as shelling out to `rg` and massaging the results. In the medium-longer term I'd be interesting in picking off any low-hanging fruit that might help my particular use-case. 

I remember reading in your blog post that the multitude of `stat` syscalls are expensive. I don't know much about the implementation of ripgrep yet, but this is something that I might be able to optimise in the rg-as-a-daemon case. It could traverse the entire directory hierarchy (and subtract files ignored by ignore files) _once_ and keep that cached in memory. Perhaps `SIGUSR1` would direct the process to reload the list of files.

Would this be a useful optimisation? The 50GB in question consists of ~700K files across ~1500 repos. Searches typically take about 3 seconds. I'm not sure if rg yet has some built-in profiling to emit how much time is spent in the dir traversal and text scanning stages.

---

_Comment by @BurntSushi on 2017-05-31 13:42_

@aidansteele To be clear, this issue is about fulltext search. It's not an issue about turning ripgrep into a daemon. :-) Turning ripgrep into a daemon is one particular implementation path for fulltext search because fulltext search generally implies the management of an index. It's hard to imagine that working well *without* some kind of daemon watching files, but that seems more like a convenience to me. It should always be possible to use ripgrep without a daemon where by the end user updates the index manually.

OK, with that said, I'll respond more directly.

> so I'm keen to wrap up ripgrep in a minimal web UI.

This sounds exciting. If you ever want to chat more about this, I'd be happy if you opened a new more targeted issue.

> In the medium-longer term I'd be interesting in picking off any low-hanging fruit that might help my particular use-case.

Thanks! Unfortunately, there's no low hanging fruit right now. Fulltext search is merely an idea in its infancy. (It's totally not even clear to me whether it should be a separate tool or not. I could make a *very good* argument for either way.) The most important thing *anyone* can do at this point in the game is help me come up with use cases. For example, if you could share more about how you envision your interaction with the tool, that would be great. Details about stat calls and daemons are better left out, but for example, things like this are useful:

* "I expect ripgrep's search to do relevance ranking on the results" 
* "I expect ripgrep's search results to be up to date with changes made to the corpus within the last 5 seconds"
* "I perceive updating ripgrep's index as expensive, so I'd like to manually control when that happens"
* "ripgrep takes programming language into account when ranking results"
* (Blatantly stealing from bitbucket here) "If I search a function name, I expect ripgrep to rank that function's definition above uses of that function"

Some of these are lofty goals---and they might not ever happen---but they are examples of the kind of thing I'm interested in at this point in time.

> I remember reading in your blog post that the multitude of stat syscalls are expensive. I don't know much about the implementation of ripgrep yet, but this is something that I might be able to optimise in the rg-as-a-daemon case. It could traverse the entire directory hierarchy (and subtract files ignored by ignore files) once and keep that cached in memory. Perhaps SIGUSR1 would direct the process to reload the list of files.

To give you an idea of how premature this is, I wouldn't expect rg-as-a-daemon to be relevant for *at least* another year, and that's only if we start working on the fulltext side of things very soon.

> Would this be a useful optimisation? The 50GB in question consists of ~700K files across ~1500 repos. Searches typically take about 3 seconds. I'm not sure if rg yet has some built-in profiling to emit how much time is spent in the dir traversal and text scanning stages.

A non-trivial amount of time is spent not only in directory traversal, but also in matching gitignore rules against each file path. I wouldn't be surprised if it shaved off a third of your search time, but I wouldn't expect much more than that.

In any case, even if we do rg-as-a-daemon, the point isn't so much to "save on directory traversal" as it is to "keep an index up to date."

---

_Comment by @DivineDominion on 2017-06-01 13:28_

Since you asked for a use case: I wish I had the power of `rg`  in my (Mac) note taking app project where I have to fiddle with my own file index -- so having a rg-daemon would be very cool since it basically means I don't have to port search and indexing across platforms and get its raw power for nearly free :)

---

_Comment by @BurntSushi on 2017-06-01 13:30_

@DivineDominion Thanks! Can you give more details on the precise interaction you'd want to see? I'm not familiar with your note taking app (or Macs, for that matter), so I don't understand what the specific integration points are. For example, can you say more about your "file index"? Do you have enough data where an index is beneficial?

---

_Comment by @BurntSushi on 2017-06-01 13:31_

I'd like to stress another point here: fulltext search isn't necessarily about making ripgrep faster. It might help for truly large scale work loads ("I want to search a 50GB repo faster than 3 seconds"), but an index isn't going to do much for the average joe. What I'm particularly interested in doing is *ranking* results as well. (And this is kind of why I might consider pushing this to a new tool, because I'm not sure how well ranking meshes with a CLI search tool.)

---

_Comment by @DivineDominion on 2017-06-01 14:12_

In an ideal world, the app offers auto-completion of note titles while you search and filters the results live. (It's already pretty fast with C strstr()) Hitting the drive for every search request (i.e. every n-th key stroke) is taxing the hardware a bit too much for my taste, though, so I work on an index at the moment. Nothing clever, just full text search based on word components.

Like this one here (Notational Velocity/nvALT):
https://www.youtube.com/watch?v=vP-rLLKL_6U&t=0m55s

Edit:
Yes, the appeal of an index only becomes reality when you can query it in-memory for my purpose. It's more about 100k notes with 200 words each that I'm dealing with. Relevancy ranking would be great for my purposes, too :)

---

_Comment by @alphapapa on 2017-07-23 16:17_

I'm a little late to this issue, but I'll share this in case it helps @BurntSushi with any ideas.

I've been maintaining and using the https://github.com/alphapapa/helm-org-rifle Emacs package for about a year and a half now.  It works well for searching Org-mode files that are already loaded into Emacs buffers, but for unopened files, it has to wait for Emacs to load each file before it can search it.  Emacs/Org users' styles vary: some use a few large Org files, while others use many smaller files, sometimes in a deep hierarchy of directories.  For example, one user uses a deep hierarchy of thousands of Org files essentially as a museum's database system.  Searching with helm-org-rifle works, but it takes some time to initially load all of the files into Emacs.

So for some time I've been looking for and thinking about solutions for searching Org files with external processes; that way Emacs would only have to load the files that the user selects results from.  And also, searching text with an optimized binary like `grep` or `rg` is faster than searching inside Emacs with even byte-compiled elisp.

Now of course, any plain-text search tool works in the general sense (`grep`, `rg`, etc), but the whole point of helm-org-rifle is to display results not as matching lines of plain text but as Org-mode entries (i.e. like Google displays matching pages, with titles and URLs, not just the matching lines in each page).  This requires the search tool to return context around the matching words up to an arbitrary token (in this case, between lines beginning and ending with one or more `*`, i.e. `^\*+\s+` in regexp terms).

The closest I've come to finding a solution is actually `git grep`, because by setting:
```
[diff "org"]
        xfuncname = "^\\*+ +.*$"
```
in `.git/config`, it provides the header of the entry the matching words are found in.  Unfortunately it doesn't seem to be completely reliable in that way; when I started testing it more seriously, I found that sometimes it just didn't do it correctly, for no apparent reason, and I wasn't able to tweak it into being reliable.

So I've looked at just about every similar tool I can find, from `grep` to `awk` to even the seemingly abandoned but uniquely powerful `agrep`, but nothing quite does the job.  I haven't looked at this in a while, but as I recall, I am basically to the point that some kind of index tool is the only suitable solution.  Of course, for the relatively simple use cases I'm aiming for (the museum example is definitely an edge case), having to run and maintain an indexer is something I want to avoid.  I would much rather let a well-optimized tool like `rg` make use of the page cache, which would be fast enough to provide several orders of magnitude of improvement.

So, for me, I wish `rg` could provide searching that would allow me to:

* Match words in arbitrary order (e.g. "one two" or "two one")
* Use boolean operators to combine and exclude words (e.g. "one NOT two", "one AND two", etc)
* Consider matching records to be everything between arbitrary separator strings (i.e. basically a non-greedy `^\*+\s+.*$`

Maybe these requirements are too specific for a general-purpose tool like `rg`, but that's my dream.  :)

Thanks.

---

_Comment by @rmccue on 2017-12-10 14:22_

Just chiming in to say I've been working on a code search service similar to Hound (using the same frontend, actually) powered by ripgrep under the hood. It's looking good so far, but this is going to be running across a massive codebase, so I imagine an index would help for performance. (Specifically, the codebase is [the entire WordPress.org plugins repository](https://github.com/markjaquith/WordPress-Plugin-Directory-Slurper), which is some 30GB+ on disk.)

It would also be quite useful to be able to rank the results, although it's probably not our main use case. (As far as I could see, there also wasn't an *actual* rg-as-a-daemon thread apart from this one.)

---

_Comment by @BurntSushi on 2017-12-10 15:27_

@rmccue Wow that's very cool! I'll be excited to see the results of that. :-)

My own personal leanings have somewhat shifted from "add fulltext search to ripgrep" to "fulltext search should probably a completely different tool, albeit with large swathes of shared implementation details with ripgrep." Note though that I am specifically thinking of ranking results, which is perhaps too ambitious. :)

---

_Comment by @BurntSushi on 2018-02-02 14:14_

I'm going to close this. It's a nice idea, but it is very nascent, and the more I've thought about this, the more I think it probably should be a separate tool. We can always revisit it.

---

_Closed by @BurntSushi on 2018-02-02 14:14_

---

_Comment by @sluongng on 2019-08-09 06:17_

Im a bit curious toward this topic now as I am running into trouble scaling up hound.
Even hound development itself seems to be quite stale at this point.

Would love to hear what are some alternative direction, perhaps involving rust or ripgrep, that we can explore?

---

_Comment by @keegancsmith on 2019-08-09 07:44_

There are alternatives to hound. Such as zoekt and Sourcegraph (disclaimer: I work at Sourcegraph).

---

_Comment by @sluongng on 2019-08-09 08:21_

> There are alternatives to hound. Such as zoekt and Sourcegraph (disclaimer: I work at Sourcegraph).

thanks for the suggestions.

Zoekt looks quite primitive, but I will invest sometime into research it. (Docs dont seem to mention RegEx though)

Sourcegraph is one of the solution we have been looking into, seems to be a bit complex and having the `EE edition` tag is a bit of a turn-off (admittedly i should do more research into it)

I found `qgrep` so far which do a form of indexing, but I think thats per dir and lack of mechanism to rebuild index on adding new file.


---

Perhaps what I am hoping for is the ability to use ripgrep (as well as equivalent `rg` binding to be used from applications level) in coupled with some indexing mechanism.

Ripgrep itself does not need to do the indexing but it could support some sort of index file/cache specification that could speed up the search (plaintext or regex).

any comment would be much appreciated @BurntSushi 

---

_Comment by @BurntSushi on 2019-08-09 11:01_

> Would love to hear what are some alternative direction, perhaps involving rust or ripgrep, that we can explore?

I don't understand what kind of answer you're hoping for here. I closed this issue and haven't given much thought to it since.

> Ripgrep itself does not need to do the indexing but it could support some sort of index file/cache specification that could speed up the search (plaintext or regex).

It already has one? You can tell ripgrep to only search a specific set of files like this: `rg <pattern> file1 file2 ... fileN`.

---

To be honest, I still have a desire to build a tool like this, but I just do not have the time. My focus right now is on improvements to `regex`. (Which is an effort I started about two years ago, and will probably take me at least another year.) Moreover, a major impediment to building a tool like this is that I personally don't have a rock solid use case to dog food it with. That almost guarantees that I'll get something about the UX of the tool very wrong. I could _invent_ a use case (i.e., clone a bunch of huge repositories), but it's fairly artificial.

_Many_ of the technical pieces required for building a tool like this already exist in the Rust ecosystem. You don't even need ripgrep. ripgrep's core is just a shell (albeit, a complex one) around a bunch of libraries. The index could be built with [`fst`](https://docs.rs/fst). There's even a cross platform crate for [listening to file system notifications](https://crates.io/crates/notify). My guess is that someone could build a _prototype_ in about a week.

But the success of a tool like this, IMO, is heavily dependent on its engineering quality, and in particular, its reliability. This is very hard to do, because the speed of the tool is the main selling point, and the speed is in turn dependent on an out-of-band index. That index needs to encapsulate state from the files being searched in a very reliable way. If the index's state gets out of sync, then searches are going to turn up incorrect or missing results. (A small number of false positives are probably tolerable, but false negatives are probably _never_ tolerable.) This state synchronization, IMO, is the most significant initial hurdle that one has to overcome to build a tool like this. Because if you give wrong results, users aren't going to trust the tool, and if users don't trust the tool, they just aren't going to use it.

---

_Comment by @alphapapa on 2019-08-09 19:00_

> Moreover, a major impediment to building a tool like this is that I personally don't have a rock solid use case to dog food it with. That almost guarantees that I'll get something about the UX of the tool very wrong. I could invent a use case (i.e., clone a bunch of huge repositories), but it's fairly artificial.

If only you were an `org-mode` user.  :)  That's my use-case: searching Org files, which are plain-text outlines whose nodes span multiple lines.  Line-based search tools work, of course, but per-line output (which omits outline headings) isn't as useful as per-node output (which treats the entire node as a result).

> That index needs to encapsulate state from the files being searched in a very reliable way. If the index's state gets out of sync, then searches are going to turn up incorrect or missing results. (A small number of false positives are probably tolerable, but false negatives are probably never tolerable.) This state synchronization, IMO, is the most significant initial hurdle that one has to overcome to build a tool like this. Because if you give wrong results, users aren't going to trust the tool, and if users don't trust the tool, they just aren't going to use it.

Yes, exactly.  John Kitchin came up with a SQLite-based indexing tool for Org files, and I used it as a base for a prototype, which works fine, but the issue is expiring old data and reindexing whole files from scratch (because any change in the plain-text source file changes the positions of all the nodes later in the file).  The bottleneck seems to be deleting all the old table rows when a file changes.

Anyway, having full-text/multi-line matching in Ripgrep would provide adequate performance for searching many large Org files, even without an out-of-band index.  I've tried all the grep-like tools I could find, but none of them were adequate to the task.

---

_Comment by @BurntSushi on 2019-08-09 19:59_

@alphapapa I don't think you're just asking for fulltext search, you're asking for something that is aware of the semantic nature of the content that is being searched. As long as you don't have gigabytes of org mode files, I imagine your use case could be solved quite well without any index at all. You'd just need something that knows how to parse org mode files to the extent that it can show the output you want.

I think fulltext search, at least as I conceptualize it here, is primarily about scale, and secondarily, potentially about relevance ranking. I think that in order to do relevance ranking well, particularly with code, you probably do need some semantic awareness. But it doesn't necessarily correspond to the same semantic awareness as needed for showing each result.

But yeah, building a search engine is a lot of work. :-)

---

_Comment by @rmchale on 2021-02-01 06:32_

Have you heard of the percolate concept?  https://docs.manticoresearch.com/2.7.5/html/searching/percolate_query.html. I learned about this for [elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-percolate-query.html).  But I think this explains it well.  Essentially its search in reverse you run documents by a “compiled” query.  I think that would be a way to accomplish this feature.   I think this could work quite quickly / on the fly because you only need to index current search document 

---

_Comment by @BurntSushi on 2021-02-01 12:45_

@rmchale Sorry, but I'm not sure I understand the relevance of percolate queries and ripgrep. Could you please be more specific?

---

_Comment by @BurntSushi on 2021-02-01 12:45_

See also: #1497 

---

_Comment by @rmchale on 2021-02-01 15:19_

> @rmchale Sorry, but I'm not sure I understand the relevance of percolate queries and ripgrep. Could you please be more specific?

It's an inside out search so you don't have to create a daemon.  You index the query and send every document through the query.  

---

_Comment by @BurntSushi on 2021-02-01 15:44_

I think you're just repeating what the link says. :) What I don't understand is why I would want to do that for ripgrep.

See my linked issue. There is no plan to make a daemon. The plan is to build an embedded IR engine, just like how Lucene works.

---

_Comment by @comex on 2021-02-01 19:15_

As far as I can tell, percolate queries aren't an alternative to indexes.  They still read through the whole search corpus just like a non-indexed search.  The use case is if you have a list of N different queries you want to run on the same corpus (you have to know all the queries in advance); with percolate queries you can read through the corpus one time instead of N times, while still giving each query its own results list.  But that doesn't really match the usage model of ripgrep.

---

_Comment by @rmchale on 2021-02-01 21:39_

ripgrep seems like its super fast about determining which files to process regex/queries/search on.  But it doesn't know anything about the files its searching until run-time.  Typically search engines like lucene and I think what you are preposing requires that you have an index built up in order to return its results.  This seems a bit counter-intuitive to how ripgrep currently works.  percolate (or lucene-monitor) you don't have to build up any index.  You take the query (or multiple queries) compile it (index it in search terms) [should be super quick if there's only one] and run it against every document that matches the ripgrep file result set.  You would throw away the index after every call to ripgrep because most likely you would be indexing a new search the next time rg was called.  To me atleast it seems like a more intuitive fit for how ripgrep works.

---

_Comment by @comex on 2021-02-01 22:02_

That isn't any different from what ripgrep already does.

---

_Comment by @rmchale on 2021-02-01 22:21_

I agree its similar to what rg is doing.  Except it doesn't support full-text search?

---

_Comment by @BurntSushi on 2021-02-01 22:42_

1. This issue is closed.
2. I linked to the issue tracking my current idea/work and it is a lot more specific then this issue.
3. Perhaps you should define what you mean by "fulltext search." (I think it was a poor choice of words to use on my part in this issue's title.)

---

_Comment by @rmchale on 2021-02-01 22:52_

Sounds good

---
