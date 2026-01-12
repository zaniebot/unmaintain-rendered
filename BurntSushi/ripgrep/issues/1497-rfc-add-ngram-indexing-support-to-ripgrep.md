```yaml
number: 1497
title: "RFC: add ngram indexing support to ripgrep"
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
  - question
assignees: []
created_at: 2020-02-22T21:42:34Z
updated_at: 2023-11-02T19:34:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1497
synced_at: 2026-01-12T16:13:23Z
```

# RFC: add ngram indexing support to ripgrep

---

_@BurntSushi_

An alternative name for this issue is "ripgrep at scale."

I've been thinking about some kind of indexing feature in ripgrep for a long
time. In particular, I created #95 with the hope of adding fulltext support to
ripgrep. But every time I think about it, I'm stymied by the following hurdles:

1. The work required to make code oriented relevance search work well.
2. The difficulty of correctly synchronizing the state of the index with the
   state of the file system.

I think what I'm coming to realize is that we could just declare that we will
do _neither_ of the above two things. My claim is that we will still end up
with something quite useful!

So I'd like to describe my vision and I would love to get feedback from folks.
I'm going to start by talking about the feature at a very high level and end
with a description of the new flags that would be added to ripgrep. I have
**not** actually built this yet, so none of this is informed by actually using
the tool and figuring out what works. I have built
[indexing tools](https://github.com/BurntSushi/imdb-rename)
before, and my day job revolves around information retrieval, so I do have some
relevant background that I'm starting with.

While the below is meant more to describe the UX of the feature, I have added a
few implementation details in places mostly for clarity purposes.

I'd very much appreciate any feedback folks have. I'm especially interested in
whether the overall flow described below makes sense and feels natural (sans
the fact that this initially won't come with anything that keeps the index in
sync). Are there better ways of exposing an indexing feature?

I'd also like to hear feedback if something below doesn't make sense or if a question about how things work isn't answer. In particular, please treat the section that describes flags as if it were docs in a man page. Is what I wrote sufficient? Or does it omit important details?

### Search is exhaustive with no ranking

I think my hubris is exposed by ever thinking that I could do code aware
relevance ranking on my own. It's likely something that requires a paid team of
engineers to develop, at least initially. Indeed,
[GitHub is supposedly working on this](https://github.blog/2018-09-18-towards-natural-language-semantic-code-search/).

The key to simplifying this is to just declare that searching with an index
will do an exhaustive search. There should be no precision/recall trade off and
no ranking. This also very nicely clarifies the target audience for ripgrep
indexing: it is specifically for folks that want searches to go faster and are
willing to put up with creating an index. Typically, this corresponds to
frequently searching a corpus that does not fit into memory. If your corpus
fits into memory, then ripgrep's existing search is _probably_ fast enough.

Use cases like semantic search, ranking results or filtering out noise are
specifically not supported by this simplification. While I think these things
are very useful, I am now fully convinced that they don't belong in ripgrep.
They really require some other kind of tool.


### ripgrep will not synchronize your index for you

I personally find the task of getting this correct to be rather daunting. While
I think this could potentially lead to a much better user experience, I think
an initial version of indexing support in ripgrep should avoid trying to do
this. In part to make it easier to ship this feature, and in part so that we
have time to collect feedback about usage patterns.

I do however think that the index should very explicitly support fast
incremental updates that are hard to get wrong. For example:

* ripgrep should avoid creating two different index entries for the same file.
  That it, ripgrep should not allow duplicates in its index.
* By default, ripgrep should not re-index files if their last modified date
  precedes their indexed date. That is, when you re-index a directory, the only
  cost you should pay is the overhead of traversing that directory and the cost
  of checking for and indexing files that have been changed since the last time
  they were indexed.
* Re-indexing a single additional file should have a _similar_ performance
  **overhead** as re-indexing many additional files at once. While it's likely
  impossible to make the performance idential between these, the big idea here
  is that the index process itself should use a type of batching that makes
  re-indexing files easier for the user.

If I manage to satisfy these things, then I think it would be fairly
straight-forward to build a quick-n-dirty index synchronizer on top of
something like the [`notify`](https://crates.io/crates/notify) crate. So, if
it's easy, why not just put this synchronization logic into ripgrep itself?
Because I have a suspicion that I'm wrong, and moreover, I'm not sure if I want
to get into the business of maintaining a daemon.


### Indexes are only indexes, they do not contain file contents

An index generated by ripgrep only contains file paths, potentially file meta
data and an inverted index identifying which ngrams occur in each file. In
order for ripgrep to actually execute a search, it will still need to read each
file.

An index does not store any offset information, so the only optimizations that
an index provides over a normal search are the following:

1. ripgrep does not need to traverse any file hierarchy. It can read all of the
   file paths to search from the index.
2. As mentioned above, ripgrep does not need to read and search the contents of
   every file. It only needs to read files that the index believes _may_
   contain a match. An index may report a false positive (but _never_ a false
   negative), so ripgrep will still need to confirm the match.

Moreover, in terms of incrementally updating the index for a particular
directory tree, ripgrep should only need to read the contents of files that
either weren't indexed previously, or are reported by the file system to have
been modified since the last time it was indexed. (ripgrep will have an option
to forcefully re-index everything, in case the last modified time is for some
reason unreliable.)

Also, as mentioned above, in addition to the file path, ripgrep will associate
some meta data with each file. This will include the time at which the file was
indexed, but might also include other metadata (like a file type, other
timestamps, etc.) that might be useful for post-hoc sorting/filtering.


### Indexes will probably not be durable

What I mean by this is that an index should never contain critical data. It
should always be able to be re-generated from the source data. This is not to
say that ripgrep will be reckless about index corruption, but its committment
to durability will almost certainly not rise to the level of an embedded
database. That is, it will likely be possible to cut the power (or network) at
an inopportune moment that will result in a corrupt index.

It's not clear to me whether it's feasible or worth it to detect index
corruption. Needing to, for example, checksum the contents of indexed files on
disk would very likely eat deeply into the performance budget at search time.
There are certainly some nominal checks we can employ that are cheap but will
not rise to the level of robustness that one gets from a checksum.

This is primarily because ripgrep will not provide a daemon that can amortize
this cost. Instead, ripgrep must be able to quickly read the index off disk and
start searching immediately. However, it may be worthwhile to provide the
_option_ to check the integrity of the index.

The primary implication from an end user level here isn't great, but hopefully
it's rare: it will be possible for ripgrep to panic as part of reading an index
in a way that is not a bug in ripgrep. (It is certainly possible to treat
reading the index as a fallible operation and return a proper error instead
when a corrupt index is found, but the `fst` crate currently does not do this
and making it do it would be a significant hurdle to overcome.)

In order to prevent routine corruption, ripgrep will adopt Lucene's "segment
indexing" strategy. A more in depth explanation of how this works can be found
elsehwere, but effectively, it represents a partioning of the index. Once a
segment is written to disk, it is never modified. In order to search the index,
one must consult all of the active segments. Over time, segments can be merged.
(Segmenting is not just done to prevent corruption, but also for decreasing
the latency of incremental index updates.)

Additionally, ripgrep will make use of advisory file locking to synchronize
concurrent operations.


### Indexs may not have a stable format at first

I expect this feature will initially be released in an "experimental" state.
That is, one should expect newer versions of ripgrep to potentially change the
index format. ripgrep will present an error when this happens, where the remedy
will be to re-index your files.


### New flags

I don't think this list of flags is exhaustive, but I do think these cover
the main user interactions with an index. I do anticipate other flags being
added, but I think those will be more along the lines of config knobs that
tweak how indexing works.

I've tried to write these as if they were the docs that will end up in
ripgrep's man page. For now, I've prefixed the flags with `--x-` since I
suspect there will be a lot of them, and this will help separate them from the
rest of ripgrep's flags.


#### `-X/--index`

Enables index search in ripgrep. Without this flag set, ripgrep will never
use an index to search.

ripgrep will find an index to search using the following procedure. Once a
step has found at least one index, subsequent steps are skipped. If an index
could not be found, then ripgrep behaves as if `-X` was not given.

1. If any indexes are provided on the command line as directory paths to the
   index itself, then all of them are searched, in sequence.
2. If `RIPGREP_INDEX_PATH` is set to a valid index, then it is searched.
3. If there exists a `.ripgrep` directory in the current working directory and
   it contains a valid index, then it is searched.
4. Step (3) is repeated, but for each parent directory of the current working
   directory.

If `-X` is given twice, then ripgrep will stop searching if an index is present
and the query makes it unable to use the index.


#### `--x-crud`

Indexes one or more directories or files. If a file has already been indexed,
then it is re-indexed. By default, a file is only re-indexed if it's last
modified time is more recent than the time it was last indexed. To force
re-indexing, use the `--x-force` flag. If a file that was previously indexed is
no longer accessible, then it is removed from the index.

The files that are indexed are determined by ripgrep's normal filtering
options.

The location of the index itself is determined by the following procedure. Once
a step has found an index, subsequent steps are skipped.

1. If `--x-index-path` is set, then the index is written to that path.
2. If `RIPGREP_INDEX_PATH` is set, then its value is used as the path.
3. If the current working directory contains a `.ripgrep` path and is a valid
   existing index, then it is updated.
4. Step (3) is repeated, but for each parent directory of the current working
   directory.
5. If an index could not otherwise be determined, then it is written to the
   current working directory at `.ripgrep`. If `.ripgrep` already exists and is
   not a valid index, then ripgrep returns an error.

If another ripgrep process is writing to the same index identified via the
above process, then this process will wait until the other is finished.

As an example, the following will create an index of all files (recursively)
in the current directory that pass ripgrep's normal smart filtering:

    $ rg --x-crud

And this will search that index:

    $ rg -X foobar

Note that the index created previously can now be updated incrementally. For
example, if you know that `foo/bar/quux` has changed, then you can run:

    $ rg --x-crud foo/bar/quux

To tell ripgrep to re-index just that file (or directory). This works even if
`foo/bar/quux` has been deleted, where ripgrep will remove that entry from its
index.


### Prior work

I believe there are three popularish tools that accomplish something similar to
what I'm trying to achieve here. That is, indexed search where the input is a
regex.

* Russ Cox's [codesearch](https://github.com/google/codesearch), which is
  famounsly described in his
  ["Regular Expression Matching with a Trigram Index"](https://swtch.com/~rsc/regexp/regexp4.html)
  article. Other tools, like
  [Hound](https://github.com/hound-search/hound)
  are based on `codesearch`.
* [qgrep](https://github.com/zeux/qgrep), which is described in more detail
  [here](https://zeux.io/2019/04/20/qgrep-internals/).
* [livegrep](https://github.com/livegrep/livegrep), which also provides a very
  nice front-end for searching. It is described in more detail
  [here](https://blog.nelhage.com/2015/02/regular-expression-search-with-suffix-arrays/).

(There are other similarish tools, like
[Recoll](https://www.lesbonscomptes.com/recoll/)
and
[zoekt](https://github.com/google/zoekt),
but these appear to be information retrieval systems, and thus not exhaustive
searching like what I've proposed above.)

ripgrep's index system will most closely resemble Russ Cox's `codesearch`. That
is, at its core, it is an inverted index with ngrams as its terms. Each ngram
will point to a postings list, which lists all of the files that contain that
trigram. The main downside of `codesearch` is that it's closer to a proof of
concept of an idea rather than a productionized thing that will scale. Its most
compelling downside is it performance with respect to incremental updates.

`qgrep` and `livegrep` both represent completely different ways to tackle this
problem. `qgrep` actually keeps a compressed copy of the data from every file
in its index. This makes its index quite a bit larger than `codesearch`, but
can do well with spinning rust hard drives due to sequential reading. `qgrep`
does support incremental updates, but will eventually require the entire index
to be rebuilt after too many of them. My plan with ripgrep is to make
incremental updates a core part of the design such that a complete re-index is
never _necessary_.

`livegrep` uses suffix arrays. I personally haven't played with this tool yet,
but mostly because it seems like incremental updates are slow or hard here.


---

_Label `enhancement` added by @BurntSushi on 2020-02-22 21:42_

---

_Label `question` added by @BurntSushi on 2020-02-22 21:42_

---

_Comment by @wezm on 2020-02-22 22:58_

Seems like a useful proposal with a sensible scope. 

Sorry if this is bike shedding minor details, but with my, "what if this was a man page hat on", I don’t think `--x-crud` is a good flag name, especially with [this definition of crud](https://www.merriam-webster.com/dictionary/crud) in mind. I would suggest `--update-index` as a possible alternative. 

---

_Comment by @aslushnikov on 2020-02-23 00:32_

I currently solve this with a combination of a slightly changed [`csearch`](https://github.com/google/codesearch/blob/master/cmd/csearch/csearch.go) that just returns a list of file candidates which I then feed to `ripgrep`.

This RFC describes exactly what I need with a very precise scope. I'd love to see this implemented!

---

_Comment by @MikeFHay on 2020-02-23 10:41_

Cool!
Would this work with the `--search-zip` flag? Typically if I'm searching uncompressed on-disk data rg is already plenty fast.

The one thing that would really excite me is if this got integrated into cloud-scale search engines like Github's code search or AWS Cloudwatch Logs search. I've always wanted to use regex instead of word search for those things. 

---

_Comment by @BurntSushi on 2020-02-23 12:07_

> Would this work with the --search-zip flag? 

Good question. I certainly think it should. It should also work with the `--pre` flag. Those flags will need to be specified during indexing though. And then probably again during search.

> The one thing that would really excite me is if this got integrated into cloud-scale search engines like Github's code search or AWS Cloudwatch Logs search. I've always wanted to use regex instead of word search for those things.

My intent is to design this such that it should be able to scale "arbitrarily," so it's certainly possible. The main problem is that at that scale, you would need to ban regexes that search too much of the corpus. It is very possible to implement this ban, but whether that's an acceptable user experience, I'm not sure. For example, the regex `\w` has to search every file. (ripgrep will be able to detect trivial cases like that and just skip using the index altogether. Similar to how a relational database's query planner might not use your index of it needs to visit most of the data in the table.)

---

_Comment by @petdance on 2020-03-16 02:23_

Nice.  I've often thought about ngram-indexing in ack, but have never pursued it.  I'm glad you did.

---

_Comment by @sluongng on 2020-03-16 08:16_

I would love to have this as I have been advocating for such indexing feature in the other issue as well.
Thanks for this RFC, I would love to take a look at the initial implementation and possibly consider to remaking [esty/hound](https://github.com/hound-search/hound) with ripgrep index :D 

---

_Comment by @ceronman on 2020-03-16 08:28_

It's great to hear that this is coming to ripgrep. I'm still using `codesearch` for a particularly large code base (around 8M loc). Notes about my experience with codesearch:

1) codesearch silently ignores indexing of large files, I think the limit is 20K ngrams. Only fix for this was to manually change and recompile codesearch. If there is such limit in ripgrep, it should be configurable  and there should be a clear warning about it when indexing.

2) Because indexing can be quite slow, sometimes, after having some changes in the code that are not yet indexing, I prefer to use ripgrep instead of csearch to make sure that everything is included. I'm not sure if this is feasible, but it would be really nice if ripgrep could search using the index and then find files older than the index and search them directly. In other words, have the option of using index only for files older than the index, and use regular search for newer files.

---

_Comment by @zoidyzoidzoid on 2020-03-16 08:51_

@MikeFHay Have you tried using [sourcegraph.com](https://sourcegraph.com/search?q=repo:%5Egithub%5C.com/BurntSushi/ripgrep%24+is%7Cwas&patternType=regexp) instead for searching repos on GitHub?

---

_Comment by @joshtriplett on 2020-03-16 10:29_

> That is, it will likely be possible to cut the power (or network) at
an inopportune moment that will result in a corrupt index.

For people who index entire directory trees at once, rather than individual files, I'm hoping that indexes can get updated by atomic rename, which would prevent the majority of index corruption.

Also, I'm hoping that the index logic is provided as a separate crate that doesn't inherently depend on integration with path searching, so that it would be possible (for instance) to index the history of a git repository.

---

_Comment by @jasonwilliams on 2020-03-16 13:21_

> Enables index search in ripgrep. Without this flag set, ripgrep will never
use an index to search.

Never is a strong word, is there a reason you wouldn’t want this to be the default at one point (in the far future). 

---

_Comment by @BurntSushi on 2020-03-16 13:56_

These are _amazing_ questions/feedback folks. Really appreciate. Keep more coming like this!

@ceronman 

> codesearch silently ignores indexing of large files, I think the limit is 20K ngrams. Only fix for this was to manually change and recompile codesearch. If there is such limit in ripgrep, it should be configurable and there should be a clear warning about it when indexing.

Yeah, I noticed this and I've been thinking on what to do about it. It is enticing to adopt a similar heuristic in ripgrep because a single large file could really screw the pooch. That is, a large file probably has a lot of different ngrams in it, so it has a high likelihood of appearing in candidate sets generated by the index. Which would then kind of defeat some of the large gains of using an index in the first place, since you wind up searching that large file---which might take as long as it takes to search thousands of tiny files.

The main reason why I'm initially not a fan of having such a limit is because it's an _additional_ filter on files on top of what ripgrep does normally, but it's a filter that only makes sense in the context of indexing. One possible alternative is to make the filter opt-in. After all, ripgrep already has a `--max-filesize` flag. I guess it depends on how likely it is for a large file to make index searches unnecessary slow for unsuspecting users.

But yes, if this does wind up being an opt-out feature specific to indexing, then ripgrep would emit a warning message for each large file skipped. And the limit would certainly be configurable.

> Because indexing can be quite slow, sometimes, after having some changes in the code that are not yet indexing, I prefer to use ripgrep instead of csearch to make sure that everything is included. I'm not sure if this is feasible, but it would be really nice if ripgrep could search using the index and then find files older than the index and search them directly. In other words, have the option of using index only for files older than the index, and use regular search for newer files.

My hope is that ripgrep will make incremental indexing fast enough where you can just drop this particular behavior. :-)

I think if this were to exist, it would probably need to be an opt-in feature. In particular, one of the key benefits that an index will bring is the ability to avoid re-walking the directory tree, which can take non-trivial time. But I agree, some kind of flag to say "use the index but search newer files like normal" seems like a good idea.

@joshtriplett 

> For people who index entire directory trees at once, rather than individual files, I'm hoping that indexes can get updated by atomic rename, which would prevent the majority of index corruption.

Ah yeah interesting. I hadn't really thought of a "blow up the world and re-index" mode, but it probably makes sense to have something like that. I do hope that incremental indexing will negate the need for such things in most cases, but sure, "re-index the world" would indeed likely take the atomic rename strategy.

> Also, I'm hoping that the index logic is provided as a separate crate 

Yes, it will be contained in the [`grep-index`](https://crates.io/crates/grep-index) crate.

> that doesn't inherently depend on integration with path searching, so that it would be possible (for instance) to index the history of a git repository.

This may be trickier to avoid. I believe the implementation path in my head does indeed focus around having a path for each thing you want to search. I'll consider the git history use case though and see if something like it can be supported. Maybe the way to think about it is not "path searching" but rather "searching documents where each document has some unique ID that can be resolved to its content." Making that pluggable at the crate level certainly seems doable.

---

_Comment by @BurntSushi on 2020-03-16 13:59_

>> Enables index search in ripgrep. Without this flag set, ripgrep will never use an index to search.
>
> Never is a strong word, is there a reason you wouldn’t want this to be the default at one point (in the far future).

I guess it just seems like surprising UX to me. Searching with an index can lead to subtle state synchronization bugs where the index is lagging behind your actual corpus (or some other sync issue). So to me, it seems like it should be opt-in.

Also, I think you might be giving more weight to "never" in this context than I intended. It's describing the behavior of a specific release of ripgrep, and not necessarily my future intent.

---

_Comment by @mpdn on 2020-03-16 17:45_

>ripgrep's index system will most closely resemble Russ Cox's `codesearch`. That
is, at its core, it is an inverted index with ngrams as its terms.

You should consider using Bloom filters instead. Totally sidesteps the large file problem, has constant size which is a lot easier to manage, and uses less space. Or XOR filters, which are supposedly more compact: https://lemire.me/blog/2019/12/19/xor-filters-faster-and-smaller-than-bloom-filters/

Edit: That is using a Bloom filter instead of the inverted index, i.e. still a bloom filter over the ngrams.

---

_Comment by @BurntSushi on 2020-03-16 17:59_

@mpdn I know I dipped into implementation details, but I'd really like to keep this ticket focused on the higher level user story so that folks are more inclined to participate. If we bog it down into implementation details, then I fear the discussion may become too intimidating for most. So with that in mind, I created #1518 so that folks who want to talk details can. :-) I responded to your comment there.

---

_Comment by @omac777 on 2020-03-16 18:47_

There is some overlap between the goals to add ngram support to ripgrep. THAT would be phenomenal! Work was started to achieve that in rust.  There was a c/c++ implementation called BigGrep, a set of tools, but the one that does the indexing is called bgindex.  At Geekweek in Nov. 2019, they started a rust re-implementation called rs-bgindex, but we only had a week there.
https://gitlab.com/geekweek-vi/4.3-geekweek/rs-bgindex
https://gitlab.com/geekweek-vi/4.3-geekweek/rs-cccslib
https://gitlab.com/geekweek-vi/4.3-geekweek/croaring-rs
Most of the infrastructure for the command-line as already been built, but the part that calls croaring-rs(roaring bitmaps capability) was never glued in.  Maybe you could take a crack at it there.
https://gitlab.com/geekweek-vi/4.3-geekweek/rs-cccslib/-/blob/master/src/CCCSBgIndexConf/mod.rs#L462
https://gitlab.com/geekweek-vi/4.3-geekweek/rs-cccslib/-/blob/master/src/CCCSBgIndexConf/mod.rs#L263
https://gitlab.com/geekweek-vi/4.3-geekweek/rs-bgindex/-/blob/master/src/main.rs#L297

---

_Comment by @BurntSushi on 2020-03-16 19:18_

@omac777 Please keep implementation details in #1518, as requested above.

---

_Comment by @joshtriplett on 2020-03-17 08:23_

On Mon, Mar 16, 2020 at 06:56:55AM -0700, Andrew Gallant wrote:
> @joshtriplett 
> 
> > For people who index entire directory trees at once, rather than individual files, I'm hoping that indexes can get updated by atomic rename, which would prevent the majority of index corruption.
> 
> Ah yeah interesting. I hadn't really thought of a "blow up the world and re-index" mode, but it probably makes sense to have something like that. I do hope that incremental indexing will negate the need for such things in most cases, but sure, "re-index the world" would indeed likely take the atomic rename strategy.

I wasn't necessarily thinking of re-indexing everything, so much as
indexing *enough* that it makes sense to write a new file containing the
new index "segment"; that file could then be written atomically.

> > Also, I'm hoping that the index logic is provided as a separate crate 
> 
> Yes, it will be contained in the [`grep-index`](https://crates.io/crates/grep-index) crate.

Excellent.

> > that doesn't inherently depend on integration with path searching, so that it would be possible (for instance) to index the history of a git repository.
> 
> This may be trickier to avoid. I believe the implementation path in my head does indeed focus around having a path for each thing you want to search. I'll consider the git history use case though and see if something like it can be supported. Maybe the way to think about it is not "path searching" but rather "searching documents where each document has some unique ID that can be resolved to its content." Making that pluggable at the crate level certainly seems doable.

A unique ID seems fine, sure.


---

_Comment by @aidansteele on 2020-04-28 23:45_

Some high-level feedback:

I am *really* delighted with your decision to *not* support relevance ordering and index synchronisation. I feel this is absolutely the right call as it means that ripgrep can very easily be dropped into a larger system. I'm generally satisfied with all the details mentioned. That said, a query regarding this:

>  ripgrep will associate some meta data with each file. This will include the time at which the file was indexed, but might also include other metadata (like a file type, other timestamps, etc.) that might be useful for post-hoc sorting/filtering.

Do you think this metadata would/should be extensible to user-defined metadata? E.g. a user might apply `work`, `oss`, `gpl`, etc tags to various files and want to narrow query results based on this. Should that be within the scope of ripgrep? I could go either way on this.

---

_Comment by @BurntSushi on 2020-04-28 23:48_

@aidansteele Thanks for the feedback. I suspect that the metadata will probably permit the library user to insert an arbitrary blob of bytes that they can use, yes. I doubt that will be exposed at the CLI level though. (Because ripgrep's CLI just really isn't expressive enough for something like that.)

---

_Comment by @aidansteele on 2020-04-28 23:50_

That sounds ideal. And I think that's a reasonable judgment on the CLI. Thanks for your great work.

---

_Comment by @luser on 2020-05-06 02:34_

First of all, I really appreciate the thought and care you've put into proposing this feature! I think your decision to make a daemon that updates the index out of scope is very reasonable.

One suggestion I have is that I think it might be better to make your `--x-crud` into a separate command. `rg` is a tool with a single purpose: searching. Making it able to use a precomputed index is a natural extension, but the index update commands are going to feel shoehorned in no matter how you expose them and the `rg` command is not really amenable to adding subcommands.

If you get a basic implementation of index creation/updating I think it would be straightforward to prototype a continuous index update process using [`watchman`](https://facebook.github.io/watchman/).

---

_Comment by @BurntSushi on 2020-05-06 02:42_

@luser Thanks for the feedback. I'll noodle on it, but as of now, I'm still inclined to keep one command. It does feel a little shoe-horned, but `rg` has been shoe-horning subcommands-as-flags since day 1. See `--files` and `--type-list` for example. But yeah, if it feels too crazy once it actually gets implemented, then a separate command is definitely a doable option. It would itself be a lot of work though.

---

_Comment by @luser on 2020-05-06 13:56_

Another way to slice that would be "`rg` runs a different command when its filename is `rg-index`" or something like that, so you could hardlink `rg` to `rg-index`. I don't think that's something you can get out of `cargo install` currently, though.

---

_Comment by @xaljer on 2020-05-24 04:11_

Can it work like gtags? gtags only provides commands to generate and update index, but doesn't maintain the index files by itself. Editor plugins can auto update the index since they know when user change some files.

By the way, if to avoid re-walking the directory tree can have significant gains, can rg learn from git tree blob to speed up searching in a git repository?

---

_Comment by @BurntSushi on 2020-05-24 16:22_

> Can it work like gtags? gtags only provides commands to generate and update index, but doesn't maintain the index files by itself. Editor plugins can auto update the index since they know when user change some files.

You mean ctags? I'm not quite sure what you mean when you say "it generates and updates the index" but also "doesn't maintain the index." These seem like contradictory statements. If all you mean is, "can ripgrep just provide commands to create, update and search the index" while letting external processes run those commands, then yes, that is exactly what's being proposed as an initial version of this feature. Please see the "ripgrep will not synchronize your index for you" section in the first comment of this issue.

> By the way, if to avoid re-walking the directory tree can have significant gains, can rg learn from git tree blob to speed up searching in a git repository?

This question isn't really on-topic for this thread, but the answer is "not really." ripgrep may search files outside of git's index. For example, an `.ignore` file may whitelist files that are in `.gitignore`, and this is an explicitly documented and useful feature. If you want to discuss this further or ask other questions, then please use [Discussions](https://github.com/BurntSushi/ripgrep/discussions).

---

_Comment by @nzig on 2020-06-15 14:12_

I really like this proposal.

It sound like the underlying assumption here is that reading the index into memory for each search will provide acceptable performance. This is a trade-off with a daemon approach which could keep the index in-memory, but has the complication of managing a daemon.

I don't have good intuition about the performance trade-off, but if it is substantial I would want to make sure that the daemon experience is good. Maybe the list of places to find the index could be extended to auto-discover such a (maybe third-party) daemon in some way, with a simple protocol? Alternatively, maybe storing the index on `tmpfs` provides enough of a speed-up?

For reference, I use ripgrep (often through vscode, sometimes from CLI) for searching code bases that I audit. For very large projects this is usually too slow, so I use and index like OpenGrok or [VsChromium](https://chromium.github.io/vs-chromium/). It would be great if I could use ripgrep for everything!

---

_Comment by @BurntSushi on 2020-06-15 14:29_

@nzig The index would be designed to be read via memory maps. So the entire index doesn't need to be read off disk immediately. This is a substantial simplification to the implementation, as it pushes the onus of managing what's on disk and what isn't to the OS.

If you want to get more into the weeds on implementation details, I'd recommend #1518 for that. I'm trying to keep this issue more focused on the user experience, which I think is a discussion that more people can participate in.

---

_Comment by @nzig on 2020-06-15 14:59_

@BurntSushi Thanks for the reply!

If I'm understanding correctly, you're saying that you think reading the index via memory-mapped file and relying on OS caching will be fast enough so that a daemon won't be needed?

I think this is on the intersection between UX and implementation. If the performance can't be good enough without a daemon, some users will want a daemon and will want the UX for that to be good as well. If the performance is acceptable, then everyone will have a good time with this proposal 😀.

---

_Comment by @BurntSushi on 2020-06-15 15:06_

Yes, I am saying that. There is precedent for this. It's how Lucene works, for example.

And yeah, I see how this is at the intersection of the UX and implementation. More or less, if you want to cross deeper into implementation details, then the other issue is better for it. :)

---

_Comment by @gurgeous on 2020-06-21 19:20_

Hey, I just noticed this thread and thought I'd chime in. I'm a heavy user of rg, and I've used/built many search tools for my various day jobs. Most recently I used codesearch quite extensively for a research project totaling 30gb across 1M files - https://freshchalk.com/blog/150k-small-business-website-teardown-2019. Thank you so much for creating and maintaining ripgrep! It's making our lives better and I appreciate your dedication.

As to your original design questions:

- **no rankings** and **no sync** -  I think it's fine to have exhaustive search with no rankings, and sync can happen as a separate process. Those constraints don't bother me at all while working with codesearch. It would be nice if search results were deterministic (alphabetical).

- **no file contents** - Is this mainly a tradeoff between disk space and speed? I definitely appreciated the speed of codesearch, even for queries that had many matches. On the other hand, the index was a pig and I was uninclined to replicate it across machines. Note that excluding file contents will complicate certain use cases. I've built search systems that distributed the index to mobile apps, for example...

- **not durable/segments/corruption** - Ripgrep is already fast enough to be used without indexing for many projects, especially when used repeatedly with disk caching. It might be helpful to consider the data set size at which indexing becomes important. I appreciated the fact that codesearch kept my index in a single file, which could be (atomically?) regenerated from scratch. Incremental indexing was not important and I setup a cron job to rebuild the index nightly. For which use case is this simple model insufficient?

- `--x-crud` Agree with others, I would gently suggest alternate naming. Perhaps `--x-build`, or even `--X-build` if you want to be pedantic (like me) :)

This sentence makes me nervouse - `If an index could not be found, then ripgrep behaves as if -X was not given.`. For serious use cases, I would prefer something that fails fast. I believe that indexed searches will be for "serious use cases" since ripgrep is so amazingly fast. If that's unpalatable, a command like `--X-explain` or `--X-verbose` would be helpful in tracking down issues related to missing (or misconfigured) indexes.

I anticipate many broken cases where people try to use un-indexed directories with indexed queries, since ripgrep is so wonderfully flexible with paths. This is likely to be more prevalent with a complex index location procedure... Maybe `-X` should always be `-X [indexpath]`. It might be useful to constrain the problem by only allowing a single target directory, or even using a separate command like `rg-x` to enforce that constraint and differentiate from good old `rg`. I'd recommend giving this UX issue serious thought if you aren't too attached to your initial proposal.

Happy to brainstorm more if that would be helpful. Very excited to start indexing!

---

_Comment by @jcushman on 2020-12-10 18:33_

Thanks for working on this! I just wanted to share another usecase in case it helps.

I work on [case.law](https://case.law/), where our core users are academic researchers doing NLP on US caselaw, and I've experimented before with using the regex crate to provide on-demand regex search and extraction of research data sets across the corpus. (Like, let's get a list of every dollar figure discussed in a US court decision along with metadata about the case.) The corpus is about 6 million documents and a couple hundred gigs of text, and we have limited web server budget so we can only share features publicly that can be efficiently implemented. The feature set you described sounds perfect -- e.g. exhaustive unranked search and managing reindexing ourselves are no problem.

The design question I'd highlight is, is there anything in the library design that would interfere with standing up a public web server that uses the index? Either anything specific to the index, like adversarial inputs that play on the index design or ability to run multiple queries simultaneously, or interactions with issues that would apply even without an index, like the ability to paginate results or shut down queries that exceed resource limits.

Feeling a little out of my depth, so those are just examples -- I'll just say I'm excited to use this to offer researchers an easier time extracting text from large online corpuses, and I hope the library design makes that doable.

---

_Comment by @BurntSushi on 2020-12-10 19:26_

@gurgeous Sorry, I am just seeing your comment now! Sorry I missed it before.

>  Thank you so much for creating and maintaining ripgrep!

:-) Thanks for the kind words! And thank you for posting feedback. It's exactly the kind of thing I was hoping to get!

> * **no file contents** - Is this mainly a tradeoff between disk space and speed? I definitely appreciated the speed of codesearch, even for queries that had many matches. On the other hand, the index was a pig and I was uninclined to replicate it across machines. Note that excluding file contents will complicate certain use cases. I've built search systems that distributed the index to mobile apps, for example...

Just to clarify here, `codesearch` doesn't put the file contents into its index either. And yes, its index is a pig, because the tool doesn't do anything too clever compression wise. With that said, IR indices, even without file contents, do tend to be pigs. I wouldn't expect anything magical to happen here. To a first approximation, I'd say the _index_ size itself will be about the same order of magnitude as the corpus size itself. Maybe half as much or right around the same size is my guess.

In terms of ripgrep, its "search by index" functionality would be fairly limited if you don't have the original corpus available. The only thing it could really tell you is the set of files which _might_ contain a match. Otherwise, ripgrep will still need to search the file itself to report matches.

So if you're going to put a ripgrep index on a mobile app, you'll need to carry the original corpus with it. There will likely be ways to reduce corpus size. e.g., You could compress each individual file and ripgrep will decompress each before searching them when used with the `-z/--search-zip` flag.

The reason for doing this is primarily simplicity. With that said, this is the kind of thing that could be added later if there was a compelling motivated to do it. Comparatively speaking, changing the indexing infrastructure to support associating some additional blob of data isn't that big of a deal. The real problem with it, IMO, is that it begets more work. Because when you start including file content in the index itself, you are now also likely responsible for keeping it compressed to keep storage requirements lower. It's also something that will increase indexing time as well.

> * **not durable/segments/corruption** - Ripgrep is already fast enough to be used without indexing for many projects, especially when used repeatedly with disk caching. It might be helpful to consider the data set size at which indexing becomes important. I appreciated the fact that codesearch kept my index in a single file, which could be (atomically?) regenerated from scratch. Incremental indexing was not important and I setup a cron job to rebuild the index nightly. For which use case is this simple model insufficient?

It's important when you want to minimize the latency between when the content changes and when it becomes available to search. Code is one such example, but really anything that has both frequently changing data and a desire for it to be searchable quickly fits this mold.

There's no doubt that this is not all use cases, but I think it's an important quality of life enhancement. I think the cron job approach you describe is perhaps "good enough," but I'd really love it if you didn't have to resort to that. It's also a difference maker in that most search indexing tooling does not support this. (And it doesn't because it's hard!) So if I'm going to go through the trouble of building something like this, I should at least try to do something that makes it obviously better than alternatives.

Also, a ripgrep index will be a directory, not a single file. And on top of that, I am currently planning on reversing course with respect to my lack-of-durability idea. I'd like to fully guarantee durability. I am perhaps biting off a bit more than I can chew, and maybe I won't start there, but that's my current plan.

I have started work on planning the design for the underlying IR engine for this, and its scope is a bit bigger than just ripgrep's use case. (But not by much.) It's a bit disorganized at the moment and not totally consistent with itself, but if anyone's interested in the details, it's here: https://github.com/BurntSushi/nakala/blob/master/doc/PLAN.md

> This sentence makes me nervouse - `If an index could not be found, then ripgrep behaves as if -X was not given.`. For serious use cases, I would prefer something that fails fast. I believe that indexed searches will be for "serious use cases" since ripgrep is so amazingly fast. If that's unpalatable, a command like `--X-explain` or `--X-verbose` would be helpful in tracking down issues related to missing (or misconfigured) indexes.

I'm also a strong proponent of failing fast, and I suspect I'll reverse course on my initial statement here. If only because it's the prudent option. It's much easier to turn an error into a non-error than the other way around.

---

_Comment by @BurntSushi on 2020-12-10 19:36_

@jcushman 

> The design question I'd highlight is, is there anything in the library design that would interfere with standing up a public web server that uses the index?

I think that in general, ripgrep was _not_ designed with a threat model that allows untrusted input (whether that be regexes or corpora). While its default regex engine promises never to take more than linear time on input (i.e., it is not subject to catastrophic backtracking), that doesn't mean it will always be lightning fast on all inputs.

And I think that generally applies to ripgrep-with-an-index as well. Consider what happens when you stand up your web server and a user decides to run the regex `\w`. It's likely that it will match every single line in your corpus. Now obviously regexes as nefariously simple as `\w` are easy to detect, but a regex that matches most of your corpus can be arbitrarily complex.

ripgrep-with-an-index does have on trick up its sleeve that could help you. Namely, if all searches use the index then it can know, quickly and cheaply, roughly how many files it will need to search to produce a complete set of results. If this number is too high (for example, the regex `\w` would automatically require ripgrep to search every file), then it would be easy to expose a flag with ripgrep that lets you reject the search.

Now, what this doesn't prevent is the normal case of a regex being slow. Namely, if you have a document in your corpus that is big and the user has crafted a regex that runs particularly slowly on that document, then it wouldn't be hard for it to peg your CPU.

Aside from that, no matter how fast ripgrep is, you would need some kind of rate limiting.

Overall, exposing regexes to end users is very difficult to do safely in a way that is 100% bullet proof. ripgrep-with-an-index could make it better, but it's not something I would ever advertise as a feature. Or at least, if I did, there would need to be dedicated work invested into ensuring that a single search request couldn't peg the CPU. That would require specific feature development inside of ripgrep.

> Either anything specific to the index, like adversarial inputs that play on the index design or ability to run multiple queries simultaneously, or interactions with issues that would apply even without an index, like the ability to paginate results or shut down queries that exceed resource limits.

I think the "adversarial inputs" I covered above. Running multiple queries would be possible, certainly. The index will support multiple concurrent readers and writers, so you can invoke as many ripgrep processes as you like.

I don't see result pagination as something that would ever get added to ripgrep. Since this is exhaustive unranked search, you could always just partition your corpus to approximate that.

Shutting down queries that exceed limits is what I had in mind when I said "specific feature development inside of ripgrep." That has an outside possibility of happening, but not in the initial implementation. Whether it happens or not depends on how much it complicates ripgrep and whether it is enough to actually unlock compelling use cases.

---

_Comment by @kokes on 2020-12-11 12:01_

@jcushman 

> The design question I'd highlight is, is there anything in the library design that would interfere with standing up a public web server that uses the index? Either anything specific to the index, like adversarial inputs that play on the index design or ability to run multiple queries simultaneously, or interactions with issues that would apply even without an index, like the ability to paginate results or shut down queries that exceed resource limits.

This has been tackled, albeit without the indexing part and at a smaller scale, by @simonw recently - see [datasette-ripgrep](https://github.com/simonw/datasette-ripgrep). He may have something to say regarding its performance, security, scale etc.

---

_Comment by @BurntSushi on 2020-12-11 13:26_

@kokes Yes, it looks like it uses a time limit to kill the ripgrep process: https://github.com/simonw/datasette-ripgrep/blob/dd97a44cd77367fa447a520e6cdbb99ef829b77f/datasette_ripgrep/__init__.py#L63-L71

That's probably the best you can do right now.

---

_Comment by @jcushman on 2020-12-11 14:23_

Thanks for these detailed thoughts, and the link!

The contract on datasette-ripgrep's `run_ripgrep` function looks totally workable, so it probably comes down to whether there are any adversarial inputs that could manage to break the contract somehow. Looks OK to me?

In case more detail helps understand the usecase: when implementing this I would add parameters to limit my `run_ripgrep` function by CPU count [or maybe mess around with `nice` and see if that's enough], and by files matched and lines matched per file. Then I'd call it in a couple of different modes, one where users could get a small number of matches interactively to test out regexes and one where they could submit jobs to a queue for exhaustive extraction. Those would have separately tuned limits on simultaneous calls / runtime / etc., which I think would be fine to keep both features useful most of the time and also ensure they didn't interfere with anything else the server was doing.

It's also fine with me if this turns out to better handled with lower level tools like the `grep-index` crate, in which case I'd just offer it as a usecase to consider when thinking about the library APIs.

---

_Comment by @BurntSushi on 2020-12-11 15:12_

> The contract on datasette-ripgrep's `run_ripgrep` function looks totally workable, so it probably comes down to whether there are any adversarial inputs that could manage to break the contract somehow. Looks OK to me?

The only other thing I can think of is memory usage. And that can come in two ways.

Firstly, it's possible for pathological file contents to provoke very high memory usage. Usually these inputs come in the form of binary files. ripgrep has heuristics to prevent this in the default case (it replaces NUL bytes with `\n`, just as GNU grep does), but if you use the `-a/--text` flag, then this behavior is disabled. If your corpus is standard plain text data with "normal" sized lines, then I don't think there are any memory usage problems of this type that you need to be aware of.

Secondly, the other type of memory usage problem comes from the regex itself. This can actually be controlled via the `--regex-size-limit` flag. The docs for that flag are unfortunately wrong. The default used to be 10MB, but it's [now 100MB](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/regex/src/config.rs#L52-L55). The `--dfa-size-limit` flag is similarish, except its default is set to 1GB. So you may want to tweak both of those flags to use lower limits.

I think that's all I've got at the moment. Please consider that I haven't put a ton of thought into this type of threat model, so I may be missing other attack vectors.

---

_Comment by @nicoburns on 2021-06-14 18:44_

@jcushman

> Thanks for working on this! I just wanted to share another usecase in case it helps.
> 
> I work on [case.law](https://case.law/), where our core users are academic researchers doing NLP on US caselaw, and I've experimented before with using the regex crate to provide on-demand regex search and extraction of research data sets across the corpus. (Like, let's get a list of every dollar figure discussed in a US court decision along with metadata about the case.) The corpus is about 6 million documents and a couple hundred gigs of text, and we have limited web server budget so we can only share features publicly that can be efficiently implemented. The feature set you described sounds perfect -- e.g. exhaustive unranked search and managing reindexing ourselves are no problem.

Have you considered using Tantivy (https://github.com/tantivy-search/tantivy) for this use case? The main developer behind Tantivy has also recently started a company "QuickWit" (https://quickwit.io/) which builds on top of Tantivy and can provide direct full-text search of data stored in object storage (S3, etc).

---

_Comment by @shashank-tyagi-sf on 2022-02-13 04:02_

@BurntSushi , My usecase is whenever sync local repo code with latest it should be able to create delta index for previous and merge it in the current local index
That way only once long-running indexing will be required but later whenever I sync it is delta based on changes.

---

_Comment by @balta2ar on 2022-02-13 10:41_

If it's okay to throw in more examples of prior work, I'd look at plocate (https://plocate.sesse.net/). Even though in its current form it's more of a drop-in replacement mlocate than a library/cli for indexing/search, it looks like it can be adapted for a wide scope of use cases, or at least it could be interesting to read about their approach. The title page says:

> plocate works by creating an inverted index over trigrams (combinations of three bytes) in the search strings, which allows it to rapidly narrow down the set of candidates to a very small list, instead of linearly scanning through every entry. It does nearly all I/O asynchronously using [io_uring](https://lwn.net/Articles/810414/) if available (Linux 5.1+), which reduces the impact of seek latency on systems without SSDs. Like mlocate and slocate, the returned file set is user-dependent, ie. a user will only see a file if find(1) would list it (all directories from the root have +rx permissions).

Not advertised, but it also uses zstd compression.

---

_Comment by @HALtheWise on 2022-02-25 23:29_

I'm somwhat late to the party, but would be really excited for this to land in ripgrep, and think you're taking exactly the right approach by deferring problems about detecting file changes and ranking results to whomever's invoking ripgrep.

In terms of interface, my intuitive expectation for the available command line flags would be slightly different than what you proposed, something more like the following:
- `-X` / `--index <paths...>`: specify one _or more_ index files to load. Any directories that ripgrep would normally need to search as part of executing the query and which are covered by one or more active indexes are instead searched using the index if ripgrep thinks that will be faster. Later indexes win. The defaut index locations you described also sound good.
- `--index-only`: raise an error if the paths ripgrep is trying to search are not fully covered by available indexes.
- `--index-ignore <paths...>`: Do _not_ trust any data in the indexes for these paths, instead re-scan them from disk as if they were never indexed in the first place.
- `--index-save <path>`: If the provided path refers to an existing index, update that index to include accurate data for all the `--index-ignore` paths. Otherwise, write a new index that contains that data (to the extent it is not already present in an index). The intent here is that using `--index-save` should gauruntee that a future ripgrep invocation including this index (at the end of the list) but _not_ `--index-ignore` will return the same results as this invocation does. This should default to the last input index if not explicitly provided.

Logically, what ripgrep is doing is equivalent to...
1. Read the indexes provided as "layers" of one logical index
2. Read the index-ignore paths and make a ephemeral extra "top" layer from them
3. If requested, save that top layer into an existing file or new file on disk
4. Do the search, using the index where available and useful

Allowing this explicit overlay structure makes a few neat things possible:
- If a file is mutated, all queries can remain instantly correct as long as I pass that file in `--index-ignore` until an updated index is available. 
- Behavior when the flags were different for index creation is well-defined (i.e. what if I made an index respecting gitignore, but am running a search ignoring it?)
- Users can decouple "search" ripgrep invocations from "index-updating" invocations, or combine them both into the same command.
- For truly giant setups, multiple users can share read-only access to a base index (i.e. an NFS share with a nightly index made from a huge git repo) and for files modified since then either pass them all to `--index-ignore` every time or keep a constantly-mutating local index.

---

_Comment by @gcflymoto on 2022-09-30 11:53_

https://github.com/phiresky/ripgrep-all the frontend to ripgrep for searching pdfs, compressed files, tarballs, zip files, word docs, excel, sqlite, etc caches the matches

"""
By default, rga caches the extracted text, if it is small enough, to a database in ~/.cache/rga on Linux, ~/Library/Caches/rga on macOS, or C:\Users\username\AppData\Local\rga on Windows. This way, repeated searches on the same set of files will be much faster. 
"""

---

_Comment by @mitchcapper on 2022-10-16 10:06_

All sounds great one comment:
> * By default, ripgrep should not re-index files if their last modified date
>   precedes their indexed date.

Consider the last modified date being stored and used for comparison instead.   Modified is fairly understood as an acceptable do we check if the file changed or not indicator but normally you want to compare against the known value rather than just a less than.   The reason being if something restores file modified times (archive tool, etc) the less than check passes but a comparison to last modified would not. 

There is precedent for the original way (say make), but I think more state based tools compare to the known modified stamp.   Per your comments you may already store this metadata so its just the choice to use it.

This would also be useful if someone say copied/restored an index and its modified time changes in the process.  That happening would cause it to assume it has all changes since it actually last ran.

---

_Comment by @dbohdan on 2023-11-02 19:34_

Another potentially relevant project: https://github.com/Genivia/ugrep-indexer. It is interesting for having a configurable index size/search speed tradeoff.

---
