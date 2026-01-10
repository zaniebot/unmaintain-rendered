```yaml
number: 10092
title: "question: Private classifiers and their effect on alternate registries"
type: issue
state: closed
author: pawamoy
labels:
  - question
assignees: []
created_at: 2024-12-22T13:27:07Z
updated_at: 2024-12-23T09:10:32Z
url: https://github.com/astral-sh/uv/issues/10092
synced_at: 2026-01-10T04:36:21Z
```

# question: Private classifiers and their effect on alternate registries

---

_Issue opened by @pawamoy on 2024-12-22 13:27_

I recently learned that PyPI will reject package uploads if the package metadata has one or more classifiers starting with `Private ::`. I got interested because some of my projects have private version that should never end up on PyPI. But I was left wondering if I could really use this classifier, because my users must be able to upload the private packages to their own private registries, if they want to, and I was afraid the classifier would cause rejection in these alternate registries too. This is not addressed in official docs, but in [your own docs](https://docs.astral.sh/uv/guides/publish/) you do say:

> It does not affect security or privacy settings on alternative registries.

That's interesting! I am now curious to know if you actually checked common alternate registries (Google Cloud Platform Artifact Registry, JFrog's Artifactory, pypiserver, devpi, others?) to confirm whether they actually don't care about such classifiers 🙂 

I have found https://github.com/astral-sh/uv/issues/8214, in which I also posted my use-case, though didn't want to derail the conversation for this specific question.

You have great documentation, thank you for it!

---

_Comment by @ncoghlan on 2024-12-23 02:43_

Private repository servers do NOT restrict the permitted trove classifiers (if they offer a classifier filtering capability at all, it's an opt-in feature when setting up a specific repository).

That's why the `Private :: ...` classifier convention emerged: PyPI checks it, nobody else does. The lack of documentation in the other repository server implementations is because not checking is the assumed default - PyPI checking them is the exceptional case (and hence the documented one).

For the docs reference:

* There's no mention of filtering uploads based on classifier values in the field specification: https://packaging.python.org/en/latest/specifications/core-metadata/#classifier-multiple-use
* PyPI docs explicitly mention the rejection of all `Private :: ...` classifiers: https://pypi.org/classifiers/


---

_Comment by @zanieb on 2024-12-23 03:48_

Glad to hear you like the documentation :)

I have not personally checked other registries, but yeah from what I understand they will ignore the classifier. Of course, some registry can do whatever it wants because the specification doesn't cover this topic.

---

_Label `question` added by @zanieb on 2024-12-23 03:48_

---

_Comment by @pawamoy on 2024-12-23 09:10_

Thank you @ncoghlan, @zanieb! The point of view of "it's not standard, so alternate registries should not implement that" is reassuring, but at the same time, yeah they can do whatever they want 😅 

OK I think we won't get further than this, closing! Thank you again ❤ 

---

_Closed by @pawamoy on 2024-12-23 09:10_

---
