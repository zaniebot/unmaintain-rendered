```yaml
number: 2492
title: Automatically ignore other git worktrees?
type: issue
state: closed
author: jeanas
labels:
  - wontfix
assignees: []
created_at: 2023-04-17T20:36:35Z
updated_at: 2023-11-22T01:11:11Z
url: https://github.com/BurntSushi/ripgrep/issues/2492
synced_at: 2026-01-12T16:13:24Z
```

# Automatically ignore other git worktrees?

---

_@jeanas_

#### Describe your feature request

When ran inside a Git repository, I would appreciate if ripgrep could only reports results from that worktree, but not from other worktrees that may be inside it. This is the behavior of `git grep`:

```
~/repos/lilypond $ git grep "parse_embedded_scheme"
lily/include/parse-scm.hh:31:SCM parse_embedded_scheme (const Input &start, Lily_parser *parser,
lily/lexer.ll:364:      SCM sval = parse_embedded_scheme (start, parser_, &parsed);
lily/lexer.ll:413:      SCM sval = parse_embedded_scheme (hi, parser_, &parsed);
lily/lexer.ll:429:      SCM sval = parse_embedded_scheme (hi, parser_, &parsed);
lily/parse-scm.cc:214:internal_parse_embedded_scheme (void *p)
lily/parse-scm.cc:268:parse_embedded_scheme (const Input &start, Lily_parser *parser,
lily/parse-scm.cc:274:  SCM result = scm_c_catch (SCM_BOOL_T, internal_parse_embedded_scheme, &ps,
~/repos/lilypond $ rg "parse_embedded_scheme"
lily/parse-scm.cc
214:internal_parse_embedded_scheme (void *p)
268:parse_embedded_scheme (const Input &start, Lily_parser *parser,
274:  SCM result = scm_c_catch (SCM_BOOL_T, internal_parse_embedded_scheme, &ps,

lily/include/parse-scm.hh
31:SCM parse_embedded_scheme (const Input &start, Lily_parser *parser,

master/lily/lexer.ll
364:	SCM sval = parse_embedded_scheme (start, parser_, &parsed);
413:	SCM sval = parse_embedded_scheme (hi, parser_, &parsed);
429:	SCM sval = parse_embedded_scheme (hi, parser_, &parsed);

lily/lexer.ll
364:	SCM sval = parse_embedded_scheme (start, parser_, &parsed);
413:	SCM sval = parse_embedded_scheme (hi, parser_, &parsed);
429:	SCM sval = parse_embedded_scheme (hi, parser_, &parsed);

master/lily/include/parse-scm.hh
31:SCM parse_embedded_scheme (const Input &start, Lily_parser *parser,

master/lily/parse-scm.cc
214:internal_parse_embedded_scheme (void *p)
268:parse_embedded_scheme (const Input &start, Lily_parser *parser,
274:  SCM result = scm_c_catch (SCM_BOOL_T, internal_parse_embedded_scheme, &ps,
```

I frequently use worktrees for things like performance comparisons of a feature branch to the master/main branch. When I use `rg`, I just want to search the code base (under the version I'm working on), but not get duplicated search results from other (a bit different) copies of the same source code.

---

_Comment by @BurntSushi on 2023-04-17 20:46_

Please provide a way to reproduce the behavior today that you don't want, and then please show the behavior that you do want. Someone else (i.e., me) should be able to run the same commands.

---

_Comment by @jeanas on 2023-04-17 20:59_

Here you go:

```
$ mkdir git-repo
$ cd git-repo
$ echo "abcdef" > file.txt
$ git init
[...]
$ git add file.txt
$ git commit -m "Commit"
[...]
$ git worktree add new-branch
[...]
$ git grep "abc"
file.txt:1:abcdef
$ rg "abc"
new-branch/file.txt
1:abcdef

file.txt
1:abcdef
```

Actual behavior: `rg "abc"` lists matches from `file.txt` and `new-branch/file.txt`. Desired behavior: `rg "abc"` only matches `file.txt`, like `git grep`, since `new-branch/file.txt` is from a different worktree.

---

_Comment by @BurntSushi on 2023-04-17 21:09_

Thank you, that makes it much clearer. I'm not sure automatically ignoring these is the right approach. It seems somewhat surprising to do so, especially since the worktree might have different contents than its "primary" checkout.

I think I'd rather see you use `.rgignore` to specifically and explicitly ignore these worktrees.

---

_Comment by @jeanas on 2023-04-17 21:32_

Personally, I view the worktree as a totally different mental space — that's why I'm rather surprised that ripgrep does *not* automatically ignore it.

I could as well have put that worktree outside of my main work directory for practical reasons. Do you see what I mean? It's hard to explain why something feels "intuitively" bizarre, but this is really the feeling I have. It's like files ignored by Git — when I do `rg pat`, I expect to see occurrences of `pat` in the code base I'm working on, as if I were using a graphical code search tool, but without noise from either build files or parallel versions of the code.

Also, it would match `git grep` better.

---

_Comment by @BurntSushi on 2023-11-22 01:11_

I thought about this and I think the juice isn't worth the squeeze.

Firstly, it's not totally obvious to me that the default behavior _ought_ to be to not search these sub-directories. ripgrep already searches git repos within git repos (a common setup when `$HOME` is a git checkout). I admit that reasonable people could disagree, and in theory a flag could be added for folks to opt into this.

But secondly, I think it should be pretty straight-forward to "work around" this instead of adding a flag to ripgrep. This is what things like `.rgignore` are for.

---

_Closed by @BurntSushi on 2023-11-22 01:11_

---

_Label `wontfix` added by @BurntSushi on 2023-11-22 01:11_

---
