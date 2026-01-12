```yaml
number: 1112
title: Consider supporting tar file
type: issue
state: open
author: upsuper
labels:
  - enhancement
  - question
  - icebox
assignees: []
created_at: 2018-11-17T20:48:42Z
updated_at: 2025-11-09T19:09:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1112
synced_at: 2026-01-12T16:13:22Z
```

# Consider supporting tar file

---

_@upsuper_

Opening this issue as suggested by @BurntSushi in #1111.

I think it would be helpful if ripgrep can support scanning archive files. Scanning tar files can briefly done via `-a` option, but there are downsides:
* you need to extract all files from the archive and scan again to identify the files you want
* the line number is unhelpfully large
* it may be wasting time on scanning files that we don't actually need

It is unclear how scanning archive files should work. It should definitely be opt-in just like `--search-zip`. We can probably add `--search-archive` or `--search-tar` for this purpose. I guess ideally archive files should probably be handled like directory rather than single file in that mode.

That should lead to some thing like
> **path/to/archive.tar/file1**
> something **matched**
>
> **path/to/archive.tar/file2**
> something else **matched**

@BurntSushi what do you think?

---

_Comment by @BurntSushi on 2018-11-17 20:59_

Thanks for filing the ticket! I think this needs to be fleshed out quite a bit more though.

> I guess ideally archive files should probably be handled like directory rather than single file in that mode.

ripgrep has fairly sophisticated filtering support. How does that interact with treating tar files as if they were a directory? e.g., If there is an applicable `*.txt` exclusion filter in play and a tar archive contains `foo.txt`, should that file also be ignored? If so, which filters are applied to it and how does that work? Personally, I suspect this will be a significant complication.

---

_Comment by @upsuper on 2018-11-17 21:16_

For the case you mentioned, it should probably ignore the `foo.txt`, but still scan other files in the archives, just like what we do for directory.

For how does that work... so it seems currently this is done via `ignore` crate, but integrating this support into that crate is probably not an option, because we would need decompression support there as well. Maybe we can change how `ignore` work, that we walk through directories in ripgrep, and feed file path into `ignore` for whether a directory should be further traversed, and whether a file should be scanned. This effectively would split `ignore` into two crates, one for walking through file system, another for checking file inclusion. It would be a non-trivial refactor. Alternatively we can add some new API to `ignore` for querying this kind of information, probably.

---

_Label `enhancement` added by @BurntSushi on 2018-11-17 21:35_

---

_Label `question` added by @BurntSushi on 2018-11-17 21:35_

---

_Label `icebox` added by @BurntSushi on 2018-11-17 21:35_

---

_Comment by @BurntSushi on 2018-11-17 21:38_

Right. This is probably blocked on refactoring and cleaning up the `ignore` crate in general. I don't expect that to happen any time soon, and I haven't had the time to write down enough of my thoughts on that for others to see.

I'll leave this open for now, but I don't think this is going to happen any time soon. Moreover, I am still not quite convinced that this feature belongs in ripgrep. It's definitely something that folks have requested before though.

---

_Comment by @mateon1 on 2019-07-18 08:54_

I suggest adding a generic `--search-archive` flag, so this can support different types of archives (like zip files) in the future.

This would be really helpful for me, as I often need to grep through a massive corpus of text, hundreds of gigabytes in size. If I use ripgrep on the unpacked corpus, I get very useful information like filenames of the files that matched, and real line numbers. Unfortunately, because of spinning disk random access times this search usually takes over 6 hours to process the whole corpus.
If I instead search the compressed version of the corpus by piping the output of `unzip -p '*'.zip`, the search only takes about an hour, but I lose information like which file a specific matching line came from, which sometimes forces me to repeat the search (or do a more specific search) on the uncompressed corpus later.

---

_Comment by @genivia-inc on 2019-11-25 20:22_

My 2c. 

