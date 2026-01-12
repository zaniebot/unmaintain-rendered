```yaml
number: 1964
title: "D407 doesn't respect indentation level?"
type: issue
state: closed
author: spaceone
labels:
  - docstring
assignees: []
created_at: 2023-01-18T16:11:46Z
updated_at: 2023-01-19T15:32:07Z
url: https://github.com/astral-sh/ruff/issues/1964
synced_at: 2026-01-12T15:54:41Z
```

# D407 doesn't respect indentation level?

---

_@spaceone_

```python
class Plugin(object):

    u"""A wrapper for blah.

    These Python modules (plugins) may have the following properties:

    :attr dict actions:
            A mapping of valid action names to function callbacks.
            These action names can be referenced by additional displayed buttons (see :attr:`buttons`).
            If a called actions does not exists the run() function is taken as fallback.
            example:
                    actions = {
                            'remove': my_remove_funct,
                    }
    :attr str title:
            A short description of the problem
            example:
                    title = _('No space left on device')
    """
```

`ruff --isolated --select D --fix foo.py` turns into

```python
class Plugin(object):

    u"""A wrapper for blah.

    These Python modules (plugins) may have the following properties:

    :attr dict actions:
            A mapping of valid action names to function callbacks.
            These action names can be referenced by additional displayed buttons (see :attr:`buttons`).
            If a called actions does not exists the run() function is taken as fallback.

    Example:
    -------
                    actions = {
                            'remove': my_remove_funct,
                    }
    :attr str title:
            A short description of the problem
            example:
                    title = _('No space left on device')
    """
```

1. The result is broken, the indentation is not respected. I am unsure what docstring format is that? RST based? at least in sphinx RST is used but I have not seen such thing. Don't know what I expect here. Maybe the ruff config could specify a doc-format.
2. The second `example:` is not detected anymore at all.

---

_Comment by @spaceone on 2023-01-18 16:23_

Another example:
```python
def simple_response(function=None, with_flavor=None, with_progress=False):
    '''If your function is as simple as: "Just return some variables"
    Before::

            def my_func(self, request):
                    variable1 = request.options.get('variable1')
                    variable2 = request.options.get('variable2')
                    flavor = request.flavor or 'default flavor'
                    if variable1 is None:
                            self.finished(request.id, None, message='variable1 is required', success=False)
                            return
                    if variable2 is None:
                    variable2 = ''
                    try:
                            value = '%s_%s_%s' % (self._saved_dict[variable1], variable2, flavor)
                    except KeyError:
                            self.finished(request.id, None, message='Something went wrong', success=False, status=500)
                            return
                    self.finished(request.id, value)

    After::

            @simple_response(with_flavor=True)
            def my_func(self, variable1, variable2='', flavor='default_flavor'):
                    try:
                            return '%s_%s_%s' % (self._saved_dict[variable1], variable2, flavor)
                    except KeyError:
                            raise UMC_Error('Something went wrong')

    '''
```

turns it into:

```python
def simple_response(function=None, with_flavor=None, with_progress=False):
    '''If your function is as simple as: "Just return some variables"
    Before::

            def my_func(self, request):
                    variable1 = request.options.get('variable1')
                    variable2 = request.options.get('variable2')
                    flavor = request.flavor or 'default flavor'
                    if variable1 is None:
                            self.finished(request.id, None, message='variable1 is required', success=False)

    Return:
    ------
                    if variable2 is None:
                    variable2 = ''
                    try:
                            value = '%s_%s_%s' % (self._saved_dict[variable1], variable2, flavor)
                    except KeyError:
                            self.finished(request.id, None, message='Something went wrong', success=False, status=500)

    Return:
    ------
                    self.finished(request.id, value)

    After::

            @simple_response(with_flavor=True)
            def my_func(self, variable1, variable2='', flavor='default_flavor'):
                    try:
                            return '%s_%s_%s' % (self._saved_dict[variable1], variable2, flavor)
                    except KeyError:
                            raise UMC_Error('Something went wrong')

    '''

```

---

_Comment by @charliermarsh on 2023-01-18 16:37_

There's a couple things going wrong here, but the biggest issue, I think, is that we don't support Sphinx-style docstrings (is that what those are? Pydocstyle doesn't either) -- only NumPy and Google. We do have a setting for this `[tool.ruff.pydocstyle.convention]`, but right now, I think its best-guess is that you're using NumPy-style docstrings, and so it's matching statements like `return` as section headers.

---

_Label `docstring` added by @charliermarsh on 2023-01-18 16:41_

---

_Comment by @tmke8 on 2023-01-18 17:39_

Pydocstyle has the `pep257` convention which I always use for sphinx-style docstrings but it doesn't enforce as much as the other two conventions in pydocstyle. [darglint](https://github.com/terrencepreilly/darglint) has full support for sphinx-style docstrings though.

Also, OP's example doesn't look like the usual sphinx style to me... here is an official example: https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html For example, the type is usually specified separately.

---

_Comment by @charliermarsh on 2023-01-18 18:15_

Yeah, we reimplemented Pydocstyle so we suffer from many of the same limitations. I want to replace it with a `darglint`-style parser that uses an actual grammar, but it's a big project and I'm prioritizing some other things above it. I know @not-my-profile is interested in that too, though.

---

_Comment by @spaceone on 2023-01-18 20:35_

Yes, it's sphinx based.

---

_Renamed from "D407 doens't respect indentation level?" to "D407 doesn't respect indentation level?" by @spaceone on 2023-01-18 22:56_

---

_Comment by @not-my-profile on 2023-01-19 03:02_

Yes I am working on a docstring parsing library for both a personal project and ruff, and I am planning to support all major docstring formats (epytext, google, numpy and sphinx). It will take me some time to publish that though.

---

_Comment by @charliermarsh on 2023-01-19 15:32_

I think this issue is larger than indentation level so going to close for now knowing that it's part of a large change.

---

_Closed by @charliermarsh on 2023-01-19 15:32_

---
