```yaml
number: 526
title: support escaping meta characters in glob patterns
type: issue
state: closed
author: gabebw
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-06-22T22:49:48Z
updated_at: 2018-03-10T14:31:10Z
url: https://github.com/BurntSushi/ripgrep/issues/526
synced_at: 2026-01-12T16:13:22Z
```

# support escaping meta characters in glob patterns

---

_@gabebw_

I have the following in [my global `.gitignore`](https://github.com/gabebw/dotfiles/blob/master/.gitignore):

```gitignore
# ...other things...
[
]
# ...other things...
```

This is because my text editor sometimes creates files with these exact names, and I want to ignore them. However, ripgrep is (I think!) parsing these lines as part of a regex:

```
$ rg anything
/Users/gbwilliams/.gitignore: line 56: error parsing glob '[': unclosed character class; missing ']'
./.gitignore: line 2: error parsing glob '[': unclosed character class; missing ']'
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

-----------------------

To reproduce:

1. `mkdir test_dir`
1. `cd test_dir`
1. `echo [ > .gitignore`
1. `echo ] >> .gitignore`
1. `rg anything`
1. See an error

Version:

```
$ rg -V
ripgrep 0.5.2
```

Debug output:

```
$ rg --debug anything
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'a',
        'n',
        'y',
        't',
        'h',
        'i',
        'n',
        'g'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(anything)], limit_size: 250, limit_class: 10 }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.sw[po]", re: "(?-u)^(?:/?|.*/).*\\.sw[po]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('p', 'p'), ('o', 'o')] }]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.py[co]", re: "(?-u)^(?:/?|.*/).*\\.py[co]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('p'), Literal('y'), Class { negated: false, ranges: [('c', 'c'), ('o', 'o')] }]) }
