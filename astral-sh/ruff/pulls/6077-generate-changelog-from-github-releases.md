```yaml
number: 6077
title: Generate changelog from GitHub releases
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: changelog
created_at: 2023-07-25T19:19:23Z
updated_at: 2023-10-06T14:18:12Z
url: https://github.com/astral-sh/ruff/pull/6077
synced_at: 2026-01-12T15:55:20Z
```

# Generate changelog from GitHub releases

---

_@zanieb_

Not a commitment to doing this, but exploring the option of using a `CHANGELOG` file.

[Rendered preview](https://github.com/astral-sh/ruff/blob/changelog/CHANGELOG.md)

Generated with the following

```python
#!/usr/bin/env python
#
# Generate a changelog file from existing GitHub releases.
#
# Usage:
#
#   generate-changelog <repo>
#
# Example: 
#
#   generate-changelog astral-sh/ruff > CHANGELOG.md
#

import os
import re
import sys

from github import Github
from github.GitRelease import GitRelease

# Header for each release tag
RELEASE_HEADER = "## {release.tag_name}"

# Substitutations for items in the body of the release
# Applied in-order (a previous replacement can be further modified by a later one)
# Expects a regular expression pattern / replacement tuple
BODY_SUBS: tuple[tuple[str, str]] = (
    # Indent these headers
    ("## New Contributors", "### New Contributors"),
    ("## Summary", "### Summary"),
    # Remove this header
    ("\n## What's Changed", ""),
    # In some older releases, the first match will not exist and all of the changes
    # will be in a single header
    ("## What's Changed", "\n\n### Changes"),
    # Remove the generated note
    (
        "<!-- Release notes generated using configuration in .github/release.yml at .* -->", 
        ""
    ),
)

def format_release(release: GitRelease) -> str:
    """Convert a release into formatted notes."""
    formatted_notes = RELEASE_HEADER.format(release=release)
    formatted_notes += "\n\n"

    body = release.body
    for pattern, replacement in BODY_SUBS:
        body = re.sub(pattern, replacement, body)
    
    # Remove an excessive whitespace blocks
    while "\r\n\r\n\r\n" in body:
        body = body.replace("\r\n\r\n\r\n", "\n\n")
    while "\n\n\n" in body:
        body = body.replace("\n\n\n", "\n\n")
    
    formatted_notes += body.lstrip()
    formatted_notes += "\n\n"
    return formatted_notes

def main(repository_name: str, token: str, limit: int | None = None):
    gh = Github(token)
    repo = gh.get_repo(repository_name)

    yield "# Changelog\n\n"

    for i, release in enumerate(repo.get_releases()):
        if limit is not None and i > limit:
            return
        if not release.body:
            # Empty release, exclude it from the changelog
            continue
        yield format_release(release)


if __name__ == "__main__":
    token = os.environ.get("GITHUB_TOKEN")
    name = sys.argv[1] if len(sys.argv) > 1 else None
    if name is None:
        print("A repository must be provided.")
        exit(1)

    for line in main(name, token):
        print(line, end="")
```

---

_Comment by @zanieb on 2023-07-25 19:24_

I previously authored a script to generate the release notes for a CHANGELOG file using GitHub's generator https://github.com/PrefectHQ/prefect/blob/main/scripts/generate-release-notes.py which we could adapt here allowing us to continue using GitHub labeling to reduce the maintenance burden of writing notes while allowing for peer review of notes. Releases can link to the header in the CHANGELOG file.

---

_Comment by @konstin on 2023-07-26 12:49_

I looked for a project that we could reuse instead of maintaining our own script and found [rhysd/changelog-from-release](https://github.com/rhysd/changelog-from-release), which also has github action. With `changelog-from-release -r https://github.com/astral-sh/ruff > CHANGELOG.md` i got [this markdown changelog](https://github.com/astral-sh/ruff/blob/konsti-rhysd-changelog-from-release/CHANGELOG.md)

---

_Comment by @zanieb on 2023-07-26 16:02_

Hm while I'm into using existing projects, I don't like the output of that one and I've not seen any real maintenance burden for having my own previously. For clarity, I'm proposing:

1. Running the above script _once_ to generate a changelog file
2. Combine the above script with the one I linked to append generated release notes to the changelog file on demand
3. Use pull requests to publish changelog entries for each release
4. Consider writing an simple internal script to create a GitHub release that links to the proper changelog entry

---

_Comment by @charliermarsh on 2023-07-27 13:56_

I'm cool with the general approach of using a script (and this one specifically) to create the changelog, what I don't fully understand is when this would happen and how it would relate to the rest of the release process. We write the release notes when we convert the release from Draft, so the changelog itself wouldn't end up being _part_ of the release at present.

---

_Closed by @zanieb on 2023-10-06 14:18_

---
