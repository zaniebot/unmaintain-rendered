```yaml
number: 1383
title: Enable preview deployments for playground
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/preview
created_at: 2022-12-26T17:57:45Z
updated_at: 2022-12-26T20:00:15Z
url: https://github.com/astral-sh/ruff/pull/1383
synced_at: 2026-01-12T05:36:31Z
```

# Enable preview deployments for playground

---

_Pull request opened by @charliermarsh on 2022-12-26 17:57_

_No description provided._

---

_@andersk reviewed on 2022-12-26 19:37_

---

_Review comment by @andersk on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:37_

https://github.com/jetli/wasm-pack-action looks like it does the same but with caching (via `@actions/tool-cache`)?

---

_@charliermarsh reviewed on 2022-12-26 19:37_

---

_Review comment by @charliermarsh on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:37_

Thank you

---

_Review comment by @charliermarsh on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:38_

I saw you'd opened some issues on the `pages-action` repo... That action seems wonky. Impossible to get good behavior with branch names etc. The Wrangler one isn't much better but at least allows passing arguments directly to `wrangler`...

![Screen Shot 2022-12-26 at 2 38 26 PM](https://user-images.githubusercontent.com/1309177/209578994-28ca025d-7020-48e6-85de-f6aeb9e25872.png)


---

_@charliermarsh reviewed on 2022-12-26 19:38_

---

_@andersk reviewed on 2022-12-26 19:47_

---

_Review comment by @andersk on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:47_

Neat. In my case (company blog) I wanted the extra comments posted by `pages-action`, but I could imagine that being too spammy here.

---

_@charliermarsh reviewed on 2022-12-26 19:50_

---

_Review comment by @charliermarsh on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:50_

Why am I not seeing those comments...? I did want them but they weren't appearing with `pages-action`.

---

_Merged by @charliermarsh on 2022-12-26 19:52_

---

_Closed by @charliermarsh on 2022-12-26 19:52_

---

_Branch deleted on 2022-12-26 19:52_

---

_Review comment by @andersk on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:52_

Hmm. Did you provide `gitHubToken: ${{ secrets.GITHUB_TOKEN }}`? It might be conditional on that.

---

_@andersk reviewed on 2022-12-26 19:52_

---

_@charliermarsh reviewed on 2022-12-26 19:53_

---

_Review comment by @charliermarsh on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:53_

Oh, I definitely did not. I was convinced it wasn't possible because there are issues about it like https://github.com/cloudflare/pages-action/issues/16.

---

_@charliermarsh reviewed on 2022-12-26 19:53_

---

_Review comment by @charliermarsh on `.github/workflows/playground.yaml`:27 on 2022-12-26 19:53_

Anyway, I'll leave it for now.

---

_@andersk reviewed on 2022-12-26 20:00_

---

_Review comment by @andersk on `.github/workflows/playground.yaml`:27 on 2022-12-26 20:00_

Now I’m remembering that the comment comes from the cloudflare-pages bot, not from github-actions, so the Actions configuration might not be relevant.

I have Git integration enabled in the CloudFlare dashboard, with **Automatic deployments** disabled, **Build command** set to `false`, and **Build comments on pull requests** enabled.

---
