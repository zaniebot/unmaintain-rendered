```yaml
number: 2332
title: "ci: Added uploading build artifacts and also build release"
type: pull_request
state: closed
author: mitchcapper
labels: []
assignees: []
base: master
head: add_ci_artifacts_pr
created_at: 2022-10-16T09:34:48Z
updated_at: 2023-07-08T12:57:40Z
url: https://github.com/BurntSushi/ripgrep/pull/2332
synced_at: 2026-01-12T18:23:14Z
```

# ci: Added uploading build artifacts and also build release

---

_@mitchcapper_

Upload build artifacts for CI to allow people to easily grab nightly builds

Ran into the long file path issue, found the fix from last year: #2049 but as there were no nightly artifacts would need to clone and build just to test.  
This also adds release building to the CI for tests (to get us both sets of artifacts).



---

_Comment by @mitchcapper on 2022-10-16 09:35_

Here is a sample as well: https://github.com/mitchcapper/ripgrep/actions/runs/3258975771

---

_Comment by @BurntSushi on 2023-07-08 12:57_

I appreciate what you're trying to do here, but I do not want the headache of maintaining nightly release builds. IMO the build process for ripgrep is very easy and so I don't think it's a huge ask to build it yourself if you want to test/use whatever is on master.

---

_Closed by @BurntSushi on 2023-07-08 12:57_

---