I was looking for the same thing as I have several archived projects (.tar.gz) from years ago that I want to grep through fast without extracting files first. Eventually I decided to implement my own grep tool for this and other reasons. Recently I added tar file searches that I had on my TODO list, see [ugrep](https://github.com/Genivia/ugrep) for the latest. Be warned that ugrep is pretty new (since mid 2019). It is stable but there may still be some rough edges, though I've tested it regularly on large test cases and tar files without issues. Supports (gzip compressed) tar v7, ustar, gnu tar, and pax extended header blocks for long file names. So right now it covers the common use cases.

Anyway, if this is not helpful to ripgrep then I hope it may be helpful to you.

You can shoot me down for not helping this community to implement tar file search in a ripgrep fork, but I'm just old fashioned and mostly code C/C++ and I'm too lazy too learn Rust.

---

_Comment by @eadmaster on 2021-05-05 08:20_

I've just found [this wrapper](https://github.com/phiresky/ripgrep-all) that adds some compressed archives support.

Older alternatives i know are `zipgrep` and [`zzfind`](http://stahlworks.com/dev/index.php?tool=zzfind).

Btw, it is not clear to me how the current `--search-zip` flag is supposed to work, can you provide some examples?



---

_Comment by @BurntSushi on 2021-05-05 11:35_

@eadmaster It's pretty simple. You give ripgrep compressed files and it searches them:

```
$ echo 'foo bar quux' > /tmp/haystack
$ rg bar /tmp/haystack
1:foo bar quux
$ gzip /tmp/haystack
$ rg bar /tmp/haystack.gz
$ rg -z bar /tmp/haystack.gz
1:foo bar quux
```

It sounds like you're trying to give ripgrep compressed _archive_ files. ripgrep doesn't support iterating over archives, even when they're uncompressed. It's orthogonal to `--search-zip`.

---

_Comment by @eadmaster on 2021-05-05 13:51_

I see, then the name `search-zip` is a bit ambiguous for me, why not changing to `search-gz` or `search-gzip`?

---

_Comment by @BurntSushi on 2021-05-05 13:58_

Because the flag name is used in other tools, change causes churn and gzip is not the only supported compression format.

---

_Comment by @knutwannheden on 2022-03-17 16:08_

Support for archives would be really nice!

Apart from being able to search the occasional downloaded `.zip` and `.tar.gz` archive, I also have another use case: I was surprised that ripgrep appeared to be so slow on a VPS hosted by a public cloud provider. It turns out that they use [Ceph](https://en.wikipedia.org/wiki/Ceph_(software)) and (at least the way it is configured) I get good throughput put high latencies when reading files (ephemeral disks are not available). In my case I have over a million files I want to search and it just takes forever. So I created a compressed `.tar.gz` archive, which I can now search using the `-a` and `-z` options, which is a lot faster. The downside here being that ripgrep can't tell me which file within the archive contains the match. This is admittedly a strange use case, but IMHO archive support would still be very nice to have.

---

_Comment by @jumarko on 2022-05-25 03:12_

I would love to see this implemented.
I also agree with @eadmaster that `search-zip` option name is confusing (https://github.com/BurntSushi/ripgrep/issues/1112#issuecomment-832704390)

---

_Comment by @BurntSushi on 2022-05-25 12:25_

The flag name isn't changing.

More comments saying "I want this" _aren't_ helpful. I have questions about how to implement this. What would be helpful is if folks could dive into answering those questions. See my previous comments.

---

_Comment by @mateon1 on 2022-05-25 22:58_

My view of how the implementation should roughly look:
 - ripgrep, with an appropriate cli flag should be able to recurse into and open archive files as a pseudo-directory, and search each file within the archive as usual (semantically, almost exactly as if the archive was a subdirectory you entered). Implementation-level, I believe you could define an `Archive` trait which you could implement for tar files, zip files, and whatever else people want to implement (rar, 7z, zpaq, whatever you can find a crate for - but most behind a feature flag most likely).
 - The Archive trait would provide methods for traversing the directories and providing whatever file reading methods ripgrep needs. Some hint methods could be useful to indicate that e.g. tar files cannot be reasonably traversed in parallel, only sequentially, while most other archive formats let you do random access to select a file.
 - ripgrep can report the matching file as `path/to/archive.zip//archived/path.txt` (exact format open to bikeshedding, maybe `::` as separator? I think I've seen antivirus software report files in archives this way)
 - ripgrep SHOULD be able to recurse into nested archives, possibly limited by some flag.

Note: I have very little understanding of how ripgrep is internally architected, maybe the `Archive` trait idea won't work, but that's the first thing that comes to mind for how to implement this.

---

_Comment by @ujay68 on 2023-11-30 11:07_

I would also like to vote for such a feature. (Recursively searching through a directory structure that contains archive formats is really painful ATM.) Some additional ideas:

- Could also support Zip-based file formats. Lots of these with different endings: Java (.jar, .war, .ear), Microsoft (.docx, .xlsx & Co.), LibreOffice (.odt & Co.) …
- If a specific command-line option is set, I would suggest to treat such archive files as if they were extracted at their location, ie, a file `./d1/d2/a.zip` would be treated, for globbing and filtering purposes, like a directory `./d1/d2/a.zip/` containing the contents of `a.zip`. (Java URIs use a the special character `!` for that, ie, paths like `d1/d2/a.zip!a.txt`.)
- Second mateon1's idea of allowing recursion into nested archives (which one has, eg, with .war and .ear).

---

_Comment by @allefeld on 2025-11-09 19:09_

I agree that this would be useful, but it should be opt-in, through an option like `--search-archive`.

Leaving handling archives to a preprocessor (which e.g. `rga` implements) will often give useless results, because the preprocessor can return only one file / stream, which might be extremely long and mix text with binary parts.

I think the extended pathname idea (path to zip + path to file in zip) is good. However, "treat such archive files as if they were extracted at their location" doesn't always work, because at least `.zip` files can store absolute paths for files. That would be best accommodated by a Java-style string, where both `d1/d2/a.zip!a.txt` (path relative to zip, similar to "as if extracted into  a directory `a.zip`") and `d1/d2/a.zip!/a.txt` (absolute path within archive) are possible.

That means that archive searching would be implemented at the level of file system traversal. For an archive file, the traversal produces not one pathname but as many extended pathnames as there are files in the archive. When the pattern search encounters such an extended pathname, it extracts contained files one-by-one (to deal with very large archives) to temporary files. For the preprocessing to work, the file extension (or generally, the part matched by one of the `--pre-glob`s) would have to be preserved on the extracted file. Matches found in the temporary file then need to be translated back into extended pathname format.

Btw., I was also misled by the option name `--search-zip`, which I interpreted as referencing `zip`, not `gzip`. Better might be `--search-compressed`.

---
