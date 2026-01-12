```yaml
number: 815
title: "user guide should anticipate common failure mode (* in $HOME/.gitignore)"
type: issue
state: closed
author: ApolloTang
labels:
  - doc
assignees: []
created_at: 2018-02-18T01:40:57Z
updated_at: 2018-02-21T01:05:58Z
url: https://github.com/BurntSushi/ripgrep/issues/815
synced_at: 2026-01-12T16:13:22Z
```

# user guide should anticipate common failure mode (* in $HOME/.gitignore)

---

_@ApolloTang_

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev )
-SIMD -AVX

#### What operating system are you using ripgrep on?

macOS High Sierra
10.13.3


#### Bug report 
First time on ripgrep, following  [User Guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md)

but recursive search does not work !

![rg-bug](https://user-images.githubusercontent.com/1150961/36347315-6bfb5794-1422-11e8-9f07-f671834875a6.png)

```
$ rg 'fn write\(' --debug
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'f',
        'n',
        ' ',
        'w',
        'r',
        'i',
        't',
        'e',
        '('
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(fn write()], limit_size: 250, limit_class: 10 }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: "**/._*", re: "(?-u)^(?:/?|.*/)\\._.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('.'), Literal('_'), ZeroOrMore]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 0 literals, 28 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 3 regexes
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./.gitignore__: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./Cargo.toml: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./CHANGELOG.md: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./complete: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./globset: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./ignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./ci: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./benchsuite: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./tests: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./Cargo.lock: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./build.rs: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./HomebrewFormula: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./grep: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./README.md: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./termcolor: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./appveyor.yml: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./COPYING: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./UNLICENSE: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./compile: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./doc: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./wincolor: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./.travis.yml: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./snapcraft.yaml: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./LICENSE-MIT: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./pkg: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
DEBUG/ignore::walk/ignore/src/walk.rs:1392: ignoring ./src: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/apollotang/.gitignore"), original: "*/", actual: "**/*", is_whitelist: false, is_only_dir: true })))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
~/Desktop/ripgrep-0.7.1
$
```

---

_Comment by @okdana on 2018-02-18 02:18_

The debug output explains it: `rg` has traversed up the file tree, where it found `~/.gitignore`, which contains the rule `*`, which makes it ignore all files.

You can control which ignore files `rg` respects with `-u`, `--no-ignore`, `--no-ignore-vcs`, `--no-ignore-parent`, and so on. `--no-ignore-parent` might be the easiest thing here.

I wonder if it would make sense for there to be an option to make `rg` stop looking for parent `.gitignore` files once it encounters a parent containing a `.git` directory? That way it'd only respect the VCS ignores for the current repo. Might not be reliable or a common enough problem, idk

---

_Comment by @ApolloTang on 2018-02-18 02:34_

Thanks for quick response @okdana, Yes I do have a legacy .gitignore in my ~/ and I completely forgot all about it.



---

_Closed by @ApolloTang on 2018-02-18 02:34_

---

_Comment by @BurntSushi on 2018-02-18 02:48_

> I wonder if it would make sense for there to be an option to make rg stop looking for parent .gitignore files once it encounters a parent containing a .git directory? That way it'd only respect the VCS ignores for the current repo. Might not be reliable or a common enough problem, idk

This should actually be the current behavior: https://github.com/BurntSushi/ripgrep/blob/9b7f420faa7fbf21db07afbdeadb0828f6752d2f/tests/tests.rs#L615-L644

The problem here is that the guide instructs folks to download a zip containing the source, which, when unpacked, isn't actually a git directory.

---

_Comment by @okdana on 2018-02-18 02:50_

Oh, lol. Well then.

---

_Label `doc` added by @BurntSushi on 2018-02-20 12:43_

---

_Comment by @BurntSushi on 2018-02-20 12:44_

I'm going to re-open this. I think the `*` in `$HOME/.gitignore` is a common enough pattern that it would probably be wise to explicitly call this out in the user guide somewhere.

---

_Reopened by @BurntSushi on 2018-02-20 12:44_

---

_Renamed from "First time on ripgrep, following User Guide, but recursive search doesn't work" to "user guide should anticipate common failure mode (* in $HOME/.gitignore)" by @BurntSushi on 2018-02-20 12:45_

---

_Closed by @BurntSushi on 2018-02-21 01:05_

---
