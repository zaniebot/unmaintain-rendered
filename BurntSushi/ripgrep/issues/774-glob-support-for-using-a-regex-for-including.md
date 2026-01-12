```yaml
number: 774
title: "glob: Support for using a regex for including/excluding an file"
type: issue
state: closed
author: keegancsmith
labels:
  - question
assignees: []
created_at: 2018-02-05T06:44:40Z
updated_at: 2018-02-16T12:22:30Z
url: https://github.com/BurntSushi/ripgrep/issues/774
synced_at: 2026-01-12T16:13:22Z
```

# glob: Support for using a regex for including/excluding an file

---

_@keegancsmith_

Regular expressions are more powerful than globs. I would like to use the same language for both matching text and filepath patterns. I believe as it is the glob pattern is compiled down into a regular expression, so this seems technically feasible. Further motivation is that there are useful collections of regular expressions in the linguist project which match filenames. EG https://github.com/github/linguist/blob/master/lib/linguist/vendor.yml I could imagine this would nicely complement the `--type` feature of rg.


---

_Comment by @BurntSushi on 2018-02-05 12:03_

Could you please provide concrete examples where you need to use regexes instead of globs? I briefly skimmed your link, but a lot of those look like they can specified as globs, so I'm not sure how you want me to interpret that.

Could you also please say where you would expect to be able to use regexes? In `.ignore` files? With the `-g/--glob` flag? When defining types? And how would you tell ripgrep to use regexes instead of globs?

Another note here is that in order to have full support for `.hgignore`, I believe we need to support regexes in `.hgignore` files, although the extent to which that is possible isn't clear because 100% support would likely require using precisely the regex engine that Mercurial uses. Mercurial I believe permits directives in their `.hgignore` files to switch between regexes and globs.

I will say that in general I'm not particularly excited about this feature. Globs are, by far, the standard when it comes to writing patterns to match file paths. The amount of work required to support regexes would be substantial and would carry with it a maintenance hazard. It will also complicate ripgrep's UX, which is a problem on its own.

---

_Label `question` added by @BurntSushi on 2018-02-05 12:03_

---

_Comment by @keegancsmith on 2018-02-05 14:13_

Examples of text search tools which use regex for path filtering:
- [chromium codesearch](https://cs.chromium.org/)
- [zoekt](https://github.com/google/zoekt) example instance at https://cs.bazel.build/

The reason I like it is the model in my head is simpler. But most of the times, I agree globs are more appropriate especially given one has to escape `.` in regexes. I was playing around with some code which wraps `rg` with zoekt's query parser which prompted me to file this question.

I also agree this is likely to be a very rarely used feature, so on reflection is not worth complicating ripgrep's UX over.

---

_Comment by @BurntSushi on 2018-02-05 14:28_

> Examples of text search tools which use regex for path filtering:

Sorry, that wasn't what I meant to ask for. Allow me to clarify. What I'd like to see are specific examples of regexes that you need to use with ripgrep that you otherwise couldn't accomplish with globs. That is, what are the specific use cases?

---

_Comment by @BurntSushi on 2018-02-05 14:29_

I guess what you're saying is that you want ripgrep to support regexes so that it can inter-operate with other tools that use regexes for file filtering. I understand that desire, but I think it is somewhat niche, and I'm not sure I could be convinced to support that feature for that reason alone.

---

_Comment by @keegancsmith on 2018-02-05 14:46_

yup, I agree it is niche. Off-hand I can't think of a recent example where I couldn't achieve what I wanted via just chaining globs.

---

_Closed by @keegancsmith on 2018-02-16 12:22_

---
