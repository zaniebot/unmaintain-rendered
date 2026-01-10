---
number: 6012
title: Software Bill of Materials (SBOM) output
type: issue
state: closed
author: chrisrodrigue
labels:
  - wish
assignees: []
created_at: 2024-08-11T23:18:36Z
updated_at: 2025-11-20T18:49:25Z
url: https://github.com/astral-sh/uv/issues/6012
synced_at: 2026-01-10T01:23:55Z
---

# Software Bill of Materials (SBOM) output

---

_Issue opened by @chrisrodrigue on 2024-08-11 23:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
`uv` is in a prime position to be able to emit an SBOM that reflects the state of the current `uv`-managed virtual environment, or ingest an SBOM to produce a managed virtual environment.

[SBOM](https://www.cisa.gov/sbom) requirements supersede any existing PEP. [Executive Order 14028](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/ ) demands SBOM documentation from all vendors to the US government by September 2023. It is reasonable to assume that `uv` may indirectly fall under this executive order if there are downstream users of `uv` that provide software to the US government.

It would be awesome if `uv` could emit an SBOM in one of the industry standard formats. [CycloneDX](https://cyclonedx.org/specification/overview/ ) is one such SBOM standard. I think `uv` may already have all the metadata necessary to output one.

SBOM support would greatly improve the security posture of `uv` and `uv`-managed projects.


---

_Comment by @kdeldycke on 2024-08-12 09:08_

As [hinted previously](https://github.com/astral-sh/uv/issues/4711#issuecomment-2266020335), you can use my [Meta Package Manager](https://github.com/kdeldycke/meta-package-manager) CLI in the mean time to export the SBOM of a `uv` environment:
```shell-session
$ mpm --uv sbom --spdx                   
info: User selection of managers by priority: > uv
info: Managers dropped by user: None
info: Print SPDX export to <stdout>
info: Export packages from uv...
```
```json
{
    "SPDXID": "SPDXRef-DOCUMENT",
    "creationInfo": {
        "created": "2024-08-02T23:29:10Z",
        "creators": [
            "Tool: meta-package-manager-5.18.1"
        ]
    },
    "dataLicense": "CC0-1.0",
    "name": "macOS-Darwin-23.6.0-arm64",
    "spdxVersion": "SPDX-2.3",
    "documentNamespace": "https://github.com/kdeldycke/meta-package-manager/releases/tag/v5.18.1/fdcf50b29504cc4c0df620af2283ed3",
    "packages": [
        {
            "SPDXID": "SPDXRef-pkg-uv-alabaster-0.7.16",
            "downloadLocation": "https://www.example.com",
            "externalRefs": [
                {
                    "referenceCategory": "PACKAGE_MANAGER",
                    "referenceLocator": "pkg:uv/alabaster@0.7.16",
                    "referenceType": "purl"
                }
            ],
            "filesAnalyzed": false,
            "name": "alabaster",
            "primaryPackagePurpose": "INSTALL",
            "supplier": "Organization: UV",
            "versionInfo": "0.7.16"
        },
        {
            "SPDXID": "SPDXRef-pkg-uv-arrow-1.3.0",
            "downloadLocation": "https://www.example.com",
            "externalRefs": [
                {
                    "referenceCategory": "PACKAGE_MANAGER",
                    "referenceLocator": "pkg:uv/arrow@1.3.0",
                    "referenceType": "purl"
                }
            ],
            "filesAnalyzed": false,
            "name": "arrow",
            "primaryPackagePurpose": "INSTALL",
            "supplier": "Organization: UV",
            "versionInfo": "1.3.0"
        },
...
```

---

_Renamed from "Software Bill of Materials (SBOM) output feature" to "FR: Software Bill of Materials (SBOM) output" by @chrisrodrigue on 2024-08-22 11:26_

---

_Comment by @chrisrodrigue on 2024-10-31 20:25_

Taking a CycloneDX BOM as *input* could also be an awesome way to set up a venv. 

Any python components in the BOM would have a PyPI purl, which uv could read and sync with `pyproject.toml` and the managed venv.

See related issue here regarding purl support: https://github.com/astral-sh/uv/issues/8265

---

_Comment by @sanmai-NL on 2024-11-23 16:37_

@chrisrodrigue, for the sake of completeness, can you please describe alternatives, including complementary tools, that can provide this information? If standard tools can output it for various build tools, why should `uv` implement this?

---

_Comment by @chrisrodrigue on 2024-11-24 01:07_

Well, SBOM is a standardized specification describing the dependencies of a software project. It is highly relevant in the realm of security and enables interoperability between various CI/CD tools such as vulnerability scanners.

[Trivy](https://trivy.dev/v0.33/docs/sbom/), [Harbor](https://goharbor.io/docs/edge/administration/sbom-integration/), and [Docker](https://docs.docker.com/reference/cli/docker/scout/sbom/) are just a few examples of tools that can understand SBOMs, but these are largely focused around OCI containers.

Tools that support generating SBOMs for Python projects usually only support `poetry.lock` or arbitrary virtual environments, rather than `uv.lock`. However, the `uv.lock` file contains most or all of the data necessary for a minimal SBOM, but it is non-standard. 

uv is the best candidate for being able to convert its own `uv.lock` into an SBOM, and it could ingest an SBOM to reproduce a virtual environment from an already attested set of dependencies.

---

_Comment by @zanieb on 2024-11-26 00:12_

This sounds cool as a `uv export` format option. I don't think it's on our immediate roadmap though.

---

_Label `wish` added by @zanieb on 2024-11-26 00:12_

---

_Comment by @chrisrodrigue on 2024-11-26 16:48_

A `uv export` format option sounds perfect!

Notes: The [CycloneDX spec](https://cyclonedx.org/docs/1.6/json/) is currently at version 1.6. It supports really rich metadata, but the only required fields in an SBOM are `bomFormat` and `bomVersion`. For `components`, the `type` and `name` fields are minimally required. I think it would be safe to use `”library”` as the default `type` for packages, and the package name as the `name`. 

For each component, the minimal correct metadata to have would likely be `purl` and `hashes` (for PyPI packages), or an `externalReferences` array (for non-PyPI packages) with the package source URL as the `url`, type set to `”distribution”`, and the `hashes` array set with `alg` and `content` for each hash. Lastly, the `dependencies` array is used to capture the dependency graph. I would recommended supporting at least version 1.5 of the spec.

(Could a reverse `uv import` option that takes an SBOM to set up a project/venv also be useful?)

---

_Comment by @zanieb on 2024-11-26 17:05_

> (Could a reverse uv import option that takes an SBOM to set up a project/venv also be useful?)

Yeah this would also be cool, but would need quite a bit of design. It'd also be useful for importing existing `pyproject.toml` from other project managers like Poetry.

If someone is interested in working on `uv export` format, I'm supportive of it — we just won't be working on it ourselves right now.

---

_Comment by @jkowalleck on 2025-03-02 19:36_

> [@chrisrodrigue](https://github.com/chrisrodrigue), for the sake of completeness, can you please describe alternatives, including complementary tools, that can provide this information? If standard tools can output it for various build tools, why should `uv` implement this?

https://pypi.org/project/cyclonedx-bom/ is able to generate SBOM from a Python (virtual) environment.

There is currently an open ticket asking for docs how it can be utilized with `uv` easily
- https://github.com/CycloneDX/cyclonedx-python/issues/857

Maybe this is something you guys can help? 

---

_Referenced in [WeblateOrg/weblate#14106](../../WeblateOrg/weblate/issues/14106.md) on 2025-03-04 16:21_

---

_Referenced in [CycloneDX/cyclonedx-python#857](../../CycloneDX/cyclonedx-python/issues/857.md) on 2025-03-05 14:52_

---

_Comment by @chrisrodrigue on 2025-04-24 13:44_

Good news: [PEP 770](https://peps.python.org/pep-0770/) was just accepted, which notably reserves the subdirectory `.dist-info/sboms`

Some highlights:

> [SBOM files in project formats](https://peps.python.org/pep-0770/#sbom-files-in-project-formats)
>
>A few additions will be made to the existing specifications.
> 
> [Built distributions](https://packaging.python.org/en/latest/glossary/#term-Built-Distribution) ([wheels](https://packaging.python.org/en/latest/glossary/#term-Wheel))
> The wheel specification will be updated to add the new registry of reserved directory names and to reflect that if the `.dist-info/sboms` subdirectory is specified that the directory contains SBOM files.
>
> [Installed projects](https://packaging.python.org/en/latest/glossary/#term-Installed-Project)
> The Recording Installed Projects specification will be updated to reflect that if the `.dist-info/sboms` subdirectory is specified that the directory contains SBOM files and that any files in this directory MUST be copied from wheels by install tools.

@konstin I know you are doing great work on the `uv_build` backend, so this might be of interest to you.



---

_Comment by @snyk-tim on 2025-09-01 15:26_

Our team at Snyk are big fans of uv and we're very excited about the direction of the project. We've been following the discussion in this issue and would love to contribute the implementation for this feature.

We have some engineering capacity to dedicate to this and want to ensure we build it in a way that aligns with the project's standards. Before we dive in, we wanted to propose an initial approach and get your feedback.

Based on the existing discussion, here's what we have in mind:

### Proposed Implementation

- Add an additional format to the `uv export` command that generates an SBOM from the lockfile.
- Use the format name `cyclonedx1.6+json` to explicitly specify the SBOM format in use. Using this approach would allow future SBOM styles to be output if required, such as XML, newer versions or SPDX SBOMs.
- Create a new `CycloneDx16Export` struct in the `uv-resolver` crate following the same pattern as `RequirementsTxtExport` with a `from_lock()` method and `Display` trait implementation.
- Add new SBOM focused structs to support a minimum CycloneDX structure for generating JSON output.
  - An alternative is to use the [official CycloneDX crate](https://github.com/CycloneDX/cyclonedx-rust-cargo/blob/main/cyclonedx-bom/README.md), which supports multiple CycloneDX versions and both JSON and XML output, but is currently limited to version 1.5. Feedback on the use of third-party dependencies would be welcome.
- The current implementation of `uv pip compile` uses the `ExportFormat` enum for the `--format` flag. This should be replaced with a format type specific to `pip compile`.

### Limitations

- The proposal is limited to adding the CycloneDX SBOM export option and does not encompass any implementation of PEP-770. The resulting SBOM can serve as a foundation for adding PEP-770 support in the future.
- The proposal does not include support for using an SBOM to create a venv. It is limited to exporting from a uv lockfile only.

Looking forward to your feedback!


---

_Comment by @woodruffw on 2025-09-04 13:20_

Hey @snyk-tim! Thanks a ton for following up on this thread.

We're excited to hear more about how you and the other Snyk folks would contribute here!

We talked about this a bit internally, and I think we _don't_ want to own any CycloneDX structure/data modeling long term, so I'd lean towards using the official CycloneDX crate as a 3p dependency (presuming you all think it's trustworthy). To whit:

1. Do you have thoughts on how hard it would be for the Snyk folks to add 1.6 support there?
2. Is 1.6 support a hard blocker, or more of a nice-to-have?

Related to the above: do you have thoughts on what the compatibility policy should be here? I'm abstractly concerned that at some point we'll want to bump the version of CycloneDX emitted, at which point anybody using `--format=cyclonedx1.5+json` (for example) would be broken and would need to both upgrade to 1.6 *and* change however they've scripted `uv export`.

Similarly, do you have thoughts on supporting *just* the JSON output, versus supporting both XML and JSON? I'm curious how many users really need both, and I figure you all have more insight there.

Apart from the above, I think the overall approach you've laid out makes a lot of sense! 

---

_Comment by @jkowalleck on 2025-09-04 14:40_

prior art:  https://pypi.org/project/cyclonedx-bom/
`uv` test bed: <https://github.com/CycloneDX/cyclonedx-python/tree/main/tests/_data/infiles/environment/via-uv>
SBOM results: 
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.0.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.1.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.2.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.3.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.4.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.5.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.6.xml.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.2.json.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.3.json.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.4.json.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.5.json.bin>
- <https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/environment/plain_via-uv_1.6.json.bin>

---

_Renamed from "FR: Software Bill of Materials (SBOM) output" to "Software Bill of Materials (SBOM) output" by @konstin on 2025-09-04 16:10_

---

_Comment by @snyk-tim on 2025-09-09 12:25_

Hey @woodruffw, thanks for the quick and positive feedback! We're excited to get started.

Great to hear you're happy to use the official CycloneDX crate. That makes the path forward much clearer.

Here are our thoughts based on your questions and our internal discussion:

### CycloneDX Versioning
Regarding the CycloneDX version, for the specific use case of representing a uv lockfile, the functional differences between CycloneDX 1.5 and 1.6 are minor. This makes 1.6 a nice-to-have but not a hard blocker.

### Format Flag and Compatibility
If you decide to introduce support for a newer version of CycloneDX, with the proposed format name `cyclonedx<version>+<format>`, we can support multiple versions and output formats at the same time without impacting existing users. The CycloneDX Rust package supports outputting multiple versions and formats so it should be easy to maintain support for multiple versions.

### JSON and XML Support
At Snyk, we almost exclusively see customers using JSON, although XML is still used occasionally. The CycloneDX Rust package supports outputting multiple versions and formats so it would be trivial to add support for XML now or in the future. For the initial implementation, we suggest focusing on supporting JSON output only.

### Summary
To summarize our updated proposal:
- Implement SBOM export using the official CycloneDX crate targeting CycloneDX 1.5.
- Add support for CycloneDX 1.5 JSON output format as `cyclonedx1.5+json`.

If this approach sounds good to you, we can begin working on an initial PR. Looking forward to hearing your thoughts!


---

_Comment by @woodruffw on 2025-09-09 14:14_

> Regarding the CycloneDX version, for the specific use case of representing a uv lockfile, the functional differences between CycloneDX 1.5 and 1.6 are minor. This makes 1.6 a nice-to-have but not a hard blocker.

Cool, I'm a +1 on moving forwards with 1.5 then 🙂 


> At Snyk, we almost exclusively see customers using JSON, although XML is still used occasionally. The CycloneDX Rust package supports outputting multiple versions and formats so it would be trivial to add support for XML now or in the future. For the initial implementation, we suggest focusing on supporting JSON output only.

This sounds good to me! I'm curious what @zanieb and others think, but I'm personally inclined to suggest that we make this `--format=cyclonedx1.5` *without* the `+json` qualifier -- we could add `+json` as a qualifier later on *if* we end up adding support for the XML form, but I don't think that's a given (since end users themselves haven't indicated any need for XML support). Do you have thoughts on that?

Apart from the above, your proposed approach sounds good to me! The flag name is just a bikeshed, so I think you can begin working on it and we can always figure that our further in 🙂 

---

_Comment by @zanieb on 2025-09-09 15:08_

I'm fine with dropping the json qualifier though I think we should allow it 

---

_Comment by @snyk-tim on 2025-09-11 15:09_

Sounds good, thank you for confirming.

We'll proceed with `--format=cyclonedx1.5` for JSON output. We'll start planning the work and will be in touch when we have a PR ready.

---

_Comment by @thomasschafer on 2025-09-24 16:44_

Hi team - following up on the conversation above, we at Snyk (@snyk-tim, @snyk-will and I) have started on the implementation, and while there's a fair bit left to do we wanted to get a bit of early feedback to see if we're heading in the right direction. One point we're unsure about is that there is an `ExportFormat` enum which is used both by the `export` and `pip compile` commands, and it doesn't seem like we'd want an SBOM schema to be a valid format for `pip compile`, so we've split the enum up. Would you mind having a quick scan of the changes [here](https://github.com/thomasschafer/uv/pull/4/) to see what you think?

---

_Assigned to @woodruffw by @woodruffw on 2025-09-24 16:57_

---

_Comment by @woodruffw on 2025-09-24 16:58_

Thanks @thomasschafer! I can take a look at those changes now.

---

_Referenced in [thomasschafer/uv#4](../../thomasschafer/uv/pulls/4.md) on 2025-10-03 16:13_

---

_Comment by @thomasschafer on 2025-10-17 13:43_

Hi team, we're trying to work out the best way to add metadata for workspace paths and PEP 508 markers to SBOMs exported from uv. We think the best way to include these would be under [component properties](https://cyclonedx.org/docs/1.5/json/#components_items_properties), e.g. for a marker:

```json
"properties": [
  {
    "name": "cdx:python:package:marker",
    "value": "python_full_version >= '3.12' or sys_platform == 'win32'"
  }
]
```

We think it would be best to namespace uv-specific properties under `cdx:uv`, following the standard approach of tools like Poetry (`cdx:poetry`), as shown [here](https://github.com/CycloneDX/cyclonedx-property-taxonomy/blob/main/cdx.md). We would contribute the namespace and taxonomy, similarly to [this PR](https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/29/files). Are you happy with this general approach?

We think we'd need properties for the following:

  * **Workspace paths:** We'd add `cdx:uv:workspace:path`, which would be for workspace packages only, containing the relative path to the package from the project root.
  * **PEP 508 markers:** We think it would make sense to add this to the `cdx:python` namespace as `cdx:python:package:marker`, given that this is not a uv-specific concept, but if this is rejected by the Python CycloneDX maintainers we'd instead use the uv namespace i.e. `cdx:uv:package:marker`.

@jkowalleck it would also be great to get your opinion here if you don't mind, as I can see that you have contributed a lot to the [cyclonedx-property-taxonomy](https://github.com/CycloneDX/cyclonedx-property-taxonomy) repo! 🙏


---

_Comment by @jkowalleck on 2025-10-17 14:03_

re https://github.com/astral-sh/uv/issues/6012#issuecomment-3415682236

> We think it would be best to namespace uv-specific properties under `cdx:uv`


**PEP 508 markers**: For the general python markers, sure, write a ticket/pullrequest to add something to the `cdx:python:*` namespace.
This would benefit everybody working with Python components.


**Workspace paths**: All these `cdx:*` properties are owned by the CycloneDX team - see https://github.com/CycloneDX/cyclonedx-property-taxonomy/blob/f7c4136bf6175d2b0bef1dc0b1303a509e20bbb2/README.md?plain=1#L83. Using one like this means, that you dont have control over it. 
I'd recommend that you register your own `uv` namespace - see <https://github.com/CycloneDX/cyclonedx-property-taxonomy?tab=readme-ov-file#registering-new-top-level-namespaces>




---

_Comment by @thomasschafer on 2025-10-17 14:20_

This is useful, thank you!

---

_Comment by @jkowalleck on 2025-10-17 14:30_

re: PEP 508 - i dont think it is appropriate to put the constraints for dependencies in properties on a component.
dependencies and their constraints go into the `components` properties - these constraints are actually defined outside the respective dependent package - they come from the other package that defines the dependency

see the following scenario:
we have python packages `A`, `B`, `C`, `D`. 
* `A` depends on `B` 
* `A` depends on `C`
* `B` depends on `D`, only if `python_full_version >= '3.12'`
* `C` depends on `D`, no matter what.

how do you intend to solve this with properties? 



---

_Comment by @thomasschafer on 2025-10-17 15:01_

It looks like uv has some method of solving this before reaching the export format-specific code - for instance, given this `pyproject.toml`:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
    "trio ; python_version > '3.11'",
    "trio ; sys_platform == 'win32'",
]

[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"
```

we get this `requirements.txt`:

```
# This file was autogenerated by uv via the following command:
#    uv export --cache-dir [CACHE_DIR]
-e .
attrs==23.2.0 ; python_full_version >= '3.12' or sys_platform == 'win32' \
    --hash=sha256:935dc3b529c262f6cf76e50877d35a4bd3c1de194fd41f47a2b7ae8f19971f30 \
    --hash=sha256:99b87a485a5820b23b879f04c2305b44b951b502fd64be915879d77a7e8fc6f1
    # via
    #   outcome
    #   trio
cffi==1.16.0 ; (python_full_version >= '3.12' and implementation_name != 'pypy' and os_name == 'nt') or (implementation_name != 'pypy' and os_name == 'nt' and sys_platform == 'win32') \
    --hash=sha256:2c56b361916f390cd758a57f2e16233eb4f64bcbeee88a4881ea90fca14dc6ab \
    --hash=sha256:68678abf380b42ce21a5f2abde8efee05c114c2fdb2e9eef2efdb0257fba1235 \
    --hash=sha256:9f90389693731ff1f659e55c7d1640e2ec43ff725cc61b04b2f9c6d8d017df6a \
    --hash=sha256:b2ca4e77f9f47c55c194982e10f058db063937845bb2b7a86c84a6cfe0aefa8b \
    --hash=sha256:bcb3ef43e58665bbda2fb198698fcae6776483e0c4a631aa5647806c25e02cc0 \
    --hash=sha256:db8e577c19c0fda0beb7e0d4e09e0ba74b1e4c092e0e40bfa12fe05b6f6d75ba \
    --hash=sha256:e6024675e67af929088fda399b2094574609396b1decb609c55fa58b028a32a1
    # via trio
exceptiongroup==1.2.0 ; python_full_version < '3.11' and sys_platform == 'win32' \
    --hash=sha256:4bfd3996ac73b41e9b9628b04e079f193850720ea5945fc96a08633c66912f14 \
    --hash=sha256:91f5c769735f051a4290d52edd0858999b57e5876e9f85937691bd4c9fa3ed68
    # via trio
idna==3.6 ; python_full_version >= '3.12' or sys_platform == 'win32' \
    --hash=sha256:9ecdbbd083b06798ae1e86adcbfe8ab1479cf864e4ee30fe4e46a003d12491ca \
    --hash=sha256:c05567e9c24a6b9faaa835c4821bad0590fbb9d5779e7caa6e1cc4978e7eb24f
    # via trio
outcome==1.3.0.post0 ; python_full_version >= '3.12' or sys_platform == 'win32' \
    --hash=sha256:9dcf02e65f2971b80047b377468e72a268e15c0af3cf1238e6ff14f7f91143b8 \
    --hash=sha256:e771c5ce06d1415e356078d3bdd68523f284b4ce5419828922b6871e65eda82b
    # via trio
pycparser==2.21 ; (python_full_version >= '3.12' and implementation_name != 'pypy' and os_name == 'nt') or (implementation_name != 'pypy' and os_name == 'nt' and sys_platform == 'win32') \
    --hash=sha256:8ee45429555515e1f6b185e78100aea234072576aa43ab53aefcae078162fca9 \
    --hash=sha256:e644fdec12f7872f86c58ff790da456218b10f863970249516d60a5eaca77206
    # via cffi
sniffio==1.3.1 ; python_full_version >= '3.12' or sys_platform == 'win32' \
    --hash=sha256:2f6da418d1f1e0fddd844478f41680e794e6051915791a034ff65e5f100525a2 \
    --hash=sha256:f4324edc670a0f49750a81b895f35c3adb843cca46f0530f79fc1babb23789dc
    # via trio
sortedcontainers==2.4.0 ; python_full_version >= '3.12' or sys_platform == 'win32' \
    --hash=sha256:25caa5a06cc30b6b83d11423433f65d1f9d76c4c6a0c90e3379eaa43b9bfdb88 \
    --hash=sha256:a163dcaede0f1c021485e957a39245190e74249897e2ae4b2aa38595db237ee0
    # via trio
trio==0.25.0 ; python_full_version >= '3.12' or sys_platform == 'win32' \
    --hash=sha256:9b41f5993ad2c0e5f62d0acca320ec657fdb6b2a2c22b8c7aed6caf154475c4e \
    --hash=sha256:e6458efe29cc543e557a91e614e2b51710eba2961669329ce9c862d50c6e8e81
    # via project
```

where the markers have been resolved. We can just use this existing logic when exporting SBOMs to get the relevant markers

---

_Comment by @jkowalleck on 2025-10-19 11:06_

these "markers" are constraints, defining UNDER WHICH CIRCUMSTANCES a dependency is about to be used.
the information decides whether a component is a dependency and shall be installed. (regardless the constraints, a dependency is a dependency, right?)

how do you plan to put this "marker" information into an SBOM? My answer would be: this information does not belong in an SBOM. 
And, I dont see a technical way to put this information in any SBOM anyway.

---

_Comment by @chrisrodrigue on 2025-10-19 13:58_

The `dependencies` property of the cdx spec documents the dependencies for each component, so I think the logic here is to use the `# via <component>` comments in the `requirements.txt` to populate the `dependsOn` array for each component with already resolved dependencies. This information appears to be cached, so it can be reused instead of attempting to resolve the dependency graph again. Does that sound right?

---

_Comment by @chrisrodrigue on 2025-10-19 14:11_

> We'll proceed with `--format=cyclonedx1.5` for JSON output. We'll start planning the work and will be in touch when we have a PR ready.

May I suggest the more explicit: 

`--schema=cyclonedx1.5 --format=json`


---

_Comment by @thomasschafer on 2025-10-19 15:50_

> these "markers" are constraints, defining UNDER WHICH CIRCUMSTANCES a dependency is about to be used. the information decides whether a component is a dependency and shall be installed. (regardless the constraints, a dependency is a dependency, right?)
> 
> how do you plan to put this "marker" information into an SBOM? My answer would be: this information does not belong in an SBOM. And, I dont see a technical way to put this information in any SBOM anyway.

Given this `pylock.toml`:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["anyio ; sys_platform == 'darwin'", "iniconfig"]

[build-system]
requires = ["setuptools>=42"]
build-backend = "setuptools.build_meta"
```

I'd suggest that the SBOM should look like this, which the WIP implementation we're working on produces:

<details>

```json
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.5",
  "version": 1,
  "serialNumber": "[SERIAL_NUMBER]",
  "metadata": {
    "timestamp": "[TIMESTAMP]",
    "tools": [
      {
        "vendor": "Astral Software Inc.",
        "name": "uv",
        "version": "[VERSION]"
      }
    ],
    "component": {
      "type": "application",
      "bom-ref": "1-project@0.1.0",
      "name": "project",
      "version": "0.1.0"
    }
  },
  "components": [
    {
      "type": "library",
      "bom-ref": "2-anyio@4.3.0",
      "name": "anyio",
      "version": "4.3.0",
      "purl": "pkg:pypi/anyio@4.3.0",
      "properties": [
        {
          "name": "uv:package:marker",
          "value": "sys_platform == 'darwin'"
        }
      ]
    },
    {
      "type": "library",
      "bom-ref": "3-idna@3.6",
      "name": "idna",
      "version": "3.6",
      "purl": "pkg:pypi/idna@3.6",
      "properties": [
        {
          "name": "uv:package:marker",
          "value": "sys_platform == 'darwin'"
        }
      ]
    },
    {
      "type": "library",
      "bom-ref": "4-iniconfig@2.0.0",
      "name": "iniconfig",
      "version": "2.0.0",
      "purl": "pkg:pypi/iniconfig@2.0.0"
    },
    {
      "type": "library",
      "bom-ref": "5-sniffio@1.3.1",
      "name": "sniffio",
      "version": "1.3.1",
      "purl": "pkg:pypi/sniffio@1.3.1",
      "properties": [
        {
          "name": "uv:package:marker",
          "value": "sys_platform == 'darwin'"
        }
      ]
    }
  ],
  "dependencies": [
    {
      "ref": "2-anyio@4.3.0",
      "dependsOn": [
        "3-idna@3.6",
        "5-sniffio@1.3.1"
      ]
    },
    {
      "ref": "3-idna@3.6",
      "dependsOn": []
    },
    {
      "ref": "4-iniconfig@2.0.0",
      "dependsOn": []
    },
    {
      "ref": "1-project@0.1.0",
      "dependsOn": [
        "2-anyio@4.3.0",
        "4-iniconfig@2.0.0"
      ]
    },
    {
      "ref": "5-sniffio@1.3.1",
      "dependsOn": []
    }
  ]
}
```

</details>

For reference, here is the `requirements.txt` that uv current exports:

<details>

```
# This file was autogenerated by uv via the following command:
#    uv export --cache-dir [CACHE_DIR]
-e .
anyio==4.3.0 ; sys_platform == 'darwin' \
    --hash=sha256:048e05d0f6caeed70d731f3db756d35dcc1f35747c8c403364a8332c630441b8 \
    --hash=sha256:f75253795a87df48568485fd18cdd2a3fa5c4f7c5be8e5e36637733fce06fed6
    # via project
idna==3.6 ; sys_platform == 'darwin' \
    --hash=sha256:9ecdbbd083b06798ae1e86adcbfe8ab1479cf864e4ee30fe4e46a003d12491ca \
    --hash=sha256:c05567e9c24a6b9faaa835c4821bad0590fbb9d5779e7caa6e1cc4978e7eb24f
    # via anyio
iniconfig==2.0.0 \
    --hash=sha256:2d91e135bf72d31a410b17c16da610a82cb55f6b0477d1a902134b24a455b8b3 \
    --hash=sha256:b6a85871a79d2e3b22d2d1b94ac2824226a63c6b741c88f7ae975f18b6778374
    # via project
sniffio==1.3.1 ; sys_platform == 'darwin' \
    --hash=sha256:2f6da418d1f1e0fddd844478f41680e794e6051915791a034ff65e5f100525a2 \
    --hash=sha256:f4324edc670a0f49750a81b895f35c3adb843cca46f0530f79fc1babb23789dc
    # via anyio
```

</details>

It's useful to have these markers in the SBOM so that vulnerability scanning tools can filter out issues that are not relevant given a particular environment. For instance, in the example above, if there was a vulnerability in `anyio@4.3.0` (which has marker `sys_platform == 'darwin'`), then customers using darwin as a platform would be vulnerable and should be informed as such, and those using another platform would not and so should not be given a false positive vulnerability finding.

Let me know if I'm missing something!

---

_Comment by @thomasschafer on 2025-10-19 15:55_

> May I suggest the more explicit:
> 
> `--schema=cyclonedx1.5 --format=json`

I don't object to this but, given how little usage there is of XML CycloneDX SBOMs, we weren't sure how useful it would be when discussing previously. Even if we do want to add a `--format` flag that can be either `json` or `xml`, I would suggest that `json` should be the default so that users can still just use `--schema=cyclonedx1.5` and get a JSON SBOM out, with an option to set `--format=xml` if needed. I'm happy to let the uv maintainers decide here

---

_Comment by @chrisrodrigue on 2025-10-19 16:47_

I don’t see anything wrong with defaulting to JSON since it s probably the most widely used format in this space!

> Let me know if I'm missing something!

Any thoughts on adding `hashes` for each component?

---

_Comment by @chrisrodrigue on 2025-10-19 16:58_

Also… I wonder if it would be a good idea to omit `"dependsOn": []` for components without dependencies. `dependsOn` doesn’t seem to be a required property per [this](https://cyclonedx.org/docs/1.5/json/#dependencies_items_dependsOn).

Maybe there is a benefit to the added lines/noise that isn’t immediately obvious?

---

_Comment by @thomasschafer on 2025-10-19 19:57_

> Any thoughts on adding `hashes` for each component?

It didn't feel immediately necessary given that there are no imminent plans for importing an SBOM into a uv project, but I'm interested to hear others' thoughts.

If we did want to add hashes then there would be some questions to address I think - for instance, where would we put them in the SBOM? They could be added as uv-namespaced properties, such as `uv:package:hashes`, or alternatively we could make use of `externalReferences` (see [here](https://cyclonedx.org/docs/1.5/json/#components_items_externalReferences)) which has a `hashes` field, but  `externalReferences` also requires fields `type` and `url` - I guess type would be `distribution` and `url` would be easy for wheels but less clear for other types of package. I think `externalReferences` makes the most sense as it is referenced in the Python namespace taxonomy [here](https://github.com/CycloneDX/cyclonedx-property-taxonomy/blob/main/cdx/python.md#cdxpythonpackagesource-namespace-taxonomy).

Maybe could be added in a separate PR when these questions are fleshed out a little more?

---

_Comment by @thomasschafer on 2025-10-19 20:03_

> Also… I wonder if it would be a good idea to omit `"dependsOn": []` for components without dependencies. `dependsOn` doesn’t seem to be a required property per [this](https://cyclonedx.org/docs/1.5/json/#dependencies_items_dependsOn).

I thought that most users would read
* empty `dependsOn` -> we looked for dependencies and there are none
* omitted `dependsOn` -> there might be some dependencies but we didn't look

I think that technically (according to the [spec](https://cyclonedx.org/docs/1.5/json/#dependencies_items_dependsOn)) an omitted `dependsOn` actually does mean that there are no deps, but I wonder if just being explicit will make users' (and maintainers') lives easier, at the cost of a very slightly larger SBOM?

---

_Comment by @zanieb on 2025-10-20 14:07_

> It's useful to have these markers in the SBOM so that vulnerability scanning tools can filter out issues that are not relevant given a particular environment

I agree having the markers in the SBOM seems useful.

Naively, I'm not sure it should use a `uv`-specific tag though? The markers are just standard Python package markers and that makes it sound like a tool-specific format. I might expect `python:package:marker`?

edit: I missed this comment https://github.com/astral-sh/uv/issues/6012#issuecomment-3415750026 which discusses this as well.

> May I suggest the more explicit:
>
> --schema=cyclonedx1.5 --format=json

I don't think this matches our existing use of `--format` here and I don't think it's worth the complexity of adding another flag. We can always consider something like this in the future if the need arises.

> Any thoughts on adding hashes for each component?

This seems reasonable to me.

edit: I agree with https://github.com/astral-sh/uv/issues/6012#issuecomment-3419914733 it seems fine to defer this work too.

> I wonder if it would be a good idea to omit "dependsOn": [] for components without dependencies. dependsOn doesn’t seem to be a required property per [this](https://cyclonedx.org/docs/1.5/json/#dependencies_items_dependsOn).

I'd probably keep it, seems like it'd make life easier for a consumer.

---

_Comment by @jkowalleck on 2025-10-20 14:30_

> > Any thoughts on adding `hashes` for each component?
> 
> It didn't feel immediately necessary given that there are no imminent plans for importing an SBOM into a uv project, but I'm interested to hear others' thoughts.
> 
> If we did want to add hashes then there would be some questions to address I think - for instance, where would we put them in the SBOM? They could be added as uv-namespaced properties, such as `uv:package:hashes`, or alternatively we could make use of `externalReferences` (see [here](https://cyclonedx.org/docs/1.5/json/#components_items_externalReferences)) which has a `hashes` field, but `externalReferences` also requires fields `type` and `url` - I guess type would be `distribution` and `url` would be easy for wheels but less clear for other types of package. I think `externalReferences` makes the most sense as it is referenced in the Python namespace taxonomy [here](https://github.com/CycloneDX/cyclonedx-property-taxonomy/blob/main/cdx/python.md#cdxpythonpackagesource-namespace-taxonomy).
> 
> Maybe could be added in a separate PR when these questions are fleshed out a little more?


are these hashes calculated and owned by`uv`? then these hashes would go into a `$.components[].property`. at best, a property owned by the UV team.

if the hashes are just the usual checksums of a package(wheel/archive) that might be downloaded to resolve a dependency, then the hashes go to external references. 

I highly suggest you look at the prior art:
- requirements
  - input: https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/infiles/requirements/with-hashes.txt  
  - SBOM result: https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/requirements/file_with-hashes_1.6.json.bin
- poetry 
  - input lockfile: https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/infiles/poetry/with-extras/lock21/poetry.lock 
  - SBOM result:  https://github.com/CycloneDX/cyclonedx-python/blob/main/tests/_data/snapshots/poetry/all-extras_with-extras_lock20_1.6.json.bin


---

_Comment by @thomasschafer on 2025-10-20 14:34_

Thanks @jkowalleck, from my understanding they are checksums so sounds like `externalReferences` is the way forward. Based on the comment [here](https://github.com/astral-sh/uv/issues/6012#issuecomment-3422243573) I think we'll leave the hashes out of this initial PR and they can be added in future using the info you've shared!

---

_Comment by @jkowalleck on 2025-10-20 14:36_

> > Also… I wonder if it would be a good idea to omit `"dependsOn": []` for components without dependencies. `dependsOn` doesn’t seem to be a required property per [this](https://cyclonedx.org/docs/1.5/json/#dependencies_items_dependsOn).
> 
> I thought that most users would read
> 
>     * empty `dependsOn` -> we looked for dependencies and there are none
> 
>     * omitted `dependsOn` -> there might be some dependencies but we didn't look
> 
> 
> I think that technically (according to the [spec](https://cyclonedx.org/docs/1.5/json/#dependencies_items_dependsOn)) an omitted `dependsOn` actually does mean that there are no deps, but I wonder if just being explicit will make users' (and maintainers') lives easier, at the cost of a very slightly larger SBOM?

nope. no `dependsOn` is the same as an empty `dependsOn` - meaning there are no dependencies.

to indicate SBOM completeness, use `$.compositions[].dependencies[]` - see https://cyclonedx.org/docs/1.6/json/#compositions_items_dependencies. Be aware, that `$.compositions[].dependencies[]` is an addition to `$.dependencies` - it does not replace it.


---

_Referenced in [thomasschafer/uv#5](../../thomasschafer/uv/pulls/5.md) on 2025-10-20 14:39_

---

_Comment by @jkowalleck on 2025-10-20 14:44_

> > these "markers" are constraints, defining UNDER WHICH CIRCUMSTANCES a dependency is about to be used. the information decides whether a component is a dependency and shall be installed. (regardless the constraints, a dependency is a dependency, right?)
> > how do you plan to put this "marker" information into an SBOM? My answer would be: this information does not belong in an SBOM. And, I dont see a technical way to put this information in any SBOM anyway.
> 
> Given this `pylock.toml`:
> ```toml
> [project]
> name = "project"
> version = "0.1.0"
> requires-python = ">=3.12"
> dependencies = ["anyio ; sys_platform == 'darwin'", "iniconfig"]
> 
> [build-system]
> requires = ["setuptools>=42"]
> build-backend = "setuptools.build_meta"
> ```
>
> I'd suggest that the SBOM should look like this, which the WIP implementation we're working on produces:
> {see https://github.com/astral-sh/uv/issues/6012#issuecomment-3419762626}
>
> For reference, here is the `requirements.txt` that uv current exports:
> {see https://github.com/astral-sh/uv/issues/6012#issuecomment-3419762626}
> 
> It's useful to have these markers in the SBOM so that vulnerability scanning tools can filter out issues that are not relevant given a particular environment. For instance, in the example above, if there was a vulnerability in `anyio@4.3.0` (which has marker `sys_platform == 'darwin'`), then customers using darwin as a platform would be vulnerable and should be informed as such, and those using another platform would not and so should not be given a false positive vulnerability finding.
> 
> Let me know if I'm missing something!

To my understanding, this provided SBOM is utterly wrong.
the properties of a component belong to a component.
the properties you used are based on information regarding a dependency constraint - they are NOT defined by the component - and they dont belong to the component. 
Furthermore, the name `up:package:marker` are simply misleading. it is a dependency constraint, the value has nothing todo with a package, or does it?

Therefore, the properties are wrong and confusing. 
the claimed use case is impossible to solve with the SBOM you provide. as explained [earlier](https://github.com/astral-sh/uv/issues/6012#issuecomment-3419578289). 




---

_Comment by @thomasschafer on 2025-10-20 14:47_

> nope. no `dependsOn` is the same as an empty `dependsOn` - meaning there are no dependencies.
> 
> to indicate SBOM completeness, use `$.compositions[].dependencies[]` - see https://cyclonedx.org/docs/1.6/json/#compositions_items_dependencies. Be aware, that `$.compositions[].dependencies[]` is an addition to `$.dependencies` - it does not replace it.

To clarify, this is what I meant when I said "technically (according to the spec) an omitted dependsOn actually does mean that there are no deps", but again I'm just trying to make the maintainer's lives easier here, which @zanieb seems to agree with based on "I'd probably keep it, seems like it'd make life easier for a consumer." in https://github.com/astral-sh/uv/issues/6012#issuecomment-3422243573.

In the case of the SBOMs exported by uv, is there much benefit to adding `$.compositions[].dependencies[]` given that we already will have all dependencies under `$.dependencies`?

---

_Comment by @jkowalleck on 2025-10-20 14:49_

> > nope. no `dependsOn` is the same as an empty `dependsOn` - meaning there are no dependencies.
> > to indicate SBOM completeness, use `$.compositions[].dependencies[]` - see https://cyclonedx.org/docs/1.6/json/#compositions_items_dependencies. Be aware, that `$.compositions[].dependencies[]` is an addition to `$.dependencies` - it does not replace it.
> 
> To clarify, this is what I meant when I said "technically (according to the spec) an omitted dependsOn actually does mean that there are no deps", but again I'm just trying to make the maintainer's lives easier here, which [@zanieb](https://github.com/zanieb) seems to agree with based on "I'd probably keep it, seems like it'd make life easier for a consumer." in [#6012 (comment)](https://github.com/astral-sh/uv/issues/6012#issuecomment-3422243573).
> 
> In the case of the SBOMs exported by uv, is there much benefit to adding `$.compositions[].dependencies[]` given that we already will have all dependencies under `$.dependencies`?

nope. not much benefit of adding `$.compositions[].dependencies[]`, unless you omit certain dependencies/components, and you want to express this. 

---

_Comment by @thomasschafer on 2025-10-20 14:53_

> To my understanding, this provided SBOM is utterly wrong. the properties of a component belong to a component. the properties you used are based on information regarding a dependency constraint - they are NOT defined by the component - and they dont belong to the component. Furthermore, the name `up:package:marker` are simply misleading. it is a dependency constraint, the value has nothing todo with a package, or does it?

This should be `cdx:python:package:marker`, I just hadn't updated it at the time!

It is relevant to a package and not a component in this case. As explained [here](https://github.com/astral-sh/uv/issues/6012#issuecomment-3415951667), uv has a method of resolving markers on dependencies to build a marker that applies to the package as a whole, which means it becomes applicable to the component in the SBOM and not the dependency.

For instance, in the `pylock.toml` [here](https://github.com/astral-sh/uv/issues/6012#issuecomment-3415951667) we have

```toml
dependencies = [
    "trio ; python_version > '3.11'",
    "trio ; sys_platform == 'win32'",
]
```

which results in

```
trio==0.25.0 ; python_full_version >= '3.12' or sys_platform == 'win32' \
```

in the `requirements.txt`, and which we will also use in the SBOM. `python_version > '3.11'` and `sys_platform == 'win32'` both apply to the dependency, but `python_full_version >= '3.12' or sys_platform == 'win32'` applies to the component as a whole, which is why we're planning on adding the latter to the SBOM and not the former.

---

_Comment by @jkowalleck on 2025-10-20 15:02_

re https://github.com/astral-sh/uv/issues/6012#issuecomment-3422430301
> This should be `cdx:python:package:marker`, I just hadn't updated it at the time!

there is no such thing `cdx:python:package:marker` yet, and there never will be.
Reason: 
- the thing you call markers are dependency constraints per [PEP508](https://peps.python.org/pep-0508/).
- CycloneDX dependencies don't know properties nor constraints.



---

_Comment by @thomasschafer on 2025-10-20 15:06_

> there is no such thing `cdx:python:package:marker` yet, and there never will be. Reason:
> 
> * the thing you call markers are dependency constraints per [PEP508](https://peps.python.org/pep-0508/).
> * CycloneDX dependencies don't know properties nor constraints.

You appeared to say in https://github.com/astral-sh/uv/issues/6012#issuecomment-3415750026 that we should add it to the `cdx:python` namespace - we can keep it in the `uv` namespace though given the objection

---

_Comment by @jkowalleck on 2025-10-20 15:12_

> > there is no such thing `cdx:python:package:marker` yet, and there never will be. Reason:
> > 
> > * the thing you call markers are dependency constraints per [PEP508](https://peps.python.org/pep-0508/).
> > * CycloneDX dependencies don't know properties nor constraints.
> 
> You appeared to say in [#6012 (comment)](https://github.com/astral-sh/uv/issues/6012#issuecomment-3415750026) that we should add it to the `cdx:python` namespace - we can keep it in the `uv` namespace though given the objection

I sayed: 
> For the general python markers, sure, write a ticket/pullrequest to add something to the cdx:python:* namespace.

In case this was a general python packaging feature, sure, pull request it for the taxonomy, and we might discuss the use and definition.

But you are not using a general python packaging feature, are you?
this resolution is unique to `uv`. 

---

_Comment by @thomasschafer on 2025-10-20 15:15_

> But you are not using a general python packaging feature, are you? this resolution is unique to `uv`.

I think the resolution is implemented by uv but I don’t see why another tool couldn’t implement the same thing. Given the uncertainty though let’s stick with the `uv` namespace

---

_Comment by @zanieb on 2025-10-20 16:00_

> the thing you call markers are dependency constraints per [PEP508](https://peps.python.org/pep-0508/).

Huh? Please see the following section of the specification https://peps.python.org/pep-0508/#environment-markers
PEP 508 does not refer to anything as a "dependency constraint", so I'm not sure where you're getting that from — can you quote the section you are referencing? The marker is a component of a dependency specification. Version constraints, which is the closest term I can think of, are a separate component.

> It is relevant to a package and not a component in this case. As explained https://github.com/astral-sh/uv/issues/6012#issuecomment-3415951667, uv has a method of resolving markers on dependencies to build a marker that applies to the package as a whole, which means it becomes applicable to the component in the SBOM and not the dependency.

Just to confirm, this understanding of uv's resolver is correct.

> But you are not using a general python packaging feature, are you? this resolution is unique to uv.

I'm not sure what this means. Resolution is always going to be unique to the resolver and each resolution can produce entirely different sets of packages so the entire SBOM is predicated on the tool-specific resolution.

---

_Comment by @chrisrodrigue on 2025-10-20 18:25_

The `components.hashes` array of the SBOM seems to be the place to document the hashes of the components of the project. Each item in that array has properties `alg` (e.g. `”SHA-256”`) and `content` (the actual hash value). `externalReferences` seems to be a catch all for various hyperlinks pertaining to the project.

---

_Comment by @jkowalleck on 2025-10-21 07:35_

> The `components.hashes` array of the SBOM seems to be the place to document the hashes of the components of the project. Each item in that array has properties `alg` (e.g. `”SHA-256”`) and `content` (the actual hash value). `externalReferences` seems to be a catch all for various hyperlinks pertaining to the project.

`components.hashes` is for the component. for example `npm` and `yarn` have internal integrity hash calculations - which could go here.

the hashes of a `requirements.txt` and other hashes that refer to files downloaded from PyPI and other sources refer to the hash of a downloadable file.
they go to `externalReferences`.
see https://github.com/astral-sh/uv/issues/6012#issuecomment-3422335660

---

_Referenced in [CycloneDX/cyclonedx-python#907](../../CycloneDX/cyclonedx-python/issues/907.md) on 2025-10-24 09:50_

---

_Comment by @thomasschafer on 2025-10-27 16:01_

@jkowalleck did you have any more thoughts on having the resolved marker under the `cdx:python` namespace, based on @zanieb's thoughts here https://github.com/astral-sh/uv/issues/6012#issuecomment-3422732821?

If you are open to it, I have a PR [here](https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/142) to add it, but I appreciate that there was some disagreement here so happy to continue the discussion.

---

_Comment by @jkowalleck on 2025-10-27 17:23_

> [@jkowalleck](https://github.com/jkowalleck) did you have any more thoughts on having the resolved marker under the `cdx:python` namespace, based on [@zanieb](https://github.com/zanieb)'s thoughts here [#6012 (comment)](https://github.com/astral-sh/uv/issues/6012#issuecomment-3422732821)?
> 
> If you are open to it, I have a PR [here](https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/142) to add it, but I appreciate that there was some disagreement here so happy to continue the discussion.

The python packaging "marker" is for dependencies, still. Using it for components instead makes no sense, or does it? 

Everybody, please be invited to discuss it here https://github.com/CycloneDX/cyclonedx-property-taxonomy/pull/142

---

_Referenced in [astral-sh/uv#16523](../../astral-sh/uv/pulls/16523.md) on 2025-10-30 19:26_

---

_Closed by @woodruffw on 2025-11-20 17:52_

---

_Comment by @bunny-therapist on 2025-11-20 18:48_

Great! Any plans to make it possible for the sbom to be included in built packages, in the PEP-specified .dist-info/sboms directory?

I suppose we can already just create an sbom and list it in the pyproject metadata, but something more encapsulated, so that sbom is built at build time? Like make it dynamic metadata created by the build backend, for example?

---
