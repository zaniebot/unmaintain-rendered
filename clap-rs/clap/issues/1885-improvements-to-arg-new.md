---
number: 1885
title: "Improvements to `Arg::new`"
type: issue
state: closed
author: pksunkara
labels:
  - C-enhancement
  - A-builder
  - E-medium
  - E-easy
assignees: []
created_at: 2020-04-30T18:26:20Z
updated_at: 2020-05-15T06:30:08Z
url: https://github.com/clap-rs/clap/issues/1885
synced_at: 2026-01-10T01:27:08Z
---

# Improvements to `Arg::new`

---

_Issue opened by @pksunkara on 2020-04-30 18:26_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

`Arg::new` has been suggested to use (along with `App::new`) in one of the roadmap issues. It is implemented but there are still a lot of `Arg::with_name` usages.

### Describe the solution you'd like

* Remove `Arg::with_name` completely (also fixing examples, docs, etc.. just search `with_name`)
* Align `Arg::new` signature with `App::new` to make them consistent by changing one of them or both. @kbknapp probably has some thoughts about this.

---

_Label `T: new feature` added by @pksunkara on 2020-04-30 18:26_

---

_Label `T: new feature` removed by @pksunkara on 2020-04-30 18:27_

---

_Label `C: app` added by @pksunkara on 2020-04-30 18:27_

---

_Label `C: args` added by @pksunkara on 2020-04-30 18:27_

---

_Label `D: easy` added by @pksunkara on 2020-04-30 18:27_

---

_Label `P2: need to have` added by @pksunkara on 2020-04-30 18:27_

---

_Label `T: enhancement` added by @pksunkara on 2020-04-30 18:27_

---

_Label `T: RFC / question` added by @pksunkara on 2020-04-30 18:27_

---

_Label `Z: good first issue` added by @pksunkara on 2020-04-30 18:27_

---

_Label `Z: mentored` added by @pksunkara on 2020-04-30 18:27_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-30 18:27_

---

_Comment by @kbknapp on 2020-04-30 18:36_

I'm :+1: on this and it was the intent for 3.0. My *only* concern is it makes churn for upgrading from 2.x to 3.x with no functional difference.

One proposal is to simply deprecate `Arg::with_name` and remove in 4.x. Of course, the counter argument is there are other breaking changes, so some churn is expected. 

It's such a small function that I'd honestly be fine either way (deprecate for 3.x or just go ahead and hard remove).

---

_Comment by @CreepySkeleton on 2020-04-30 18:43_

Spoiler: @pksunkara and I have been talking about some sort of automatized update tool based on `rust_analyzer` 😏  Click the button, become happy!

We'll get to it as soon as we deal wit more pressing stuff.

---

_Comment by @kbknapp on 2020-04-30 18:49_

That would be awesome. I had considered some kind of upgrade script, but there certain cases where the intent isn't 100% known because clap 2.x didn't allow enough granularity (`multiple(true)`, etc.). Looking forward to see how this progresses!

---

_Comment by @pksunkara on 2020-04-30 21:33_

I had the idea for auto-upgrade tool long before working in rust. But with rust-analyzer doing most of the heavy lifting and me also needing it for another project of mine, I started to seriously consider this and try to see if I can do an MVP for clap v3. I have some workflow and API designs, but haven't written them down anywhere.

https://github.com/pksunkara/cargo-up. I should be working on it heavily from next week. Just trying to finish up some of the current tasks I was working on.

---

_Closed by @bors[bot] on 2020-05-15 06:30_

---
