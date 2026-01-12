```yaml
number: 357
title: filter out duplicate results
type: issue
state: closed
author: timotheecour
labels: []
assignees: []
created_at: 2017-02-11T23:17:26Z
updated_at: 2019-07-10T15:55:41Z
url: https://github.com/BurntSushi/ripgrep/issues/357
synced_at: 2026-01-12T16:13:21Z
```

# filter out duplicate results

---

_@timotheecour_

`rg pattern dir1/dir2 dir1`
return duplicate results
```
dir1/dir2/file_containin_pattern
dir1/dir2/file_containin_pattern
```

could we filter out duplicate results? (at least as an option)

the use case for `dir1/dir2 dir1` is to list relevant files in `dir1/dir2` ahead of the other ones in `dir1`


---

_Comment by @BurntSushi on 2017-02-11 23:25_

Unlikely. The reason is that determining which results are duplicates is actually quite hard despite the conceptual simplicity. (The file paths alone don't tell the whole story.)

One possible work-around for you is to execute two searches:

```
$ rg pattern dir1/dir2
$ rg pattern -g '!dir1/dir2' dir1
```

The second command won't search `dir1/dir2`, which achieves what you want I think.

---

_Comment by @BurntSushi on 2017-02-11 23:26_

If you're curious about what goes into determining whether two files are the same or not, you can check out the [`same-file`](https://github.com/BurntSushi/same-file) crate, which ripgrep does use in some limited circumstances (unrelated to this feature request though).

---

_Comment by @timotheecour on 2017-02-13 02:02_

* @BurntSushi thanks for the link. After looking at implementation, I wonder why posix function `realpath` can't be used (on linux/osx) or `GetFullPathName` (on windows)?

eg: using d syntax which i'm more familiar with:
```d
auto is_same_file(string a, string b){return realPath(a)==realPath(b);}
// realPath is a simple D wrapper around posix realpath from C
```

* still though, a partially working filtering would be better than nothing at all, maybe with proper caveats in docs or naming it `--experimental-filter-dups` (and maybe only on `posix` for now)


---

_Comment by @BurntSushi on 2017-02-13 03:13_

Computing the real path of every file is extremely expensive. This option is also not possible to implement in constant memory. It is a non starter.

I'm not particularly interested in adding experimental half baked features.

---

_Comment by @BurntSushi on 2017-02-13 03:14_

I think the answer here is to work around it. If you tell the tool to search the same directory twice, then it should show the same results twice.

---

_Closed by @BurntSushi on 2017-02-13 03:14_

---

_Comment by @timotheecour on 2017-02-17 18:33_

@BurntSushi 

* honest question: is calling `realpath` on a file really more expensive than opening the file and doing the regex search? It would obviously only be run on the files that pass the various file filters ripgrep already uses

> This option is also not possible to implement in constant memory

* how is that different from `--sort-files` in that respect? `--sort-files`has to use O(N) memory where N=number of output lines produced

EDIT: actually since we're searching a file-system tree, I guess we can do better than O(N) memory by doing breadth first listing and sorting each directory so it'd be O(D) where D=max number of entries per directory; ignore last point. 
Indeed, I agree it dedup across symlinks has to be O(N) because each output file is potentially a symlink to a previous output file. Still though, I would not expect that to be a real problem for most cases: number of output files produced by an `rg` search should typically fit in memory.




---

_Comment by @mqudsi on 2019-07-09 19:52_

@BurntSushi I agree with your reasoning 100% on why rg shouldn't try to deduplicate two textually different paths, however I am wondering if there can be an option to prevent the following behavior:

```
> cd (mktemp -d)
> echo foo > ./foo
> rg . -l ./ ./foo
./foo
./foo
```

without needing to pipe its output to another command to deal with (to avoid buffering and to be able to use `--color always` without needing to decode then re-add the colors during the filter stage).

---

_Comment by @BurntSushi on 2019-07-09 19:55_

I don't think it's ripgrep's responsibility to deduplicate the arguments you give it. If you don't want it searching the same directory twice, then don't give the same directory twice. i.e., Do something to deduplicate the arguments before handing them to ripgrep.

---

_Comment by @mqudsi on 2019-07-09 19:56_

That's certainly fair enough; I was just checking if you would be open to having this be an option within ripgrep itself.

---

_Comment by @mqudsi on 2019-07-10 15:55_

(I just saw your reply again by chance and it occurred to me to point out that, strictly speaking, the inputs to `rg` *are* deduplicated, but the problem is that they are nested.)

---
