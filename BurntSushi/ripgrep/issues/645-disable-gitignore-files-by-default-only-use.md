```yaml
number: 645
title: Disable .gitignore files by default, only use .ignore files?
type: issue
state: closed
author: sunshowers
labels:
  - question
assignees: []
created_at: 2017-10-20T18:46:30Z
updated_at: 2018-10-26T09:09:45Z
url: https://github.com/BurntSushi/ripgrep/issues/645
synced_at: 2026-01-12T16:13:22Z
```

# Disable .gitignore files by default, only use .ignore files?

---

_@sunshowers_

Hi! Does ripgrep provide a way to disable gitignore parsing by default and only rely on `.ignore` files?

---

Some surrounding context:

We've deployed ripgrep to thousands of developers at Facebook and people really love it overall. But we have thousands of files that are tracked but match ignore rules, and ripgrep skips over those files by default.

(Facebook uses Mercurial, but for various reasons the source of truth for ignore rules is still gitignores. We have a script that reads a repo with gitignores and generates a set of hgignores to go along with it. This shouldn't be relevant to the rest of the discussion.)

There's a fundamental incompatibility between the way ripgrep looks at ignore rules and the way VCS systems look at ignore rules. `.gitignore` (and `.hgignore`, `.svnignore` etc) only applies to files *not already tracked* by the VCS. But since ripgrep is unaware of what files are tracked by the VCS, it applies gitignore rules to all files including tracked ones.

At Facebook we have thousands of files that are tracked but match ignore rules, and ripgrep skips over them by default. I would be very surprised if other repos (especially large ones) wouldn't hit the same issue.

It would be great if we could maintain a separate list of rules in `.ignore` and then have `rg` not try and read `.gitignore` rules at all. Even better if this could be configured per-repository, but a global config would also be OK.

I guess we could deploy `rg` as a shell script that calls the actual `rg` binary with `--no-ignore-vcs`, but then sometimes people do want to apply the ignore rules and there doesn't appear to be a `--ignore-vcs` flag.

---

_Comment by @BurntSushi on 2017-10-21 01:18_

> Hi! Does ripgrep provide a way to disable gitignore parsing by default and only rely on .ignore files?

Hiya! In today's ripgrep, other than `--no-ignore-vcs`, yes, there is technically a way to do this that I *think* satisfies your use case. If you *start* your `.ignore` file with a blanket whitelist, e.g., `!*`, then it will override every single gitignore file that ripgrep reads (because any `.ignore` file takes precedence over all `.gitignore` files, the exact rules are [documented here](https://docs.rs/ignore/0.2.2/ignore/struct.WalkBuilder.html#ignore-rules)). The immediate problem you'll notice with this is that it will whitelist all hidden files as well, which overrides ripgrep's logic to skip hidden files. You can work around this by using the pattern `![!.]*`, which white lists every file that doesn't start with a `.`.

Note that it will be interesting to see whether such a rule causes ripgrep to run more slowly on your repo. If your gitignore files are already extensive, then my guess is no, but it's worth checking just in case.

With that said, to respond more generally:

* I do agree that there is an impedance mismatch here between git and ripgrep for exactly the reason you state.
* In general, I think the "right" solution to this is to make `--no-ignore-vcs` work for you, such that an `--ignore-vcs` flag is available. This is really part of #196, which is to add config files to ripgrep, which in turn needs precisely this behavior: the ability to override any flag that has been set as a default in the config file. Unfortunately, that issue has been stalled for some time, but it is still something I'd like to do.

The other solution that I don't think you mentioned is to add explicit whitelists for files that are tracked by git but would otherwise be ignored. My guess is that you've already considered that solution and ruled it out. :-)

---

_Label `question` added by @BurntSushi on 2017-10-21 01:24_

---

_Comment by @sunshowers on 2017-10-22 00:00_

