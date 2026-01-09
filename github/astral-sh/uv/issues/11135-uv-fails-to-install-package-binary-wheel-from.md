---
number: 11135
title: uv fails to install package (binary wheel) from internal index while pip succeeds
type: issue
state: closed
author: kzorba
labels:
  - bug
assignees: []
created_at: 2025-01-31T17:01:44Z
updated_at: 2025-02-06T11:13:27Z
url: https://github.com/astral-sh/uv/issues/11135
synced_at: 2026-01-07T13:12:18-06:00
---

# uv fails to install package (binary wheel) from internal index while pip succeeds

---

_Issue opened by @kzorba on 2025-01-31 17:01_

### Summary

Hello,

We have an internal company index and have developed a Python C extension module that is available on the index as a binary wheel (multilinux like `<module>-0.1.1-cp310-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_28_x86_64.whl`).

pip (22.0.2) installs the package fine

```

# kzorba @ kzorba-dev01 in ~/WorkingArea/<project> on git:<branch> x std-venv [16:34:24]
$ pip install -i <internal_index_url> --no-cache-dir <module>
Looking in indexes: <internal_index_url>
Collecting <module>
  Downloading <internal_index_url>/%2Bf/177/6c35106dc4acc/<module>-0.1.1-cp310-abi3-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_28_x86_64.whl (60 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 60.9/60.9 KB 167.9 MB/s eta 0:00:00
Installing collected packages: <module>
Successfully installed <module>-0.1.1
(std-venv)
 
```

Unfortunately uv fails on the same installation:

```
$ uv pip install -v --index <internal_index_url> <module>
DEBUG uv 0.5.20
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/home/kzorba/WorkingArea/<project>/venv/dev-3.10/bin/python3` (active virtual environment)
Using Python 3.10.12 environment at: venv/dev-3.10
DEBUG Acquired lock for `venv/dev-3.10`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.12
DEBUG Solving with target Python version: >=3.10.12
DEBUG Adding direct dependency: <module>*
DEBUG Found stale response for: <internal_index_url>/<module>
DEBUG Sending revalidation request for: <internal_index_url>/<module>
DEBUG Found modified response for: <internal_index_url>/<module>
WARN Skipping file for <module>:
WARN Skipping file for <module>:
WARN Skipping file for <module>:
WARN Skipping file for <module>: +searchhelp
WARN Skipping file for <module>: +status
WARN Skipping file for <module>: 0.1.1
WARN Skipping file for <module>: <module>
WARN Skipping file for <module>: latest
WARN Skipping file for <module>: packages
WARN Skipping file for <module>: pypi
WARN Skipping file for <module>: stable
WARN Skipping file for <module>: stable
DEBUG Searching for a compatible version of <module> (*)
DEBUG No compatible version found for: <module>
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of <module> and you require <module>, we can conclude that your requirements are unsatisfiable.

      hint: `<module>` was found on <internal_index_url>, but not at the requested version (all versions of <module>).
      A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By default, uv will only consider versions that
      are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted,
      use `--index-strategy unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were defined.
DEBUG Released lock at `/home/kzorba/WorkingArea/<project>/venv/dev-3.10/.lock`
```

My understanding is that something is wrong with the html info of the internal index. Issue #11089 seems similar.

How can I help with further investigation? This issue will prevent us from using `uv` in our project and we are already hooked with it :) 

Best regards

### Platform

Linux 5.15.0-113-generic x86_64 GNU/Linux

### Version

uv 0.5.20

### Python version

Python >= 3.10

---

_Label `bug` added by @kzorba on 2025-01-31 17:01_

---

_Comment by @kzorba on 2025-01-31 17:05_

Just tested with latest uv version (0.5.26) and the problem is still there. 

---

_Comment by @zanieb on 2025-01-31 17:07_

What kind of index server are you using?

If you access the index without authentication does it return a 404 / 403 / 401 (etc.) or does it just return an empty list of packages?

---

_Assigned to @zanieb by @zanieb on 2025-01-31 17:07_

---

_Comment by @zanieb on 2025-01-31 17:16_

Are you using the `/simple` endpoint for your index?

---

_Comment by @kzorba on 2025-01-31 17:21_

Here is the relevant html that I receive from the package endpoint (sensitive info removed), hope it helps. Thanks for the amazing response time!

