```yaml
number: 2043
title: Feature - Add a flag to return the intersection of results if multiple patterns are given 
type: issue
state: closed
author: ctittel
labels:
  - duplicate
assignees: []
created_at: 2021-10-26T18:38:14Z
updated_at: 2025-05-26T13:22:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2043
synced_at: 2026-01-12T16:13:24Z
```

# Feature - Add a flag to return the intersection of results if multiple patterns are given 

---

_@ctittel_

The flag `-e` / `--regexp` allows one to search multiple patterns.

Currently, a search `rg -e pattern1 -e pattern2` returns the union of the results that would be returned by the two searches `rg -e pattern1` and `rg -e pattern2`. 
The files returned by `rg -e pattern1 -e pattern2` are the files that contain at least one of [`pattern1`, `pattern2`].

My request is to add a flag (e.g. `--conjunction`) where files have to contain all the given patterns to be a match.
So the command `rg -e pattern1 -e pattern2 --conjunction` would return the files that contain both `pattern1'` and `pattern2`

Another thing that has to be taken into account here is whether multiline search is performed or not. 
If multiline search is performed, the patterns could be allowed to be on different lines, if multiline search is not performed the patterns could be required to all be on the same line.

I currently use this command to iteratively filter the files to get the files where all given patterns are present:

```bash
for QUERY in $TERMS; do
   FILES=$(echo "$FILES" | xargs -d $'\n' rg -L -H --multiline --multiline-dotall --files-with-matches $QUERY)
done
```
 
Simply constructing a regex like `pattern1 .* pattern2` is not an option here, because the patterns should be allowed to appear in any order.
For two patterns you would have to search for `pattern1 .* pattern2` and `pattern2 .* pattern1`.
For three patterns, there would of course be 6 different orders in which the patterns could appear, etc.

So the flag would not strictly be required in the multiline-search case (as I have demonstrated), but it would be much more elegant.
In the single-line search case however (where you want all patterns in the same line, in any order) such a flag would be required, because the only other way would be to construct a very long and complicated regular expression.

---

_Comment by @BurntSushi on 2021-10-26 19:42_

Dupe of https://github.com/BurntSushi/ripgrep/issues/875

---

_Closed by @BurntSushi on 2021-10-26 19:42_

---

_Label `duplicate` added by @BurntSushi on 2021-10-26 19:42_

---

_Comment by @davideuler on 2025-05-26 13:21_

In case you need the multiple keyword match to implement an AND logic. I've implemented a version to support the feature.

https://github.com/davideuler/ripgrep

Sometimes you need to find lines that contain several specific words, but the order or exact positioning of these words doesn't matter. The `--and` flag is designed for this purpose. It allows you to specify a set of space-separated keywords, and only lines containing *all* of those keywords will be matched.

**Syntax:**

```
rg --and "keyword1 keyword2 keyword3" [path...]
```
or with a primary pattern:
```
rg --and keyword1 --and keyword2 --and keyword3 [path...]
```


---
