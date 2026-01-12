```yaml
number: 2847
title: Switch from mkdocs to mdbook
type: pull_request
state: closed
author: not-my-profile
labels: []
assignees: []
draft: true
base: main
head: mdbook
created_at: 2023-02-13T08:04:18Z
updated_at: 2023-02-14T05:23:28Z
url: https://github.com/astral-sh/ruff/pull/2847
synced_at: 2026-01-12T15:55:11Z
```

# Switch from mkdocs to mdbook

---

_@not-my-profile_

In 28c9263722b77b0bd78eeebccfab98c33e92f2d2 I introduced automatic
linkification of option references in rule documentation,
which automatically converted the following:

    ## Options

    * `namespace-packages`

to:

    ## Options

    * [`namespace-packages`]

    [`namespace-packages`]: ../../settings#namespace-packages

While the above is a correct CommonMark[1] link definition,
what I was missing was that we used mkdocs for our documentation
generation, which as it turns out uses a non-CommonMark-compliant
Markdown parser, namely Python-Markdown[2], which contrary to CommonMark
doesn't support link definitions containing code tags.[3]
Mkdocs doesn't support CommonMark[4][5].

This commit therefore switches our documentation to use mdbook[6]
instead, which uses the pulldown-cmark[7] CommonMark implementation,
sparing us from having to deal with such parser idiosyncracies.

[1]: https://commonmark.org/
[2]: https://github.com/Python-Markdown/markdown/
[3]: https://github.com/Python-Markdown/markdown/issues/280
[4]: https://github.com/mkdocs/mkdocs/issues/361
[5]: https://github.com/mkdocs/mkdocs/issues/1835
[6]: https://github.com/rust-lang/mdBook
[7]: https://github.com/raphlinus/pulldown-cmark



---

_Comment by @charliermarsh on 2023-02-13 15:25_

Hmm... mdBook is a good tool, and I appreciate the work you put in here, but MkDocs is far more popular in the Python ecosystem and will be more familiar to our users. I'm partial to keeping MkDocs around, even if it requires workarounds for some of its idiosyncrasies.

> As the original implementation (markdown.pl) works this way, I see no reason to change. After all, if you need to have code inside a link, you can use a reference name in the [second set of brackets](http://johnmacfarlane.net/babelmark2/?normalize=1&text=%5B%60hello%60%5D%5Bid%5D%0A%0A%5Bid%5D%3A+world%27). That works in all implementations.

From the linked issue: do you understand what this is describing? The link is broken for me.

---

_Comment by @not-my-profile on 2023-02-13 16:00_

Yes I understand what's being described. Instead of ``[`namespace-packages`]`` we would have to do e.g. ``[`namespace-packages`][namespace-packages]`` ... since that however is unnecessarily verbose (we wouldn't want it to show up in the `ruff rule` output), we'd have to employ a regex hack to automatically perform that substitution.

I am personally not interested in implementing regex hacks for deviations from well-established standards. From a website user standpoint I don't think the tool that generated the website should matter. Also note that if we want to strictly lint our Markdown that is much easier when it is CommonMark-compliant, since that is actually well defined.

---

_Comment by @charliermarsh on 2023-02-13 17:44_

> From a website user standpoint I don't think the tool that generated the website should matter.

I agree with this sentence (seems hard to disagree with!), but it's an inaccurate description of the change. Unless I'm mistaken, we're not just using a different tool to _generate _the website -- the entire UI of website itself is changing dramatically, since we're moving from Material to mdBook.

I'd prefer not to change this right now. Maybe we'll change our minds in the future. In the meantime, I'd rather find another solution for doc links. If you don't want to work on that, no worries, I'll figure something out.


---

_Comment by @not-my-profile on 2023-02-13 19:27_

I think from an UX perspective there is very little difference between mkdocs and mdBooks ... both have a sidebar on the left and a search on top ... they even have the same keyboard shortcut `s` to search ... though mdBook also lets you navigate to the previous/next page with the arrow keys. I'd say the most notable difference is the color theme ... which could be tweaked if deemed important.

---

_Comment by @charliermarsh on 2023-02-13 19:49_

#2867 is somewhat related, as we need a solution for that too.

---

_Comment by @not-my-profile on 2023-02-14 05:23_

Closing this since you're not interested.

---

_Closed by @not-my-profile on 2023-02-14 05:23_

---
