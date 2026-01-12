```yaml
number: 1371
title: Feature - Search all casing permutations
type: issue
state: closed
author: j-martin
labels:
  - enhancement
assignees: []
created_at: 2019-09-11T02:33:53Z
updated_at: 2023-10-09T23:42:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1371
synced_at: 2026-01-12T16:13:23Z
```

# Feature - Search all casing permutations

---

_@j-martin_

### Describe your question, feature request, or bug.

The idea is to be able to expand beyond the concept of smart casing. More often than enough, I need to search code with different casing. For example, `PascalCasing` (Java, Go, Python classes, etc.) `camelCasing` (Java, Go, etc.), `snake_casing` (python, ruby, etc.), `kebab-casing` (clojure, etc.). 

It would be wonderful if there was a quick way to search `aCoolVariable`, `a-cool-variable` or `a_cool_variable` and have it to expand as a case insensitive regex like `a[_-]?cool[_-]?variable`.

The feature would be activated with a flag of some sort.

---

_Comment by @BurntSushi on 2019-09-11 02:39_

I suspect something like this would also be quite useful for languages like Nim. (It was suggested in a PR somewhere, but nobody made a separate ticket for it.)

I think the meat if this work requires a thorough specification for how the regex pattern is transformed. What you have so far is just a start. Does someone want to take a crack at this?

An issue with this feature is that we likely won't be able to support it when PCRE2 is used, since PCRE2 does not expose tools for correct syntax transformations.

---

_Comment by @BurntSushi on 2020-03-15 16:07_

I'm still willing to have something like this in ripgrep. But the UX story here is really the hard part. Some questions off the top of my head:

* What happens when the given pattern is a regex and not just a literal? (Maybe ripgrep should return an error?)
* What are the precise set of transformations that ripgrep performs? Is this configurable? (I want to say "no.")
    * But perhaps we do need this. e.g., Languages like Nim probably have a concrete specification on which variable names are equivalent, which might not include literally every possible transform one might want in a feature like this. So maybe we need a special `--nim` flag?
* What happens when multiple patterns are given? I guess they should all be transformed.

There is definitely a potential here for the transformed regexes to blow up in size, thus leading to an overall degradation in performance. In particular, I suspect this will either render literal optimizations inert or otherwise much less effective. But still, I see this as being a useful feature.

---

_Label `enhancement` added by @BurntSushi on 2020-03-15 16:07_

---

_Comment by @BurntSushi on 2020-03-15 16:08_

cc @dom96 

---

_Comment by @j-martin on 2020-03-16 04:06_

> What happens when the given pattern is a regex and not just a literal? (Maybe ripgrep should return an error?)

I would expect it to be an alternative or similar to fix strings (`-F`) and would not support regex at all. 

As you noted handling regex would make things substantially more complicated (and potentially bad performance), but also if someone wants regex I would expect them to want to control these casing permutations themselves.

> What are the precise set of transformations that ripgrep performs? Is this configurable? (I want to say "no.")

No, as far as I am concerned.

> What happens when multiple patterns are given? I guess they should all be transformed.

Yes, I think so.

---

_Comment by @kastiglione on 2020-03-16 06:53_

I've wanted a feature like this for a while. 

My experience: When I want this, I write my search as `a.?cool.?variable`. I have smart case enabled by default so this matches aCoolCariable, a_cool_variable, etc. It also catches the use as a phrase, in comments. This could produce false positives, but in practice  (for me) I can't recall a single time that it happened. I also can't remember combining such a search with additional regex. The only thing I'd want to combine it with is `-w`.

---

_Comment by @BurntSushi on 2020-03-17 21:54_

@kastiglione Could you maybe restate that in terms of what this feature should provide? For example, I imagine you'd want to say `rg --smart-ident acoolvariable` and have that rewritten to something like `rg a.?c.?o.?o.?l.?v.?a.?r.?i.?a.?b.?l.?e.?`, which isn't quite the same as what you're writing. The latter will match a lot more. I don't think ripgrep can be much smarter here. It kind of has to allow extract characters at any position between the letters.

I guess we can be a bit more restrictive. e.g.,

`bar` -> `(?i)[-_]?b[-_]?a[-_]?r[-_]?`

Does this look right to folks? I think it's at least required to support the Nim case, for example.

---

_Comment by @j-martin on 2020-03-17 22:17_

@BurntSushi I was hoping for a smarter heuristic as far as deciding where to put the separators, but it seems totally fine as an MVP and should work in most cases. If not it would be the time for the user to write the regex by hand.

I would definitely go with the more restrictive regex `[-_]` since the `.?` would pickup `:` or `.` which would create issues with method calls and whatnot.  As noted by @kastiglione `--word-regexp` would solve the issue, but I don't think it should be required. After all, those characters are themselves word boundaries.

---

_Comment by @dom96 on 2020-03-17 23:26_

> What happens when the given pattern is a regex and not just a literal? (Maybe ripgrep should return an error?)

I agree with @j-martin in that I don't think this feature should be supported in conjunction with regex.

> Languages like Nim probably have a concrete specification on which variable names are equivalent, which might not include literally every possible transform one might want in a feature like this. So maybe we need a special --nim flag?

This is true. But for a generic tool like ripgrep I think it's fine to not offer this. It depends on the design philosophy of ripgrep though, do you think it makes sense to implement language-specific features? If not, we can just as well create a wrapper script/program around ripgrep which builds a regex pattern and passes it to ripgrep. That way the burden of maintenance of this feature isn't on you :)

In general, being able to search in a style insensitive manner is really a useful feature for users of all languages and it's really nice to see it being considered as a feature in ripgrep!


---

_Comment by @BurntSushi on 2023-10-09 23:42_

Revisiting this, I think I'm going to pass on this. It looks like this feature probably has some pretty tricky UX questions, and it isn't clear to me how broadly applicable it is. If someone can come up with something relatively simple and wants to submit a patch, I might be open to that. If so, feel free to ask to re-open the issue here with a proposal.

---

_Closed by @BurntSushi on 2023-10-09 23:42_

---
