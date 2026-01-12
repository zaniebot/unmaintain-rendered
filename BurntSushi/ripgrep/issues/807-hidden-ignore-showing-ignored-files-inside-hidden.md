```yaml
number: 807
title: "--hidden + .ignore showing ignored files inside hidden directories"
type: issue
state: closed
author: cpixl
labels:
  - bug
assignees: []
created_at: 2018-02-14T19:45:55Z
updated_at: 2018-02-14T23:17:20Z
url: https://github.com/BurntSushi/ripgrep/issues/807
synced_at: 2026-01-12T16:13:22Z
```

# --hidden + .ignore showing ignored files inside hidden directories

---

_@cpixl_

#### What version of ripgrep are you using?
ripgrep 0.8.0
-SIMD -AVX

#### What operating system are you using ripgrep on?
Arch Linux

#### Describe your question, feature request, or bug.

The `--hidden` and `.ignore` file combination is not working well for ignored files inside hidden directories e.g. a `.a/b/c` file shouldn't appear if `.ignore` contains `.a/b`.

#### If this is a bug, what are the steps to reproduce the behavior?

```bash
mkdir -p .a/b .a/c
touch .a/b/file .a/c/file
echo .a/b > .ignore
ln -rsf .ignore .gitignore
rg --debug --hidden --files .
```

#### If this is a bug, what is the actual behavior?

This is the current output:
```
DEBUG/rg::config/src/config.rs:42: /home/user/.config/ripgrep/ripgreprc: arguments loaded from config file: ["--smart-case", "--follow"]
DEBUG/rg::args/src/args.rs:143: final argv: ["rg", "--smart-case", "--follow", "--debug", "--hidden", "--files", "."]
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: true
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG/globset/globset/src/lib.rs:396: glob converted to regex: Glob { glob: ".local/share/nvim/{bundle,undo,swap}", re: "(?-u)^\\.local/share/nvim/(swap|undo|bundle)$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('.'), Literal('l'), Literal('o'), Literal('c'), Literal('a'), Literal('l'), Literal('/'), Literal('s'), Literal('h'), Literal('a'), Literal('r'), Literal('e'), Literal('/'), Literal('n'), Literal('v'), Literal('i'), Literal('m'), Literal('/'), Alternates([Tokens([Literal('s'), Literal('w'), Literal('a'), Literal('p')]), Tokens([Literal('u'), Literal('n'), Literal('d'), Literal('o')]), Tokens([Literal('b'), Literal('u'), Literal('n'), Literal('d'), Literal('l'), Literal('e')])])]) }
DEBUG/globset/globset/src/lib.rs:401: built glob set; 4 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG/globset/globset/src/lib.rs:401: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG/globset/globset/src/lib.rs:401: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
.ignore
.gitignore
.a/c/file
.a/b/file
```

#### If this is a bug, what is the expected behavior?

The same files shown by `git status` (after a `git init && git add .`):

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .a/c/file
	new file:   .gitignore
	new file:   .ignore

```

As you can see, the `.a/b/file` was ommited by Git, but not by ripgrep.


---

_Comment by @okdana on 2018-02-14 19:55_

FYI, the problem doesn't occur if you do `rg --files --hidden` or `rg --files --hidden $PWD` or even `rg --files --hidden ./` — you explicitly need to use `.` for the path

---

_Comment by @cpixl on 2018-02-14 19:56_

[`fd`](https://github.com/sharkdp/fd) is showing a similar behavior with `fd --hidden --type file`, which makes me believe it may be related to [`ignore`](https://docs.rs/ignore).


---

_Label `bug` added by @BurntSushi on 2018-02-14 20:54_

---

_Closed by @BurntSushi on 2018-02-14 23:16_

---

_Comment by @BurntSushi on 2018-02-14 23:17_

@okdana's observation was spot-on and led me to exactly the place where this needed to be fixed. Thanks @dan-santana for the awesome bug report!

---
