```yaml
number: 2407
title: "ripgrep not respecting included gitconfig files when determining git's core.excludesfile"
type: issue
state: closed
author: richardebeling
labels:
  - duplicate
assignees: []
created_at: 2023-01-31T13:46:23Z
updated_at: 2023-01-31T18:58:10Z
url: https://github.com/BurntSushi/ripgrep/issues/2407
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep not respecting included gitconfig files when determining git's core.excludesfile

---

_@richardebeling_

#### What version of ripgrep are you using?
ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
```
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep_13.0.0_amd64.deb
sudo dpkg -i ripgrep_13.0.0_amd64.deb
```

#### What operating system are you using ripgrep on?
Ubuntu 20.04

#### Describe your bug.
Similar to #2392, but other root cause: Ripgrep doesn't respect the `core.excludesfile` git configuration if it is set in a configuration file that is imported using git's [`include.path` setting](https://git-scm.com/docs/git-config#_includes).

#### What are the steps to reproduce the behavior?
```bash
mkdir rg_global_included_gitignore
cd rg_global_included_gitignore
git init

git config --global include.path $(pwd)/included_gitconfig
git config --file $(pwd)/included_gitconfig core.excludesfile $(pwd)/global_gitignore

echo "*.sql" > ./global_gitignore
touch backup backup.sql

git status  # doesn't list backup.sql as untracked

rg --files --debug
```

#### What is the actual behavior?
rg produces this output:
```
# DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
# backup
# backup.sql
# global_gitignore
# included_gitconfig
```

#### What is the expected behavior?
`backup.sql` shouldn't have been listed, it is ignored by the global ignore file `./global_gitignore` that is set as `core.excludesfile` in `./included_gitconfig`, which in turn is included into `~/.gitconfig` using the `include.path` mechanism.

To me it looks like [the parsing](https://github.com/BurntSushi/ripgrep/blob/fe97c0a152cabc1bc07ec36b4b1e27cd230c3014/crates/ignore/src/gitignore.rs#L536-L550) doesn't attempt to find and follow configuration includes, so this currently can't work.

---

_Comment by @BurntSushi on 2023-01-31 18:57_

Duplicate of #1014 and #2062.

---

_Closed by @BurntSushi on 2023-01-31 18:57_

---

_Label `duplicate` added by @BurntSushi on 2023-01-31 18:58_

---
