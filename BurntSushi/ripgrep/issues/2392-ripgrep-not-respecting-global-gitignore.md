```yaml
number: 2392
title: ripgrep not respecting global .gitignore
type: issue
state: closed
author: Avishayy
labels:
  - bug
  - rollup
assignees: []
created_at: 2023-01-06T01:38:45Z
updated_at: 2023-11-21T04:51:56Z
url: https://github.com/BurntSushi/ripgrep/issues/2392
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep not respecting global .gitignore

---

_@Avishayy_

#### What version of ripgrep are you using?
```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew`

#### What operating system are you using ripgrep on?

macOS Ventura 13.1

#### Describe your bug.

I have some local backup files in my dev repo and I'd like to ignore them from all `rg` functionalities. Since they exist on my local machine I don't add them to the repo tracked .gitignore, so I configured a global gitignore but it seems to not work. https://github.com/BurntSushi/ripgrep/issues/9 seems to create support for it, but it's quite old already so perhaps it's now broken.

#### What are the steps to reproduce the behavior?

```sh
$ git config core.excludesFile
~/.gitignore

$ cat ~/.gitignore
*.sql

$ rg --files | rg backup
backup
backup_21_07_2022.sql
backup_11-12-22.sql
db_backup.sql

$ git add db_backup.sql
The following paths are ignored by one of your .gitignore files:
db_backup.sql
hint: Use -f if you really want to add them.
hint: Turn this message off by running
hint: "git config advice.addIgnoredFile false"

$ echo "*.sql" >> .gitignore

$ rg --files | rg backup
backup
```

You can see that git refuses to add the file since it is ignored by the global .gitignore and then that ripgrep would only respect it if is present in the working directory.

#### What is the actual behavior?
Patterns from global .gitignore are ignored by ripgrep

#### What is the expected behavior?
Patterns from global .gitignore are respected by ripgrep

#### What do you think ripgrep should have done?
Ignore the patterns present in the global .gitignore

---

_Comment by @Avishayy on 2023-01-06 01:42_

Using the following paths for the global path doesn't work either:
`$HOME/.gitignore`
`/Users/user/.gitignore`
(but it does work for git as expected)
So it's not an issue of not resolving the tilde or something of sorts.

---

_Comment by @BurntSushi on 2023-01-06 01:45_

Please include `--debug` output, as requested in the issue template.

Also, a small reproducible example that I can run would be helpful.

---

_Comment by @Avishayy on 2023-01-06 01:54_

Sorry for not doing this yet, I found the root cause.
I edited the .gitconfig file manually and written the path as a string, i.e.
```
[core]
	excludesFile = "~/.gitignore"
```
(which git is happy with)

Changing it to a regular form
```
[core]
	excludesFile = ~/.gitignore
```

Makes ripgrep respect the global config as well, I'll attach a reproducer soon.


---

_Comment by @Avishayy on 2023-01-06 02:00_

```sh
mkdir rg_global_gitignore
cd rg_global_gitignore
git init

# adding the config this way will strip the quotation marks
#git config --global core.excludesFile "~/.gitignore"

# open ~/.gitconfig and add:
#
# [core]
# 	excludesFile = "~/.gitignore"

echo "*.sql" > ~/.gitignore

touch backup backup.sql backup123.sql db_backup.sql
rg --files --debug
```

Output is
```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
backup.sql
backup
backup_21_07_2022.sql
backup123.sql
backup_11-12-22.sql
db_backup.sql
```

---

_Comment by @BurntSushi on 2023-01-06 13:21_

My guess is that this can be fixed with a simple tweak to the regex used for parsing this file: https://github.com/BurntSushi/ripgrep/blob/bc5504932764d8d4735bf955f6f7e04a95f151b8/crates/ignore/src/gitignore.rs#L600

---

_Label `bug` added by @BurntSushi on 2023-01-06 13:21_

---

_Comment by @Kentokamoto on 2023-10-11 14:43_

I'd like to take a look into fixing this since it looks like a pretty straight forward task. From what I was able to gather, this seems to be a problem when .gitconfig files have double quotes surrounding the value for `excludesFile`. If this is a small change to the regex under [ripgrep/crates/ignore/src/gitignore.rs](https://github.com/BurntSushi/ripgrep/blob/bc5504932764d8d4735bf955f6f7e04a95f151b8/crates/ignore/src/gitignore.rs#L600), then we probably might be able to get away with adding the double quote character to a character set along with whitespaces. So the new regex would look like
```
r"(?im)^\s*excludesfile\s*=[\x22\s]*(.+?)[\x22\s]*$"gm
```

---

_Label `rollup` added by @BurntSushi on 2023-10-15 12:36_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
