```yaml
number: 1408
title: Scarf install instructions
type: pull_request
state: closed
author: aviaviavi
labels: []
assignees: []
base: master
head: master
created_at: 2019-10-25T23:51:42Z
updated_at: 2020-02-19T17:40:53Z
url: https://github.com/BurntSushi/ripgrep/pull/1408
synced_at: 2026-01-12T18:23:13Z
```

# Scarf install instructions

---

_@aviaviavi_

This change adds install instructions for the [Scarf package manager](https://scarf.sh).

I'd be happy to transfer package ownership, or add co-maintainers. Otherwise, I'll maintain the Scarf package myself, so no action necessarily required on anyone else's part. 

---

_Comment by @BurntSushi on 2020-02-15 20:10_

I hadn't heard of Scarf before, so I perused its homepage. I appreciate what you're trying to do in terms of funding FOSS, but I'm not sure I'm willing to get behind the data collection you're doing by default. I'm going to close this PR for now, but here are some possible avenues forward/ways I could be convinced:

* Is the data collection that Scarf is doing standard among other package managers? (Especially ones that are already in my README.) I don't think it is, but I confess that I am not intimately familiar with every package manager in my README.
* A _possible_ middle ground would be to include Scarf instructions in the README but plaster a giant warning that installation via Scarf will by default send data back to the Scarf package maintainers.

The reason why I am being a bit of a hardass about this is that I am very very deeply opposed to _ever_ adding any kind of telemetry to ripgrep, opt-in or otherwise. It would potentially be a huge boon to discovering usability problems, but the costs in trust and risks due to the networking code would be absolutely enormous given the kind of tool that ripgrep is (a tool that explicitly reads all of your data/code). Because of this position, it is my view that recommending a package manager in the README that does _any_ kind of telemetry by default is at least some kind of violation of this trust. And I really really really do not want to violate that trust. I don't even want to come _anywhere close_ to violating that kind of trust. I want to be completely in the clear.

Either way, if you want to pursue this, I'd recommend opening a new issue to discuss this. At that point, hopefully others can weigh in on this issue.

---

_Closed by @BurntSushi on 2020-02-15 20:10_

---

_Comment by @aviaviavi on 2020-02-19 17:40_

Thanks for the thoughtful response here @BurntSushi. 

With regard to telemetry in other package managers: No other managers I'm aware of go as far as to collect usage telemetry, but they _do_ still have telemetry. For example, Homebrew sends analytics to Google Analytics by default. The difference is that they do it to understand their user behavior, find broken packages and show download counts. Scarf does all of this too but is mainly trying to help package authors leverage their data to charge companies for their work.

In any case, I understand that telemetry, in general, is what you want to avoid. Since Scarf fundamentally is all about helping OSS developers, we're looking into other features we can provide package authors and users that could ship without analytics altogether. I'll open another PR when we have something more compelling.

Thanks for your time, and thanks for this excellent tool!

---
