```yaml
number: 11757
title: Improve the readability of the documentation navigation
type: issue
state: open
author: zanieb
labels: []
assignees: []
created_at: 2025-02-24T20:09:58Z
updated_at: 2025-02-25T10:08:17Z
url: https://github.com/astral-sh/uv/issues/11757
synced_at: 2026-01-10T03:50:31Z
```

# Improve the readability of the documentation navigation

---

_Issue opened by @zanieb on 2025-02-24 20:09_

Split out from https://github.com/astral-sh/uv/issues/5605#issuecomment-2673805201

---

_Comment by @zanieb on 2025-02-24 20:21_

Regarding

> I'd encourage you to avoid horizontal navigation b/c it takes the scanning from a single dimension (vertical) to two dimensions (vertical + horizontal) and perhaps three dimensions if you have to click into a page in a section before you see it's horizontal navigation change. Makes getting the lay of the land much more difficult.

The horizontal nav would be major sections, like "Getting started", "Guides", "Concepts", "Reference". It's rare to need to switch between these to get the lay of the land — you probably know which section to start in. We'd then be able to uncollapse other nav, like the "Projects section which would avoid a need to click into content to see more content.

I agree there are trade-offs though.

---

_Comment by @rsyring on 2025-02-24 21:55_

Maybe you could post links to other projects whose docs use horizontal navs?  But, even then, I think it's a YAGNI issue and the time spent moving to a horizontal nav could be spent on other things.

I read a lot of docs, as I'm sure you probably do too, and I've never noticed docs where the left nav was too tall.  Per the link I posted on #5605, vertical scanning is not a problem for a typical user.  It's expected/intuitive.  And uv already has a process for collapsing docs with child elements.  So we have two layers of nav on the left plus a page TOC on the right.  That seems like it should be sufficient IMO.

The docs I've noticed in the past couple years that seem really good are often using this same nav layout and I think it "just works".

Maybe get the left nav visual aesthetics cleaned up, per the recent discussions on #5605.  The work @cmsirbu did is a good step in the right direction even if it's not the final destination. That seems like an easy win then we could wait to see if more community feedback is still negative around navigation usability.


---

_Comment by @rsyring on 2025-02-24 22:14_

> The horizontal nav would be major sections, like "Getting started", "Guides", "Concepts", "Reference". It's rare to need to switch between these to get the lay of the land — you probably know which section to start in.

As an example, here is what the Mise docs look like without the context of the content of each section.  This is similar to what you'd get with horizontal nav where you don't see the content of each section until you are in the section:

----

#### Guides

- Getting Started
- Walkthrough
- Installing mise
- IDE Integration
- Continuous Integration

#### Configuration
#### Dev Tools
#### Environments
#### Tasks
#### About
#### Advanced
#### CLI Reference

----


You could guess at what is in some of them.   Like the CLI Reference.  But I've used mise and the docs a lot over the last year and I can say the above is losing a ton of value compared to the [current nav](https://mise.jdx.dev/getting-started.html).  I've learned a lot about mise just from scanning the nav, seeing something interesting/novel, and clicking into it.

I wouldn't give up the context of the full nav unless I had a really good reason to.

---

_Comment by @abitrolly on 2025-02-25 10:03_

Is there "a heatmap" of most popular pages? Which people use often (and get back to).

---

_Comment by @abitrolly on 2025-02-25 10:08_

Padding makes the nav slightly more readable. And I would add filter field to menu too.

```css
padding-left: 1em
```

![Image](https://github.com/user-attachments/assets/f67d7d6b-ca7e-4d9e-97ef-9bb5568ff271)

---
