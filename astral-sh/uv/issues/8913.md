```yaml
number: 8913
title: latest release fails on widonws CICD
type: issue
state: closed
author: joamatab
labels: []
assignees: []
created_at: 2024-11-08T01:31:17Z
updated_at: 2024-11-08T18:15:33Z
url: https://github.com/astral-sh/uv/issues/8913
synced_at: 2026-01-10T04:36:20Z
```

# latest release fails on widonws CICD

---

_Issue opened by @joamatab on 2024-11-08 01:31_

Amazing job on uv, tests run very fast now,

however last release broke our tests for windows

https://github.com/gdsfactory/gdsfactory/actions/runs/11734371332/job/32690273059

---

_Comment by @charliermarsh on 2024-11-08 01:39_

It looks like it can't find uv? We now install to `~/.local/bin` by default and it's not on your path. Check out the release notes including the breaking change from v5: https://github.com/astral-sh/uv/releases/tag/0.5.0.

---

_Comment by @TimChild on 2024-11-08 02:42_

Yep, that got my CI too... But a pretty quick fix as charliermarsh pointed out.

You also have the option to specify the version when you install to avoid issues in future. 
e.g. `curl -LsSf https://astral.sh/uv/0.5.0/install.sh | sh` (or 0.4.30) if you want the latest version before this change.

---

_Comment by @hutcho on 2024-11-08 03:20_

I did a uv self update, and my windows lost the location of uv. After the update, uv wasn't a recognised command anymore.

---

_Comment by @zanieb on 2024-11-08 03:42_

Oh awkward. Sorry this broke your CI — I presume it only worked before because the Cargo path is special-cased on their runners?

>  did a uv self update, and my windows lost the location of uv. After the update, uv wasn't a recognised command anymore.

Can you open a new issue? This one is for CI / CD. Briefly, a `uv self update` should not change the install location so I'm curious how it broke — it's possible there's a bug upstream.

---

_Comment by @TimChild on 2024-11-08 17:21_

@zanieb Yea, I had the Cargo path set in a Docker file that my CI depended on. So CI was just where I happened to notice it. Easy to fix, and now I know that I can version the install too. Thanks for the good work!

---

_Comment by @zanieb on 2024-11-08 18:15_

Thanks for following up!

---

_Closed by @zanieb on 2024-11-08 18:15_

---