Thanks! Looks like `!*` works to disable gitignore parsing, but there doesn't appear to be a difference wrt hidden files caused if I replace `!*` with `![!.]*`. I think matching hidden files is OK for us, so consider this problem mostly resolved.

Is the exact syntax for `.ignore` documented somewhere? I couldn't find any docs in the `ignore` create.

---

_Comment by @sunshowers on 2017-10-22 00:03_

Looking at `--debug` output I noticed that `.gitignore` files were still being parsed. It would be a cool optimization to skip over those files if `.ignore` is sufficiently broad.

Having this `.ignore` file doesn't make ripgrep appreciably slower, but skipping over `.gitignore` files with `--no-ignore-vcs` causes ripgrep to be around 33% faster for our repo :)

---

_Comment by @sunshowers on 2017-10-22 00:17_

Also, for anyone who stumbles on this in the future:

> The other solution that I don't think you mentioned is to add explicit whitelists for files that are tracked by git but would otherwise be ignored. My guess is that you've already considered that solution and ruled it out. :-)

Yeah, the problem is not with building that list of files (Mercurial makes it really easy to figure that out: `hg files -I 'set:hgignore()'`). The problem is keeping that list up to date in a repository which thousands of engineers are committing to. :)

---

_Comment by @BurntSushi on 2017-10-22 00:29_

@sid0 Here's an example that shows the difference between `!*` and `![!.]*`:

```
[andrew@Cheetah ripgrep-645] tree
.
├── bar
├── baz
├── foo
└── quux

0 directories, 4 files
[andrew@Cheetah ripgrep-645] tree -a
.
├── bar
├── baz
├── foo
├── .git
│   ├── branches
│   ├── config
│   ├── description
│   ├── HEAD
│   ├── hooks
│   │   ├── applypatch-msg.sample
│   │   ├── commit-msg.sample
│   │   ├── post-update.sample
│   │   ├── pre-applypatch.sample
│   │   ├── pre-commit.sample
│   │   ├── prepare-commit-msg.sample
│   │   ├── pre-push.sample
│   │   ├── pre-rebase.sample
│   │   ├── pre-receive.sample
│   │   └── update.sample
│   ├── info
│   │   └── exclude
│   ├── objects
│   │   ├── info
│   │   └── pack
│   └── refs
│       ├── heads
│       └── tags
├── .gitignore
├── .ignore
└── quux

10 directories, 20 files
[andrew@Cheetah ripgrep-645] cat .gitignore 
quux
bar
[andrew@Cheetah ripgrep-645] cat .ignore
![!.]*
quux
[andrew@Cheetah ripgrep-645] rg -l test
baz
foo
bar
```

Now change `![!.]*` to `!*`, now ripgrep shows hidden files:

```
[andrew@Cheetah ripgrep-645] cat .ignore
!*
quux
[andrew@Cheetah ripgrep-645] rg -l test
baz
bar
.git/hooks/pre-applypatch.sample
.git/hooks/pre-rebase.sample
.git/hooks/pre-commit.sample
foo
.git/hooks/applypatch-msg.sample
.git/hooks/commit-msg.sample
.git/hooks/pre-receive.sample
```

> Is the exact syntax for .ignore documented somewhere? I couldn't find any docs in the ignore create.

It should be the same as `.gitignore`, but the glob implementation itself is in the `globset` crate, and its syntax is documented here: https://docs.rs/globset/0.2.0/globset/#syntax

> Looking at --debug output I noticed that .gitignore files were still being parsed. It would be a cool optimization to skip over those files if .ignore is sufficiently broad.

> Having this .ignore file doesn't make ripgrep appreciably slower, but skipping over .gitignore files with --no-ignore-vcs causes ripgrep to be around 33% faster for our repo :)

Yeah that'd be neat. 33% is quite amazing. Your `.gitignore` files must be craaaazy! ripgrep (well, `globset`) already does a ton of optimizations here, and actually avoids compiling a glob matcher (which is actually implemented via translation to a regex) if it doesn't have to, e.g., for simple globs like `foo` or `*.foo`.

