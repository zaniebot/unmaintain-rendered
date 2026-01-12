```yaml
number: 778
title: Negative .gitignore pattern seem to be ignored
type: issue
state: closed
author: rnestler
labels:
  - wontfix
assignees: []
created_at: 2018-02-07T12:49:35Z
updated_at: 2024-03-08T13:36:27Z
url: https://github.com/BurntSushi/ripgrep/issues/778
synced_at: 2026-01-12T16:13:22Z
```

# Negative .gitignore pattern seem to be ignored

---

_@rnestler_

#### What version of ripgrep are you using?
```
ripgrep 0.7.1
-AVX -SIMD
```

#### What operating system are you using ripgrep on?

Linux 4.13.0-31-generic #34~16.04.1-Ubuntu SMP Fri Jan 19 17:11:01 UTC 2018 x86_64 x86_64 GNU/Linux

#### Describe your question, feature request, or bug.

riggrep seems to interpret negative patterns (starting with `!`) in .gitignore differently than git

#### If this is a bug, what are the steps to reproduce the behavior?

Given the following git repository:
```
% tree
.
├── build
│   └── foo
├── build-64
│   └── foo
└── buildtools
    └── test.txt
```

with this `.gitignore` file:
```
% cat .gitignore 
build*
!buildutils
```

#### If this is a bug, what is the actual behavior?

rg can't match stuff in buildtools. git grep is perfectly fine with it:
```
% git grep foo
buildtools/test.txt:Test to find: foobar
% rg foo
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```
Debug output: https://gist.github.com/rnestler/f58d58b8350009e228cb1a0210f1b4d8

#### If this is a bug, what is the expected behavior?

Return the same results as `git grep`


---

_Comment by @BurntSushi on 2018-02-07 14:15_

@rnestler Thanks for the bug report! Unfortunately this report is a little confused in that your posted `.gitignore` contains `!buildutils` instead of what I think was intended to be `!buildtools`. So in that sense, your whitelist pattern won't match. However, if I fix that and use `!buildtools`, then it still doesn't work, but the debug output becomes more useful:

```
DEBUG/ignore::walk/ignore/src/walk.rs:1395: whitelisting ./buildtools: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "!buildtools", actual: "**/buildtools", is_whitelist: true, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./build-64: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "build*", actual: "**/build*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./buildtools/test.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "build*", actual: "**/build*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./build: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "build*", actual: "**/build*", is_whitelist: false, is_only_dir: false })))
```

Notice that `./buildtools` does indeed get whitelisted and ripgrep descends into it, but `./buildtools/test.txt` gets ignored because it matches your `build*` pattern. One way to fix that is to change your `build*` rule to `**/build*`, which will ensure the `build*` pattern won't match through another slash. This does indeed fix your particular case.

The reason why this bug exists is because `git grep` searches based on *what is tracked by git*, where as ripgrep has to *guess* what is tracked by git by looking your `.gitignore`. Some things will always fall through the cracks using this strategy.

---

_Closed by @BurntSushi on 2018-02-07 14:15_

---

_Label `wontfix` added by @BurntSushi on 2018-02-07 14:15_

---

_Comment by @rnestler on 2018-02-08 13:57_

> Unfortunately this report is a little confused in that your posted .gitignore contains !buildutils instead of what I think was intended to be !buildtools. So in that sense, your whitelist pattern won't match.

Oh I'm so sorry about that. I tried to create a minimal working example and screwed up 🤦‍

> Notice that ./buildtools does indeed get whitelisted and ripgrep descends into it, but ./buildtools/test.txt gets ignored because it matches your build* pattern. One way to fix that is to change your build* rule to **/build*, which will ensure the build* pattern won't match through another slash. This does indeed fix your particular case.

According to my understanding of the debug output ripgrep changes the pattern to `**/build*` anyways (`original: "build*", actual: "**/build*"`) but since it doesn't behave the same I'm obviously misunderstanding something 😉 I guess it's related to the `literal_separator` option which results in a slightly different regex:

```
DEBUG:globset: glob converted to regex: Glob { glob: "**/build*", re: "(?-u)^(?:/?|.*/)build.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d'), ZeroOrMore]) }

DEBUG:globset: glob converted to regex: Glob { glob: "**/build*", re: "(?-u)^(?:/?|.*/)build[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d'), ZeroOrMore]) }
```


> The reason why this bug exists is because git grep searches based on what is tracked by git, where as ripgrep has to guess what is tracked by git by looking your .gitignore. Some things will always fall through the cracks using this strategy.

But the ripgrep still interprets the .gitignore pattern differently than git doesn't it?

I mean given:

