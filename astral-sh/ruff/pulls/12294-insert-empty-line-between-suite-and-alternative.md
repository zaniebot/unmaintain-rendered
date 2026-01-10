```yaml
number: 12294
title: Insert empty line between suite and alternative branch after def/class
type: pull_request
state: merged
author: konstin
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: konsti/fix-def-end-of-block
created_at: 2024-07-12T08:26:41Z
updated_at: 2024-07-15T10:59:35Z
url: https://github.com/astral-sh/ruff/pull/12294
synced_at: 2026-01-10T21:47:02Z
```

# Insert empty line between suite and alternative branch after def/class

---

_Pull request opened by @konstin on 2024-07-12 08:26_

When there is a function or class definition at the end of a suite followed by the beginning of an alternative block, we have to insert a single empty line between them.

In the if-else-statement example below, we insert an empty line after the `foo` in the if-block, but none after the else-block `foo`, since in the latter case the enclosing suite already adds empty lines.

```python
if sys.version_info >= (3, 10):
    def foo():
        return "new"
else:
    def foo():
        return "old"
class Bar:
    pass
```

To do so, we track whether the current suite is the last one in the current statement with a new option on the suite kind.

Fixes #12199


---

_Review requested from @MichaReiser by @konstin on 2024-07-12 08:26_

---

_Label `formatter` added by @konstin on 2024-07-12 08:26_

---

_Comment by @github-actions[bot] on 2024-07-12 08:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+15 -0 lines in 14 files in 5 projects; 1 project error; 48 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L42'>setup.py~L42</a>
```diff
 
     def yellow(text: str) -> str:
         return f"{colorama.Fore.YELLOW}{text}{colorama.Style.RESET_ALL}"
+
 except ModuleNotFoundError:
 
     def _plain(text: str) -> str:
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L193'>src/bokeh/command/subcommands/file_output.py~L193</a>
```diff
 
                 def indexed(i: int) -> str:
                     return filename
+
             else:
 
                 def indexed(i: int) -> str:
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L49'>src/bokeh/core/has_props.py~L49</a>
```diff
     F = TypeVar("F", bound=Callable[..., Any])
 
     def lru_cache(arg: int | None) -> Callable[[F], F]: ...
+
 else:
     from functools import lru_cache
 
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/locking.py#L93'>src/bokeh/document/locking.py~L93</a>
```diff
         @wraps(func)
         async def _wrapper(*args: Any, **kw: Any) -> None:
             await func(*args, **kw)
+
     else:
 
         @wraps(func)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/test_has_props.py#L556'>tests/unit/bokeh/core/test_has_props.py~L556</a>
