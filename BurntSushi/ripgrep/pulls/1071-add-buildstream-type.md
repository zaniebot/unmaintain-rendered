```yaml
number: 1071
title: Add Buildstream type
type: pull_request
state: merged
author: bochecha
labels: []
assignees: []
merged: true
base: master
head: buildstream
created_at: 2018-09-27T21:19:22Z
updated_at: 2018-09-28T12:33:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1071
synced_at: 2026-01-12T18:23:13Z
```

# Add Buildstream type

---

_@bochecha_

BuildStream is a Free Software tool for building/integrating software stacks.: https://buildstream.gitlab.io/buildstream/

It uses recipes written in YAML, in files with the `.bst` extension.

---

I wonder about the name if the type. I used `buildstream` because that's the name of the project, but the command line is `bst` so that's another possible name.

The latter is also shorter, which is nice when using `rg --type=bst …`.

Do you have a policy on this? Can we add both?

---

_Comment by @BurntSushi on 2018-09-28 11:42_

> Do you have a policy on this? Can we add both?

Not particularly. My general policy has been a bit vague. Ideally, if we add very short abbreviations, then they should be popular enough that virtually anyone will recognize them. My sniff test has been that it's something I would recognize. I also have been trying to avoid multiple names that refer to the same thing, although there are some that exist, I'd prefer not to add more.

Unfortunately, I've never heard of buildstream before. Who is using it? I'm hesitant to add a short name.

---

_Comment by @bochecha on 2018-09-28 12:04_

> Unfortunately, I've never heard of buildstream before. Who is using it? I'm hesitant to add a short name.

Buildstream is mostly developed by a company called Codethink, so I guess they use it for their customers?

That being said, in terms of public open source projects using it, there's at least:

* the Freedesktop Sdk: https://gitlab.com/freedesktop-sdk/freedesktop-sdk/
* GNOME: https://gitlab.gnome.org/GNOME/gnome-build-meta/
* the Trustable project: http://trustable.gitlab.io/

Buildstream is a buildsystem for stacks of projects (e.g operating systems, container images, …) rather than individual projects (e.g the Autotools, Meson, Cargo, …). It pilots the latter for each projects integrated in the stack, builds everything reproducibly in a controlled environment in a build sandbox, etc…

Really impressive tech to build OSes with. :slightly_smiling_face: 

---

_Merged by @BurntSushi on 2018-09-28 12:32_

---

_Closed by @BurntSushi on 2018-09-28 12:32_

---

_Comment by @BurntSushi on 2018-09-28 12:32_

Let's just go with the longer name for now. There are other things in the file types with names that are similarly long. Plus, if you have zsh completion setup, then file types will auto-complete. :-)

---

_Branch deleted on 2018-09-28 12:33_

---
