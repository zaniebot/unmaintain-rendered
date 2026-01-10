```yaml
number: 4479
title: "Request: adding dlint"
type: issue
state: open
author: AbdealiLoKo
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-05-17T20:45:13Z
updated_at: 2024-04-09T22:18:25Z
url: https://github.com/astral-sh/ruff/issues/4479
synced_at: 2026-01-10T11:09:47Z
```

# Request: adding dlint

---

_Issue opened by @AbdealiLoKo on 2023-05-17 20:45_

I use dlint in my project.
Supporting the rules from dlint - https://github.com/dlint-py/dlint would be great

---

_Renamed from "Support for dlint" to "Request: adding dlint" by @AbdealiLoKo on 2023-05-17 20:45_

---

_Comment by @qdegraaf on 2023-05-17 22:15_

Started on this and listed the rules to port. Will give it a go as I find time. Most of the rules seem easy enough, some are no longer relevant in Python 3 and can't be ported. Will see how far I get 

---

_Label `plugin` added by @charliermarsh on 2023-05-18 00:44_

---

_Comment by @qdegraaf on 2023-05-20 16:43_

The following rules are covered in https://github.com/charliermarsh/ruff/pull/4482 which is ready for final review:
- [x] DUO119: BadShelveUse (Renamed to ShelveUse in Ruff)
- [x] DUO120: BadMarshalUse (Renamed to MarshalUse in Ruff)
- [x] DUO104: BadEvalUse (Renamed to EvalUse in Ruff)
- [x] DUO105: BadExecUse (Renamed to ExecUse in Ruff)
- [x] DUO110: BadCompileUse (Renamed to CompileUse in Ruff)

Left TODO are:

- [ ] DUO102: BadRandomGeneratorUse
- [ ] DUO103: BadPickleUse
- [ ] DUO106: BadOsUse
- [ ] DUO107: BadXmlUse
- [ ] DUO108: BadInputUse
- [ ] DUO109: BadYamlUse
- [ ] DUO111: BadSysUse
- [ ] DUO112: BadZipfileUse
- [ ] DUO115: BadTarfileUse
- [ ] DUO116: BadSubprocessUse
- [ ] DUO121: BadTempfileUse
- [ ] DUO122: BadSslModuleAttributeUse
- [ ] DUO123: BadRequestsUse
- [ ] DUO124: BadXmlrpcUse
- [ ] DUO127: BadDuoClientUse
- [ ] DUO128: BadOneLoginKwargUse
- [ ] DUO129: BadOneLoginModuleAttributeUse
- [ ] DUO130: BadHashLibUse
- [ ] DUO131: BadUrllib3ModuleAttributeUse
- [ ] DUO132: BadCryptographyModel
- [ ] DUO132: BadUrllibKwargUse
- [ ] DUO133: BadPycryptoUse
- [ ] DUO135: BadDefusedXmlUse
- [ ] DUO136: BadXmlsecModuleAttributeUse
- [ ] DUO137: BadItsDangerousKwargUse
- [ ] DUO138: BadReCatastrophicUse

I will continue in small batches right after the first PR is merged, but these TODOs can then easily be split among multiple interested contributors.

---

_Comment by @charliermarsh on 2023-05-22 14:33_

Hmm, it looks like many of these are implemented in Bandit (and in Ruff's Bandit reimplementation). I think it's worth auditing the list to understand which of these should be considered aliases of existing rules before implementing any further diagnostics.

---

_Comment by @qdegraaf on 2023-05-22 14:58_

Good call, will cease further work until then. @AbdealiLoKo do you maybe have a list of rules that you use in DLint that aren't covered by other plugins already in Ruff? 

---

_Comment by @AbdealiLoKo on 2023-05-24 08:47_

I haven't compared them myself.
We do use both bandit and dlint in our application and we seem to just run both (even if there are similar rules)

---

_Comment by @bastimeyer on 2023-07-04 19:04_

> * DUO138: BadReCatastrophicUse

https://github.com/dlint-py/dlint/blob/master/docs/linters/DUO138.md

This one is particularly interesting. Does ruff implement similar rules for checking regular expressions which are vulnerable to ReDoS attacks? [I couldn't find anything](https://github.com/search?q=repo%3Aastral-sh%2Fruff%20ReDoS), unless I'm blind. Having such rules is pretty important considering that it's not just about code quality but actually about critical bugs.

DUO138 implementation:

- https://github.com/dlint-py/dlint/blob/307b301cd9e280dcd7a7f9d5edfda3d58e4855f5/dlint/linters/bad_re_catastrophic_use.py#L69
- https://github.com/dlint-py/dlint/tree/307b301cd9e280dcd7a7f9d5edfda3d58e4855f5/dlint/redos

---

_Comment by @qdegraaf on 2023-07-06 16:57_

Yeah I can't find anything like that yet either. I'll see if I can find some time to make that the first DLint rule and can audit the rest of the list after.

---

_Comment by @qdegraaf on 2023-07-08 21:40_

> > * DUO138: BadReCatastrophicUse
> 
> https://github.com/dlint-py/dlint/blob/master/docs/linters/DUO138.md
> 
> This one is particularly interesting. Does ruff implement similar rules for checking regular expressions which are vulnerable to ReDoS attacks? [I couldn't find anything](https://github.com/search?q=repo%3Aastral-sh%2Fruff%20ReDoS), unless I'm blind. Having such rules is pretty important considering that it's not just about code quality but actually about critical bugs.
> 
> DUO138 implementation:
> 
>     * https://github.com/dlint-py/dlint/blob/307b301cd9e280dcd7a7f9d5edfda3d58e4855f5/dlint/linters/bad_re_catastrophic_use.py#L69
> 
>     * https://github.com/dlint-py/dlint/tree/307b301cd9e280dcd7a7f9d5edfda3d58e4855f5/dlint/redos

I've opened a [draft PR](https://github.com/astral-sh/ruff/pull/5614) which adds the `regex_syntax` crate and all the boilerplate. Actually adding the logic/check will be a bit more difficult. `dlint` relies on `sre_parser` and builds an OP tree around that. In Rust, if we don't want to build a regex parser from the ground up, we have to rely on `regex_syntax` as far as I can tell which uses a different representation for parsed regex strings. In order to cover the same ground as `dlint` I will have to more thoroughly understand when catastrophic backtracking can happen and then build similar checks using the representation provided by the crate. Unsure how far I'll get, PR is open so any feedback/input/help is welcomed.

EDIT: Closed the PR for now. I think we need an overall plan for how to handle Python regex in Ruff before I can make sensible choices on what to do with the particulars of this specific rule. 

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @khanfarhan10 on 2024-04-09 21:28_

The ReDoS attack is a notorious bug and I've faced this attack myself rather unexplicably.

Can we not add this plugin as an additional and optional dependency to ruff and call `dlint` via a subprocess equivalent to Ruff.

I understand that this may slow down ruff altogether again, but it shall be a user opted dependency and won't be installed for everyone.

I wonder if we could implement something like this for now and move ahead with migrating the python code to RUST gradually in phases?

---

_Comment by @khanfarhan10 on 2024-04-09 21:31_

@charliermarsh @qdegraaf please convey your thoughts if we are going to add this to Ruff. 

P. S. - would contribute myself, but my rust knowledgebase is NIL.

---

_Comment by @zanieb on 2024-04-09 22:18_

It's not feasible for us to add Python tools as dependencies and call into them — we are very unlikely to do so. Not only is it slow, but each has an entirely different diagnostic system than us. I don't think doing so would meet our bar for user experience.

---
