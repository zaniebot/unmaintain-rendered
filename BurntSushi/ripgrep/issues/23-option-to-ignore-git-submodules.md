```yaml
number: 23
title: Option to ignore git submodules
type: issue
state: closed
author: Mmdixon
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2016-09-23T18:10:24Z
updated_at: 2023-12-27T00:58:44Z
url: https://github.com/BurntSushi/ripgrep/issues/23
synced_at: 2026-01-12T16:13:21Z
```

# Option to ignore git submodules

---

_@Mmdixon_

`git grep` by default does not search for files within a git submodule, even though they appear like normal directories.
`rg` on the other hand will by default recurse into these directories.

Since `.gitignore` is supported, it would be convenient to support how git searches when there are submodules. (Either by default and turn off with one of the `-u` flags, or provide some other flag besides explicitly black listing the submodule folders.)


---

_Comment by @BurntSushi on 2016-09-24 02:17_

How can `rg` detect whether a directory is a submodule or not?

(Running a `git` command is not something I want to do.)


---

_Comment by @BurntSushi on 2016-09-25 01:52_

I'd like to close this. If there's a simple way to detect submodules that doesn't sacrifice performance, I might be willing to support that. One thing I'm worried about is that I personally don't have much experience with submodules, so I don't know whether it's the Right Thing to ignore them or not. My suspicion is yes, it is right, because I tend to see them used as a way to vendor dependencies.


---

_Comment by @mengelbrecht on 2016-09-25 02:43_

If it is going to be implemented please make it optional. Internal libraries shared between multiple projects can also be included as submodules. In that case I want to search them too e.g. when performing a global refactoring.


---

_Label `question` added by @BurntSushi on 2016-09-25 19:07_

---

_Comment by @Mmdixon on 2016-09-26 13:34_

@BurntSushi there will be a `.gitmodules` file with a list of the following format:

```
[submodule "myName"]
    path = src/myName
    url = someUrl
# repeat
```

Here, the `path` has the folder `src/myName` the start of the submodule (which is another git repository).


---

_Comment by @Kha on 2016-09-26 13:43_

More importantly, there should be a `.git` _file_ in the submodule workdir. So a general option to ignore nested repositories should cover submodules.


---

_Comment by @Mmdixon on 2016-09-26 14:06_

ah that is true too,  a `.git` file instead of folder in the submodule directory.


---

_Comment by @BurntSushi on 2016-11-06 19:08_

I think the `.git` file thing is a newer feature of `git` though?

I think I'd like to ask that you use `.ignore` files to explicitly skip searching submodule directories.

With that said, if someone submitted a PR to the `ignore` crate that added support for ignoring submodules (which was disabled by default), then I'd be OK adding a new flag to ripgrep to enable that.


---

_Label `enhancement` added by @BurntSushi on 2016-11-06 19:08_

---

_Label `help wanted` added by @BurntSushi on 2016-11-06 19:08_

---

_Label `question` removed by @BurntSushi on 2016-11-06 19:08_

---

_Comment by @BurntSushi on 2016-11-19 14:55_

I'm going to close this as part of a process of culling the issue tracker to things that are likely to be implemented. I think my previous comment still stands, but I would still rather not add a new flag and instead just insist that this be part of using ripgrep's `.ignore` file functionality.


---

_Closed by @BurntSushi on 2016-11-19 14:55_

---

_Comment by @BatmanAoD on 2023-12-27 00:58_

Would you still be open to a PR on the `ignore` crate to support ignoring nested git directories?

> I think the `.git` file thing is a newer feature of `git` though?

If I understand correctly, the idea is just to check for the existence of the standard `.git/` directory containing `git` metadata, so not a new feature of `git` at all. I.e., the check would be something like:

* when searching in a git repository rooted at `/repo/root`,
* if `/repo/root/foo/bar/.git/` exists,
* ignore all of `/repo/root/foo/bar`.

---
