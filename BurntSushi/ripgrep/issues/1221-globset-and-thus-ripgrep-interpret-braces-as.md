```yaml
number: 1221
title: "globset (and thus ripgrep) interpret braces ({}) as alternative (\"a or b\") whereas git doesn't"
type: issue
state: open
author: anntzer
labels:
  - bug
assignees: []
created_at: 2019-03-13T15:07:32Z
updated_at: 2023-04-18T07:48:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1221
synced_at: 2026-01-12T16:13:23Z
```

# globset (and thus ripgrep) interpret braces ({}) as alternative ("a or b") whereas git doesn't

---

_@anntzer_

#### What version of ripgrep are you using?

ripgrep 0.9.0
-SIMD -AVX

#### How did you install ripgrep?

Fedora 29 distro package.

#### What operating system are you using ripgrep on?

Fedora 29.

#### Describe your question, feature request, or bug.

globset (and thus ripgrep) interpret braces (`{}`) as alternative ("a or b") whereas git doesn't, causing a different interpretation of entries containing braces in .gitignore.

#### If this is a bug, what are the steps to reproduce the behavior?

```
git init foo && cd foo
touch 'test.txt' 'test.txt{}'
echo '*.txt{}' >.gitignore
git status
rg --files --debug
```

#### If this is a bug, what is the actual behavior?

```
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore
        foo.txt

nothing added to commit but untracked files present (use "git add" to track)

$ rg --files --debug
DEBUG/grep::search//usr/share/cargo/registry/grep-0.1.9/src/search.rs:195: original regex HIR pattern:
(?:[Zz]{0})*
DEBUG/grep::search//usr/share/cargo/registry/grep-0.1.9/src/search.rs:197: transformed regex HIR pattern:
(?:[Zz]{0})*
DEBUG/globset//usr/share/cargo/registry/globset-0.4.1/src/lib.rs:429: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/globset//usr/share/cargo/registry/globset-0.4.1/src/lib.rs:424: glob converted to regex: Glob { glob: "**/*.txt{}", re: "(?-u)^(?:/?|.*/).*\\.txt$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('t'), Literal('x'), Literal('t'), Alternates([Tokens([])])]) }
DEBUG/globset//usr/share/cargo/registry/globset-0.4.1/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/ignore::walk//usr/share/cargo/registry/ignore-0.4.3/src/walk.rs:1414: ignoring ./foo.txt: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*.txt{}", actual: "**/*.txt{}", is_whitelist: false, is_only_dir: false })))
DEBUG/ignore::walk//usr/share/cargo/registry/ignore-0.4.3/src/walk.rs:1414: ignoring ./.git: Ignore(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "!.git", actual: "**/.git", is_whitelist: true, is_only_dir: false })))))
.gitignore
foo.txt{}
```

#### If this is a bug, what is the expected behavior?

`rg --files` should list `foo.txt` and ignore `foo.txt{}` (matching git's behavior), not the opposite.


---

_Comment by @okdana on 2019-03-13 16:16_

IMO, one thing that ripgrep probably *should* do, regardless of whether it disables brace expansion in `.gitignore` files (which i suppose would require a setting to toggle, communication of that setting, &c.), is let brace sets without alternates just fall through. This is how bash and zsh both behave:

```
% print -r {}
{}
% print -r {foo}
{foo}
% print -r {foo,bar}
foo bar
```

It still wouldn't match Git's behaviour exactly, but i imagine it'd be a lot harder to trigger the difference in practice.

---

_Comment by @yroyon on 2019-03-31 21:12_

I was about to open a new issue, but my problem is the same as this one.

Reproducer:

```
$ ls -A
.gitignore
$ cat .gitignore
{{a}}
$ rg whatevs
./.gitignore: line 1: error parsing glob '{{a}}': nested alternate groups are not allowed
$ rg --version
ripgrep 0.10.0 (rev 0913972104)
```

Note that `{{something}}` is a legal filename.  There are tools out there, data shippers for Elasticsearch, that use dirnames like `{{cookiecutter.beat}}`.  I'm getting boatloads of error messages whenever I use ripgrep near those guys.

---

_Comment by @BurntSushi on 2019-04-03 13:11_

This ticket is a duplicate of #1183, but since this one has more activity, I'm going to close #1183.

---

_Label `bug` added by @BurntSushi on 2019-04-12 11:36_

---

_Comment by @gofri on 2023-04-18 07:36_

@BurntSushi

This one raises an issue for my application of ripgrep, so I'm willing to work on a PR for it if the following sounds ok to you:
1. Adding `literal_alternates : bool` to GlobOptions. Parsing the `Alternates` token with respect to this option.
2. For explicit globs add `--glob-literal-alternates` and `--pre-glob-literal-alternates` to ripgrep and set in the GlobOptions.
3. For ignore files auto discovery, add `--ignore-file-literal-alternates` and a corresponding `literal_alternates` to  `GitignoreBuilder` (similar to case_sensitive).

Please let me know if that'd be potentially merged.
Also, if it does, let me know if I missed use cases.

---

The main issue I see is `create_gitignore()` which wraps the functionality for directories and would need to be modified to also accept this new flag. That is a breaking change, so I guess that would have to go to the next semver (I can add a new function and replace the calls in `ignore/src/dir.rs` without breaking APIs, but that'd make the code a bit messy, so let me know what you think about that specifically)

---
