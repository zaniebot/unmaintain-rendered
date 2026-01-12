```yaml
number: 2777
title: "Negation pattern/whitelisting is ignored from `--ignore-file`"
type: issue
state: closed
author: noirbizarre
labels:
  - doc
  - rollup
assignees: []
created_at: 2024-04-09T17:41:40Z
updated_at: 2025-10-11T02:07:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2777
synced_at: 2026-01-12T16:13:24Z
```

# Negation pattern/whitelisting is ignored from `--ignore-file`

---

_@noirbizarre_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.

### How did you install ripgrep?

Pacman

### What operating system are you using ripgrep on?

Archlinux

### Describe your bug.

Negation patterns are ignored from `--ignore-file` while documentation says it should take precedence

### What are the steps to reproduce the behavior?

- Create an empty repository (`mkdir case && cd case && git init`)
- create an hidden file: `touch .hidden`
- add `.hidden` to the `.gitignore` file
- create a `search.ignore` file whitelisting the `.hidden` file: `!.hidden`
- run `rg --files --hidden --ignore-file=search.ignore`


### What is the actual behavior?

`.hidden` file is not whitelisted and is missing from result while it should be there.

Here the actual output with `rg --files --hidden --ignore-file=search.ignore --debug`:
```console
rg: DEBUG|rg::flags::parse|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Files)
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: archxps
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:448: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 4 literals, 6 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ignore-0.4.22/src/walk.rs:1799: ignoring ./.hidden: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: ".hidden", actual: "**/.hidden", is_whitelist: false, is_only_dir: false })))
search.ignore
.git/HEAD
.git/config
.git/description
.git/info/exclude
.git/hooks/update.sample
.git/hooks/sendemail-validate.sample
.git/hooks/push-to-checkout.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/pre-receive.sample
.git/hooks/pre-rebase.sample
.git/hooks/pre-push.sample
.git/hooks/pre-merge-commit.sample
.git/hooks/pre-commit.sample
.git/hooks/pre-applypatch.sample
.git/hooks/post-update.sample
.git/hooks/fsmonitor-watchman.sample
.git/hooks/commit-msg.sample
.git/hooks/applypatch-msg.sample
.gitignore
```

### What is the expected behavior?

Ignore provided with `--ignore-file` should take precedence and whitelist previously ignored patterns.
Here's the expected output:

```console
$ rg --files --hidden --ignore-file=search.ignore
search.ignore
.gitignore
.git/config
.git/description
.git/HEAD
.git/info/exclude
.git/hooks/pre-merge-commit.sample
.git/hooks/pre-commit.sample
.git/hooks/pre-applypatch.sample
.git/hooks/post-update.sample
.git/hooks/fsmonitor-watchman.sample
.git/hooks/commit-msg.sample
.git/hooks/applypatch-msg.sample
.git/hooks/update.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/sendemail-validate.sample
.git/hooks/pre-receive.sample
.git/hooks/push-to-checkout.sample
.git/hooks/pre-rebase.sample
.git/hooks/pre-push.sample
.hidden
```

### Note

Adding the same negation pattern to the `.ignore` file works as expected and whitelist `.hidden`:

```console
rg: DEBUG|rg::flags::parse|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Files)
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1109: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: archxps
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:1270: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ripgrep-14.1.0/crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:448: glob converted to regex: Glob { glob: ".vscode/*", re: "(?-u)^\\.vscode/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true, empty_alternates: false }, tokens: Tokens([Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), Literal('/'), ZeroOrMore]) }
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 4 literals, 6 basenames, 1 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|globset|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/globset-0.4.14/src/lib.rs:453: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::walk|/home/noirbizarre/.cargo/registry/src/index.crates.io-6f17d22bba15001f/ignore-0.4.22/src/walk.rs:1802: whitelisting ./.hidden: Whitelist(IgnoreMatch(Gitignore(Glob { from: Some("./.ignore"), original: "!.hidden", actual: "**/.hidden", is_whitelist: true, is_only_dir: false })))
.ignore
search.ignore
.git/HEAD
.git/config
.git/description
.git/info/exclude
.git/hooks/update.sample
.git/hooks/sendemail-validate.sample
.git/hooks/push-to-checkout.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/pre-receive.sample
.git/hooks/pre-rebase.sample
.git/hooks/pre-push.sample
.git/hooks/pre-merge-commit.sample
.git/hooks/pre-commit.sample
.git/hooks/pre-applypatch.sample
.git/hooks/post-update.sample
.git/hooks/fsmonitor-watchman.sample
.git/hooks/commit-msg.sample
.git/hooks/applypatch-msg.sample
.gitignore
.hidden
```
Given standard ignore patterns are properly handled from `--ignore-file`, it means that only `--ignore-file` negation patterns are broken

---

_Comment by @BurntSushi on 2024-04-09 18:02_

This is correct behavior. From the man page:

```
       The precedence of ignore rules is as follows, with later items overriding earlier items:

       •  Files given by --ignore-file.

       •  Global gitignore rules, e.g., from $HOME/.config/git/ignore.

       •  Local rules from .git/info/exclude.

       •  Rules from .gitignore.

       •  Rules from .ignore.

       •  Rules from .rgignore.
```

