```yaml
number: 12035
title: "Feature Request: Production Bundling for Python – A Single File Deployment Approach"
type: issue
state: closed
author: ImreC
labels:
  - enhancement
assignees: []
created_at: 2025-03-07T09:16:36Z
updated_at: 2025-05-20T09:38:47Z
url: https://github.com/astral-sh/uv/issues/12035
synced_at: 2026-01-12T16:00:53Z
```

# Feature Request: Production Bundling for Python – A Single File Deployment Approach

---

_@ImreC_

### Summary

uv is an excellent package manager, and I’ve been very happy with its performance. However, deploying Python code to production remains cumbersome. Even for simple scripts, we often need to manage requirements.txt files, virtual environments, or zip entire site-packages directories just to execute a script in production.

A major pain point in Python development is its import structure, which relies heavily on PYTHONPATH, `__init__.py` files, and a rigid folder hierarchy. This makes modularizing code difficult, especially for environments like AWS Lambda, where you often have multiple smaller functions reusing the same code and therefore the local folder structure diverges from what Python expects at runtime. I have tried many ways to tackle this so far, but always have come short, or just reverted to writing `cp` commands in a shell script. Of course you are able to use Docker images, but this adds a massive layer of overhead, to something that shouldn't be this hard. 

Other ecosystems, like TypeScript, use module bundlers that allow developers to write modular code and then perform a build step to produce a single, self-contained file—tree-shaken, transpiled, and ready to run with no additional dependencies. A similar approach for Python would significantly simplify deployment. If tree shaking is possible, it would also reduce the amount of data that you need to copy to the cloud environment, increasing the speed of deployment.

I would like to propose adding a "production bundle" feature to uv that packages an entire Python project into a single .py file, including all necessary modules, while remaining executable on any system with Python installed. Please see the example below. The specific use case being deploying Python code to production. Not providing a program as an executable for an OS to an end user (as I have seen proposed in other issues). 

Maybe this is not possible due to some of Python's wiring. Always happy to learn about some of this as well. It seems like a hard thing to do. It also seems like something that if possible, the uv team is very well positioned to do and would simplify deployment of Python code a lot.

### Example

Imagine a project where two AWS Lambda functions share a common interface/ module:
```
my_project/
│── lambda_a/
│   ├── handler.py
│── lambda_b/
│   ├── handler.py
│── interface/
│   ├── __init__.py
│   ├── utils.py
│   ├── database.py
```
- interface/ contains shared logic (e.g., utils.py for helper functions and database.py for database connections).
- Each handler.py file in lambda_a/ and lambda_b/ imports from interface/.

Current Deployment Workflow

To deploy lambda_a/handler.py and lambda_b/handler.py separately, developers typically:

- Install dependencies in a virtual environment.
- Copy interface/ into each Lambda package.
- Zip everything with site-packages/ and upload.
- Maintain multiple Lambda layers or zip files to avoid duplication.

This setup is fragile, requiring duplication, PYTHONPATH tweaks, and manual packaging of shared modules.
Proposed Solution with UV

With a production bundling command based on entrypoints like:
```
uv bundle lambda_a/handler.py -o lambda_a_bundled.py
uv bundle lambda_b/handler.py -o lambda_b_bundled.py
```
UV would:

- Resolve and inline all dependencies, including interface/ without requiring manual copying.
- Flatten each Lambda function into a single file that Python can execute directly.
- Bundle third-party dependencies (e.g., boto3, requests) without needing site-packages/.
- Minimize unnecessary files, acting as a "tree shaker" where possible.
- Ensure Python can still interpret and run the output file without requiring further setup.

The output would be:
```
lambda_a_bundled.py  # Self-contained Lambda function
lambda_b_bundled.py  # Self-contained Lambda function
```
Each file would contain all necessary imports including dependencies if possible, removing the need to copy the interface/ folder or adjust import paths. 

---

_Label `enhancement` added by @ImreC on 2025-03-07 09:16_

---

_Comment by @stevapple on 2025-03-07 21:57_

> UV would:
> 
> * Resolve and inline all dependencies, including interface/ without requiring manual copying.

I don't think this is ever possible due to the dynamic nature of Python. Consider the following case:
```python
# lib_a.py
import lib_b
...

# lib_b.py
def func_a():
    import lib_a
    ...
```

This will not lead to a dependency circuit in Python as `import lib_a` happens lazily while `lib_a` and `lib_b` will never be imported twice. This will lead to indefinite recursion when applying "inlining", because `lib_a` and `lib_b` are indeed depending (part of themselves) on each other.

> * Bundle third-party dependencies (e.g., boto3, requests) without needing site-packages/.

If we give up making a single self-contained file, this is basically re-implementing [PEP-582](https://www.python.org/dev/peps/pep-0582/), which was unfortunately rejected.

---

_Comment by @ImreC on 2025-03-11 16:20_

This does not seem like a problem that can not be overcome to me because "rolling up the code" makes it irrelevant what imports what at some point. So the idea is to always base it on an entry point. This would mean that you start at the initial method being called and then traverse the AST until you have collected all methods that are actually used in a single file. Using this method, all the original imports can be basically ignored at some point. The only thing you are interested in is what methods are being used, but if they originate from another file you can basically just remove the import statement and include the method in `bundle.py`. 

There will be cases where you are unsure (f.e. boto3 or other dynamic Python magic), but in this case my suggestion would be to just be conservative and include everything. This would lead to a way larger file then necessary, but it will still be smaller than what you would have if you would include a full site-packages every time. 

Another blocker could be other files than .py, for example C dependencies or something. I don't have enough knowledge about the inner workings of Python to properly assess whether this is a major issue, but maybe some embedded pre-compiled code or something can work in these cases? 

To me it seems of a lot of value for the Python ecosystem to have such a thing as it would simplify deployment of Python code by a lot. It also seems like a very interesting problem to work on. I at some point started to do this myself, but I lack a deep understanding about the inner workings of Python (which could also make this whole proposal useless if what I am proposing is not possible). Getting that understanding would take me a lot of time on top of building a solution for this. I am hoping for somebody who is more adept then me to take on this challenge and people who created a killer package manager seem very well equipped to do this. 

PEP-582 seems like a good suggestion also, because indeed all the issues that they are mentioning are similar to what this is supposed to solve. Seeing that is was discussed for more than 4 years before being rejected makes a solution unlikely to come from that angle in the near future. 

---

_Closed by @ImreC on 2025-05-20 09:38_

---
