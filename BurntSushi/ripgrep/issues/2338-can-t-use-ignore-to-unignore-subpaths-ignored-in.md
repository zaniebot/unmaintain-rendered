```yaml
number: 2338
title: "Can't use .ignore to unignore subpaths ignored in .gitignore"
type: issue
state: closed
author: RedBeard0531
labels:
  - duplicate
assignees: []
created_at: 2022-10-24T11:16:32Z
updated_at: 2022-10-24T13:24:34Z
url: https://github.com/BurntSushi/ripgrep/issues/2338
synced_at: 2026-01-12T16:13:24Z
```

# Can't use .ignore to unignore subpaths ignored in .gitignore

---

_@RedBeard0531_

#### What version of ripgrep are you using?

```
> rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

But I think this is a "forever" bug since I remember running into it when rg was first released, but didn't file a bug at the time.

#### How did you install ripgrep?

Installed from Arch linux repos

#### What operating system are you using ripgrep on?

Linux

#### Describe your bug.

`rg` (and other tools using the `ignore` crate like `fd`) don't look in un-ignored subpaths in `.ignore` (eg `!/path/subpath`) if a parent path is ignored by `.gitignore` (eg `/path`).

Usecase: build directory is in .gitignore. A subpath of the build directory contains generated source files. I would like `rg` (and similar tools) to consider the generated source files, but not other build artifacts.

#### What are the steps to reproduce the behavior?

```sh
git init # otherwise .gitignore is ignored
echo '/dir' > .gitignore
echo '!/dir/sub/' > .ignore
mkdir -p dir/sub
echo foo > dir/sub/blah
rg foo --debug
```

#### What is the actual behavior?

No files are examined.

```
> rg foo --debug
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(foo)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.ignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./dir: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "/dir", actual: "dir", is_whitelist: false, is_only_dir: false })))
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

#### What is the expected behavior?

It should have looked in `dir/sub/blah`. Note that putting `!/dir` in `.ignore` _does_ work in this case, but for the real codebase I _do_ want the equivalent of `dir` to be ignored except for a few selected paths.

---

_Comment by @BurntSushi on 2022-10-24 12:01_

Yeah this has been reported before, although finding the issues for it is actually a little challenging. #1050 looks pretty close to this, so I'll mark this as a duplicate.

I'm not sure this will ever get fixed. The operational model here is that ripgrep doesn't descend into directories that have been ignored. This lets ripgrep skip over entire parts of the tree. Imagine a very large `build` directory for example (or something else), and ripgrep slowing way down even though it has been marked as ignored. That would be very surprising behavior. Therefore, the only real way to fix this sort of issue, as far as I can tell, is to do analysis on the globs and only descend into such directories if it's _possible_ for some other whitelist rule to unignored a subpath. That feels like a very tricky analysis to do correctly.

The other reason why I'm not sure this will ever be fixed is that it's often possible to re-write your ignore rules to work-around this issue. For example, you could ignore `/dir/*` and then whitelist `!/dir/sub`. That doesn't necessarily work so nicely in all such instances of this bug, but it might work for you.

---

_Closed by @BurntSushi on 2022-10-24 12:01_

---

_Label `duplicate` added by @BurntSushi on 2022-10-24 12:01_

---

_Comment by @RedBeard0531 on 2022-10-24 13:24_

That case is a bit different, or at least harder to handle efficiently, because both paths were globs, so it isn't statically clear what path is being unignored. In the case where a _specific_ path is unignored, it should be possible to treat that effectively as an additional "root" path for scanning as long as it appears to be a subdirectory of an exiting root path. (I'm using "root" in the sense of gc roots, as in where you start your traversal. I don't know what you call that concept internally.) So in this case, `rg foo` could be treated as an equivalent of `rg foo . ./dir/sub`. This means that you *don't* actually need to descend or look at `./dir`'s files at all and can just skip directly to `./dir/sub` (along with everything else in `./` that isn't ignored of course).

I was able to work around it but it ends up being a bit gross. It basically requires a 3-step dance in the `.ignore` file: 1) `!/path` to unignore the dir, 2) `/path/*` to reignore everything inside, 3) `!/path/want` to re-unignore the stuff I don't want ignored. I assume I would need to do that for each intermediate subdirectory in if I wanted to unignore `/path/to/something/deep`, which gets even grosser (although I didn't test because the files/dirs I want to unignore were only 1 layer deep). Also I assume that it would be even slower than a "direct" unignored path, because now you need to expand and glob-match each directory, even though you know where you need to look.

Of course, since there _is_ a workaround, I can understand you not wanting to invest effort to make this case work well.

---