```
<!doctype html>
<html>
    <head>
        <title>packages/stable/: <module> versions</title>
        
    
    <link rel="stylesheet" type="text/css" href="https://<internal_host>/+static-4.0.8/style.css" />
    

    
    <script src="https://<internal_host>/+static-4.0.8/jquery-1.11.1.min.js"></script>
    <script src="https://<internal_host>/+static-4.0.8/moment-2.8.2.min.js"></script>
    <script src="https://<internal_host>/+static-4.0.8/common.js"></script>
    


    </head>
    <body>
        
    <div class="header">
        
    <form method="get" id="search" action="https://<internal_host>/+search">
        
    <h1><a href="https://<internal_host>/">devpi</a></h1>

        <input type="text" size="60" name="query" autofocus />
        <input type="submit" value="Search" />
        <span class="help">
            <a href="https://<internal_host>/+searchhelp">How to<br /> search?</a>
        </span>
        <div class="query_doc inline" style="display: none">
            <span class="help">
                <a href="#">Close help</a>
            </span>
            
    <p>
                To specify a term which contains spaces, use single quotes like this:
                <code>'term with spaces'</code></p>
    <p>
                By using a search like <code>fieldname:term</code>,
                you can search in the following fields:<br /><dl><dt><code>classifiers</code></dt><dd>
                The <a href="https://pypi.org/pypi?%3Aaction=list_classifiers" target="_blank">trove classifiers</a> of a package.
                Use single quotes to specify a classifier, as they contain spaces:
                <code>classifiers:'Programming Language :: Python :: 3'</code></dd><dt><code>index</code></dt><dd>The name of the index. This is only the name part, without the user. For example: <code>index:pypi</code></dd><dt><code>keywords</code></dt><dd>The keywords of a package.</dd><dt><code>name</code></dt><dd>The package name. For example: <code>name:devpi-client</code></dd><dt><code>path</code></dt><dd>The path of the package in the form '/{user}/{index}/{name}'.  For example: <code>path:/root/pypi/devpi-server</code></dd><dt><code>serial</code></dt><dd>Undocumented</dd><dt><code>type</code></dt><dd>
                The type of text.
                One of <code>project</code> for the project name,
                <code>title</code> for the title of a documentation page,
                <code>page</code> for a documentation page,
                or one of the following project metadata fields:
                <code>author</code>, <code>author_email</code>,
                <code>description</code>, <code>keywords</code>,
                <code>summary</code>. For example: <code>type:page</code>
                </dd><dt><code>user</code></dt><dd>The user name.</dd></dl></p>
    <p>
                End a term with an asterisk to search by prefix like this: <code>path:/fschulze/*</code></p>
    <p>
                Group query clauses with parentheses.</p>
    <p>
                Use the <code>AND</code>, <code>OR</code>,
                <code>ANDNOT</code>, <code>ANDMAYBE</code>, and <code>NOT</code><br />
                operators to further refine your search.<br />
                Write them in all capital letters, otherwise they will be interpreted as search terms.<br />
                An example search would be: <code>devpi ANDNOT client</code></p>
    <p>
                Boost a term by adding a circumflex followed by the boost value like this:
                <code>term^2</code></p>

        </div>
    </form>
    <script type="text/javascript">
    //<![CDATA[
        $(function() {
            $('.help a').click(function() {
                var $help = $('.query_doc.inline');
                // is there a docview iframe?
                var $iframe = $('iframe');
                if ($iframe.length && $help.is(':hidden')) {
                    // then hide the iframe's scrollbar
                    // (double scrollbar doesn't look nice)
                    $('body', $iframe[0].contentWindow.document
                      ).css('overflow', 'hidden');
                }
                $help.slideToggle({
                    complete: function () {
                        if ($iframe.length && $help.is(':hidden')) {
                            // give focus and scrollbar back to iframe
                            var iframeWindow = $iframe[0].contentWindow;
                            iframeWindow.focus();
                            $('body', iframeWindow.document
                              ).css('overflow', 'auto');
                        }
                    }
                });
                return false;
            });
        });
    //]]>
    </script>

        <div id="navigation">
            <span>
                <a href="https://<internal_host>/">devpi</a>
            </span>
            <span>
                <a href="https://<internal_host>/packages">packages</a>
            </span>
            <span>
                <a href="https://<internal_host>/packages/stable">stable</a>
            </span>
            <span>
                <a href="https://<internal_host>/packages/stable/<module>"><module></a>
            </span>
            
    <a class="statusbadge ok"
       href="https://<internal_host>/+status">
        ok
    </a>

        </div>
        
    

        
    </div>

        <div id="content">
        <h1>packages/stable/: <module> versions</h1>

        <p class="infonote">
            Because this project isn't in the <code>mirror_whitelist</code>,
            no releases from <strong>root/pypi</strong> are included.
        </p>

        <p>Latest version on stage is: <a href="https://<internal_host>/packages/stable/<module>/latest">0.1.1</a></p>

        <p>Python C extension module <rest of module description> </p>

        <table class="versions">
            <thead>
                <tr>
                    <th>Index</th>
                    <th>Version</th>
                    <th>Documentation</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td><a href="https://<internal_host>/packages/stable">packages/stable</a></td>
                    <td><a href="https://<internal_host>/packages/stable/<module>/0.1.1">0.1.1</a></td>
                    <td></td>
                </tr>
            </tbody>
        </table>
        </div>
        
    <footer>
        
        
    <ul class="footer-versions">
        <li>devpi-server-6.5.1</li>
        <li>devpi-web-4.0.8</li>
    </ul>

    </footer>

    </body>
</html>

```

---

_Comment by @kzorba on 2025-01-31 17:27_

We have 3 "directories" for dev, staging, production/stable packages, I use a url like 

`https://<index_host>/packages/stable`

No http auth, I can access this directly from the internal machines. Hopefully this answers your question.

---

_Comment by @zanieb on 2025-01-31 17:30_

We expect a [Simple API](https://packaging.python.org/en/latest/specifications/simple-repository-api/#base-html-api) compliant result, e.g., `view-source:pypi.org/simple/anyio/`

It looks like you're using devpi. devpi serves a non-complaint index (which I believe there is legacy support for in pip) at its root path. I think you need to add `/+simple` to the end of your index URL.

---

_Comment by @kzorba on 2025-01-31 17:31_

Using 

`https://<internal_index_host>/packages/stable/+simple/`

solved it!!!!

Thanks a lot, keep up the excellent work :) 

---

_Comment by @zanieb on 2025-01-31 17:34_

Yep no problem.

We're tracking support for that index type in https://github.com/astral-sh/uv/issues/4907

---

_Closed by @zanieb on 2025-01-31 17:34_

---

_Referenced in [astral-sh/uv#11136](../../astral-sh/uv/issues/11136.md) on 2025-01-31 17:37_

---

_Referenced in [astral-sh/uv#11276](../../astral-sh/uv/issues/11276.md) on 2025-02-06 11:12_

---

_Comment by @Ada-lave on 2025-02-06 11:13_

Thx

---
