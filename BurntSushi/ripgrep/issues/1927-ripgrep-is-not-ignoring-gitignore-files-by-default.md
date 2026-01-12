```yaml
number: 1927
title: "ripgrep is not ignoring .gitignore files by default!"
type: issue
state: closed
author: ronyAhmed1200
labels:
  - invalid
assignees: []
created_at: 2021-07-06T08:47:33Z
updated_at: 2021-07-06T18:31:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1927
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep is not ignoring .gitignore files by default!

---

_@ronyAhmed1200_

#### What version of ripgrep are you using?

`ripgrep 11.0.2`
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

i used apt to install it. 

#### What operating system are you using ripgrep on?

It is linux mint 20.1 which is ubuntu derivative. 

#### Describe your bug.

I have installed `ripgrep` in ubuntu 20.04. Basically i want to use it to find my code quickly in vim. But when i run `:rg` in vim, it is searching between all the 300000 files. I think it is listing all the `.gitignore` files.. So it turns out really difficult to search in **300000** files.  

![image](https://user-images.githubusercontent.com/75775786/124569133-7f14cf00-de67-11eb-9524-c5c9d1da57f9.png)

This image when i tried to search source with ripgrep:
![image](https://user-images.githubusercontent.com/75775786/124569498-d155f000-de67-11eb-9442-ed50bd55afbe.png)


#### What is the expected behavior?

If should not include .gitignore files by default. 


---

_Comment by @BurntSushi on 2021-07-06 08:57_

The version of ripgrep you're using is almost two years old. Please upgrade.

Otherwise, this ticket is not actionable. I can't debug your vim setup. Please provide a CLI reproduction.

---

_Comment by @ronyAhmed1200 on 2021-07-06 09:29_

Well, I have upgraded it..now the version is: 
`ripgrep 12.1.1 (rev 7cb211378a)`
 But still getting the same behavior!! 

And how to provide a cli reproduction.. you can see it in the image.. 

---

_Comment by @ronyAhmed1200 on 2021-07-06 10:50_

now after upgrading it is showing me that i need to search across all 9870000+ files !! What ?? 

![image](https://user-images.githubusercontent.com/75775786/124588067-3581af80-de7a-11eb-88aa-24c0e4d691a9.png)


---

_Comment by @BurntSushi on 2021-07-06 10:59_

If you can't provide an actual `rg` command to run, then this issue isn't actionable.

---

_Closed by @BurntSushi on 2021-07-06 10:59_

---

_Label `invalid` added by @BurntSushi on 2021-07-06 10:59_

---

_Comment by @ronyAhmed1200 on 2021-07-06 12:46_

I have to run it with `:Rg` command in vim. But I am sure the issue is not with `:Rg` or `rg` command, but it is actually **a bug or issue** of the app. How am I sure?

Well, I have run with the `rg `command in my terminal. From there you can see that it is showing me the word i searched in `nodemodules` folder which is actually ignored by `.gitignore` file.. 

![image](https://user-images.githubusercontent.com/75775786/124602230-75e92980-de8a-11eb-85e7-43039b366969.png)

And another thing is depressing. Why did you close a issue or bug before solving out!! 

---

_Comment by @BurntSushi on 2021-07-06 12:55_

I can't keep repeating myself. I can only say, "i need a reproduction" in so many ways. An image isn't good enough. **Fill out the issue template.** Give me a way to reproduce what you're seeing.

This is the last response I'll give to you unless I see a reproduction.

---

_Comment by @ronyAhmed1200 on 2021-07-06 17:38_

**reproduction**

https://gist.github.com/ronyAhmed1200/f317f019e67ec64fbebfd4636954e3f2

---

_Comment by @BurntSushi on 2021-07-06 17:42_

@ronyAhmed1200 That's not a reproduction. **I need to be able to reproduce the problem.** Which git repo should I clone to run that search?

And you didn't include the full output as far as I can tell.

---

_Comment by @ronyAhmed1200 on 2021-07-06 17:59_

This result is produced with [This repo](https://github.com/ronyAhmed1200/Robofriends)

And my terminal only produced that result.. Well, u can test it with this repo.. 

---

_Comment by @BurntSushi on 2021-07-06 18:14_

No. Your output is very clearly truncated.

Running with `-l` to just print the files matched, this is what I get:

```
$ ls -l
total 1468
drwxrwxr-x 3 andrew users    4096 Jul  6 14:02 build
drwxrwxr-x 2 andrew users    4096 Jul  6 14:02 public
drwxrwxr-x 4 andrew users    4096 Jul  6 14:02 src
-rw-rw-r-- 1 andrew users    1023 Jul  6 14:02 package.json
-rw-rw-r-- 1 andrew users 1480930 Jul  6 14:02 package-lock.json
-rw-rw-r-- 1 andrew users      75 Jul  6 14:02 README.md

