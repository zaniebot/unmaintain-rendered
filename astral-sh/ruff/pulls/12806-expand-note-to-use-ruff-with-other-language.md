```yaml
number: 12806
title: Expand note to use Ruff with other language server in Kate
type: pull_request
state: merged
author: tfardet
labels:
  - documentation
assignees: []
merged: true
base: main
head: kate-multiple-servers
created_at: 2024-08-11T21:05:34Z
updated_at: 2024-08-20T07:05:50Z
url: https://github.com/astral-sh/ruff/pull/12806
synced_at: 2026-01-12T15:55:42Z
```

# Expand note to use Ruff with other language server in Kate

---

_@tfardet_

## Summary

Provide instructions to use Ruff together with other servers in the Kate editor.
Because Kate does not support running multiple servers for the same language, one needs to use the ``python-lsp-server`` (pylsp) tool.


---

_@dhruvmanila reviewed on 2024-08-12 05:23_

Thank you for working on this!

I'm a bit hesitant on including this section with all the details as provided here. We had a section for `python-lsp-ruff` before the stable release of the Ruff language server (https://github.com/astral-sh/ruff/blob/7a7c601d5ed294a3c868b5e83f757105e0a189b8/docs/integrations.md#language-server-protocol-unofficial) but we removed it in favor of promoting the native language server. Another important difference with `python-lsp-ruff` is that it uses the `ruff` executable which means that the [server settings](https://docs.astral.sh/ruff/editors/settings) won't be considered if provided.

That said, what we could do is to expand on the `important` note section stating that if someone would like to use Ruff along with another language server, the `python-lsp` can be used as a language server along with the `python-lsp-ruff` plugin. We should also state that the Ruff plugin used here would be through the executable. You might want to paraphrase this.

What do you think about this?

---

_Label `documentation` added by @dhruvmanila on 2024-08-12 05:23_

---

_Comment by @tfardet on 2024-08-12 05:54_

Hi @dhruvmanila could you elaborate on the settings part?
As far as I can tell, you can provide a toml file via the [pylsp-ruff "config" option](https://github.com/python-lsp/python-lsp-ruff?tab=readme-ov-file#configuration). Do you mean that the ``ruff`` executable and the language server have different options, or did I misunderstand?

Otherwise, just let me tell you why I did it that way:

- I don't think many people are knowledgeable about tools like ruff, jedi, or pylsp, so providing a config file they can simply copy-paste and have things work (well, almost, since they need to specify the path) is desirable
- Quite frankly, I don't think anyone would want to use the current configuration instructions for Kate since they make it lose basic goto functionalities
- It also took me quite a while to figure this out, so I thought I would just save others the trouble by providing everything 😉
- using "details", it doesn't really take much space and I would not expect these instructions to be much of a maintenance burden

That being said, I understand if you do not want to risk that burden, or having people asking about how to use pylsp-ruff in your issue tracker.
In that case let me know if adding a disclaimer at the top of the details to tell people they should ask pylsp-ruff people if anything doesn't work would be enough, otherwise I'll just remove the details and paraphrase what you said, as requested.

---

_Assigned to @dhruvmanila by @MichaReiser on 2024-08-14 08:48_

---

_Comment by @dhruvmanila on 2024-08-14 09:38_

> As far as I can tell, you can provide a toml file via the [pylsp-ruff "config" option](https://github.com/python-lsp/python-lsp-ruff?tab=readme-ov-file#configuration). Do you mean that the `ruff` executable and the language server have different options, or did I misunderstand?

I mean the server settings as mentioned here: https://docs.astral.sh/ruff/editors/settings. Some of them controls the behavior of the server itself (`ruff.lint.enable`, etc.) while others overrides the config values (`ruff.lint.select`, etc.).

I think your reasoning makes sense but I'd rather prefer to re-use existing resources from other documentations. There have been some confusion in the past with how the server settings are suppose to work like https://github.com/astral-sh/ruff/issues/12778 and https://github.com/astral-sh/ruff/issues/12514 where the users were confused that the server settings weren't being considered when running the linter / formatter.

I think having a general guide on how to setup the `python-lsp-server` in Kate would be useful to more users while having it here would only benefit the Ruff users.
I think a _general_ user of `python-lsp-server` would find it more useful to refer to a guide on setting it up in Kate rather than a _Ruff_ user. It's not about maintenance burden but thinking on how much benefit does this provide to Ruff users. I hope this reasoning makes sense.

That said, I think it would be more useful to expand the note by informing the users that they could use `python-lsp-server` along with `python-lsp-ruff` to use Ruff with an existing language server along with relevant references like [`python-lsp-ruff` configuration options](https://github.com/python-lsp/python-lsp-ruff#configuration).

---

_Comment by @tfardet on 2024-08-14 10:00_

> I mean the server settings as mentioned here: https://docs.astral.sh/ruff/editors/settings. Some of them controls the behavior of the server itself (`ruff.lint.enable`, etc.) while others overrides the config values (`ruff.lint.select`, etc.).
> 
> I think your reasoning makes sense but I'd rather prefer to re-use existing resources from other documentations. There have been some confusion in the past with how the server settings are suppose to work like #12778 and #12514 where the users were confused that the server settings weren't being considered when running the linter / formatter.

OK, I think this should probably be mentioned in the note, then.
Since you have much more experience with this confusion issue, would you have an idea about how to phrase that?


> I think having a general guide on how to setup the `python-lsp-server` in Kate would be useful to more users while having it here would only benefit the Ruff users. I think a _general_ user of `python-lsp-server` would find it more useful to refer to a guide on setting it up in Kate rather than a _Ruff_ user. It's not about maintenance burden but thinking on how much benefit does this provide to Ruff users. I hope this reasoning makes sense.

As I mentioned, I don't think people know much about these tools and would expect them to have heard (like me) that ruff is super fast and so they would want to use it and would just arrive on the ruff docs without prior knowledge.

However, I  completely understand your reasoning and definitely agree that it would be better to have the detailed doc where it belongs (somewhere in the pylsp ecosystem) and just link to it in the ruff documentation.

> That said, I think it would be more useful to expand the note by informing the users that they could use `python-lsp-server` along with `python-lsp-ruff` to use Ruff with an existing language server along with relevant references like [`python-lsp-ruff` configuration options](https://github.com/python-lsp/python-lsp-ruff#configuration).

I'll propose something along those lines, then, and maybe I'll make another PR once I have added the detailed configuration somewhere in the pylsp documentation.

---

_Review requested from @dhruvmanila by @dhruvmanila on 2024-08-16 05:46_

---

_Review request for @dhruvmanila removed by @dhruvmanila on 2024-08-16 05:46_

---

_Review requested from @dhruvmanila by @dhruvmanila on 2024-08-16 05:46_

---

_@dhruvmanila approved on 2024-08-20 06:11_

I expanded the "important" note for the Kate setup guide with a workaround for using Ruff with another language server. Feel free to provide any thoughts on that. I'll merge this for now but happy to take any follow-ups if you have any in mind. Thanks for the suggestion and opening this PR.

---

_Renamed from "Instructions to use Ruff with other servers in Kate" to "Expand note to use Ruff with other language server in Kate" by @dhruvmanila on 2024-08-20 06:12_

---

_Merged by @dhruvmanila on 2024-08-20 06:18_

---

_Closed by @dhruvmanila on 2024-08-20 06:18_

---

_Comment by @tfardet on 2024-08-20 07:05_

OK, that seems reasonable! I'll try to propose something to python-lsp people, thanks!

---
