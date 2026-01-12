```yaml
number: 8831
title: "can't do local pip install of whl files in requirements.txt when sys_platform is used "
type: issue
state: closed
author: hsnoil
labels:
  - error messages
assignees: []
created_at: 2024-11-05T16:28:07Z
updated_at: 2024-11-05T21:07:08Z
url: https://github.com/astral-sh/uv/issues/8831
synced_at: 2026-01-12T15:59:35Z
```

# can't do local pip install of whl files in requirements.txt when sys_platform is used 

---

_@hsnoil_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When you have a local whl entry inside the requirements.txt and add `; sys_platfom ==`  to it, it fails to work.

Here is an example:

```
#uv pip install -r requirements.txt
error: Couldn't parse requirement in: `requirements.txt` at position 612
  Caused by: Expected path (./test.whl;`) to end in a supported file extension: `.whl`, `.zip`, `.tar.gz`, `.tar.bz2`...
./test.whl; sys_platfom == "win32"
^^^^^^^^
```

As you can see the `;` is for some reason included in the file type

---

_Comment by @charliermarsh on 2024-11-05 16:28_

You need to include a space before the `;`. I believe this is compliant with the grammar.

---

_Comment by @hsnoil on 2024-11-05 16:30_

@charliermarsh  that works, thanks! Though pip works without the space. And I've seen many make requirements.txt files without the space so it may make sense to make it compatible?

---

_Comment by @hsnoil on 2024-11-05 16:45_

@charliermarsh I took a quick look and from what I understand the whitespace is optional.

https://pip.pypa.io/en/stable/reference/requirement-specifiers/#requirement-specifiers

says it is based on PEP508 which says

> Non line-breaking whitespace is mostly optional with no semantic meaning. The sole exception is detecting the end of a URL requirement.

https://peps.python.org/pep-0508/#whitespace

---

_Comment by @zanieb on 2024-11-05 16:52_

The key is "**The sole exception is detecting the end of a URL requirement.**" — paths are URLs

---

_Comment by @charliermarsh on 2024-11-05 16:53_

Yeah it's not optional in the [grammar](https://peps.python.org/pep-0508/#complete-grammar).

---

_Comment by @zanieb on 2024-11-05 16:54_

This is important because there can be `;` in a URI, e.g.

```
❯ touch 'foo;bar'
❯ ls
foo;bar
```

---

_Comment by @hsnoil on 2024-11-05 17:16_

I see, I saw it more of a path than a URL as a url would be like file://, path is though a part of the url. I guess I misunderstood.

That said, the code is anyways looking for extensions like "whl", correct? So if whl; is seen as an extension, it should be possible to fix it right? And this would make it compatible with way pip does it. If not at the very least an error to explain why it doesn't work otherwise people coming from pip may just assume the issue is on uv's end.

---

_Comment by @charliermarsh on 2024-11-05 17:16_

I thought we _did_ show a custom error here. That part I can look into. Maybe it regressed at some point.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-05 17:16_

---

_Label `error messages` added by @charliermarsh on 2024-11-05 17:16_

---

_Comment by @hsnoil on 2024-11-05 17:53_

Another question, looking at the spec to do urls you have to do:

```
test@./test.whl ; sys_platform == 'win32'
```
In the case of:
```
./test ; sys_platform == 'win32'
```
it is already not a url by the spec as the spec needs a `@` sign. Running the spec tester script in PEP508 confirms that. But it works without `@` sign on both UV and pip

I am trying to find the spec for this implementation without `@` but can't seem to find it. It is possible pip supports it because it uses a different spec for it.

---

_Comment by @charliermarsh on 2024-11-05 17:54_

Yeah `./test ; sys_platform == 'win32'` isn't actually spec-compliant to begin with -- it's not covered by PEP 508. It's something that pip supports but AFAIK it's just implementation-defined.

---

_Comment by @charliermarsh on 2024-11-05 18:05_

Ok it looks like we show the "right" error message for HTTPS URLs, but not file paths.

---

_Comment by @hsnoil on 2024-11-05 18:08_

So if it isn't in the grammar to begin with and was added as pip compatibility, then it makes sense to include `./test.whl; sys_platform == 'win32'` as well, no? 

Especially since you are already looking for extensions like `.whl` so if the extension ends with `whl;` it can just remove the `;` and re-evaluate it. That shouldn't cause any errors of compatibility with urls that use `;`


---

_Comment by @charliermarsh on 2024-11-05 18:10_

Candidly, I don't want to have incompatible parsing between the two URL formats. It's a source of bugs and confusion for users if the behaviors differ between the two.

---

_Comment by @charliermarsh on 2024-11-05 18:24_

Okay, I think we can make this work. I agree it's not super intuitive right now.

---

_Closed by @charliermarsh on 2024-11-05 21:07_

---
