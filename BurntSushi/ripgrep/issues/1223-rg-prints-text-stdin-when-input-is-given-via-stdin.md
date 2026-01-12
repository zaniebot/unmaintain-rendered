```yaml
number: 1223
title: "rg prints text \"<stdin>\" when input is given via stdin"
type: issue
state: closed
author: arunvelsriram
labels:
  - invalid
  - question
assignees: []
created_at: 2019-03-18T13:48:22Z
updated_at: 2019-08-01T21:46:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1223
synced_at: 2026-01-12T16:13:23Z
```

# rg prints text "<stdin>" when input is given via stdin

---

_@arunvelsriram_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install rg`

#### What operating system are you using ripgrep on?

`OS X High Sierra`

#### Describe your question, feature request, or bug.

When I do `rg` over content passed through `stdin`, the output contains the text "&lt;stdin&gt;" along with the matches. It started to happen recently after I updated some of my `bash` configs and those are not related to `rg`. Does anyone know why this is happening?

```
$ ls | rg test
<stdin>
test.json
test.txt
test.yaml
```



---

_Comment by @BurntSushi on 2019-03-18 13:51_

Please follow the issue template and include the `--debug` output. Please also include `--trace` output.

---

_Comment by @arunvelsriram on 2019-03-18 14:01_

Sorry missed that.

```
$ ls | rg --debug test
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
<stdin>
test.json
test.txt
test.yaml
```

```
$ ls | rg --trace test
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE|grep_searcher::searcher|grep-searcher/src/searcher/mod.rs:685: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|grep-searcher/src/searcher/core.rs:61: searcher core: will use fast line searcher
<stdin>
test.json
test.txt
test.yaml
```

---

_Comment by @BurntSushi on 2019-03-18 14:28_

My _guess_ is that this is a user error somewhere. I cannot reproduce it. I was hoping the debug output would show the argv, but it looks like that isn't quite working as I'd like. Do you have an `rg` alias somewhere? Run `alias rg` or `command -V rg` to see. For example, this is how things could be messed up:

```
$ alias rg="rg --with-filename"
$ ls | rg test
<stdin>
test.py
```

More concretely, try

```
$ ls | \rg test
test.py
```

to force that you aren't using an alias.

---

_Comment by @arunvelsriram on 2019-03-18 17:04_

It's not aliased as far as I checked.

```
$ alias rg
-bash: alias: rg: not found
```

```
$ command -V rg
rg is /usr/local/bin/rg
```

```
ls | \rg test
<stdin>
test.json
test.txt
test.yaml
```

---

_Comment by @BurntSushi on 2019-03-18 17:09_

Then I don't know. You'll likely need to debug this yourself since I cannot reproduce it and I can't quite figure out how this could be happening. The first step here would be to just compile master and see if the bug has been fixed already. If not, you'll want to debug this code:

https://github.com/BurntSushi/ripgrep/blob/09139721047b1cda6ad88dbf89dc5fa74c66a3a2/src/args.rs#L1471-L1482

Namely, it seems like that's returning true for you, but I don't know why.

---

_Comment by @arunvelsriram on 2019-03-18 19:12_

Thanks for the input.

I noticed one more thing. Its happening only in my home directory. It feels like a user error to me as well. I will debug this further and update here.

---

_Comment by @BurntSushi on 2019-04-08 12:29_

I'm going to close this out since I can't reproduce it. Please feel free to comment again if you have more details!

---

_Closed by @BurntSushi on 2019-04-08 12:29_

---

_Label `invalid` added by @BurntSushi on 2019-04-08 12:29_

---

_Label `question` added by @BurntSushi on 2019-04-08 12:30_

---

_Comment by @iiey on 2019-05-28 13:04_

please reopen this issue as a bug because I have the same problem:

 ```
16.04.1-Ubuntu

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

```
$ ls | rg --trace a
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(A), Complete(a)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/build*", re: "(?-u)^(?:/?|.*/)build.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 1 literals, 3 basenames, 6 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 1 regexes
TRACE|grep_searcher::searcher|grep-searcher/src/searcher/mod.rs:685: generic reader: searching via roll buffer strategy
TRACE|grep_searcher::searcher::core|grep-searcher/src/searcher/core.rs:61: searcher core: will use fast line searcher
<stdin>:a
```

```
$ ls | rg --debug a
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(A), Complete(a)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:424: glob converted to regex: Glob { glob: "**/build*", re: "(?-u)^(?:/?|.*/)build.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('b'), Literal('u'), Literal('i'), Literal('l'), Literal('d'), ZeroOrMore]) }
DEBUG|globset|globset/src/lib.rs:429: built glob set; 1 literals, 3 basenames, 6 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 1 regexes
<stdin>:a
```

**Step to reproduce it** :
1. Create a folder with some subdirs:
```
mkdir a b c
ls | rg a
a
```
2. Create a folder with minus symbol "-"
```
mkdir -
ls | rg a
<stdin>:a
```

_Edit_: it yields same problem when I'm under folder contained '-' directory regardless what we grep:
```
$ cat text.txt | rg abc
<stdin>:abc def
<stdin>:def abc
```

---

_Comment by @BurntSushi on 2019-05-28 13:10_

@arunvelsriram Could you confirm whether this is the same underlying issue? i.e., Does the output of `ls` include a `-` file/directory somewhere?

---

_Comment by @arunvelsriram on 2019-05-28 18:44_

@BurntSushi Actually, I changed my machine recently and didn't face the issue after that. However, I tried the steps provided by @iiey and am able to replicate the issue.

---

_Comment by @arunvelsriram on 2019-05-28 18:45_

Sorry, am not sure if my old machine listed a `-` on `ls`.

---

_Comment by @jpninanjohn on 2019-05-31 11:13_


I had a look into the suggested part of the code and tried to debug this. 

https://github.com/BurntSushi/ripgrep/blob/09139721047b1cda6ad88dbf89dc5fa74c66a3a2/src/args.rs#L1188-L1204

When paths are empty, a default path is given which is what is got by `paths.get(0)`. This default path is also `-`.

Most cases when there is no directory with the name "-",  `paths.get(0).map_or(false, |p| p.is_dir())` will return `false`, in the case there is such a directory called "-", it will return `true`.  

> Then I don't know. You'll likely need to debug this yourself since I cannot reproduce it and I can't quite figure out how this could be happening. The first step here would be to just compile maste
r and see if the bug has been fixed already. If not, you'll want to debug this code:
> 
> https://github.com/BurntSushi/ripgrep/blob/09139721047b1cda6ad88dbf89dc5fa74c66a3a2/src/args.rs#L1471-L1482
> 
> Namely, it seems like that's returning true for you, but I don't know why.


The solution I am thinking is to skip the check if its a directory, if the path is the default one. I'm not sure if this is correct though. 

---

_Comment by @jpninanjohn on 2019-06-03 08:50_

@BurntSushi 

---

_Comment by @BurntSushi on 2019-06-03 09:19_

I don't need to be pinged. Either someone can investigate and fix this issue, or I'll do it myself when I get a chance. This is not a high priority bug.

---

_Reopened by @BurntSushi on 2019-08-01 21:26_

---

_Closed by @BurntSushi on 2019-08-01 21:46_

---
