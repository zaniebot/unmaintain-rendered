```yaml
number: 1518
title: ngram indexing implementation details
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2020-03-16T17:55:56Z
updated_at: 2025-12-20T17:18:00Z
url: https://github.com/BurntSushi/ripgrep/issues/1518
synced_at: 2026-01-12T16:13:23Z
```

# ngram indexing implementation details

---

_@BurntSushi_

This is split off from #1497 as a space to discuss specific implementation details of the indexing strategy. I'd like to keep this discussion separate so that folks won't feel put off from participating in a higher level discussion around the UX.

TODO: I'll fill in a sketch of my high level plan when I get a chance.

---

_Comment by @BurntSushi on 2020-03-16 17:57_

@mpdn 

>> ripgrep's index system will most closely resemble Russ Cox's codesearch. That
>> is, at its core, it is an inverted index with ngrams as its terms.

> You should consider using Bloom filters instead. Totally sidesteps the large file problem, has constant size which is a lot easier to manage, and uses less space. Or XOR filters, which are supposedly more compact: https://lemire.me/blog/2019/12/19/xor-filters-faster-and-smaller-than-bloom-filters/
>
> Edit: That is using a Bloom filter instead of the inverted index, i.e. still a bloom filter over the ngrams.

Could you say more about how the use of a bloom filter avoids the large file problem? Specifically, where the large file problem occurs because it winds up as being a frequent false positive that ripgrep then needs to exhaustively search. Since it's a large file, that search will take a long time, potentially negating the benefits of indexed search in the first place.

---

_Label `enhancement` added by @BurntSushi on 2020-03-16 17:57_

---

_Label `help wanted` added by @BurntSushi on 2020-03-16 17:57_

---

_Comment by @mpdn on 2020-03-16 18:12_

>Could you say more about how the use of a bloom filter avoids the large file problem? ...

*That* problem persists. It avoids the problem of large files making lots of ngrams and filling up your index. That is usually a problem with inverted indices, but I suppose that might not be as much of an issue for n-gram only inverted indices.

You could partition large files into chunks and built indices/bloom filters on that level instead. N-grams at the boundaries could be problematic. Might be more of a v2 feature than an MVP one.

---

_Comment by @BurntSushi on 2020-03-16 19:02_

Yeah, at least initially, I'm trying to avoid going down the road of having to modify the existing search architecture to support searching anything more granular than a single file. Once you go down that route, you run into all sorts of thorny issues, mostly around presenting the results back to the user. e.g., Supporting things like line numbers and the various context related flags.

But yes, large files will definitely fill up the postings lists disproportionately. That in and of itself isn't so much of a problem. The main problem in this space is very common ngrams that themselves have very large postings lists. Traversing those lists ends up taking quite a bit of time. I'm hoping to mitigate that somewhat:

* The query planner should be able to detect ahead of time, to some degree, whether it will produce a large number of candidate documents to search before traversing the posting lists. In that case, we just don't use the index at all. This sounds like a bummer, but this "don't use the index" path will happen quite a bit in other ways too. e.g., for the regex `\w`.
* It should be possible to use a chunking mechanism in the posting lists that permits skipping large portions of it when a very common ngram is intersected with a not-so-common ngram. Lucene does similar optimizations, but since it's an IR engine, it has a bit more flexibility. e.g., If you need to search a union of ngrams that are all super common, then Lucene can make a precision/recall trade off and turn that into an intersection query. ripgrep will have no such luxuries!

