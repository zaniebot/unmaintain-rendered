```yaml
number: 19247
title: "[`flake8_bandit`] Fix S412 false negatives for any imports from `wsgiref.handlers` and `twisted.web.twcgi`"
type: pull_request
state: open
author: danparizher
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: fix-19242
created_at: 2025-07-09T22:25:18Z
updated_at: 2025-07-11T17:47:19Z
url: https://github.com/astral-sh/ruff/pull/19247
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8_bandit`] Fix S412 false negatives for any imports from `wsgiref.handlers` and `twisted.web.twcgi`

---

_Pull request opened by @danparizher on 2025-07-09 22:25_

## Summary

Fixes #19242

---

_Comment by @github-actions[bot] on 2025-07-09 22:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-07-10 15:29_

Thanks, this looks like what I had in mind in #19242. My only hesitations are that this is a bit of a divergence from the upstream linter and also an expansion of the rule. However, I think we might diverge in the general implementation of these rules, and we could gate the expansion behind `preview` if we're worried about that part.

I also think these are probably pretty niche modules to import, so I'm not sure how much impact this will have either way.

@MichaReiser what do you think? To summarize the issue:
- S412 only flags `from wsgiref.handlers import CGIHandler`, not other ways of importing
- The upstream linter also flags calls to the insecure code (`wsgiref.handlers.CGIHandler()`)

I suggested flagging the whole module import (this PR) as an easy fix that looks to be in line with our other S rules, but a more robust fix might be flagging calls like upstream. That could be a bigger change that might also affect our other S rules.

---

_Label `rule` added by @ntBre on 2025-07-10 15:29_

---

_Label `needs-decision` added by @ntBre on 2025-07-10 15:29_

---

_Comment by @MichaReiser on 2025-07-11 16:18_

> and we could gate the expansion behind preview if we're worried about that part.

The rule itself is still preview


Hmm not sure. Does the same security concern exist for `IISCGIHandler`, `BaseCGIHandler`, `SimpleHandler`, .... If not, restricting the entire `handlers` import is too restrictive. 

 > The upstream linter also flags calls to the insecure code (wsgiref.handlers.CGIHandler())

We should rename the rule if we do so because it then flags the usages and not the imports

---

_Comment by @MichaReiser on 2025-07-11 16:22_

What claude thinks:

<details>
I'll fetch the specific wsgiref handlers documentation to analyze each handler individually.Based on the wsgiref.handlers documentation and my HTTPoxy research, here's a specific analysis of each handler's vulnerability status:

## **VULNERABLE Handlers:**

### **wsgiref.handlers.CGIHandler**
**VULNERABLE** - This is explicitly mentioned in the HTTPoxy documentation as vulnerable. Python code must be deployed under CGI to be vulnerable, and this usually means using wsgiref.handlers.CGIHandler. This handler uses `sys.stdin`, `sys.stdout`, `sys.stderr` and `os.environ`, making it directly susceptible to environment variable pollution from HTTP headers.

### **wsgiref.handlers.IISCGIHandler** 
**VULNERABLE** - This is a specialized subclass of `CGIHandler` designed for Microsoft's IIS web server. Since it inherits from `CGIHandler` and operates in the same CGI environment where `os.environ` gets polluted by HTTP headers, it has the same vulnerability.

### **wsgiref.handlers.BaseCGIHandler**
**VULNERABLE** - This is the base class for CGI handlers that `CGIHandler` inherits from. The documentation states it's "intended for use with software other than HTTP origin servers" and is designed for "gateway protocol implementation (such as CGI, FastCGI, SCGI, etc.)". Since it operates in CGI-like environments where HTTP headers become environment variables, it's vulnerable.

## **NOT VULNERABLE Handlers:**

### **wsgiref.handlers.SimpleHandler**
**NOT VULNERABLE** - The documentation specifically states this is "designed for use with HTTP origin servers" and "If you are writing an HTTP server implementation, you will probably want to subclass this instead of BaseCGIHandler." Since regular WSGI environments keep user-supplied values in a separate WSGI 'environ' map, they are not vulnerable. The os.environ['HTTP_PROXY'] remains unchanged when a Proxy header is sent.

### **wsgiref.handlers.BaseHandler**
**NOT VULNERABLE** - This is an abstract base class for running WSGI applications. It's designed for HTTP origin servers, not CGI environments. The documentation shows it maintains separate environment handling and doesn't directly expose `os.environ` to HTTP header pollution.

## **Key Distinction:**

The vulnerability comes down to whether the handler operates in a **CGI environment** where:
- HTTP headers are mixed into environment variables
- `os.environ['HTTP_PROXY']` can be set by a malicious `Proxy:` header

**CGI-based handlers** (CGIHandler, IISCGIHandler, BaseCGIHandler) are vulnerable because they operate in environments where this header-to-environment variable mapping occurs.

**HTTP origin server handlers** (SimpleHandler, BaseHandler) are not vulnerable because they maintain proper separation between HTTP headers and environment variables, keeping user-supplied data in separate WSGI environ dictionaries.

</details>

So I think outright banning the package isn't the right fix.

---

_Comment by @ntBre on 2025-07-11 17:47_

> The rule itself is still preview

:facepalm: My mistake, didn't notice that

This is the [implementation](https://github.com/python/cpython/blob/252e2f710ea376a38c4545dd758e03d331c1eaad/Lib/wsgiref/handlers.py#L487-L508) of `BaseCGIHandler`:

```python
class BaseCGIHandler(SimpleHandler):
    origin_server = False
```

but I guess the `origin_server` might make all the difference. I'm not too familiar with CGI, so I'm happy to defer to Claude.

I think we may need to rename and reimplement the rule to catch calls in that case. I suspect, but still haven't confirmed, that this is the case for the other `flake8-bandit` rules too.

---