$ rg -l data
package-lock.json
build/static/js/main.54f0344c.chunk.js.map
src/Containers/App.js
build/static/js/runtime-main.7222f5d4.js.map
build/static/css/2.c1f4f5f4.chunk.css
public/index.html
build/static/css/2.c1f4f5f4.chunk.css.map
build/static/js/2.db7782eb.chunk.js
build/static/js/2.db7782eb.chunk.js.map
```

As far as I can tell, none of those files are in your `.gitignore`. But your `ls` output is different than mine. Yours has a `node_modules` directory. Mine does not. I guess I'm supposed to do something to generate that? I ran `yarn install` and got this:

```
$ ls -l
total 2008
drwxrwxr-x    3 andrew users    4096 Jul  6 14:02 build
drwxrwxr-x 1040 andrew users   36864 Jul  6 14:08 node_modules
drwxrwxr-x    2 andrew users    4096 Jul  6 14:02 public
drwxrwxr-x    4 andrew users    4096 Jul  6 14:02 src
-rw-rw-r--    1 andrew users    1023 Jul  6 14:02 package.json
-rw-rw-r--    1 andrew users 1480930 Jul  6 14:02 package-lock.json
-rw-rw-r--    1 andrew users      75 Jul  6 14:02 README.md
-rw-rw-r--    1 andrew users  512064 Jul  6 14:08 yarn.lock
```

OK, re-running ripgrep:

```
$ rg -l data
yarn.lock
package-lock.json
src/Containers/App.js
public/index.html
build/static/js/main.54f0344c.chunk.js.map
build/static/js/runtime-main.7222f5d4.js.map
build/static/css/2.c1f4f5f4.chunk.css
build/static/js/2.db7782eb.chunk.js
build/static/css/2.c1f4f5f4.chunk.css.map
build/static/js/2.db7782eb.chunk.js.map
```

The only additional file is `yarn.lock` which is also not excluded by your `.gitignore` file, so this also looks correct.

I see no results from `node_modules`.

---

_Comment by @BurntSushi on 2021-07-06 18:16_

And running `rg -l data --debug` has this line:

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "node_modules/", actual: "**/node_modules", is_whitelist: false, is_only_dir: true })))
```

So if you're seeing something different, then you either have some other `.gitignore` or `.ignore` file somewhere, or you have a ripgrep config file. Either way, `--debug` output will tell you. **Like the issue template you only partially filled out said.**

---

_Comment by @ronyAhmed1200 on 2021-07-06 18:29_

Thanks for assisting me. May be I have missed something !

So now after runing `rg -l data --debug`, I see this.. 

```
DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(data)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/npm-debug.log*", re: "(?-u)^(?:/?|.*/)npm\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('n'), Literal('p'), Literal('m'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/yarn-debug.log*", re: "(?-u)^(?:/?|.*/)yarn\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('y'), Literal('a'), Literal('r'), Literal('n'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/yarn-error.log*", re: "(?-u)^(?:/?|.*/)yarn\\-error\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('y'), Literal('a'), Literal('r'), Literal('n'), Literal('-'), Literal('e'), Literal('r'), Literal('r'), Literal('o'), Literal('r'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/lerna-debug.log*", re: "(?-u)^(?:/?|.*/)lerna\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('l'), Literal('e'), Literal('r'), Literal('n'), Literal('a'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 2 literals, 29 basenames, 6 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 4 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "node_modules/", actual: "**/node_modules", is_whitelist: false, is_only_dir: true })))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; package-lock.json
0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
public/index.html
build/static/js/main.54f0344c.chunk.js.map
build/static/js/runtime-main.7222f5d4.js.map
src/Containers/App.js
build/static/js/2.db7782eb.chunk.js
build/static/css/2.c1f4f5f4.chunk.css.map
build/static/css/2.c1f4f5f4.chunk.css
build/static/js/2.db7782eb.chunk.js.map

```

Which is clearly not same as yours. Interesting, which ripgrep version produced that result in your test? bcz i think the problem is with the version maybe.. 

---

_Comment by @BurntSushi on 2021-07-06 18:31_

Your debug output says it is ignoring node_modules.

I'm using the latest version of ripgrep: 13.0.0.

On Tue, Jul 6, 2021, 14:29 Rony Ahmed ***@***.***> wrote:

> Thanks for assisting me. May be I have missed something !
>
> So now after runing rg -l data --debug, I see this..
>
> DEBUG|grep_regex::literal|crates/regex/src/literal.rs:58: literal prefixes detected: Literals { lits: [Complete(data)], limit_size: 250, limit_class: 10 }
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/npm-debug.log*", re: "(?-u)^(?:/?|.*/)npm\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('n'), Literal('p'), Literal('m'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
> DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/yarn-debug.log*", re: "(?-u)^(?:/?|.*/)yarn\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('y'), Literal('a'), Literal('r'), Literal('n'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
> DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/yarn-error.log*", re: "(?-u)^(?:/?|.*/)yarn\\-error\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('y'), Literal('a'), Literal('r'), Literal('n'), Literal('-'), Literal('e'), Literal('r'), Literal('r'), Literal('o'), Literal('r'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
> DEBUG|globset|crates/globset/src/lib.rs:426: glob converted to regex: Glob { glob: "**/lerna-debug.log*", re: "(?-u)^(?:/?|.*/)lerna\\-debug\\.log[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, Literal('l'), Literal('e'), Literal('r'), Literal('n'), Literal('a'), Literal('-'), Literal('d'), Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('.'), Literal('l'), Literal('o'), Literal('g'), ZeroOrMore]) }
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 2 literals, 29 basenames, 6 extensions, 0 prefixes, 0 suffixes, 2 required extensions, 4 regexes
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./node_modules: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "node_modules/", actual: "**/node_modules", is_whitelist: false, is_only_dir: true })))
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
> DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
> DEBUG|globset|crates/globset/src/lib.rs:431: built glob set; package-lock.json
> 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
> public/index.html
> build/static/js/main.54f0344c.chunk.js.map
> build/static/js/runtime-main.7222f5d4.js.map
> src/Containers/App.js
> build/static/js/2.db7782eb.chunk.js
> build/static/css/2.c1f4f5f4.chunk.css.map
> build/static/css/2.c1f4f5f4.chunk.css
> build/static/js/2.db7782eb.chunk.js.map
>
>
> Which is clearly not same as yours. Interesting, which ripgrep version
> produced that result in your test?
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1927#issuecomment-874987998>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADPPYX3UG7AGDDGWF47O23TWNDRVANCNFSM474DMAHQ>
> .
>


---