Popping up a level, it seems to me like a bloom filtered based index requires a completely different architecture than an inverted index. It seems to me like it almost requires building up a bunch of chunks of the corpus, where each chunk has a bloom filter (or similar) index. If your query passes the bloom filter, then you must search the entire chunk. And then it seems like you need to repeat this for every chunk. (At least, I think this is how [qgrep works](https://zeux.io/2019/04/20/qgrep-internals/). The section "Filtering searched chunks" seems to hint that this is the case.) That would hamper the best case because you _always_ need to search the index for every chunk.

Now, the n-gram indexing story I have in my head _also_ uses a chunking strategy. The difference is that the chunks can be merged with low memory usage in linear time. This is essential to the incremental indexing story. The incremental indexing story for the bloom filter approach looks a bit less clear to me, although I haven't thought about it too carefully.

Anyway, I will continue to noodle on this. Thanks for the thoughts. Keep them coming if you have them. :-)

---

_Comment by @BurntSushi on 2020-03-16 19:22_

@omac777 I don't have any plans to use a roaring bitmap, so I don't think that work applies.

Also, none of your gitlab links work for me. It seems I need an account to see the repos?

---

_Comment by @mpdn on 2020-03-16 19:46_

>It seems to me like it almost requires building up a bunch of chunks of the corpus, where each chunk has a bloom filter (or similar) index.

I don't really see why that is the case. In the simplest case, you just have a bloom filter for each file. When querying you build a bloom filter of all your query n-grams and use bitwise operations to check that whether all of your query is in the bloom filter of the file. You then just linearly search all the filters and search the files if there is a match.

Linear searching the filters might sound slow, but since it's just bitwise ops, easily SIMD-izable, and scans memory linearly, it's usually quite fast. Also consider that the cost in the number of n-grams in the query is constant. 

There are hierarchical filtering schemes, but it seems unclear if they are necessary here.

---

_Comment by @BurntSushi on 2020-03-16 19:57_

> I don't really see why that is the case. In the simplest case, you just have a bloom filter for each file. When querying you build a bloom filter of all your query n-grams and use bitwise operations to check that whether all of your query is in the bloom filter of the file. You then just linearly search all the filters and search the files if there is a match.

Yes. That sounds exactly like building a bunch of chunks of the corpus, where each chunk is a single file. The best case performance for an inverted index sounds much better. A hierarchical scheme for bloom filters appears required to achieve that. Otherwise, it doesn't seem like it scales.

> Linear searching the filters might sound slow, but since it's just bitwise ops, easily SIMD-izable, and scans memory linearly, it's usually quite fast. 

Yes, I understand that. But you're still needing to do this for every chunk.

It looks like Lucene has a bloom filter postings format, but appears to be specific to fields that have low frequency values (like primary keys). Do you know of any other established IR systems that use a bloom filter instead of an inverted index?

---

_Comment by @mpdn on 2020-03-16 21:35_

>That sounds exactly like building a bunch of chunks of the corpus, where each chunk is a single file.

Okay, but I don't see the difference vs. an inverted index. An inverted index could also refer to a "chunk" of documents instead of referring to a single document.

The best case for the inverted index is a single n-gram that matches a single document.

The best case for the bloom filter is a combination of n-grams that only matches a single document. That seems like a more likely case to me, due to the nature of n-grams. Inverted indices might not help here, because the n-grams might be individually common but rare in combination.

Also consider that inverted indices have a bigger construction and maintenance cost. But I do guess it helps that this is unlikely to be a write-heavy situation, and the inverted indices don't have to store the position in the document as well.

 >Do you know of any other established IR systems that use a bloom filter instead of an inverted index?

Heh, so this is when I admit my bias. :)

I work at @humio where we use a bloom-filter based system search to do regex search (and more) of log data. We have just 2 levels of bloom filters for petabyte sized clusters (though we use more than just bloom filters for filtering).

---

_Comment by @luizirber on 2020-03-16 22:20_

As an aside, this problem is very similar to k-mer indexing in bioinformatics, and there is a [recent literature review][0] which brings up many of the points of this inverted index/hierarchical BF discussion going on here.

[0]: https://www.biorxiv.org/content/10.1101/866756v1

---

_Comment by @BurntSushi on 2020-03-16 22:30_

> Okay, but I don't see the difference vs. an inverted index. An inverted index could also refer to a "chunk" of documents instead of referring to a single document.

I think we're misunderstanding each other. An "optimal" inverted index involves these operations:

* Look up the ngrams in the query in the inverted index. This gets you a set of posting lists.
* Apply the boolean ngram query derived from the regex to the posting lists.
* Return every candidate in every posting list that satisfies the boolean ngram query.

In the "best" case---pretty much any time you have an uncommon ngram---you do a very small number of operations. But the bloom filter chunking mechanism you're proposing requires visiting every chunk. There is no "best" case that lets you visit a very small subset of the corpus.

> The best case for the inverted index is a single n-gram that matches a single document.
>
> The best case for the bloom filter is a combination of n-grams that only matches a single document. That seems like a more likely case to me, due to the nature of n-grams. Inverted indices might not help here, because the n-grams might be individually common but rare in combination.

Right. The problem there is traversing large postings lists. That can largely be mitigated by chunking the posting lists into blocks. So long as there is a rare ngram somewhere, the intersection can make traversing the longer posting lists very fast. Lucene does this. Each block has its maximum document ID. If you know you can't have any document IDs less than `N`, then you can skip all blocks whose maximum is less than `N`. I suspect you can do even better here by just not looking at long posting lists in an intersection with an uncommon ngram.

My speculation is that the best case with an inverted index is much better than the best case with a bloom filter. And even in the bloom filter's best case, the inverted index is going to do well. 

> Heh, so this is when I admit my bias. :)
>
> I work at @humio where we use a bloom-filter based system search to do regex search (and more) of log data. We have just 2 levels of bloom filters for petabyte sized clusters (though we use more than just bloom filters for filtering).

Hah okay. Fair. I guess I should revise my question to: do you know of any other established IR systems _that I can study in detail_ that use a bloom filter instead of an inverted index?

It sounds like this is kind of boiling down into a speculative thing. I'm biased because I'm most familiar with inverted index style IR systems. I know them well. I know how to do incremental updating with them and I know where they do poorly. I'm much less familiar with how to build an IR system using bloom filters, which is just a totally different architecture with fundamentally different trade offs. As is expected, these trade offs are deeply rooted in what kinds of queries are being performed, and my best guess at this point is that an inverted index gives better coverage there.

---

_Comment by @BurntSushi on 2020-03-16 23:13_

