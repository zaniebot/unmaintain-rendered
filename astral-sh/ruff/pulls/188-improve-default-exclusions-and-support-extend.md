```yaml
number: 188
title: Improve default exclusions and support extend-exclude
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/extend
created_at: 2022-09-14T21:30:12Z
updated_at: 2022-09-15T02:52:53Z
url: https://github.com/astral-sh/ruff/pull/188
synced_at: 2026-01-12T15:55:04Z
```

# Improve default exclusions and support extend-exclude

---

_@charliermarsh_

This PR changes `--exclude` to use regular expressions rather than globs, which matches Black's behavior. We also now match Black's default list of exclusions, and support a `--extend-exclude` command-line argument and `pyproject.toml` field.

Resolves #176.


---

_Comment by @charliermarsh on 2022-09-14 21:30_

\cc @jack1142 

---

_Comment by @Jackenmen on 2022-09-14 21:45_

Could you do something like Black does where multi-line regexes are treated as if they had a VERBOSE flag set? You can see an example of usage here:
https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#configuration-format

It could maybe make some sense to have a single regex instead of a list of them for performance but it's not like a user can't just use a single regex when you give them ability to put regexes in a list. Having it be a string may make it more obvious it's a regex though, not sure.

I also wonder whether it would make sense to have something in similar vein to `--force-exclude` which is present in both flake8 and black as it can be useful with editor integration and other tools. Might make more sense to make a separate feature request for it though, it's just something I thought about just now :smile: 

---

_Comment by @charliermarsh on 2022-09-14 21:47_

I did consider that but I kind of hate that setup (lol), I always found it really confusing and hard to get right as a user. But my preferences aren't that important here. Do you think that would be more approachable / conventional for users?


---

_Comment by @Jackenmen on 2022-09-14 21:48_

Is this in reference to the verbose syntax or some other part of my comment?

---

_Comment by @charliermarsh on 2022-09-14 22:09_

Ah sorry -- not a clear response. It's in reference to the multi-line regex; as in, I tend to prefer a list of patterns to a multi-line regex.


---

_Comment by @Jackenmen on 2022-09-14 22:26_

I think a list of globs is more approachable than a regex but way more limiting. As for a list of regexes vs multi-line regex, I'm not really sure. I've seen Black and pre-commit accept a single regex where the former auto-switches to verbose when it sees a multi-line string and the latter requires you to do it using standard inline flags (prefixing the regex with `(?x)`, not sure if Rust's regex engine supports it?).
It varies between tools - flake8 and isort offer a list of globs, pylint offers a list of regexes like you do in this PR.

I like the verbose flag for regexes so I might not really be objective here. I find it somewhat less clunky than specifying a list of regexes (when I use a list I sort of expect to just be able to specify globs and not have to escape dots) but as I said earlier this kind of implementation is not unheard of (pylint does it).

By the way, I think it would be worth documenting how files are matched against the specified patterns so that the user knows whether they need to put `^` (e.g. `re.match()` vs `re.search()`) and against what kind of path format it's matched against (e.g. is it path relative to project directory or something else).

---

_Comment by @charliermarsh on 2022-09-14 22:30_

Thanks, that's really helpful (and very impressed by your breadth of knowledge). I'm wondering if I should just revert back to a list of globs, which seems simplest...


---

_Comment by @charliermarsh on 2022-09-14 23:54_

Ok, my plan here is to just mimic isort and flake8 -- so you can provide a list of globs, and we'll check both the basename and absolute path. The logic looks like this:

![Screen Shot 2022-09-14 at 7 53 49 PM](https://user-images.githubusercontent.com/1309177/190282639-e465aa51-f73e-49b7-909b-3f3cee16207a.png)


---

_Merged by @charliermarsh on 2022-09-15 02:21_

---

_Closed by @charliermarsh on 2022-09-15 02:21_

---

_Branch deleted on 2022-09-15 02:21_

---

_Comment by @Jackenmen on 2022-09-15 02:34_

I missed your comment about this, I'll try testing it out tomorrow. Mostly curious about how the matching against absolute path vs basename works in particular I want to check whether matching it against two different strings makes it difficult to do some things.

I have a question about an idea from my earlier comment:
> I also wonder whether it would make sense to have something in similar vein to --force-exclude which is present in both flake8 and black as it can be useful with editor integration and other tools. Might make more sense to make a separate feature request for it though, it's just something I thought about just now :smile:

Should I make a new issue for it?

---

_Comment by @charliermarsh on 2022-09-15 02:52_

Yeah new issue would be great. Right now, I think the excludes "overpower" the files provided on the command-line so we basically have --force-exclude behavior for all excludes. (So, I'll need to change files passed directly to "override" excludes, and then add --force-exclude.)


---
