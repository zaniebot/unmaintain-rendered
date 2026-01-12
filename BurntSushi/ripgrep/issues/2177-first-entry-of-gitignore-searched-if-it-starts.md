```yaml
number: 2177
title: First entry of gitignore searched if it starts with BOM
type: issue
state: closed
author: tvrg
labels:
  - bug
  - rollup
assignees: []
created_at: 2022-04-11T12:00:47Z
updated_at: 2025-09-20T01:08:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2177
synced_at: 2026-01-12T16:13:24Z
```

# First entry of gitignore searched if it starts with BOM

---

_@tvrg_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

pacman -S ripgrep

#### What operating system are you using ripgrep on?

Arch Linux

#### Describe your bug.

When a .gitignore starts with a BOM, ripgrep does not ignore the first entry of the .gitignore. git itself ignores the first entry.

#### What are the steps to reproduce the behavior?

```
$ echo "\xef\xbb\xbftest\nignoreme" > .gitignore && echo "test" > ignoreme
$ rg test
```

#### What is the actual behavior?

```
$ rg --debug test
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/npm-debug.log*", re: "(?-u)^(?:/?|.*/)npm\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('n'), Literal('p'), Literal('m'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/yarn-debug.log*", re: "(?-u)^(?:/?|.*/)yarn\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('y'), Literal('a'), Literal('r'), Literal('n'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:416: glob converted to regex: Glob { glob: "**/yarn-error.log*", re: "(?-u)^(?:/?|.*/)yarn\\-error\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('y'), Literal('a'), Literal('r'), Literal('n'), Literal('-'), Literal('e'), Literal('r'), Literal('r'), Literal('o'), Literal('r'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 1 literals, 6 basenames, 3 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 3 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
ignoreme
1:test
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### What is the expected behavior?

It should ignore `ignoreme` because git does so as well. See e.g. `git status`.


---

_Comment by @BurntSushi on 2022-04-11 12:15_

I believe the description of your bug report is correct, but your reproduction isn't. `rg` isn't ignoring `ignoreme` in your particular example because of the BOM, it's not ignoring it because it doesn't respect `.gitignore` outside of a git repository. You either need to run `git init` or `mv .gitignore .ignore`. Here's my repro:

```
$ echo "\xef\xbb\xbfignoreme1\nignoreme2" > .ignore && echo "test" > ignoreme1 && echo "test" > ignoreme2
$ rg test
ignoreme1
1:test
```

But `rg test` should return no results. Instead, it only ignores `ignoreme2` and not `ignoreme1`. If I drop the BOM from the `.ignore` file, then ripgrep correctly ignores both `.ignoreme1` and `.ignoreme2`.

I suspect the fix will be to tweak this code to handle the BOM:

https://github.com/BurntSushi/ripgrep/blob/ced5b92aa93eb47e892bd2fd26ab454008721730/crates/ignore/src/gitignore.rs#L386-L408

---

_Label `bug` added by @BurntSushi on 2022-04-11 12:15_

---

_Comment by @tvrg on 2022-04-11 12:33_

Oh, yes, sorry for the incomplete reproduction. I was actually running the commands in an initialized git repo.

I'll try to fix this in the next days with the help of your pointer above if nobody else is faster.



---

_Comment by @starthal on 2024-04-17 19:58_

Here is what Git does: https://github.com/git/git/blob/21306a098c3f174ad4c2a5cddb9069ee27a548b0/utf8.c#L794-L803
```c
const char utf8_bom[] = "\357\273\277";

int skip_utf8_bom(char **text, size_t len)
{
	if (len < strlen(utf8_bom) ||
	    memcmp(*text, utf8_bom, strlen(utf8_bom)))
		return 0;
	*text += strlen(utf8_bom);
	return 1;
}
```

If `ignore` intends to match its behavior then it should just skip this sequence in the first line.

---

_Label `rollup` added by @BurntSushi on 2025-07-11 20:33_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
