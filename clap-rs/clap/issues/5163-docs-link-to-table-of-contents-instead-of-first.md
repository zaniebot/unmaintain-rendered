```yaml
number: 5163
title: "docs: link to table of contents instead of first chapter for tutorial"
type: issue
state: open
author: Finchiedev
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-docs
assignees: []
created_at: 2023-10-08T02:08:08Z
updated_at: 2023-12-07T15:40:08Z
url: https://github.com/clap-rs/clap/issues/5163
synced_at: 2026-01-12T16:14:16Z
```

# docs: link to table of contents instead of first chapter for tutorial

---

_@Finchiedev_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master (1806e28)

### Describe your use case

The "quick links" section at the top of clap's [docs.rs page](https://docs.rs/clap/latest/clap/index.html) currently links to the first chapter of both the builder and derive tutorials. I have found that when distracted or just skimming the documentation it is quite confuse this first chapter for the entire tutorial.

### Describe the solution you'd like

Change the link to point to the table of contents rather than the first chapter.

### Alternatives, if applicable

- Leave it as is
- Embed an index in the "quick links" section
- Improve the visibility of the `next` module re-exported in every chapter

### Additional Context

These changes are very small, I've made them available on a [public fork](https://github.com/Finchiedev/clap/tree/link-to-index/) if they're worth merging.

---

_Label `C-enhancement` added by @Finchiedev on 2023-10-08 02:08_

---

_Label `A-docs` added by @epage on 2023-10-09 18:48_

---

_Label `S-waiting-on-decision` added by @epage on 2023-10-09 18:48_

---

_Comment by @epage on 2023-10-09 18:50_

I copied this from `winnow` when I pulled in a tutorial as I wanted the introduction first.  However, I find that with clap I'm constantly jumping to the table of contents to get to a specific section to show to people.  I've been unsure how representative my workflow is.

Looking back at it, I would be worried about the stutter of going to the tutorial (ToC) to just have to go to the tutorial (start).  I like the introduction  or quick start to be right there.

Will have to think more on this and would appreciate more input in the form of what people are trying to accomplish when they go to the tutorial, why it works or doesn't, and how they feel any suggested change may impact someone just wanting to read a tutorial.

---

_Referenced in [clap-rs/clap#5199](../../clap-rs/clap/issues/5199.md) on 2023-11-08 19:08_

---

_Comment by @Finchiedev on 2023-12-07 12:02_

Thanks for all of the context! There's definitely value in reducing the friction of navigating between the docs homepage and the first page of the tutorial. Similar to your workflow, I am usually interested with finding a particular topic, already knowing the general solution but looking for a reference, which a table of contents suits very well. I didn't even realize there _was_ a table of contents or more than one page for quite a while, re-exporting pages as modules to appear in the sidebar and footer is very useful but as a frequent user of docs.rs I usually filter this out as noise. As you mentioned it's hard to know how common this is, I can only speak to my own experience.

Another solution that I'm surprised I didn't mention in the initial message: embed a small table of contents on every page, or at least on the first page. That way users are still dropped straight into the content, but it's clear what sections are covered, and the "jumping around" use-case works great as well.

---

_Comment by @epage on 2023-12-07 15:40_

I think having a "topics we'll cover" at the end of the first page would make sense.  Granted that'd be "below the fold" which is annoying but what "fits" with the design.

---