@luizirber Interesting paper. I [worked in that field](https://academic.oup.com/bioinformatics/article/29/13/i283/189542) in a former life.

I found it a bit difficult to pick out any specific details that help this discussion though. Parts of it were quite dense. I also wonder what the impact of alphabet size has, although that's partially mitigated by just picking a larger `k`.

---

_Comment by @mpdn on 2020-03-17 17:11_

>But the bloom filter chunking mechanism you're proposing requires visiting every chunk.

Well, yes, unless you have a hierarchical filter. But a hierarchical filter using eg. the file hierarchy wouldn't be difficult to implement.

>Hah okay. Fair. I guess I should revise my question to: do you know of any other established IR systems that I can study in detail that use a bloom filter instead of an inverted index?

Bing replaced their inverted indices with bloom filters with supposedly 10x query capacity: https://danluu.com/bitfunnel-sigir.pdf. Granted, what makes sense in a large scale context may not make as much sense on the scale of ripgrep.

---

_Comment by @BurntSushi on 2020-03-17 17:20_

Thanks, I'll take a look at that paper.

---

_Comment by @bleuge on 2020-03-30 09:28_

I am not a pro, but I was thinking you could store a hash of each file in its metadata, this makes possible to detect duplicate files, and so a smaller index, and faster search (if you hit a dup fle). I don't know the possibility of this happening in normal/large codebases. Maybe is not common, or not enough to take in account. As this needs further logic to maintain links between identical files.
Also, I think that with some testing codebases, you could test many things as the possibility of using a bloom filter instead of trigrams, etc... What I mean is that a lot of these questions could be checked with some testing, as you already said, we have here a domain focused problem, not needed to implement a full IR system.
I worked with n-grams sometimes, and they are fast to index. Maybe large postings for very common trigrams could be a problem, but I guess this could be implemented in some fast ways, I've read some things about integer-compression lists, and results are impressive.

Sorry the rant, just an amateur that always liked to play with IR.

---

_Comment by @BurntSushi on 2020-03-30 11:58_

There are two strategies for dealing with long posting lists. One is to indeed use integer compression via some method of delta encoding. The other is to encode it as a skip list. This comes in handy when intersecting a common ngram with an uncommon one. You can skip through the vast majority of things in the common posting list without having to decode every item in the list.

>  you could test many things as the possibility of using a bloom filter instead of trigrams, etc...

There is a lot of work hiding in that "could."

---

_Comment by @CLIDragon on 2021-07-23 07:06_

Hey @BurntSushi,
 I'm currently working on implementing ngram indexing in rust with the aim of eventually integrating with ripgrep. I have a (imo) fairly effective method of extracting ngrams from an ascii file, however, as I am fairly new to rust and IR, I have a couple of questions about extracting ngrams. 

- **How should I handle linebreaks?** Currently, I just ignore them, but that doesn't feel like the best method. I tried breaking strings into lines, but that leaves the question of how to deal with lines with characters less than `n`.
- **How should I handle Unicode?** Should I extract scalar values? Break characters into graphemes? Or just pretend that characters outside of ascii don't exist?

---

_Comment by @BurntSushi on 2021-07-23 12:47_

With respect to line breaks, I suspect we need them for multi-line search. So I would include them.

With respect to Unicode, for ripgrep in particular, we want bytes. This doesn't mean we ignore "characters outside of ASCII." It just means that we operate at the byte level. If we choose anything other than bytes, then we lose the ability for search to work on data that isn't valid UTF-8.

I haven't thought carefully about these things yet, so I could be wrong. They are just my instincts.

>  aim of eventually integrating with ripgrep.

If you are doing a lot of work where the main goal is to get that work merged into ripgrep, please first discuss your plan with me by opening a new issue.

---

_Comment by @kenorb on 2025-12-20 17:08_

Some projects implementing Bloom filters which could be helpful:

- Go: <https://github.com/DCSO/bloom>
- C: <https://github.com/ryancdotorg/brainflayer/blob/master/bloom.c>
- Rust: <https://github.com/kenorb/bloom> (and few other algos)

Although using bloom filters approach could be slow in general for large entries as per the following article:
<https://blog.cloudflare.com/when-bloom-filters-dont-bloom/> (see also: [mmuniq-hash.c](https://github.com/cloudflare/cloudflare-blog/blob/master/2020-02-mmuniq/mmuniq-hash.c)). Also the design flaw is that when the pool is overfilled, it can produce a lot of false positives.

Better alternatives which can be considered:
- Binary search (e.g. Eytzinger), see: <https://algorithmica.org/en/eytzinger> & <https://en.wikipedia.org/wiki/Multiplicative_binary_search> (around 28 reads for 250M entries)
- Minimal Perfect Hash (MPH), see: <https://randorithms.com/2019/09/12/MPH-functions.html> & <https://en.wikipedia.org/wiki/Perfect_hash_function> (only 1 read for 250M entries, 0 false positives); Example implementation of Bloom-filter based minimal perfect hash function library in C++: <https://github.com/rizkg/BBHash>

---
