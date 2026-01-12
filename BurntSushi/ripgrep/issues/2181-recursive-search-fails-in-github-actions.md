```yaml
number: 2181
title: Recursive search fails in GitHub Actions
type: issue
state: closed
author: AndrewSouthpaw
labels:
  - wontfix
assignees: []
created_at: 2022-04-15T01:44:47Z
updated_at: 2022-08-16T20:53:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2181
synced_at: 2026-01-12T16:13:24Z
```

# Recursive search fails in GitHub Actions

---

_@AndrewSouthpaw_

#### What version of ripgrep are you using?

```
# local
$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

# github action
ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Followed these instructions:

```
          curl -LO https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep_13.0.0_amd64.deb
          sudo dpkg -i ripgrep_13.0.0_amd64.deb
```

#### Describe your bug.

I'm running v13.0.0, and it works fine locally, but not in CI with GitHub Actions:

Local

```
$ rg '<<<<<< HEAD'
modules/mypkg/package.json
1:<<<<<< HEAD

modules/mypkg/package-lock.json
3:  <<<<<< HEAD
```

When I run it in CI with these commands:

```
          echo 'checking merge conflicts with explicit path'
          rg '<<<<<< HEAD' modules/mypkg/package-lock.json
          echo 'checking merge conflicts recursively'
          rg '<<<<<< HEAD'
```

It works fine for the explicit path but not for the recursive search.

```
(output)
checking merge conflicts with explicit path
  <<<<<< HEAD
checking merge conflicts recursively
Error: Process completed with exit code 1.
```

Any ideas what's going on?

---

_Comment by @BurntSushi on 2022-04-15 01:56_

Probably something related to Actions attaching something to stdin when it shouldn't, thus ripgrep tries to search stdin.

The work around is easy: don't rely on auto detection. Use `rg foo ./`.

---

_Closed by @BurntSushi on 2022-04-15 01:56_

---

_Label `wontfix` added by @BurntSushi on 2022-04-15 01:57_

---

_Comment by @AndrewSouthpaw on 2022-04-15 02:20_

Yep that did the trick, I never would've guessed to do that. Thanks for the quick response!

---

_Comment by @casey on 2022-08-16 20:36_

I just hit this, and it was pretty hard to debug. This is definitely not ripgrep's fault, but documenting this somewhere might be helpful for users who hit it.

---

_Comment by @casey on 2022-08-16 20:53_

Created a discussion [here](https://github.com/orgs/community/discussions/29721), hopefully they revert this.

---
