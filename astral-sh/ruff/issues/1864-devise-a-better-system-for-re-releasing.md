```yaml
number: 1864
title: "Devise a better system for \"re-releasing\""
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-01-14T04:55:25Z
updated_at: 2023-03-21T23:50:24Z
url: https://github.com/astral-sh/ruff/issues/1864
synced_at: 2026-01-12T15:54:41Z
```

# Devise a better system for "re-releasing"

---

_@charliermarsh_

Right now, release builds are triggered by creating a release on GitHub. If the build fails, the only way to "re-run" is to create a new release. So we have a few versions that have just never been released. [v0.0.221](https://github.com/charliermarsh/ruff/actions/runs/3916906363/jobs/6696361593) is one example, where I had to push a fix, then re-release as v0.0.222.

---

_Comment by @charliermarsh on 2023-01-14 04:57_

Actually, for that build, I'm going to try deleting the release + tag, and then re-creating it with the updated `main`.

---

_Label `internal` added by @charliermarsh on 2023-01-14 04:57_

---

_Comment by @not-my-profile on 2023-01-14 08:28_

(Unrelated sidenote: I don't think we need to bump the version for `ruff_dev` at all since it's just internal tooling.)

---

_Comment by @bilderbuchi on 2023-01-18 10:18_

Depending on your desired workflow, you could insert an additional `release_prep` between `release` and the others it `uses` (moving those to `release_prep`).
Then, you could fire the `release_prep` either on PRs that have a `release` (or other appropriate) label, or on pushes to a `release` branch, or any other condition that you like.
When that check is successful, you can push a release commit with git tag & Github release, and the full machinery should run as it does now.

---

_Comment by @stinodego on 2023-01-19 22:04_

Creating a release also creates a tag. You can have your workflow trigger on a new tag. If anything goes wrong, you only need to update the tag to point to the new commit that fixes the issue, which will re-trigger the workflow. I think that solves your original problem.

---

_Comment by @bilderbuchi on 2023-01-19 22:09_

Hm, I always create and push the git tag first (manually), and then the Github release.
Also, updating the tag would mean rewriting public history, no? I guess it depends on the project if that is OK.

---

_Comment by @stinodego on 2023-01-19 22:13_

I figure updating a tag is fine as long as there is no associated release on PyPI yet. But as you say, I'm sure there's different philosophies there. But this issue calls for a way to re-release, which this offers.

---

_Comment by @charliermarsh on 2023-01-19 22:29_

Ah yeah, interesting. So the release would create a tag, which _could_ fail; then I could fix whatever bug caused it to fail; then I could push the tag again to the newer SHA, and the job would run again, and succeed. Is that right?

---

_Comment by @stinodego on 2023-01-20 05:25_

Exactly.

So you would change the release workflow trigger to:

```yaml
on:
  push:
    tags:
      - '**'
```

This will be triggered when you do a manual GitHub release, as it creates a new tag.

Then, if the workflow fails, you add a new commit with a fix, and you update the tag like so:

```
git tag -f <tag-name> <commit-hash> && git push -f origin <tag-name>
```

This will re-trigger the release workflow and now (hopefully) successfully release to PyPI.

### Limitations

Note that the new commit, and any commits you merge between releasing and merging that fix, will not automatically be added to the release changelog of the existing release. But they will be part of the release.

So after updating the tag, you'd have to manually edit the existing release to re-generate the changelog. The auto-generate should pick up the new tags just fine and generate the correct changelog, so this is no issue at all.



---

_Comment by @charliermarsh on 2023-03-21 23:50_

I used the above strategy once, it's a bit manual for us right now (since I have to change the release files) but it works great if needed. I've also started to run a release build prior to creating the release which catches non-transient issues.

---

_Closed by @charliermarsh on 2023-03-21 23:50_

---
