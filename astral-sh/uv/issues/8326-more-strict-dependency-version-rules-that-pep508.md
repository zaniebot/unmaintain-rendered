---
number: 8326
title: More strict dependency version rules that PEP508
type: issue
state: closed
author: Veritaris
labels:
  - question
assignees: []
created_at: 2024-10-18T11:43:45Z
updated_at: 2024-10-21T09:26:58Z
url: https://github.com/astral-sh/uv/issues/8326
synced_at: 2026-01-10T01:24:27Z
---

# More strict dependency version rules that PEP508

---

_Issue opened by @Veritaris on 2024-10-18 11:43_

command that was invoked: `uv lock`
platform: `macOS Sonoma 14.6.1`
current uv version: `uv 0.4.24 (b9cd54913 2024-10-17)`

Hi team! 
Found such more strict beahaviour rather that described in PEP508
If I define dependency as following in `pyproject.toml`:
```toml
dependencies = [
    "httpx==*",
]
```
than after `uv lock` I get the following error:
```
ValueError: invalid pyproject.toml config: `project.dependencies[0]`.
configuration error: `project.dependencies[0]` must be pep508
```

In [docs](https://docs.astral.sh/uv/concepts/dependencies/#pep-508) at PEP508 paragraph it's told that
> A star can be used for the last digit with equals, e.g. foo ==2.1.* will accept any release from the 2.1 series. Similarly, ~= matches where the last digit is equal or higher, e.g., foo ~=1.2 is equal to foo >=1.2,<2, and foo ~=1.2.3 is equal to foo >=1.2.3,<1.3.

But according to PEP508 [grammar](https://peps.python.org/pep-0508/#complete-grammar)
```
wsp           = ' ' | '\t'
version_cmp   = wsp* <'<=' | '<' | '!=' | '==' | '>=' | '>' | '~=' | '==='>
version       = wsp* <( letterOrDigit | '-' | '_' | '.' | '*' | '+' | '!' )+>
version_one   = version_cmp:op version:v wsp* -> (op, v)
version_many  = version_one:v1 (wsp* ',' version_one)*:v2 -> [v1] + v2
versionspec   = ('(' version_many:v ')' ->v) | version_many
```
it is also valid to define version via bare wildcard (`*`)

Was restricting of wildcard usage done intentionally? I understand that I can just remove version constraint, but `uv add httpx` adds line as in my example
```bash
uv add httpx
```
```toml
dependencies = [
    "httpx==0.27.2",
]
```

---

_Comment by @notatallshaw on 2024-10-18 14:09_

Packaging also considers this invalid:

```
>>> import packaging.requirements

>>> packaging.requirements.Requirement("httpx==1")
<Requirement('httpx==1')>

>>> packaging.requirements.Requirement("httpx==1.*")
<Requirement('httpx==1.*')>

>>> packaging.requirements.Requirement("httpx==*")
Traceback (most recent call last):
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/requirements.py", line 36, in __init__
    parsed = _parse_requirement(requirement_string)
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/_parser.py", line 62, in parse_requirement
    return _parse_requirement(Tokenizer(source, rules=DEFAULT_RULES))
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/_parser.py", line 80, in _parse_requirement
    url, specifier, marker = _parse_requirement_details(tokenizer)
                             ~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/_parser.py", line 124, in _parse_requirement_details
    marker = _parse_requirement_marker(
        tokenizer,
    ...<5 lines>...
        ),
    )
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/_parser.py", line 145, in _parse_requirement_marker
    tokenizer.raise_syntax_error(
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~^
        f"Expected end or semicolon (after {after})",
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        span_start=span_start,
        ^^^^^^^^^^^^^^^^^^^^^^
    )
    ^
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/_tokenizer.py", line 167, in raise_syntax_error
    raise ParserSyntaxError(
    ...<3 lines>...
    )
packaging._tokenizer.ParserSyntaxError: Expected end or semicolon (after name and no valid version specifier)
    httpx==*
         ^

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    packaging.requirements.Requirement("httpx==*")
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^
  File "/home/damian/opensource/support/uv/8326/.venv/lib/python3.13/site-packages/packaging/requirements.py", line 38, in __init__
    raise InvalidRequirement(str(e)) from e
packaging.requirements.InvalidRequirement: Expected end or semicolon (after name and no valid version specifier)
    httpx==*
         ^
```

> it is also valid to define version via bare wildcard (*)

Are you *sure*? I would prefer this issue either raised on the [packaging repo](https://github.com/pypa/packaging/issues) or the [packaging discussion board ](https://discuss.python.org/c/packaging/14) before uv breaks compatibility with packaging here (and thus breaks a uv generated `pyproject.toml` with anything using `packaging` to parse).

---

_Label `compatibility` added by @zanieb on 2024-10-18 14:34_

---

_Label `needs-decision` added by @zanieb on 2024-10-18 14:34_

---

_Label `upstream` added by @zanieb on 2024-10-18 14:34_

---

_Comment by @charliermarsh on 2024-10-18 14:46_

I'm fairly certain that `==*` is not valid. From a glance, there are lots of things that are valid in the grammar that aren't actually valid PEP 508 requirements. For example, it looks like `==!+!+!` would be valid? But yes, I agree with @notatallshaw that this would be better discussed elsewhere since we're in conformance with all the rest of the ecosystem's behavior.

---

_Comment by @Veritaris on 2024-10-18 19:41_

> I would prefer this issue either raised on the [packaging repo](https://github.com/pypa/packaging/issues)

I'll create another issue there, thanks, but I've create it here since I faced issue using **uv**

> Are you sure?

> I'm fairly certain that ==* is not valid.

Yes, and it _is_, let's compile grammar from PEP508 and see parse result:
```
>>> from parsley import makeGrammar
>>> grammar = """
wsp           = ' ' | '\t'
version_cmp   = wsp* <'<=' | '<' | '!=' | '==' | '>=' | '>' | '~=' | '==='>
version       = wsp* <( letterOrDigit | '-' | '_' | '.' | '*' | '+' | '!' )+>
version_one   = version_cmp:op version:v wsp* -> (op, v)
version_many  = version_one:v1 (wsp* ',' version_one)*:v2 -> [v1] + v2
versionspec   = ('(' version_many:v ')' ->v) | version_many
urlspec       = '@' wsp* <URI_reference>
marker_op     = version_cmp | (wsp* 'in') | (wsp* 'not' wsp+ 'in')
python_str_c  = (wsp | letter | digit | '(' | ')' | '.' | '{' | '}' |
                 '-' | '_' | '*' | '#' | ':' | ';' | ',' | '/' | '?' |
                 '[' | ']' | '!' | '~' | '`' | '@' | '$' | '%' | '^' |
                 '&' | '=' | '+' | '|' | '<' | '>' )
dquote        = '"'
squote        = '\\''
python_str    = (squote <(python_str_c | dquote)*>:s squote |
                 dquote <(python_str_c | squote)*>:s dquote) -> s
env_var       = ('python_version' | 'python_full_version' |
                 'os_name' | 'sys_platform' | 'platform_release' |
                 'platform_system' | 'platform_version' |
                 'platform_machine' | 'platform_python_implementation' |
                 'implementation_name' | 'implementation_version' |
                 'extra' # ONLY when defined by a containing layer
                 ):varname -> lookup(varname)
marker_var    = wsp* (env_var | python_str)
marker_expr   = marker_var:l marker_op:o marker_var:r -> (o, l, r)
              | wsp* '(' marker:m wsp* ')' -> m
marker_and    = marker_expr:l wsp* 'and' marker_expr:r -> ('and', l, r)
              | marker_expr:m -> m
marker_or     = marker_and:l wsp* 'or' marker_and:r -> ('or', l, r)
                  | marker_and:m -> m
marker        = marker_or
quoted_marker = ';' wsp* marker
identifier_end = letterOrDigit | (('-' | '_' | '.' )* letterOrDigit)
identifier    = < letterOrDigit identifier_end* >
name          = identifier
extras_list   = identifier:i (wsp* ',' wsp* identifier)*:ids -> [i] + ids
extras        = '[' wsp* extras_list?:e wsp* ']' -> e
name_req      = (name:n wsp* extras?:e wsp* versionspec?:v wsp* quoted_marker?:m
                 -> (n, e or [], v or [], m))
url_req       = (name:n wsp* extras?:e wsp* urlspec:v (wsp+ | end) quoted_marker?:m
                 -> (n, e or [], v, m))
specification = wsp* ( url_req | name_req ):s wsp* -> s
# The result is a tuple - name, list-of-extras,
# list-of-version-constraints-or-a-url, marker-ast or None
URI_reference = <URI | relative_ref>
URI           = scheme ':' hier_part ('?' query )? ( '#' fragment)?
hier_part     = ('//' authority path_abempty) | path_absolute | path_rootless | path_empty
absolute_URI  = scheme ':' hier_part ( '?' query )?
relative_ref  = relative_part ( '?' query )? ( '#' fragment )?
relative_part = '//' authority path_abempty | path_absolute | path_noscheme | path_empty
scheme        = letter ( letter | digit | '+' | '-' | '.')*
authority     = ( userinfo '@' )? host ( ':' port )?
userinfo      = ( unreserved | pct_encoded | sub_delims | ':')*
host          = IP_literal | IPv4address | reg_name
port          = digit*
IP_literal    = '[' ( IPv6address | IPvFuture) ']'
IPvFuture     = 'v' hexdig+ '.' ( unreserved | sub_delims | ':')+
IPv6address   = (
                  ( h16 ':'){6} ls32
                  | '::' ( h16 ':'){5} ls32
                  | ( h16 )?  '::' ( h16 ':'){4} ls32
                  | ( ( h16 ':')? h16 )? '::' ( h16 ':'){3} ls32
                  | ( ( h16 ':'){0,2} h16 )? '::' ( h16 ':'){2} ls32
                  | ( ( h16 ':'){0,3} h16 )? '::' h16 ':' ls32
                  | ( ( h16 ':'){0,4} h16 )? '::' ls32
                  | ( ( h16 ':'){0,5} h16 )? '::' h16
                  | ( ( h16 ':'){0,6} h16 )? '::' )
h16           = hexdig{1,4}
ls32          = ( h16 ':' h16) | IPv4address
IPv4address   = dec_octet '.' dec_octet '.' dec_octet '.' dec_octet
nz            = ~'0' digit
dec_octet     = (
                  digit # 0-9
                  | nz digit # 10-99
                  | '1' digit{2} # 100-199
                  | '2' ('0' | '1' | '2' | '3' | '4') digit # 200-249
                  | '25' ('0' | '1' | '2' | '3' | '4' | '5') )# %250-255
reg_name = ( unreserved | pct_encoded | sub_delims)*
path = (
        path_abempty # begins with '/' or is empty
        | path_absolute # begins with '/' but not '//'
        | path_noscheme # begins with a non-colon segment
        | path_rootless # begins with a segment
        | path_empty ) # zero characters
path_abempty  = ( '/' segment)*
path_absolute = '/' ( segment_nz ( '/' segment)* )?
path_noscheme = segment_nz_nc ( '/' segment)*
path_rootless = segment_nz ( '/' segment)*
path_empty    = pchar{0}
segment       = pchar*
segment_nz    = pchar+
segment_nz_nc = ( unreserved | pct_encoded | sub_delims | '@')+
                # non-zero-length segment without any colon ':'
pchar         = unreserved | pct_encoded | sub_delims | ':' | '@'
query         = ( pchar | '/' | '?')*
fragment      = ( pchar | '/' | '?')*
pct_encoded   = '%' hexdig
unreserved    = letter | digit | '-' | '.' | '_' | '~'
reserved      = gen_delims | sub_delims
gen_delims    = ':' | '/' | '?' | '#' | '(' | ')?' | '@'
sub_delims    = '!' | '$' | '&' | '\\'' | '(' | ')' | '*' | '+' | ',' | ';' | '='
hexdig        = digit | 'a' | 'A' | 'b' | 'B' | 'c' | 'C' | 'd' | 'D' | 'e' | 'E' | 'f' | 'F'
"""
>>> compiled = makeGrammar(grammar, {})
>>> compiled("httpx==*").specification()
('httpx', [], [('==', '*')], None)
```

---

_Comment by @Veritaris on 2024-10-18 19:47_

I've also looked into **packaging** tool repo and didn't find if they write anything about PEP508 in their [docs](https://packaging.pypa.io/en/stable/index.html):
> This library provides utilities that implement the interoperability specifications which have clearly one correct behaviour (eg: [PEP 440](https://peps.python.org/pep-0440/)) or benefit greatly from having a single shared implementation (eg: [PEP 425](https://peps.python.org/pep-0425/)).

So, I even wouldn't complain about this problem in **uv** but this project _does_ mention PEP508 rules

---

_Comment by @konstin on 2024-10-18 19:55_

> I understand that I can just remove version constraint, but uv add httpx adds line as in my example

That should not happen and is a bug in uv. Can you provide a reproducer? I tried `uv init foo && cd foo && uv add httpx` and got:

```
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "httpx>=0.27.2",
]
```

---

The syntax defined by the PEP 508 grammar is more lenient than the semantically valid values. https://peps.python.org/pep-0440/#version-matching says:

> The specified version identifier must be in the standard format described in [Version scheme](https://peps.python.org/pep-0440/#version-scheme), but a trailing .* is permitted on public version identifiers as described below.

That is, there needs to be a dot before the star, which in turn needs a number before it to be valid,

I don't think we need to extend the spec here, allowing all versions is covered by a plain `httpx`.

---

_Label `upstream` removed by @konstin on 2024-10-18 19:55_

---

_Label `compatibility` removed by @konstin on 2024-10-18 19:55_

---

_Label `needs-decision` removed by @konstin on 2024-10-18 19:55_

---

_Label `needs-mre` added by @konstin on 2024-10-18 19:55_

---

_Comment by @Veritaris on 2024-10-18 20:45_

Hi konstin!
Ohhh, sorry, I see a code block is lost in my original post – there is no problem with `uv add X`, just a little bit of work, required to define that I want to use every possible version  (to remove >= and version itself and leave bare requirement name). I'll edit orig post now 

Regarding to 440 - well, I don't want to say that there are no other specification 😅 
But the only one that I found in docs, related to packages version specification, is 508 

---

_Label `needs-mre` removed by @konstin on 2024-10-18 20:59_

---

_Label `question` added by @konstin on 2024-10-18 20:59_

---

_Label `needs-mre` added by @konstin on 2024-10-18 20:59_

---

_Label `needs-mre` removed by @konstin on 2024-10-18 21:02_

---

_Comment by @charliermarsh on 2024-10-21 00:18_

> Yes, and it is, let's compile grammar from PEP508 and see parse result:

Right, I'm not claiming that the grammar forbids it. I'm claiming that it's not a valid PEP 508 specifier despite the grammar allowing it. As Konsti said, the grammar is more lenient than the semantically valid values, like my `==!+!+!+` example above. (I've also never seen this used or supported in any Python project.) It would be better to raise this elsewhere if you have a problem with the spec itself!

---

_Closed by @charliermarsh on 2024-10-21 00:18_

---

_Comment by @Veritaris on 2024-10-21 08:55_

Well, I agree that PEP 508 is too wide and allows something that other devs are not using 

In such case can we please stop mentioning it in wiki (https://docs.astral.sh/uv/concepts/dependencies/#pep-508) and use PEP 440 instead? 

---

_Comment by @konstin on 2024-10-21 09:26_

PEP 508 refers to PEP 440 for version specifiers, since dependency specifiers include a version specifier as substring (and PEP 508 doesn't specify any version or version specifier semantics):

> Versions may be specified according to the [PEP 440](https://peps.python.org/pep-0440/) rules. (Note: URI is defined in [std-66](https://datatracker.ietf.org/doc/html/rfc3986.html)):

The canonical source has move to https://packaging.python.org/en/latest/specifications/dependency-specifiers/ though, and PEP 440 to https://packaging.python.org/en/latest/specifications/version-specifiers/, so we should refer to those specs instead: #8411

---
