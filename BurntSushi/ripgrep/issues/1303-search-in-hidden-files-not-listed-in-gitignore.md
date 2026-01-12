```yaml
number: 1303
title: search in hidden files not listed in .gitignore
type: issue
state: closed
author: minusf
labels:
  - invalid
assignees: []
created_at: 2019-06-17T11:13:54Z
updated_at: 2020-09-18T20:01:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1303
synced_at: 2026-01-12T16:13:23Z
```

# search in hidden files not listed in .gitignore

---

_@minusf_

#### What version of ripgrep are you using?

```
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew`

#### What operating system are you using ripgrep on?

macOS mojave 10.14.5

#### Describe your question, feature request, or bug.

I have an issue with `--hidden` behaving like `-uu`...

First of all, why have 2 ways to do the same...

Second, there is no easy way to honour `.gitignore` but still search in hidden files...

In my reading of the manual:

```
DESCRIPTION
       ripgrep (rg) recursively searches your current directory for
       a regex pattern. By default, ripgrep will respect your .gitignore
       and automatically skip hidden files/directories and binary files.
```
and later:
```
 --hidden
           Search hidden files and directories. By default, hidden files and
           directories are skipped. Note that if a hidden file or a directory
           is whitelisted in an ignore file, then it will be searched even if
           this flag isn't provided.

           This flag can be disabled with --no-hidden.
```

So by default: .gitignore respected, hidden files skipped.

Just by adding a hidden flag, it surprises me that the other default behaviour: respecting .gitignore gets turned off as well.  This is not reflected in the documentation either, and i think it's reasonable to expect that other defaults remain as they are.

So how do I search in hidden files tracked byt git?  `--hidden --ignore` also does not work, but as respecting .gitignore is default, i would find that confusing as well...

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ ls -a1
.git/
.gitignore
.some_config_file

$ rg --stats needle          

0 matches
0 matched lines
0 files contained matches
1 files searched
0 bytes printed
381 bytes searched
0.000026 seconds spent searching
0.004888 seconds

$ rg --hidden --stats needle
.some_config_file
1:needle

1 matches
1 matched lines
1 files contained matches
18 files searched       <---- this should be "2" (.gitignore + .some_config_file)
27 bytes printed
19689 bytes searched
0.000216 seconds spent searching
0.004950 seconds
```


---

_Comment by @BurntSushi on 2019-06-17 11:23_

This is intended behavior. `--hidden` is orthogonal from gitignore. When you specify `--hidden`, you're explicitly disabling ripgrep's filter on hidden files. Therefore, it searches your `.git` directory. Your `.git` directory is presumably not in any ignore file (but since you didn't provide the contents of your ignore files, I cannot say for sure), so ripgrep searches it, _because you told it to._

If you only want to search _some_ hidden files, then, for example, add `!.some_config_file` to an `.ignore` file. As the docs you quoted say, whitelisted hidden files will be searched even when `--hidden` isn't used.

> First of all, why have 2 ways to do the same...

`-uu` is not the same as `--hidden`. `-uu` is the same as `--no-ignore --hidden`. The example you've given isn't sufficient to discriminate them, so it only _looks_ the same to you.

---

_Closed by @BurntSushi on 2019-06-17 11:23_

---

_Label `invalid` added by @BurntSushi on 2019-06-17 11:23_

---

_Comment by @minusf on 2019-06-17 12:34_

i understand where you are coming from, but `.git` is special right...

`rg` or `rg --ignore` will ignore `.git`, whether it's in `.gitignore` or not.  it's implied. 

but `rg --ignore --hidden` will not.  it breaks this implicit expectation: to make `rg --hidden` skip `.git` requires to have `.git` in `.gitignore`, and that is quite uncommon afaik.

this assymetry is what is confusing me.

as rg is by default ".git conscious", i am making the argument that `--hidden` should be too, and i should have to jump through the same hoops (`--no-ignore`) when i want to "search in hidden files regardless of ignores" as when i am searching in "non-hidden files regardless of ignores"...  it just seems more consistent to me.

if you still think this is not a bug, let me just close with this:

> For example, the --no-ignore flag (listed below) disables ripgrep's gitignore logic, but the --ignore flag (not listed below) enables it.

```
$ cat .gitignore
$ rg --hidden --no-ignore needle
.some_config_file
1:needle

.git/needle
1:needle
$ rg --hidden --ignore needle   
.some_config_file
1:needle

.git/needle
1:needle
```

(if by "gitignore logic" you also mean "ignore .git implicitly")

or

all i am asking is how can i search also in hidden files tracked by git without putting `.git` in `.gitignore` ?




---

_Comment by @BurntSushi on 2019-06-17 12:40_

> i understand where you are coming from, but `.git` is special right...
>
> `rg` or `rg --ignore` will ignore `.git`, whether it's in `.gitignore` or not.  it's implied. 

ripgrep does not treat `.git` as special. It treats it as a hidden directory, and whether ripgrep searches or not follows from whether it is told to search hidden files (absent any other globs or ignore rules).

> i am making the argument that --hidden should be too

I know. And I disagree for reasons already stated.

> all i am asking is how can i search also in hidden files tracked by git without putting .git in .gitignore ?

I already told you in my first comment. Put `!.some_config_file` into a `.ignore` file that lives right next to `.gitignore`.

Or put `.git` in your `.gitignore`. Or a `.ignore`. Or whatever.

---

_Comment by @minusf on 2019-06-17 12:51_

> ripgrep does not treat .git as special. It treats it as a hidden directory, and whether ripgrep searches or not follows from whether it is told to search hidden files (absent any other globs or ignore rules).

thanks, this was the missing piece of the puzzle for me.  now `--hidden` makes much more sense.

(unfortunately i am not a fan of "dont-ignore" rules in ignore files ¯\_(ツ)_/¯ )

---

_Comment by @raven-rock on 2020-09-18 20:01_

I had a similar use case @minusf, i.e. search hidden dot files yet ignore the `.git` directory and anything else defined in the supported ignore files. I solved it by combining the `--hidden` and `--glob` flags: `rg --hidden --glob "!**/.git/**" [ARGS....]`. For convenience, I set an alias. You can see/reproduce the behavior with:

```bash
$ cd $(mktemp -d)
$ git init --quiet
$ touch normal_file .hidden_file_i_want_searched
$ rg --files
normal_file
$ rg --files --hidden
normal_file
.git/hooks/update.sample
.git/hooks/pre-push.sample
.git/hooks/pre-applypatch.sample
.git/hooks/pre-merge-commit.sample
.git/hooks/post-update.sample
.git/hooks/prepare-commit-msg.sample
.git/hooks/pre-receive.sample
.git/hooks/fsmonitor-watchman.sample
.git/hooks/applypatch-msg.sample
.git/hooks/pre-commit.sample
.git/hooks/pre-rebase.sample
.git/hooks/commit-msg.sample
.git/description
.git/info/exclude
.git/HEAD
.hidden_file_i_want_searched
.git/config
$ rg --files --hidden --glob '!**/.git/**'
normal_file
.hidden_file_i_want_searched
$ alias myrg='rg --hidden --glob "!**/.git/**"'
$ myrg --files
normal_file
.hidden_file_i_want_searched
$ touch .hidden_file_i_want_ignored
$ echo .hidden_file_i_want_ignored > .gitignore
$ rg --files --hidden --glob '!**/.git/**'
normal_file
.hidden_file_i_want_searched
.gitignore
```

---