```diff
         class DupeProps(hp.HasProps):
             bar = AngleSpec()
             bar_units = String()
+
     except RuntimeError as e:
         assert str(e) == "Two property generators both created DupeProps.bar_units"
     else:
```

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 lines across 2 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/langchain-ai/langchain/blob/0da5078cad3a689e51cd451c7cac63ac35b5accd/libs/community/tests/unit_tests/chat_message_histories/test_sql.py#L10'>libs/community/tests/unit_tests/chat_message_histories/test_sql.py~L10</a>
```diff
 
     class Base(DeclarativeBase):
         pass
+
 except ImportError:
     # for sqlalchemy < 2
     from sqlalchemy.ext.declarative import declarative_base
```
<a href='https://github.com/langchain-ai/langchain/blob/0da5078cad3a689e51cd451c7cac63ac35b5accd/libs/core/langchain_core/messages/utils.py#L765'>libs/core/langchain_core/messages/utils.py~L765</a>
```diff
 
         def list_token_counter(messages: Sequence[BaseMessage]) -> int:
             return sum(token_counter(msg) for msg in messages)  # type: ignore[arg-type, misc]
+
     else:
         list_token_counter = token_counter  # type: ignore[assignment]
 
```

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+4 -0 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/python/typeshed/blob/f863db6bc5242348ceaa6a3bca4e59aa9e62faaa/stdlib/dbm/gnu.pyi#L44'>stdlib/dbm/gnu.pyi~L44</a>
```diff
 
     if sys.version_info >= (3, 11):
         def open(filename: StrOrBytesPath, flags: str = "r", mode: int = 0o666, /) -> _gdbm: ...
+
     else:
         def open(filename: str, flags: str = "r", mode: int = 0o666, /) -> _gdbm: ...
```
<a href='https://github.com/python/typeshed/blob/f863db6bc5242348ceaa6a3bca4e59aa9e62faaa/stdlib/inspect.pyi#L334'>stdlib/inspect.pyi~L334</a>
```diff
             locals: Mapping[str, Any] | None = None,
             eval_str: bool = False,
         ) -> Self: ...
+
     else:
         @classmethod
         def from_callable(cls, obj: _IntrospectableCallable, *, follow_wrapped: bool = True) -> Self: ...
```
<a href='https://github.com/python/typeshed/blob/f863db6bc5242348ceaa6a3bca4e59aa9e62faaa/stdlib/ipaddress.pyi#L46'>stdlib/ipaddress.pyi~L46</a>
```diff
         def __ge__(self, other: Self) -> bool: ...
         def __gt__(self, other: Self) -> bool: ...
         def __le__(self, other: Self) -> bool: ...
+
     else:
         def __ge__(self, other: Self, NotImplemented: Any = ...) -> bool: ...
         def __gt__(self, other: Self, NotImplemented: Any = ...) -> bool: ...
```
<a href='https://github.com/python/typeshed/blob/f863db6bc5242348ceaa6a3bca4e59aa9e62faaa/stdlib/ipaddress.pyi#L84'>stdlib/ipaddress.pyi~L84</a>
```diff
         def __ge__(self, other: Self) -> bool: ...
         def __gt__(self, other: Self) -> bool: ...
         def __le__(self, other: Self) -> bool: ...
+
     else:
         def __ge__(self, other: Self, NotImplemented: Any = ...) -> bool: ...
         def __gt__(self, other: Self, NotImplemented: Any = ...) -> bool: ...
```

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+3 -0 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/indico/indico/blob/b5c193fb11a8d2e399352160a7b60153ac906907/indico/modules/events/contributions/util.py#L110'>indico/modules/events/contributions/util.py~L110</a>
```diff
             if not c.speakers:
                 return True, None
             return False, speakers[0].get_full_name(last_name_upper=False, abbrev_first_name=False).lower()
+
     elif sort_by == BOASortField.board_number:
         key_func = attrgetter('board_number')
     elif sort_by == BOASortField.session_board_number:
```
<a href='https://github.com/indico/indico/blob/b5c193fb11a8d2e399352160a7b60153ac906907/indico/web/flask/templating.py#L63'>indico/web/flask/templating.py~L63</a>
```diff
             if isinstance(item, str):
                 item = item.lower()
             return natural_sort_key(item)
+
     else:
         sort_func = natural_sort_key
 
```
<a href='https://github.com/indico/indico/blob/b5c193fb11a8d2e399352160a7b60153ac906907/indico/web/flask/util.py#L78'>indico/web/flask/util.py~L78</a>
```diff
             # Indico RH
             def wrapper(**kwargs):
                 return obj().process()
+
         else:
             # Some class we didn't expect.
             raise ValueError(f'Unexpected view func class: {obj!r}')
```

