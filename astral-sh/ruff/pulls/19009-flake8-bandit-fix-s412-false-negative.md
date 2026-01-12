```yaml
number: 19009
title: "[`flake8-bandit`] Fix `S412` false negative"
type: pull_request
state: closed
author: MeGaGiGaGon
labels:
  - rule
  - preview
assignees: []
base: main
head: fix-S412-false-negative
created_at: 2025-06-28T06:21:12Z
updated_at: 2025-07-09T18:34:14Z
url: https://github.com/astral-sh/ruff/pull/19009
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-bandit`] Fix `S412` false negative

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR fixes a false negative in [suspicious-httpoxy-import (S412)](https://docs.astral.sh/ruff/rules/suspicious-httpoxy-import/#suspicious-httpoxy-import-s412) if the suspicious import is imported directly, ie `import wsgiref.handlers.CGIHandler` and `import twisted.web.twcgi.CGIScript`

## Test Plan

<!-- How was it tested? -->

Added additional cases to test file

---

_Comment by @github-actions[bot] on 2025-06-28 06:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-06-30 14:06_

I think the documentation might just be wrong. I tried importing the `twisted` example directly, and it didn't work:

```pycon
>>> import twisted.web.twcgi.CGIScript
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    import twisted.web.twcgi.CGIScript
ModuleNotFoundError: No module named 'twisted.web.twcgi.CGIScript'; 'twisted.web.twcgi' is not a package
>>> import twisted.web.twcgi
>>> twisted.web.twcgi.CGIScript
<class 'twisted.web.twcgi.CGIScript'>
```

I didn't try `wsgiref`, but I don't think you can directly import it either.

---

_Comment by @MeGaGiGaGon on 2025-06-30 17:11_

Looking at the docs, it looks like you are right:
[`twisted.web.twcgi`](https://docs.twisted.org/en/stable/api/twisted.web.twcgi.html) lists [`CGIScript`](https://docs.twisted.org/en/stable/api/twisted.web.twcgi.CGIScript.html) as a class
[`wsgiref.handlers`](https://docs.python.org/3/library/wsgiref.html#module-wsgiref.handlers) also lists [`CGIHandler`](https://docs.python.org/3/library/wsgiref.html#wsgiref.handlers.CGIHandler) as a class
So these two rules still have a false positive, just a different one than I first thought: https://play.ruff.rs/d56653d2-80ff-4af4-adfa-390008cc39b4
```py
from twisted.web.twcgi import CGIScript  # S412

import twisted.web.twcgi
twisted.web.twcgi.CGIScript  # no lint

from wsgiref.handlers import CGIHandler  # S412

import wsgiref.handlers
wsgiref.handlers.CGIHandler  # no lint
```
> [!NOTE]
> This only applies to `S409`, the rest of the lints in `suspicious_imports` are for modules, so they can't be used like this

For fixing this, I see two possible paths:
- Move the lint from targeting the classes to their parent modules
  - This looks like a good option for `twisted.web.twcgi.CGIScript`, since in the docs all of `twisted.web.twcgi`'s members are `CGI`-related
  - I'm not sure how good of an option this is for `wsgiref.handlers.CGIHandler`, since `wsgiref.handlers` also has stuff for `WSGI`, and I don't know if that's the same thing as `CGI`.
- Keep the lints as they are, but also add a check for the usage of these classes as well
  - Might want to move the lint in this case? Since it wouldn't fit in that well with the rest of the `suspicious_imports` then
  - Might also want to add more classes to the lint in this case, since I see more `CGI` related things in the docs for both `twisted.web.twcgi` and `wsgiref.handlers`

---

_Comment by @ntBre on 2025-06-30 17:33_

Could you check what the upstream linter does? It's not the most satisfying answer because I think we could be more robust here by using either of your suggestions, but I'm tempted just to update the docs and leave the code alone if this is how the upstream linter works.

I think checking the qualified name at the usage site rather than at the import is how the airflow rules work, for example, which I think aligns with your second suggestion. That would probably be the best solution, but I think we'd need to apply it to all of these rules.

---

_Label `rule` added by @ntBre on 2025-06-30 17:33_

---

_Label `preview` added by @ntBre on 2025-06-30 17:33_

---

_Comment by @MeGaGiGaGon on 2025-06-30 17:58_

In `flake8-bandit`, it doesn't have any of the actual checking code, but the tests are pretty clear that they do support the non-direct-import version:
https://github.com/tylerwince/flake8-bandit/blob/main/tests/httpoxy_cgihandler.py
```py
import requests
import wsgiref.handlers

def application(environ, start_response):
    r = requests.get('https://192.168.0.42/private/api/foobar', timeout=30)
    start_response('200 OK', [('Content-Type', 'text/plain')])
    return [r.content]

if __name__ == '__main__':
    wsgiref.handlers.CGIHandler().run(application)
```
https://github.com/tylerwince/flake8-bandit/blob/main/tests/httpoxy_twisted_script.py
```py
from twisted.internet import reactor
from twisted.web import static, server, twcgi

root = static.File("/root")
root.putChild("login.cgi", twcgi.CGIScript("/var/www/cgi-bin/login.py"))
reactor.listenTCP(80, server.Site(root))
reactor.run()
```
I think the functionality is coming from https://github.com/PyCQA/bandit, where it defines `B412` here (`flake8-bandit` changes the prefix from `B` to `S` since `flake8-bugbear` was first):
https://github.com/PyCQA/bandit/blob/main/bandit/blacklists/imports.py#L375-L389
```py
    sets.append(
        utils.build_conf_dict(
            "import_httpoxy",
            "B412",
            issue.Cwe.IMPROPER_ACCESS_CONTROL,
            [
                "wsgiref.handlers.CGIHandler",
                "twisted.web.twcgi.CGIScript",
                "twisted.web.twcgi.CGIDirectory",
            ],
            "Consider possible security implications associated with "
            "{name} module.",
            "HIGH",
        )
    )
```
And checks for it in imports, import froms, and calls: `return {"Import": sets, "ImportFrom": sets, "Call": sets}`. It also has the same tests as `flake8-bandit`. I couldn't figure how how it actually does the config -> test generation for use inside the actual runner.

---

_Comment by @ntBre on 2025-06-30 18:09_

Ah nice, I made it to `imports.py` but didn't see how it did the config to actual check transform either. It sounds like we may need a bigger update to these rules.

Testing empirically, it looks like they do flag both imports and calls:

```shell
$ cat <<EOF | uvx --with flake8-bandit flake8 --select S -
from twisted.web.twcgi import CGIScript  # S412
import twisted.web.twcgi  # ok
from wsgiref.handlers import CGIHandler  # S412
import wsgiref.handlers  # ok


twisted.web.twcgi.CGIScript()  # S412
wsgiref.handlers.CGIHandler()  # S412
CGIScript()  # S412
EOF
Unable to find qualified name for module: stdin
stdin:1:1: S412 Consider possible security implications associated with CGIScript module.
stdin:3:1: S412 Consider possible security implications associated with CGIHandler module.
stdin:7:1: S412 Consider possible security implications associated with twisted.web.twcgi.CGIScript module.
stdin:8:1: S412 Consider possible security implications associated with wsgiref.handlers.CGIHandler module.
stdin:9:1: S412 Consider possible security implications associated with twisted.web.twcgi.CGIScript module.
```

---

_Comment by @MeGaGiGaGon on 2025-06-30 18:56_

It looks like `flake8-bandit` does lint on the module "uses", but only if you structure it as the module itself being called. 
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
import xml.dom

xml.dom.minidom()
PS ~\Desktop\New_folder\ruff>uvx --with flake8-bandit flake8 --select S issue.py
Unable to find qualified name for module: issue.py
issue.py:3:1: S408 Using xml.dom.minidom to parse untrusted XML data is known to be vulnerable to XML attacks. Replace xml.dom.minidom with the equivalent defusedxml package, or make sure defusedxml.defuse_stdlib() is called.
PS ~\Desktop\New_folder\ruff>py issue.py
Traceback (most recent call last):
  File "~\Desktop\New_folder\ruff\issue.py", line 3, in <module>
    xml.dom.minidom()
    ^^^^^^^^^^^^^^^
AttributeError: module 'xml.dom' has no attribute 'minidom'
```
Using things from inside the module don't get the lint (assuming they aren't part of another lint)
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
import xml.dom.minidom

xml.dom.minidom.Node()
PS ~\Desktop\New_folder\ruff>uvx --with flake8-bandit flake8 --select S issue.py
Unable to find qualified name for module: issue.py
issue.py:1:1: S408 Using xml.dom.minidom to parse untrusted XML data is known to be vulnerable to XML attacks. Replace xml.dom.minidom with the equivalent defusedxml package, or make sure defusedxml.defuse_stdlib() is called.
PS ~\Desktop\New_folder\ruff>py issue.py
PS ~\Desktop\New_folder\ruff>
```

---

_Review requested from @ntBre by @MichaReiser on 2025-07-07 12:55_

---

_Comment by @ntBre on 2025-07-09 17:19_

My read of the thread here is that the changes in this PR aren't really correct, and we should instead revise the rule more thoroughly. Is that right?

If that's the case, we might want to close this PR and open a follow-up issue summarizing our findings here, unless you'd rather repurpose this PR.

---

_Comment by @MeGaGiGaGon on 2025-07-09 17:31_

Sounds good, do you want to open it or should I? I don't think I have the capacity/understanding to make the needed changes. 

---

_Closed by @MeGaGiGaGon on 2025-07-09 17:31_

---

_Comment by @ntBre on 2025-07-09 17:34_

I'll try to write one up, but let me know if I get anything wrong!

---

_Branch deleted on 2025-07-09 18:34_

---
