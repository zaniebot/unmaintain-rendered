```yaml
number: 2244
title: "add: MkDocs GitHub Pages"
type: pull_request
state: closed
author: juftin
labels:
  - documentation
assignees: []
base: main
head: main
created_at: 2023-01-27T07:46:39Z
updated_at: 2023-01-28T03:58:33Z
url: https://github.com/astral-sh/ruff/pull/2244
synced_at: 2026-01-12T15:55:07Z
```

# add: MkDocs GitHub Pages

---

_@juftin_

## Summary

Addresses: https://github.com/charliermarsh/ruff/issues/1398

I saw you mentioned mdBook in the above issue - MkDocs is a strong markdown based static site that could work in the meantime before nicer solutions get built with mdBooks. 

This PR adds a [mkdocs](https://www.mkdocs.org/) site with the [mkdocs-material](https://squidfunk.github.io/mkdocs-material/) theme. I think it does a good job of making the `README` easier to digest - and will work nicely when/if the docs are split up too. 

Once implemented you're ready to host a free site at https://charliermarsh.github.io/ruff

<img width="1389" alt="image" src="https://user-images.githubusercontent.com/49741340/215032095-24c561b3-49d8-4eb0-8b1d-022840c7d5c8.png">

Let me know what you think, and if you're interested - I'm happy to help customize and improve the docs as well. Here's the site temporarily hosted from my fork: https://juftin.com/ruff/

## Implementation

### CI/CD

`mkdocs` GitHub Actions Workflow copies `README.md` to `docs/index.md`, commits any changes to the `main` branch. Finally it uses `mkdocs` to publish the static HTML site to the `gh-pages` branch.

## Running Locally

```shell
pipx install mkdocs
pipx inject mkdocs mkdocs-material
mkdocs serve --livereload
```

## Changes Needed

### GH Pages

You may need to activate GitHub Pages to source from the `gh-pages` branch here: https://github.com/charliermarsh/ruff/settings/pages

<img width="774" alt="image" src="https://user-images.githubusercontent.com/49741340/215031832-ce901e3a-0f4a-425b-9ae4-57c519138787.png">

### Personal Access Token

You'll need to add a Personal Access Token as a GitHub secret for this to commit changes to the main branch since it's protected - I'm referencing the `PERSONAL_ACCESS_TOKEN` secret name.


---

_Comment by @messense on 2023-01-27 09:47_

I‘d recommend build and host on Netlify instead, gh-pages will make `git clone` size grow much larger in the long run.

---

_Comment by @Jackenmen on 2023-01-27 10:10_

GitHub pages can be deployed using GH Actions without having to keep a dedicated branch:
https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow

---

_Comment by @messense on 2023-01-27 11:36_

> GitHub pages can be deployed using GH Actions without having to keep a dedicated branch:
> https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow

Great!

---

_Comment by @charliermarsh on 2023-01-27 12:40_

Thanks so much for this! It's so cool to see.

Let me think a bit on (1) MkDocs vs. mdBook, and (2) deployment / hosting, prior to merging this. It's hard to roll back a public site / artifact, so I want to ensure I've given all the tradeoffs sufficient thought.

On (2): we already host the playground at [https://play.ruff.rs/](https://play.ruff.rs/) on Cloudflare Pages, so I'll likely tweak this setup to use Pages too for simplicity with DNS and such. But that's easy for me to change.


---

_Label `documentation` added by @charliermarsh on 2023-01-27 12:40_

---

_Comment by @juftin on 2023-01-27 16:06_

Sounds great - the nice thing about MkDocs is that it was extremely low effort to set up. If you decide to go with it, great. If not, no worries. Excited to see the future of docs @ ruff!

---

_Comment by @charliermarsh on 2023-01-28 03:58_

I had to make some follow-up changes to this, but it just merged as #2287. I would've pushed here, but didn't have edit rights -- sorry to create a separate PR, but hopefully you're credited as the author in Git since I preserved you in the co-commit. Really appreciate you pushing us here!

---

_Closed by @charliermarsh on 2023-01-28 03:58_

---