DEBUG:globset: built glob set; 1 literals, 25 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
DEBUG:ignore::dir: /Users/gbwilliams/.gitignore: line 56: error parsing glob '[': unclosed character class; missing ']'
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.sw[po]", re: "(?-u)^(?:/?|.*/).*\\.sw[po]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('s'), Literal('w'), Class { negated: false, ranges: [('p', 'p'), ('o', 'o')] }]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.py[co]", re: "(?-u)^(?:/?|.*/).*\\.py[co]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('p'), Literal('y'), Class { negated: false, ranges: [('c', 'c'), ('o', 'o')] }]) }
DEBUG:globset: built glob set; 1 literals, 25 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
/Users/gbwilliams/.gitignore: line 56: error parsing glob '[': unclosed character class; missing ']'
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
./.gitignore: line 2: error parsing glob '[': unclosed character class; missing ']'
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```



---

_Comment by @okdana on 2017-06-23 07:35_

From [`gitignore(5)`](https://git-scm.com/docs/gitignore):

>Otherwise, Git treats the pattern as a shell glob suitable for consumption by fnmatch(3) with the FNM_PATHNAME flag

From [`glob(7)`](http://man7.org/linux/man-pages/man7/glob.7.html):

>A string is a wildcard pattern if it contains one of the characters '?', '\*' or '['.... An expression "[...]" where the first character after the leading '[' is not an '!' matches a single character, namely any of the characters enclosed by the brackets.... One can remove the special meaning of '?', '\*' and '[' by preceding them by a backslash, or, in case this is part of a shell command line, enclosing them in quotes.

From [`fnmatch(3)`](http://man7.org/linux/man-pages/man3/fnmatch.3.html):

>RETURN VALUE Zero if string matches pattern, FNM_NOMATCH if there is no match or another nonzero value if there is an error.

I made a simple C program to call `fnmatch()` with the pattern `[` and see what it returns — it gives `2`, which is one of those other non-zero error values (`FNM_NOMATCH` is equal to `1`).

And here is how git itself handles the `gitignore` pattern `[`:

```
heartswap:/tmp % mkdir repo && cd repo
heartswap:repo % git init
Initialized empty Git repository in /private/tmp/repo/.git/
heartswap:repo % touch '['

# Without .gitignore
heartswap:repo % git status -s
?? [

# With [ in .gitignore
heartswap:repo % echo '[' > .gitignore
heartswap:repo % git status -s
?? .gitignore
?? [

# With \[ in .gitignore
heartswap:repo % echo '\[' > .gitignore
heartswap:repo % git status -s
?? .gitignore
```

Sooo... i think git is behaving as documented: It treats `[` as an invalid pattern, just like `fnmatch()` would. `rg` behaves the same way, with one difference — it actually *tells you* that it's invalid. Personally i think the feedback is kind of nice.

In any case, you can fix the problem by escaping the pattern like this: `\[`

Note that `]` is NOT an invalid pattern — it is treated literally by `fnmatch()`, git, and `rg`.

---

_Comment by @BurntSushi on 2017-06-23 11:32_

@okdana's analysis is almost correct (thank you!). The only thing you missed is that ripgrep doesn't actually support the `\[` formulation. Instead, you'd have to write `[[]`.

I think if `fnmatch` and `git` support the `\[` formulation, then we probably should too (similarly for other meta characters). I think this bug fix would be entirely confined to the `globset` crate, which has its own glob parser.

---

_Renamed from "rg tries to parse `[` and `]` in gitignore" to "support escaping meta characters in glob patterns" by @BurntSushi on 2017-06-23 11:32_

---

_Label `enhancement` added by @BurntSushi on 2017-06-23 11:32_

---

_Label `help wanted` added by @BurntSushi on 2017-06-23 11:32_

---

_Comment by @okdana on 2017-06-23 19:36_

Oh, i didn't actually test `\[` in `rg`, just the others. Oops.

So, i *believe* i've got [a solution](https://github.com/BurntSushi/ripgrep/compare/9e51b18...okdana:feature/globset/escaping?expand=1) that works fine on UNIX... but it occurred to me that this has implications for Windows: It looks like `globset` tries to support (un-escaped) `\` as a path separator on that platform, and that seems fundamentally incompatible with consistent escaping behaviour.

Does *git* support `\` as a path separator in `.gitignore` on Windows? I don't have access to a Windows machine to test, but a cursory Google search and a glance at [the source](https://github.com/git/git/blob/master/wildmatch.c) seem to suggest it doesn't... i think.

Not sure what to do 🤔

---

_Comment by @gabebw on 2017-06-23 22:22_

@okdana Thank you for the in-depth analysis of what gitignore does with brackets! That's super cool to know!

---

_Comment by @elirnm on 2017-06-24 05:19_

@okdana From (non-exhaustively) testing git 2.13.0 on Windows 7 it doesn't appear to support `\` as a path separator.

---

_Comment by @elirnm on 2017-06-25 20:39_

@okdana Except there might be other repercussions if globset doesn't treat `\` as a path separator on Windows? If globset is used to parse the globs given by the `-g` option as well as the .gitignore globs, then could cause issues with `\` because tab-completion of paths on Windows terminals completes with `\`, even if `/` was typed by the user earlier.

So if globset is used to parse the `-g` option then not treating `\` as a path separator would essentially break terminal filepath completion on Windows. If globset isn't used for `-g` then this doesn't matter, of course.

The [C# glob matcher](https://github.com/SLaks/Minimatch/) that I'm familiar with has an `AllowWindowsPaths` option that disables all escape characters but treats `\` as a path separator, but introducing that here might be unwanted or beyond the scope of this issue.

---

_Comment by @okdana on 2017-06-25 22:35_

I suppose you could do something like have it check to see if the character following `\` is a meta-character, and, if not, normalise it to `/` on Windows like it does now. That way, if you passed `-g 'path\to\file.ext'` it would match as expected.

Most glob meta-characters are illegal in Windows file paths anyway, so there would probably never be a situation where you were trying to match a file name containing a literal `*` or whatever. I don't think brackets are illegal, though, so in cases like this one it might get weird — if you had a file path like `dir\subdir\[`, you would have to do `-g 'dir\subdir\\\[`.

I suspect that Windows user + glob containing sub-directory + file name containing meta-character is an exceptionally unlikely set of circumstances, so it'd probably work out *most* of the time in the real world. It does feel a little weird though — it certainly is non-standard, and it might give users the false impression that git supports this behaviour.

FWIW, i looked into how other projects handle globs on Windows:

* Mercurial's `hgignore` globbing, Ruby's `File.fnmatch()`, Perl's `File::FnMatch`, and PHP's `fnmatch()` all use `\` for escaping and `/` for path separators. (The Perl package doesn't work on Windows at all since it defers to the system `fnmatch()`.)

* Python's `fnmatch` module is poorly named, since its functions don't work like `fnmatch()` at all. None of them support escaping. *Some* of them normalise slashes.

* PHP's `glob()` supports both `/` and `\` on Windows. It seems to handle this by simply disabling escaping on that platform.

If i were a Windows user i think i'd just deal with the tab-completion not working — presumably it already fails to work, as it does on UNIX, in several cases: when you're doing negation (`!`), when you've given an 'absolute' path (leading `/`), or when you've got actual wild-cards in the pattern. But that's me 🤷‍♀️

---

_Comment by @elirnm on 2017-06-26 05:11_

@okdana Absolute paths do complete, on Powershell at least (the leading `\` or `/` is replaced with `C:\` or whatever the current drive is), but the other situations fail to work. I would personally be fine with tab completion not working in globs, but I also don't use paths in globs to ripgrep very much anyway.

I guess the real potential issue with changing the behavior of `\` in the `-g` argument is not the loss of tab completion but the failure (and maybe not for obvious reasons to a user) of paths separated with `\` in the `-g` option, even if typed that way by the user. But then, the behavior would match with how git parses globs and how you have to match `\` in regexes, so it might not be a big issue (especially if it's documented behavior). I don't think there's necessarily any problem with saying "`\` in paths in globs isn't supported because ripgrep parses globs in a way that's essentially industry-standard for cross-platform programs" (though I'm sure there's people out there who think otherwise). And even the programs that do support `\` in paths usually do so simply by having a "Windows mode" where escaping is completely disabled, not by being clever in some way.

---

_Comment by @okdana on 2017-06-26 05:37_

>Absolute paths do complete, on Powershell at least (the leading \ or / is replaced with C:\ or whatever the current drive is)

Sorry, i meant `gitignore`-style 'absolute' paths, where the leading `/` represents the root of the repository or search path, not the root of the file system. In that case i believe the tab-completion will fail, unless your CWD is the file-system root or you happen to have a file structure on the file-system root that mirrors your search directory.

>I guess the real potential issue with changing the behavior of \ in the -g argument is not the loss of tab completion but the failure (and maybe not for obvious reasons to a user) of paths separated with \ in the -g option, even if typed that way by the user.

Yeah, i can certainly see how it would be confusing if (from the user's perspective) the glob just silently failed to work. This tool was recommended to me in the first place because i found `ag`'s glob-handling confusing and esoteric, so i obv wouldn't want that inflicted on anyone else.

I suppose `rg` could print an informational warning on Windows if someone supplies a glob where `\` precedes a non-meta-character... but then i imagine you would have to implement partial parsing in `rg` (since printing warning messages doesn't seem like `globset`'s job), and maybe some users would find it irritating and want to turn it off, so you'd need to deal with that....

idk. At this point i feel like i'm getting out into the weeds over-analysing this so i'll shut up for a bit :|

---

_Comment by @jakwings on 2017-10-16 05:20_

Currently, if you want `**` to work for Windows paths, you have to replace all backslashes in the path with `/`. And if you do so, this is a reason to support backslash escaping. (I prefer to escape any character after `\`.)

---

_Closed by @BurntSushi on 2018-03-10 14:31_

---
