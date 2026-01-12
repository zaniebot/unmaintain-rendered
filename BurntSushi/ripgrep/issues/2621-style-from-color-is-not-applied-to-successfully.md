```yaml
number: 2621
title: Style (from --color) is not applied to successfully matched tab (\t) characters
type: issue
state: closed
author: condekind
labels:
  - invalid
assignees: []
created_at: 2023-10-06T00:45:55Z
updated_at: 2023-10-06T00:56:42Z
url: https://github.com/BurntSushi/ripgrep/issues/2621
synced_at: 2026-01-12T16:13:24Z
```

# Style (from --color) is not applied to successfully matched tab (\t) characters

---

_@condekind_

### What version of ripgrep are you using?

ripgrep 13.0.0
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)

### How did you install ripgrep?

cargo install --features 'pcre2 simd-accel' ripgrep

### What operating system are you using ripgrep on?

Up-to-date (2023-10-05) Arch Linux

### Describe your bug.

Ripgrep doesn't apply the provided style to matched tab characters.
I ran some tests with unicode defined whitespaces and it applies the style to them as expected. The exceptions are tab, zero-width characters and control/multiline-related characters (which is understandable, except for tab, unless I'm missing something)

### What are the steps to reproduce the behavior?

Run ripgrep on a string/file containing one or more tab characters, providing a style (through --color) that would make it visible (e.g., bg, underline) and using a search pattern that matches just the tab (or, alternatively, any whitespace).

All 6 runs below match successfully, as they should - but only the last 2 have the style applied to the matching part (just for reference, as they use regular space instead of tab)
```bash
# Input: tab character around braces; Search pattern: tabs only
printf '[\t]' | rg --colors 'match:bg:cyan' --colors 'match:style:underline'         '\t';
printf '[\t]' | rg --colors 'match:bg:cyan' --colors 'match:style:underline' --count '\t';

# Input: tab character around braces; Search pattern: any whitespace
printf '[\t]' | rg --colors 'match:bg:cyan' --colors 'match:style:underline'         '\s';
printf '[\t]' | rg --colors 'match:bg:cyan' --colors 'match:style:underline' --count '\s';

# Input: regular space around braces; Search pattern: any whitespace
printf '[ ]'  | rg --colors 'match:bg:cyan' --colors 'match:style:underline'         '\s';
printf '[ ]'  | rg --colors 'match:bg:cyan' --colors 'match:style:underline' --count '\s';
```

### What is the actual behavior?

Git doesn't like colors, but since the issue is about style, I'm also providing a small screenshot, as the text output doesn't show the problem:

![2023-10-05-213501_581x86_scrot](https://github.com/BurntSushi/ripgrep/assets/8984737/32b491c1-6d57-44e8-b711-e9b6c9ac984b)

I tested it with terminals=(kitty, urxvt, alacritty) and shells=(zsh, bash).
The issue happens in all of them.

Commands run with --debug and output (It seems to me the debug info won't matter for this, though):
```
echo ''
# Input: tab character around braces; Search pattern: tabs only
printf '[\t]' | rg --debug --colors 'match:bg:cyan' --colors 'match:style:underline'         '\t';
echo ''
# Input: tab character around braces; Search pattern: any whitespace
printf '[\t]' | rg --debug --colors 'match:bg:cyan' --colors 'match:style:underline'         '\s';
echo ''
# Input: regular space around braces; Search pattern: any whitespace
printf '[ ]'  | rg --debug --colors 'match:bg:cyan' --colors 'match:style:underline'         '\s';
echo ''

DEBUG|grep_regex::literal|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-regex-0.1.11/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(\t)], limit_size: 250, limit_class: 10 }
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:426: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
[   ]

DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:426: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
[   ]

DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:426: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
DEBUG|globset|/home/foo/.local/share/cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.13/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
[ ]
```

### What is the expected behavior?

ripgrep should've applied the provided style to all matching characters in the output

---

_Comment by @BurntSushi on 2023-10-06 00:56_

This is correct behavior. `\t` isn't a printable character. It's a control character. It moves the cursor.

Consider trying different programs with different terminal emulators. You should see that none of them colorize tab characters.

---

_Closed by @BurntSushi on 2023-10-06 00:56_

---

_Label `invalid` added by @BurntSushi on 2023-10-06 00:56_

---
