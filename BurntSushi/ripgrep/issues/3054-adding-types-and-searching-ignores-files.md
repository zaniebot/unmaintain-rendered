```yaml
number: 3054
title: adding types and searching ignores files.
type: issue
state: closed
author: egidijus
labels:
  - invalid
assignees: []
created_at: 2025-05-28T08:54:37Z
updated_at: 2025-09-23T01:30:19Z
url: https://github.com/BurntSushi/ripgrep/issues/3054
synced_at: 2026-01-12T16:13:25Z
```

# adding types and searching ignores files.

---

_@egidijus_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)
```


### How did you install ripgrep?

yay -S riprep

### What operating system are you using ripgrep on?

endeavouros, Kernel: Linux 6.12.29-1-lts

### Describe your bug.

I am unable to configure types using "B" types.
When using "A" types, rg ignores type.


```
$ env| grep -i rip
RIPGREP_CONFIG_PATH=/home/egidijus/.ripgreprc
```

## A TYPES 

I have tried both, 
```
## add js type
--type-add=js:*.{js,ts}*

# 'web' type.
--type-add=web:*.{html,css,js}*

# 'docker' type.
--type-add=docker:*.{dockerfile,Dockerfile,docker-compose.yml}*

# justfile
--type-add=just:*.justfile
```
above ignores just files

```
$ rg --type-list | grep -i just -B2
julia: *.jl
jupyter: *.ipynb, *.jpynb
just: *.justfile
```

```
$ rg --debug direnv -tjust 
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/egidijus/.ripgreprc: arguments loaded from config file: ["--glob=!.git/", "--glob=!node_modules/", "--type-add=js:*.{js,ts}*", "--type-add=web:*.{html,css,js}*", "--type-add=docker:*.{dockerfile,Dockerfile,docker-compose.yml}*", "--type-add=just:*.justfile", "--colors=line:style:bold", "--max-depth=5", "--smart-case", "--hidden"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: endy
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:393: opened gitignore file: ./.git/info/exclude
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./justfile: Ignore(IgnoreMatch(Types(Glob(UnmatchedIgnore))))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.envrc: Ignore(IgnoreMatch(Types(Glob(UnmatchedIgnore))))
```


and ...

## B TYPES

```
--type-add
js:*.{js,ts}*
docker:*.{dockerfile,Dockerfile,docker-compose.yml}*
just:*.justfile
web:*.{html,css,js}*
```

```
rg --type-list | grep -i just -B2
```
empty output

```
$ rg --debug direnv -tjust 
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/egidijus/.ripgreprc: arguments loaded from config file: ["--glob=!.git/", "--glob=!node_modules/", "--type-add", "js:*.{js,ts}*", "docker:*.{dockerfile,Dockerfile,docker-compose.yml}*", "just:*.justfile", "web:*.{html,css,js}*", "--colors=line:style:bold", "--max-depth=5", "--smart-case", "--hidden"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 3
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: endy
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: unrecognized file type: just
```


### What are the steps to reproduce the behavior?


```
$ env| grep -i rip
RIPGREP_CONFIG_PATH=/home/egidijus/.ripgreprc
```

## A TYPES 

I have tried both, 
```
## add js type
--type-add=js:*.{js,ts}*

# 'web' type.
--type-add=web:*.{html,css,js}*

# 'docker' type.
--type-add=docker:*.{dockerfile,Dockerfile,docker-compose.yml}*

# justfile
--type-add=just:*.justfile
```
above ignores just files




## B TYPES

```
--type-add
js:*.{js,ts}*
docker:*.{dockerfile,Dockerfile,docker-compose.yml}*
just:*.justfile
web:*.{html,css,js}*
```



### What is the actual behavior?

## A TYPES


```
$ rg --type-list | grep -i just -B2
julia: *.jl
jupyter: *.ipynb, *.jpynb
just: *.justfile
```

```
$ rg --debug direnv -tjust 
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/egidijus/.ripgreprc: arguments loaded from config file: ["--glob=!.git/", "--glob=!node_modules/", "--type-add=js:*.{js,ts}*", "--type-add=web:*.{html,css,js}*", "--type-add=docker:*.{dockerfile,Dockerfile,docker-compose.yml}*", "--type-add=just:*.justfile", "--colors=line:style:bold", "--max-depth=5", "--smart-case", "--hidden"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 0
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1108: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=false, stdin_consumed=false, mode=Search(Standard))
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1118: heuristic chose to search ./
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: endy
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 1 required extensions, 0 regexes
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 8 thread(s)
rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
rg: DEBUG|ignore::gitignore|crates/ignore/src/gitignore.rs:393: opened gitignore file: ./.git/info/exclude
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./justfile: Ignore(IgnoreMatch(Types(Glob(UnmatchedIgnore))))
rg: DEBUG|ignore::walk|crates/ignore/src/walk.rs:1799: ignoring ./.envrc: Ignore(IgnoreMatch(Types(Glob(UnmatchedIgnore))))
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```


## B TYPES

```
rg --type-list | grep -i just -B2
```
empty output

```
$ rg --debug direnv -tjust 
rg: DEBUG|rg::flags::config|crates/core/flags/config.rs:41: /home/egidijus/.ripgreprc: arguments loaded from config file: ["--glob=!.git/", "--glob=!node_modules/", "--type-add", "js:*.{js,ts}*", "docker:*.{dockerfile,Dockerfile,docker-compose.yml}*", "just:*.justfile", "web:*.{html,css,js}*", "--colors=line:style:bold", "--max-depth=5", "--smart-case", "--hidden"]
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1083: number of paths given to search: 3
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1094: is_one_file? false
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1269: found hostname for hyperlink configuration: endy
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1279: hyperlink format: ""
rg: unrecognized file type: just
```


### What is the expected behavior?


both syntax of setting custom types work:

```
--type-add
js:*.{js,ts}*
docker:*.{dockerfile,Dockerfile,docker-compose.yml}*
just:*.justfile
web:*.{html,css,js}
```

and

```
# ## add js type
--type-add=js:*.{js,ts}*

# 'web' type.
--type-add=web:*.{html,css,js}*
```

search custom types

---

_Comment by @BurntSushi on 2025-05-28 12:08_

This issue is very hard for me to understand. Your "B TYPES" configuration looks wrong. I don't know why you expect it to work. ripgrep is even printing an error. 

Your "A TYPES" configuration looks okay. I think you are saying it still isn't doing what you expect, but it is very unclear here because you don't provide an MRE. Please provide an MRE.

---

_Closed by @BurntSushi on 2025-09-23 01:30_

---

_Label `invalid` added by @BurntSushi on 2025-09-23 01:30_

---
