```yaml
number: 695
title: rg skips directories that have been checked into git, but whose names match a .gitignore pattern
type: issue
state: closed
author: mosesn
labels: []
assignees: []
created_at: 2017-11-28T19:22:33Z
updated_at: 2017-11-29T16:46:37Z
url: https://github.com/BurntSushi/ripgrep/issues/695
synced_at: 2026-01-12T16:13:22Z
```

# rg skips directories that have been checked into git, but whose names match a .gitignore pattern

---

_@mosesn_

I recently ran into an issue where rg doesn't look in all directories.

```
$ rg 'libthrift-0.5.0' 'src/java/com/twitter/search/common'
... (didn't find it)
```

```
$ rg 'libthrift-0.5.0' 'src/java/com/twitter/search/common/images'
... (found it)
```

I thought maybe the name "images" was special, but I didn't find anything promising in the rg repository.

Sorry for the sort of crummy bug report.  I looked at the permissions for the "images" directory that it doesn't want to search recursively, and it doesn't look weird.  Happy to include any other information, but I didn't really know what to say.

---

_Comment by @okdana on 2017-11-28 19:33_

Try it with `--debug`

---

_Comment by @BurntSushi on 2017-11-28 19:44_

@mosesn The first sentence of the README says:

> ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern while **respecting your gitignore rules.**

Your bug report doesn't mention anything about your gitignore rules, so I can't tell whether you've account for that or not.

---

_Comment by @mosesn on 2017-11-29 16:14_

good question–it's not ignored according to git

```
$ git check-ignore 'src/java/com/twitter/search/common/images'; echo $?
1
```

but when I output the debug information, it seems to think it's ignored

```
DEBUG:ignore::walk: ignoring src/java/com/twitter/search/common/images: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/mnakamura/workspace/sources/2/.gitignore"), original: "images", actual: "**/images", is_whitelist: false, is_only_dir: false })))
```

I looked in the .gitignore file, and it does include "images".  And indeed, when I turn on --no-index, it seems to be ignored.

```
$ git check-ignore --verbose --no-index 'src/java/com/twitter/search/common/images'; echo $?
.gitignore:128:images	src/java/com/twitter/search/common/images
0
```

With that said, now that it's in the index, it's no longer subject to .gitignore, according to git.  So I would expect it to be inspected by rg.  Should I close this ticket and file a feature request to also search files that are in .gitignore, but also in the git index?

---

_Comment by @BurntSushi on 2017-11-29 16:16_

> Should I close this ticket and file a feature request to also search files that are in .gitignore, but also in the git index?

It has been requested but it's not happening. The recommended work-around is to explicitly whitelist files that are in your `.gitignore` but you still want to search. You can do that in either a `.gitignore` or a `.ignore` file.

---

_Closed by @BurntSushi on 2017-11-29 16:16_

---

_Comment by @BurntSushi on 2017-11-29 16:17_

@mosesn See also https://github.com/BurntSushi/ripgrep/issues/645

---

_Comment by @mosesn on 2017-11-29 16:39_

If anyone else is curious, I did some follow-up research in how other tools handle this.

git grep looks in the index directly, so it gets this for free.
```
$ git grep 'libthrift-0.5.0' 'src/java/com/twitter/search/common'
(FINDS IT)
```

if you tell it to not use the index but to use gitignore, it doesn't find it.
```
$ git grep --exclude-standard --no-index 'libthrift-0.5.0' 'src/java/com/twitter/search/common'
(DOESN'T FIND IT)
```

If you search using ag, it does find it (???)

```
$ ag 'libthrift-0.5.0' src/java/com/twitter/search/common
(FINDS IT)
```

However, if you search using ag from the root of the repository, and then grep for the directory, it's not there.

```
$ ag 'libthrift-0.5.0' | grep 'src/java/com/twitter/search/common'; echo $?
1
```

So it seems like ag only looks for .gitignore files in the directories you specify for them to search.

I think rg handles this the right way–it's quite confusing that ag sees different results depending upon which directory you search from.

---

_Renamed from "don't skip directories" to "rg skips directories that have been checked into git, but whose names match a .gitignore pattern" by @mosesn on 2017-11-29 16:39_

---

_Comment by @BurntSushi on 2017-11-29 16:46_

@mosesn Yeah, `git grep` has one-up on us here. With respect to `ag`, its `gitignore` support is incredibly buggy. I say that because when I was first building ripgrep, I'd often use `ag` as my standard of correctness. But I soon realized that this was folly, because it gets a lot of the `gitignore` rules wrong. (The irony is that I say that in an issue that highlights what's most reasonably considered a `wontfix` bug in ripgrep on the very same functionality! :rofl:)

There are a couple other tools that claim to respect gitignores such as `pt` and `sift`, which each have their own independent implementation.

---