```
% cat .gitignore 
build*
!buildtools

% tree
.
├── build
│   └── foo
├── build-64
│   └── foo
└── buildtools
    ├── bar (untracked)
    └── test.txt
```
`git status` still picks up the new file `bar` in the buildtools folder:
```
% git --version
git version 2.14.1
% git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	buildtools/bar
```

When I remove the white-list pattern `git status` won't pick it up.

So is this still a wont-fix or could this actually be solved? If it provides any value I could open a new issue. But I totally understand that converting the .gitignore globs to regexes is a brittle thing where it's almost impossible to get all edge-cases right. So I'm perfectly fine with just adjusting my .gitignore patterns 😊 

Also thanks a lot for creating and maintaining ripgrep!

---

_Comment by @BurntSushi on 2018-02-08 14:10_

> I guess it's related to the literal_separator option which results in a slightly different regex:

Indeed! When `literal_separator` is enabled, it prevents `*` from matching a `/`. Per `man gitignore`, this behavior is enabled when a literal `/` appears in the pattern. That's why I suggested `**/build*`. The presence of `/` in this case causes `literal_separator` to be enabled which in turn prevents `*` from matching a `/`.

> git status still picks up the new file bar in the buildtools folder:

Again, there is an impedance mismatch here. That's because `buildtools` is presumably tracked according to git because `buildtools/test.txt` is tracked.

> So is this still a wont-fix or could this actually be solved? If it provides any value I could open a new issue. But I totally understand that converting the .gitignore globs to regexes is a brittle thing where it's almost impossible to get all edge-cases right. So I'm perfectly fine with just adjusting my .gitignore patterns 😊

Just to clarify here, converting globs to regexes is an implementation detail, and _that_ is certainly not the problem here. The conversion process is sound. The problem here are the semantics. Basically, if you come up with a consistent rule that ripgrep can use here to produce the same behavior as git, then I'm all ears. I can't immediately see one, but perhaps a different perspective is what's needed. Certainly, a simple rule like "`*` never matches a `/`" isn't possible because things like `*.rs` would stop working (matching happens relative to the corresponding `.gitignore`, so a `*` in that case really needs to match through a `/`).

Note, if you want to brainstorm here, I **strongly** urge you to internalize the contents of `man gitignore` first. :)

---

_Comment by @rnestler on 2018-02-08 15:52_

> Again, there is an impedance mismatch here. That's because buildtools is presumably tracked according to git because buildtools/test.txt is tracked.

Ok so I untracked everything in buildtool and it will still show up because it is not ignored. But of course that means `git grep` won't find a thing in there 😉 
```
% git  status --ignored
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	buildtools/

Ignored files:
  (use "git add -f <file>..." to include in what will be committed)

	build-64/
	build/
```

> Note, if you want to brainstorm here, I strongly urge you to internalize the contents of man gitignore first. :)

Will do that! And also run some more experimentation and dig a bit into the ripgrep source code. I already feel a bit bad for consuming your time 😉 

---

_Comment by @BurntSushi on 2018-02-08 15:57_

> Ok so I untracked everything in buildtool and it will still show up because it is not ignored. But of course that means git grep won't find a thing in there 😉

The output of `git status` in this case is consistent with ripgrep. Remember, ripgrep knows to not ignore `buildtools` itself. The problem is that `build*` will cause everything underneath `buildtools` to get ignored because the `buildtools` whitelist rule only applied to that specific directory. For example, another way to solve your problem instead of `**/build*`  is to use `!buildtools*`. That will re-enable the whitelist on things starting with `buildtools`. But this would also cause directories like `buildtools-wat` to be whitelisted as well, and perhaps that isn't intentional. The way to fix that would be to keep the `!buildtools` whitelist and add a `!buildtools/**` whitelist in addition.

> Will do that! And also run some more experimentation and dig a bit into the ripgrep source code. I already feel a bit bad for consuming your time 😉

You shouldn't. So long as we're being productive and moving toward a resolution, then I'm very happy to participate. :)

---

_Comment by @rnestler on 2024-03-08 13:35_

I just noticed that this seems now fixed:

```
% rg --version
ripgrep 14.1.0
% cat .gitignore
build*
!buildtools
% tree
.
├── build
│   └── foo
├── build-64
│   └── foo
└── buildtools
    └── test.txt

4 directories, 3 files

% git grep foo
buildtools/test.txt:Test to find: foobar

% rg foo
buildtools/test.txt
1:Test to find: foobar
```

Noticed while trying to explain to somebody why it's important to not have recursive patterns by accident in `.gitignore` and using this bug report as an example :slightly_smiling_face: 

---
