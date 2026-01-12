```yaml
number: 969
title: Configure ripgrep to search in a certain gitignored directory 
type: issue
state: closed
author: hiqsol
labels:
  - question
assignees: []
created_at: 2018-06-26T14:26:59Z
updated_at: 2019-08-05T15:44:16Z
url: https://github.com/BurntSushi/ripgrep/issues/969
synced_at: 2026-01-12T16:13:22Z
```

# Configure ripgrep to search in a certain gitignored directory 

---

_@hiqsol_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### How did you install ripgrep?

Ubuntu deb package.

#### What operating system are you using ripgrep on?

Ubuntu 16.04.3

#### Describe your question, feature request, or bug.

I have `vendor` directory ignored in .gitignore.
I want ripgrep to search in `./vendor` directory in all of my projects but not in vendor directories deeper in the tree.
I've found a working solution with `.ignore` file with single line `!/vendor`.
But I would prefer to have it configured globally (e.g. in `.ripgreprc`) and not per project basis.

I tried `-g vendor` option and `--ignore-file ~/myglobal.ignore` with no success.

Could you please give me idea how to achieve such behavior.

---

_Comment by @BurntSushi on 2018-06-26 14:47_

`--ignore-file` cannot work because it's specifically designed to have low precedence. It takes effect *after* "project specific" files are applied. That is, when there are two competing ignore rules, the one that is more local to the project wins. `--ignore-file` is indeed a "global" sets of rules, so it is always overridden.

Using `-g` unfortunately isn't going to get you anywhere either. Namely, `-g vendor` specifies a whitelist, which implies everything you search must match the whitelist. If you look at the `--debug` output of ripgrep, you'll see that with `-g vendor`, your `vendor` directory will get whitelisted, but since the files inside that directory (and any other files) don't match `-g vendor`, they are excluded from search.

You could use `-g vendor -g 'vendor/**'`, but that will _only_ search the files in your `vendor` directory, which I'm assuming doesn't achieve what you want (and would be done more simply as `rg pattern vendor`).

So the only solution available to you is a project specific `.ignore` file. Moreover, I argue that this is in fact the only suitable solution in general. Even if ripgrep provided a different kind of global ignore file that overruled project specific settings, it wouldn't be able to enforce your rule in general because your rule is specifically expressed *relative* to some other directory (whether it's your CWD or a project root or whatever). Therefore, a `.ignore` (or `.rgignore`) is the right answer.

---

_Closed by @BurntSushi on 2018-06-26 14:47_

---

_Label `question` added by @BurntSushi on 2018-06-26 14:47_

---

_Comment by @BurntSushi on 2018-06-26 14:50_

Here is a somewhat idiosyncratic combination of glob flags that might strictly satisfy your question, but almost certainly screws something else up (like not respecting the rest of your `.gitignore`):

```
$ rg -g '*' -g '!.*' -g '!*/**/vendor' <pattern>
```

---

_Comment by @hiqsol on 2018-06-26 15:09_

Many thanks for such detailed explanation!
I'll stick to `.ignore`.

---

_Comment by @fcying on 2018-07-04 06:14_

you can use `--no-ignore`  and set your own exclude in ripgreprc.

---

_Comment by @BurntSushi on 2018-07-04 11:32_

@fcying That doesn't make any sense to me. Could you please elaborate?

---

_Comment by @elquimista on 2019-07-29 11:41_

@BurntSushi @fcying's comment worked perfectly for my case. The folder `node_modules` is usually gitignored, but I sometimes need to search inside installed module folder when I'm deeply involved in debugging.

I tried the following and didn't work:
```
rg -g 'node_modules/<package_name>/**/*.js' '<pattern>'
```

And adding `--no-ignore` did the magic:
```
rg --no-ignore -g 'node_modules/<package_name>/**/*.js' '<pattern>'
```

---

_Comment by @BurntSushi on 2019-07-29 11:48_

@elquimista This is over-complicating it. I'd definitely recommend reading the [guide's section on automatic filtering](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering).

Firstly, you can use `-u` instead of `--no-ignore`, where `-u` is a convenient synonym for telling ripgrep to disable handling of ignore rules.

Secondly, if `node_modules` is ignored, then specifying the `node_modules` directory itself will always override that. That is, if you do `rg foo some-file` and `some-file` is in your `.gitignore`, then ripgrep won't ignore because you explicitly told ripgrep to search it. So your command should be able to be reduced to just this:

```
$ rg '<pattern>' node_modules/<package_name> -tjs
```

---

_Comment by @dustinfreeman on 2019-08-05 15:44_

I'm currently working in a large git repo with some tracked files in the index whose pattern is also gitignored. In my situation, I was trying to use ripgrep to search the content of these files but they didn't appear because I did not know they were gitignored.

I believe this is a not-uncommon circumstance: in general a pattern is gitignored, but the repo has a few files that needed to be explicitly added.

I know this is perhaps wishful thinking, but it would be wonderful if rip grep's [automatic filtering](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering) was clever enough to include files tracked in the git index, even if their pattern is in the `.gitignore`. 

---
