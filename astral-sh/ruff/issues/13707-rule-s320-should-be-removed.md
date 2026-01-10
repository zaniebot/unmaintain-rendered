```yaml
number: 13707
title: Rule S320 should be removed
type: issue
state: closed
author: pythonweb2
labels:
  - rule
assignees: []
created_at: 2024-10-10T21:12:46Z
updated_at: 2025-06-13T06:45:31Z
url: https://github.com/astral-sh/ruff/issues/13707
synced_at: 2026-01-10T11:09:55Z
```

# Rule S320 should be removed

---

_Issue opened by @pythonweb2 on 2024-10-10 21:12_

[This comment](https://github.com/astral-sh/ruff/pull/10154#issuecomment-1995783400) seems to have been overlooked (at least was not responded to).

If lxml is fixed, then from what I can understand of rule S320, it should be removed as well.

---

_Comment by @MichaReiser on 2024-10-14 16:53_

Do you have a reference that `lxml` is fixed. Checking `defusedxml`'s documentation it seems that lxml is still vulnerable to entity expansions which is what S320 warns about. https://pypi.org/project/defusedxml/

---

_Comment by @MichaReiser on 2024-10-14 17:05_

I also found https://github.com/PyCQA/bandit/issues/767 but it's very unclear to me what the recommendation is... 

---

_Label `rule` added by @MichaReiser on 2024-10-14 17:05_

---

_Comment by @pythonweb2 on 2024-10-14 17:08_

There is this section in the [FAQ page](https://lxml.de/FAQ.html):


> Some of the most famous excessive content expansion attacks use XML entity references. Luckily, entity expansion is mostly useless for the data commonly sent through web services and can simply be disabled, which rules out several types of denial of service attacks at once. This also involves an attack that reads local files from the server, as XML entities can be defined to expand into the content of external resources. Consequently, version 1.2 of the SOAP standard explicitly disallows entity references in the XML stream.
> 
> To disable entity expansion, use an XML parser that is configured with the option resolve_entities=False. Then, after (or while) parsing the document, use root.iter(etree.Entity) to recursively search for entity references. If it contains any, reject the entire input document with a suitable error response. In lxml 3.x, you can also use the new DTD introspection API to apply your own restrictions on input documents. Since version 5.x, lxml disables the expansion of external entities (XXE) by default. If you really want to allow loading external files into XML documents using this functionality, you have to explicitly set resolve_entities=True.


---

_Comment by @pythonweb2 on 2024-10-14 17:10_

The current lxml version is 5.0, which seems to be safe by default against entity expansion (since it disables it).

So, idk if ruff is able to check the version of lxml, or perhaps check to see if there are any places where `resolve_entities=True` is being used? That seems like that would be a more useful thing to check for.

---

_Comment by @MichaReiser on 2024-10-14 17:15_

> The current lxml version is 5.0, which seems to be safe by default against entity expansion (since it disables it).

Yeah, it *seems* but I would like some more confirmation before deprecating a rule. 

> So, idk if ruff is able to check the version of lxml, or perhaps check to see if there are any places where resolve_entities=True is being used? That seems like that would be a more useful thing to check for.

Checking the version is beyond what Ruff can do today but checking for `resolve_entities` is a possibility for a new rule

---

_Comment by @pawel-slowik on 2024-11-17 20:39_

> The current lxml version is 5.0, which seems to be safe by default against entity expansion (since it disables it).

This is not entirely true. The 5.0 version only disables the expansion of **external** entities by default:

<https://lxml.de/FAQ.html#how-do-i-use-lxml-safely-as-a-web-service-endpoint>
> Since version 5.x, lxml disables the expansion of **external** entities (XXE) by default.

<https://lxml.de/5.0/changes-5.0.0.html>
> LP#1742885: lxml no longer expands **external** entities (XXE) by default to prevent the security risk of loading arbitrary files and URLs.

The default value for the `resolve_entities` argument has changed from `True` to `'internal'`, **not to `False`**: <https://github.com/lxml/lxml/commit/b38cebf2f846e92bd63de4488fd3d1c8b568f397#diff-d96803514b65b282182a7d8d4ab1c076c8e7726761aecf5bb8b5eb8355d14324R1581>

And it is still `'internal'` as of 5.3.0: <https://github.com/lxml/lxml/blob/lxml-5.3.0/src/lxml/parser.pxi#L1600>

The change protects from a file inclusion attack, but not from quadratic blowup.

---

_Comment by @JeremyVriens on 2025-02-07 09:27_

Bandit removed both S410 and S320 from their blacklist since Bandit 1.8.1 (source: https://github.com/PyCQA/bandit/releases/tag/1.8.1). 
Since S410 has already been removed from Ruff (source: https://github.com/astral-sh/ruff/pull/10154), my proposal is to also remove S320.

---

_Added to milestone `v0.10` by @MichaReiser on 2025-02-07 09:49_

---

_Comment by @ngnpope on 2025-02-14 16:18_

> Checking defusedxml's documentation it seems that lxml is still vulnerable to entity expansions which is what S320 warns about

The documentation was changed on a pre-release version. Alas, it seems that defusedxml is now largely unmaintained.

- https://pypi.org/project/defusedxml/0.7.1/#defusedxml-lxml
- https://pypi.org/project/defusedxml/0.8.0rc2/#defusedxml-lxml

---

_Removed from milestone `v0.10` by @MichaReiser on 2025-03-12 14:52_

---

_Added to milestone `v0.11` by @MichaReiser on 2025-03-12 14:52_

---

_Comment by @MichaReiser on 2025-03-12 14:53_

I put up a PR to deprecate the rule (https://github.com/astral-sh/ruff/pull/16680). We should remove it in Ruff 0.11 (or 0.12)

---

_Comment by @domWalters on 2025-04-13 19:51_

I think this issue can be closed now?

S320 is unavailable in my installed copy of `0.11.5`.

---

_Comment by @ntBre on 2025-04-14 00:36_

S320 was _deprecated_ in 0.11 and its removal is on the 0.12 milestone, so we should probably leave this open until 0.12. I'm curious what you mean about it being unavailable, though. When I run 0.11.5, I get this output:

```
$ ruff --version
ruff 0.11.5
$ ruff check --select S320
warning: Rule `S320` is deprecated and will be removed in a future release.
warning: No Python files found under the given path(s)
All checks passed!
```

which reports it as deprecated, as expected. When I run this on the example from the docs, I also get a diagnostic.


---

_Comment by @domWalters on 2025-05-05 19:56_

My apologies, I got confused as my `ruff` LSP was recommending to remove the `# noqa: S320` comments.

---

_Comment by @MichaReiser on 2025-06-13 06:45_

Closing as this will ship with 0.12

---

_Closed by @MichaReiser on 2025-06-13 06:45_

---
