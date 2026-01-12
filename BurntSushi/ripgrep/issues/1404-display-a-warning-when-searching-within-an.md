```yaml
number: 1404
title: Display a warning when searching within an ignored directory
type: issue
state: closed
author: Manishearth
labels:
  - enhancement
  - rollup
  - gitignore
assignees: []
created_at: 2019-10-15T01:35:51Z
updated_at: 2021-06-01T01:51:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1404
synced_at: 2026-01-12T16:13:23Z
```

# Display a warning when searching within an ignored directory

---

_@Manishearth_

#### What version of ripgrep are you using?
```
$ rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
Cargo

#### What operating system are you using ripgrep on?
Ubuntu

#### Describe your question, feature request, or bug.

Sometimes you end up inside a directory that's actually gitignored, and search for stuff. Of course, no file will get searched, and you will be confused.

If ripgrep ends up not searching any files and it happens to be in a situation where there _are_ files to search that it cannot because of the ignore file, it should probably display a warning saying "some files were skipped because of the ignore file, try `--no-ignore-vcs` to include". 

I don't know if ripgrep ever shows warnings, but this would be a nice place to do so.

---

_Comment by @BurntSushi on 2019-10-15 01:59_

This feature actually existed at one point. Unfortunately, it looks like it has evaporated, probably intentionally during a refactor. Getting this back in is a good idea.

---

_Comment by @coderkalyan on 2019-10-22 00:01_

Hi, I'd like to try to implement this for #hacktoberfest. I understand the feature request, but does someone have a simple example I could use to test this feature? In addition, what's the layout of this codebase - where would this feature be implemented?

---

_Comment by @genivia-inc on 2020-01-20 19:40_

Removing UI usage confusion is always a good thing. I am sure I'm preaching to the choir here, but I'd suggest either with by issuing warnings, or even better by [POLA](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) design.

Following POLA, files and directories specified as arguments to `rg` (i.e. to be explicitly searched) should never be ignored. It's utterly confusing when that happens. So I came up [with an alternative](https://github.com/Genivia/ugrep#ignore) that both respects `.gitignore` and files and directories specified as arguments to be searched. Furthermore, I though it would be a good idea to have option `--stats` extended to show the selection criteria applied to the search results and the locations of each `.gitignore` found when applied to the results.

For example, searching the working directory that has a `.gitignore`, while also searching the `Makefile` even though it is excluded by a `.gitignore` rule, now produces predictable results:
~~~
$ ugrep -rl --ignore-files --stats 'c++' . Makefile
...
Makefile
...
Searched 43 files with 8 threads in 1 directory: 42 matching
The following pathname selections and restrictions were applied:
--ignore-files='.gitignore'
  ./.gitignore exclusions were applied to .
~~~
Narrowing this down to searching just the `Makefile` when option `--ignore-files` is also used produces predictable results:
~~~
$ ugrep -l --ignore-files --stats 'c++' Makefile
Makefile
Searched 1 file: 1 matching
The following pathname selections and restrictions were applied:
--ignore-files='.gitignore'
~~~
Note that in this case it is very clear the `.gitignore` was not applied to the results. This makes sense as there is no directory to search recursively.

Perhaps this approach is useful. Others have found it to be less confusing, especially when adding option `--ignore-files` to a command alias such as `alias egrep='ugrep --ignore-files'` for example.

---

_Comment by @BurntSushi on 2020-01-20 21:20_

@genivia-inc Sorry, but this is the ripgrep issue tracker, not ugrep's. ripgrep will search files that are explicitly given as arguments but would otherwise be ignored via `.gitignore`. So that shouldn't be a problem and it isn't the same bug reported in this ticket.

---

_Comment by @genivia-inc on 2020-01-22 16:24_

@burntsushi Your sarcasm is inappropriate. I am merely pointing to an alternative approach to report `.gitignore` exclusions afterwards e.g. with `--stats`. We are all benefitting from open discussions, right?

I agree that warnings are fine too. Just sharing my observation as potentially useful, as I stated in my closing argument. Whatever works best to deal with POLA problems and improving the overall UX is up to users to decide, which should be applauded.

---

_Comment by @BurntSushi on 2020-01-22 17:16_

I wasn't being sarcastic. This isn't the place to unsolicitedly discuss tangentially related issues that are specific to some other tool. I'm going to mark further such comments from you as off topic.

---

_Label `enhancement` added by @BurntSushi on 2020-03-15 16:36_

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:28_

---

_Comment by @goto-engineering on 2020-12-05 02:14_

So I just tried this, and the behavior is slightly more subtle.

With this .gitignore:
```
ignored_dir
```

running `rg` from the root does not search files in `ignored_dir`. However, if you cd into `ignored_dir` and then search, `rg` will find things. This is likely because the directory was ignored, not the files in the directory.

If we add the glob to .gitignore:
```
ignored_dir/**
```
Then `cd` into `ignored_dir` and searching there does not find anything.

So in the literal sense, `rg` already behaves as the OP wishes it to. It seems to me that a warning for the glob case would be helpful, though, especially in cases where literally every candidate file is being ignored. (I've actually run into this in real-life usage when I had forgotten I'm in `node_modules` or something of that nature.)

@BurntSushi Does that seem like a reasonable idea? Would you say only in the everything-ignored case or in every case where there's an ignore on some files that were under consideration?

---

_Comment by @BurntSushi on 2020-12-05 03:44_

That's the idea and it's how the original warning message worked. If ripgrep is invoked with no arguments and nothing is searched, then ripgrep should emit a warning. The particulars of git ignore rules are just motivating use cases. The existence of the warning message shouldn't need to care about git ignore rules specifically.

---

_Comment by @goto-engineering on 2020-12-05 04:05_

Can you point me to a commit/place where this feature existed? Happy to try and bring it back.

---

_Comment by @BurntSushi on 2020-12-05 16:52_

@goto-engineering https://github.com/BurntSushi/ripgrep/blob/6799dcfc0ee63d741cd721c3311852a1b01449d8/src/main.rs#L409-L413

I think it got removed when I moved things over to libripgrep in 0.10.0. I can't remember why.

---

_Comment by @goto-engineering on 2020-12-05 18:26_

Got it, I'll take a look. Thanks!

---

_Label `rollup` added by @BurntSushi on 2021-05-30 14:17_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
