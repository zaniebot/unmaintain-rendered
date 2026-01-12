```yaml
number: 11698
title: "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line."
type: issue
state: closed
author: EdSaleh
labels: []
assignees: []
created_at: 2024-06-02T21:17:11Z
updated_at: 2024-06-07T19:30:08Z
url: https://github.com/astral-sh/ruff/issues/11698
synced_at: 2026-01-12T15:54:51Z
```

# [Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line.

---

_@EdSaleh_

Hello,

Please create a new lint rule to require that each block must end with # or #. or a new line.
This would work as a curly braces blocks found in C style languages.
Also, this could also be used to reformat python code by code editor to fix any indentation issues.

Users can also use a different word symbol or use a new line for this purpose. (#., #, #/, newline, etc). The user has the option to select the keyword they would like to have for the project. The difference between this proposal (:\n code_block \n#.) and curly braces ({}) in C languages is that curly braces are also indentation insensitive while this proposal is indentation sensitive just like indentation style languages.

There could also be a configurable threshold limit for block lines for which the `code_block_ending_marker` rule is activated and would apply (example: block_lines > 1 lines by default). 
Users can also use new line instead of # or #. by configuring `code_block_ending_marker_word` if it fits their project development.


Example:

```python
import string
def atbash(sequence: str) -> str:
    """
    Example
    """
    output = ""
    for i in sequence:
        extract = ord(i)
        if 65 <= extract <= 90:
            output += chr(155 - extract)
        #.
        elif 97 <= extract <= 122:
            output += chr(219 - extract)
        #.
        else:
            output += i
        #.
    #.
    return output
#.
```

There are cases where strict block guarantees is required. Also, when copying python code sometimes spacing is not aligned correctly and having code with this style with editor support, editor would fix incorrect spacing easily.

```python
if(True):
print(True)
print(True)
#
if(True):
print(True)
print(True)
#
```

OR

```python
if(True):
print(True)
print(True)
#.
if(True):
print(True)
print(True)
#.
```
would be aligned correctly by the editor to:

```python
if(True):
 print(True)
 print(True)
#
if(True):
 print(True)
 print(True)
#
```
OR
```python
if(True):
 print(True)
 print(True)
#.
if(True):
 print(True)
 print(True)
#.
```

Thank you,

---

_Renamed from "[Feature] new rule to require a comment #. at end each python block." to "[Feature] new lint rule to require a comment #. at end each python block." by @EdSaleh on 2024-06-02 21:20_

---

_Renamed from "[Feature] new lint rule to require a comment #. at end each python block." to "[Feature] New lint rule to require that each block must end with # or #. or a new line." by @EdSaleh on 2024-06-04 09:02_

---

_Renamed from "[Feature] New lint rule to require that each block must end with # or #. or a new line." to "[Feature] new lint rule to require that each block must end with # or #. or a new line." by @EdSaleh on 2024-06-04 09:03_

---

_Comment by @AlexWaygood on 2024-06-04 12:23_

Hi, thanks for the suggestion! Unfortunately, I think very few of our users would want to switch this rule on, essentially for the same reasons explained to you by the pylint maintainers in https://github.com/pylint-dev/pylint/issues/9684. As such, I think this probably wouldn't be worth the maintenance burden for us, unfortunately. So we'll decline this one :-)

---

_Closed by @AlexWaygood on 2024-06-04 12:23_

---

_Comment by @EdSaleh on 2024-06-04 18:20_

> Hi, thanks for the suggestion! Unfortunately, I think very few of our users would want to switch this rule on, essentially for the same reasons explained to you by the pylint maintainers in [pylint-dev/pylint#9684](https://github.com/pylint-dev/pylint/issues/9684). As such, I think this probably wouldn't be worth the maintenance burden for us, unfortunately. So we'll decline this one :-)

Sure, but I don't think issues from a different project should be relavent here. The purpose of this issue is to let python and other language developers know of this idea and to create a discussion around it, discussion to identify whether this feature could be useful or not and why, and the decision around it is for the project developers to choose.

---

_Comment by @EdSaleh on 2024-06-04 19:16_

I have some experience with creating proposal for lints, I have also contributed to rust lint previously and my proposal was completed and merged.
https://github.com/rust-lang/rust-clippy/issues/2685

---

_Comment by @zanieb on 2024-06-04 19:24_

Hi!

We use issues from other projects (pylint, flake8, black, etc.) to help inform our design decisions here because we want to have cohesion between various tools in the Python ecosystem.

However, regardless of the issue in pylint, I think that this rule would not be used by most of our users and is antithetical to general Python design principles. It is currently out of scope for what we'd want to include in Ruff. Maybe once we have support for plugins, it'd make sense there.

I have no problem with discussion continuing around this idea. Anyone is welcome to comment in this issue.

---

_Renamed from "[Feature] new lint rule to require that each block must end with # or #. or a new line." to "[Feature] new lint rule to require that each block must end with # or #. (hashdot) or a new line." by @EdSaleh on 2024-06-04 22:30_

---

_Renamed from "[Feature] new lint rule to require that each block must end with # or #. (hashdot) or a new line." to "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line." by @EdSaleh on 2024-06-05 19:19_

---

_Renamed from "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line." to "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line. And new lint rule for declaring variables to have #declare or #var comment" by @EdSaleh on 2024-06-05 21:54_

---

_Renamed from "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line. And new lint rule for declaring variables to have #declare or #var comment" to "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line. And new lint rule for declaring variables to have #declare or #var comment." by @EdSaleh on 2024-06-05 21:55_

---

_Renamed from "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line. And new lint rule for declaring variables to have #declare or #var comment." to "[Feature] new lint rule to require that each block must end with # or #. (HashtagDot) or a new line." by @EdSaleh on 2024-06-05 22:06_

---