In other words, `--ignore-file` has the lowest precedence and is overruled by your `.gitignore`. The whitelist rule in `.ignore` works because `.ignore` specifically has higher precedence.

The docs for `--ignore-file` appear misleading or perhaps even wrong:

```
       --ignore-file=PATH
           Specifies a path to one or more gitignore formatted rules files.  These patterns are applied after the  patterns  found  in
           .gitignore,  .rgignore  and  .ignore  are applied and are matched relative to the current working directory. Multiple addi‐
           tional ignore files can be specified by using this flag repeatedly. When specifying multiple ignore  files,  earlier  files
           have lower precedence than later files.

           If  you  are looking for a way to include or exclude files and directories directly on the command line, then use -g/--glob
           instead.
```

This should be rephrased to say that they have lower precedence than rules from other ignore files.

---

_Label `doc` added by @BurntSushi on 2024-04-09 18:02_

---

_Comment by @noirbizarre on 2024-04-09 18:13_

OK, make sense.

Is there a way to provide a global override file so ?

Main use case: I have some personal files which I ignore globally using a global git ignore (let's say `.envrc` or `.mise.toml`) that I know I always want to find in search results (this is why I have `!.mise.toml` and `!.envrc` in my `~/.config/search.ignore`)

Ideally, to make everyone happy, there should be both:
- a `--ignore-file-override` parameter or equivalent to be able to specify on-demand
- a `$RG_IGNORE_FILE_OVERRIDE` environment variable to be able to specify once globally or by environment

If there is no such possibility, would you accept a pull-request adding it (it might take time, I'm no rust expert, still learning) ?

---

_Comment by @BurntSushi on 2024-04-09 18:31_

Sure, you could create a `/.rgignore` since any `.rgignore` file has precedence over all other `.gitignore` files, for example. ripgrep will ascend parent directories to find ignore files.

Otherwise, no, I'm not accepting any major changes to how ignore rules work in ripgrep right now. Not until the system has been re-thought and probably redesigned.

---

_Comment by @noirbizarre on 2024-04-09 18:54_

Too bad, one `.ignore` or `.rgignore` by repository doesn't really scale because I have a huge amount of repositories to handle (I started with that and then submitted the issue when I realized the amount of file I have to create, a lifetime of projects history, each one having a combination of multiple files or directories I gitignored but need to match using rg).

I'll wait for the redesign. Thanks for the quick and precise answers !

---

_Comment by @BurntSushi on 2024-04-09 19:02_

You don't need to put a `.rgignore` in each repository. I'm not sure where you got that from. You only need to put it in a shared parent directory. Hence the suggestion to use `/.rgignore`. That's `/`, the root of your _file system_. Not the root of each project directory. Of course, it doesn't have to be in the root of your file system. If all your repos are in `~/clones`, then you could put it at `~/clones/.rgignore`.

---

_Comment by @noirbizarre on 2024-04-09 19:41_

> You only need to put it in a shared parent directory. 

Oh, that will do it !! Thanks for the tip, I wasn't aware that parent files are processed ! 🙏🏼 
**Edit:** working just fine, solved my issue !

> I'm not sure where you got that from.

Hard to answer. This has been a long journey (not) finding this right info:

The `rg --help` section about `--ignore-file` only talks about the current working directory for those files. And this is also the one that made me think there was a bug given it says clearly `These patterns are applied after the patterns found in .gitignore...`

```
--ignore-file=PATH
        Specifies a path to one or more gitignore formatted rules files. These
        patterns are applied after the patterns found in .gitignore, .rgignore
        and .ignore are applied and are matched relative to the current working
        directory. Multiple additional ignore files can be specified by using
        this flag repeatedly. When specifying multiple ignore files, earlier
        files have lower precedence than later files.
```

Then I found the [User Guide dedicated section](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering) referenced in the README, but it only talks about git specific ignore files (including only globals from `core.excludesFile` and current dir `.ignore` that override, no reference to `--ignore-file` nor to parent files).

So I read comments from all the PRs on the topics trying to find data and one was saying that only the current repository `.(rg|git|)ignore` files are handled (and that `rg` is converging toward using `.ignore`) as well as the "global ignore files" but without details on what it means.

I wasn't even aware that there is a `man` page, it's not said once in the README, the User guide or the `--help` (or more precisely I missed it if this is the case).

So clearly a documentation issue and I might submit PR clarifying that between the readme, the help, the user guide and the manpage. Would you accept such pull request ?

---

_Comment by @BurntSushi on 2024-04-09 21:09_

> Hard to answer. This has been a long journey (not) finding this right info

Ah yeah, sorry, I was referencing [my prior comment](https://github.com/BurntSushi/ripgrep/issues/2777#issuecomment-2045840094) where I suggested `/.rgignore`. :-)

Otherwise I agree that the whole UX around ignore rules is sub-optimal. There are also legitimate bugs with its implementation. So it's all a bit of a mess unfortunately, but _mostly_ works.

---

_Comment by @BurntSushi on 2024-04-09 21:09_

> So clearly a documentation issue and I might submit PR clarifying that between the readme, the help, the user guide and the manpage. Would you accept such pull request ?

Yes I'd be happy to try and work with you on that. Thank you!

---

_Label `rollup` added by @BurntSushi on 2025-10-11 01:45_

---

_Closed by @BurntSushi on 2025-10-11 02:07_

---