I'm personally unlikely to implement said optimization because I do still consider `--no-ignore-vcs` with a corresponding `--ignore-vcs` to be the "right" answer here. The optimization is probably pretty tricky to implement in the current code as well, since you need to turn it on and off depending on where you are in the directory tree during traversal. (If the `.ignore` file that contains `!*` is no longer in scope, then you need to go back to matching gitignores.)

> Yeah, the problem is not with building that list of files (Mercurial makes it really easy to figure that out: `hg files -I 'set:hgignore()'`). The problem is keeping that list up to date in a repository which thousands of engineers are committing to. :)

Interesting! My mind has trouble thinking at that level of development scale. Pretty neat tidbit, thanks!

-----

My feeling is that this particular issue can be closed, where the `--ignore-vcs` option is covered by #196, or have I missed anything else?

---

_Comment by @sunshowers on 2017-10-22 20:37_

> Here's an example that shows the difference between !* and ![!.]*:

Ah, looks like it works for hidden files in the top level but not in subdirectories (try adding e.g. `dir1/.foo`). But `!**/[!.]*` works fine for hidden files in subdirectories too. This is a bit weird, I guess.

---

_Comment by @BurntSushi on 2017-10-22 20:49_

@sid0 Ah right yeah, if a glob doesn't contain a `/`, then the glob is applied to the entire path (relative to the originating ignore file), so `![!.]*` only works on top-level items. The `!**/[!.]*` glob correctly checks the basename of a path. (These are gitignore semantics.)

---

_Comment by @sunshowers on 2017-10-22 23:15_

Thanks then!

---

_Closed by @sunshowers on 2017-10-22 23:15_

---

_Comment by @BurntSushi on 2018-03-23 15:02_

@sid0 I forgot to update this issue. The latest release of ripgrep now has a `--ignore-vcs` flag (to complement the `--no-ignore-vcs` flag), in addition to support for persistent configuration files.

---

_Comment by @tagwint on 2018-10-25 09:32_

Hi there, 
what would be a suggested way to have .svn folders ignored from grep, yet having other folders started with a dot still be searched ?
I tried
`rg --hidden --ignore-vcs something
`
and this still searches in .svn

```
rg --version
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)
```

UPD: I ended up with explicitly specifying glob to exclude:
`rg --glob "!.svn/" --hidden something`

since it looks like --ignore-vcs is overridden by --hidden 
and the other files/folders with dot  in front (these i want to be included in search) considered hidden





---

_Comment by @BurntSushi on 2018-10-25 11:10_

@tagwint You might consider reading the [GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering) for help on how to deal with filtering. Improvements are welcome to the guide to make things clearer.

To answer your question, just add `.svn` to an `.ignore` file:

```
$ mkdir .svn .foo
$ echo sherlock > .svn/test
$ echo sherlock > .foo/test
$ rg sherlock -l
$ rg sherlock -l --hidden
.foo/test
.svn/test
$ echo '.svn' > .ignore
$ rg sherlock -l --hidden
.foo/test
```

---

_Comment by @tagwint on 2018-10-26 09:08_

Thanks for suggestion, @BurntSushi 
The GUIDE is good and many cases are explained well, althouth paritcularly --ignore-vcs option is not mentioned in there.
I usually do searches in different servers/locations and cannot expect  .ignore files are present there.
modifying/creating .ignore files in each location would be less efficient than specifying excludes directly in command line. Currently I am quite happy with the approach I described myself and which is efficiently the same as you suggested in your example.
It just feels not quite logical to me that when  --hidden option is there, --ignore-vcs option is not respected 
for at least .svn/ folders that are considered vcs related. If --hidden is supposed to win over --ignore-vcs by design (which is rather not obvious), I'd suggest mentioning that in the documentation for completeness reason.


---