</p>
</details>
<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (+1 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/mesonbuild/meson-python/blob/063fe8bc8ad6984c29b24a0f72a14e1ced091a9c/mesonpy/_compat.py#L29'>mesonpy/_compat.py~L29</a>
```diff
 
     def read_binary(package: str, resource: str) -> bytes:
         return importlib.resources.files(package).joinpath(resource).read_bytes()
+
 else:
     read_binary = importlib.resources.read_binary
 
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Converted to draft by @konstin on 2024-07-12 08:38_

---

_Comment by @konstin on 2024-07-12 09:03_

Not sure if this counts as bugfix or breaking change, i'm fine with merging it before the next breaking release.

---

_Comment by @MichaReiser on 2024-07-12 09:10_

> Not sure if this counts as bugfix or breaking change, i'm fine with merging it before the next breaking release.

I think we have to gate this behind preview according to our version policy. The version policy only allows formatting style changes that address instabilities or formatting that lead to invalid syntax.

The ecosystem checks look good :)

---

_Marked ready for review by @konstin on 2024-07-12 10:01_

---

_Review request for @MichaReiser removed by @konstin on 2024-07-12 10:02_

---

_Review requested from @MichaReiser by @konstin on 2024-07-12 10:02_

---

_Label `preview` added by @MichaReiser on 2024-07-15 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:49 on 2024-07-15 07:22_

Nit: Any specific reason why you decide to introduce a `format` function for except handlers rather than adding a field to the options? I'm fine with either, I'm mainly surprised that you went with a format function here, but decided to introduce an `Options` struct for `match_case`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_match.rs`:39 on 2024-07-15 07:25_

I'm probably blind but what do we need the `peekable` for?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_match.rs`:51 on 2024-07-15 07:26_


```suggestion
            let last_suite_in_statement = Some(case) == cases.last();
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_try.rs`:71 on 2024-07-15 07:27_

I don't think that's technically possible. A try statement must always have an `except` handler or a `finally` body. That's why it should be fine to assume `false` for `Try`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:35 on 2024-07-15 07:29_

It's not just statements, e.g. it's also includes `case` in match statements. Maybe `WithSuite`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:67 on 2024-07-15 07:40_

I find this a surprising default. I think we should remove it and update the remaining call sites to be explicit about it.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:504 on 2024-07-15 07:47_

This seems to be a 1:1 copy from

https://github.com/astral-sh/ruff/blob/5f4f4d89710228dfa3908fdaf0de669e9ef81426/crates/ruff_python_formatter/src/statement/suite.rs#L200-L249

It would be great if we can avoid copying the code here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/newlines.py`:255 on 2024-07-15 07:48_

Can we add a few tests related to suppression comments? 

I'm worried that we now add empty lines even if the entire suite is suppressed. 

---

_@MichaReiser approved on 2024-07-15 07:50_

Nice and thanks for working on this! 

This overall looks good to me. We should add a few tests around suppression comments to make sure we don't add new lines into suppressed ranges

---

_@konstin reviewed on 2024-07-15 08:24_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_match.rs`:39 on 2024-07-15 08:24_

Good catch, not needed anymore 

---

_@konstin reviewed on 2024-07-15 08:30_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:35 on 2024-07-15 08:30_

I find last suite in statement describes it well; the rule is that we insert empty lines except when the statement ends directly after it, because then it's the task of whatever contains the suite to insert empty lines. A different name would be `ends_at_end_of_statement`. For match-case, we capture whether the end of the (case-)suite ends at the end of the match-statement.

---

_@konstin reviewed on 2024-07-15 08:33_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:67 on 2024-07-15 08:33_

We unfortunately need a default for `AsFormat`, otherwise i'm all for making this explicit in call sites.

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/newlines.py`:255 on 2024-07-15 08:42_

I added one, is that what you had in mind?

---

_@konstin reviewed on 2024-07-15 08:42_

---

_@konstin reviewed on 2024-07-15 10:54_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/except_handler_except_handler.rs`:49 on 2024-07-15 10:54_

It's now `Options` everywhere.

---

_@MichaReiser reviewed on 2024-07-15 10:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:67 on 2024-07-15 10:57_

Hmm, that's unfortunate but fair enough.

---

_@MichaReiser reviewed on 2024-07-15 10:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/newlines.py`:255 on 2024-07-15 10:59_

Looks good, thanks

---

_Merged by @konstin on 2024-07-15 10:59_

---

_Closed by @konstin on 2024-07-15 10:59_

---

_Branch deleted on 2024-07-15 10:59_

---
