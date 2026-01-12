```yaml
number: 2315
title: Enable Dependabot for the Actions workflows
type: pull_request
state: closed
author: LingMan
labels: []
assignees: []
base: master
head: dependabot
created_at: 2022-09-29T03:34:08Z
updated_at: 2022-09-29T12:28:10Z
url: https://github.com/BurntSushi/ripgrep/pull/2315
synced_at: 2026-01-12T18:23:14Z
```

# Enable Dependabot for the Actions workflows

---

_@LingMan_

Dependabot automatically files PRs for updatable dependencies. As configured it watches all workflow files in `.github/workflows` for possible updates to any of the Actions depended upon.

---
It also supports updating rust dependencies, but I've kept this minimal to gauge interest. See https://github.com/eminence/terminal-size/pull/42 for an example of a PR you'll also automatically receive after merging this.


---

_@BurntSushi approved on 2022-09-29 11:25_

Hmmm, okay, I'll give this a try, but might end up removing it if it's too noisy. I didn't know Dependabot existed for GitHub Actions.

I'm pretty sure I do not want to enable this for Rust dependencies. I do update them occasionally, but I like to do it manually and deliberately. With Dependabot, I'll feel like I'm running in a hamster wheel. :-)

---

_Comment by @LingMan on 2022-09-29 11:36_

Yeah, if you have the lock file committed, Dependabot can be a bit overwhelming. Would be great if it could be configured to submit a single PR updating all dependencies in one go to cut down on the hamster wheel a bit.

For Github Actions it shouldn't be particularly noisy. You'll get a few PRs initially but actions update to a new major version relatively rarely. 

---

_Closed by @BurntSushi on 2022-09-29 12:03_

---

_Branch deleted on 2022-09-29 12:28_

---
