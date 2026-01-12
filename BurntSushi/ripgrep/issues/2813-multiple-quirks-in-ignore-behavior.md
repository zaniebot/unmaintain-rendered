```yaml
number: 2813
title: Multiple quirks in ignore behavior
type: issue
state: closed
author: mcandre
labels:
  - invalid
assignees: []
created_at: 2024-05-21T16:27:26Z
updated_at: 2024-08-27T19:06:38Z
url: https://github.com/BurntSushi/ripgrep/issues/2813
synced_at: 2026-01-12T16:13:24Z
```

# Multiple quirks in ignore behavior

---

_@mcandre_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 14.4.1

### Describe your bug.

* rg fails to ignore common junk directories by default, such as `.git/`, `.svn/`, `build/`, `node_modules/`, `target/`, `vendor/`, etc.
* rg appears not to respect `--glob` flags to ignore the same in `~/.ripgreprc`.
* rg documents four different mechanisms dealing with ignore behavior, but only the `.ignore` file works.

### What are the steps to reproduce the behavior?

`rg "./..."` in a large, complex git repository.

`./...` is Go CLI syntax. Here, we naively plug it into the ripgrep regular expression engine, which interprets the string as a pattern matching any single character followed by a forward slash followed by any three characters. Which often matches machine generated, binary files in the `.git` directory. This clobbers the console with very many files that are not normally relevant to user queries.

### What is the actual behavior?

```
$ rg './...'
.git/info/exclude
1:# git ls-files --others --exclude-from=.git/info/exclude

.git/objects/f7/4591dae4979c7527b0eb045183b5004b7f1a9a
4:?-?M??w??I?Y??>t??ò??uam?mk?N???)????I??q????????4J??̫??(
                                                          /3G????C2k?????<???.T??<]??l;?Zw?l        uW????h?

.git/objects/af/bb51d56eb3c2271c5af6b59c6ec98b5e46e688
1:xmR]o?0?s~ŕ?CYUP[????u? t!L?Фr9vf;???:??*??s????=H}???????2O?1d?]???8ͷA?      ?O???ɹ?~?JK?*?M?a?fS?pK???6?G]7܉?D???ߚ?`?nPUF?M?]+)?A=FQ4??Q?NH???
                                                                        ?????gHu??????WҤ ??0???M|?}?Z7??33?ܽ
                                 >?????Z/K2֊K(????p???R?hR?U?e???v?m??|??jFڎO7?UP??'Q?_hތ?????0??A??f
```

Even worse, ripgrep dumps binary sequences directly to the console, corrupting my terminal.

I have to run `reset` (UNIX) each time afterwards to account for that.

### What is the expected behavior?

* ripgrep should respect `--glob` exclusions in `~/.ripgreprc`, which are often documented online by various ripgrep users.
* ripgrep should automatically exclude common VCS directories (`.git`, `.svn`) and common build system directories (`build/`, `node_modules/`, `target/`, `vendor/`) by default.
* ripgrep should deprecate `.rgignore`, presenting a warning when that file is present.

---

_Comment by @BurntSushi on 2024-05-21 16:33_

There is way too much going on in this issue. ripgrep _does_ ignore things like `.git` by default because it's a hidden directory and ripgrep ignores hidden directories by default. So there is more going on here than what you're showing. Maybe you have an alias or a config file influencing things here.

You also don't show any concrete example and don't show the `--debug` output. The `--debug` output should explain why `.git` is being searched.

Directories like `node_modules` or `target` are usually in your `.gitignore`.

I don't know why you're suggesting that `.rgignore` should be deprecated.

---

_Comment by @mcandre on 2024-08-27 18:45_

ripgrep has too many, confusing mechanisms to manage exclusion patterns.

---

_Comment by @BurntSushi on 2024-08-27 19:06_

Closing due to insufficient data from the OP 

---

_Closed by @BurntSushi on 2024-08-27 19:06_

---

_Label `invalid` added by @BurntSushi on 2024-08-27 19:06_

---
