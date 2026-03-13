# Distributed Tracing Standalone Documentation

## Overview

Create standalone documentation for the Red Hat OpenShift distributed tracing platform, following the same pattern established by the OTel (OpenTelemetry) standalone docs.

## Rules and Workflow

### OTel is the reference — check structure AND content

The `standalone-otel-docs-3.8` branch on GitHub (`openshift/openshift-docs`) is the reference implementation. When migrating content, always compare **both**:
1. **Structure**: directory layout, symlinks, topic map, distro map
2. **File contents**: diff OTel standalone modules/snippets against main's versions to catch within-file changes (e.g., header comment path updates, vale fixes, attribute usage)

Never assume a verbatim copy from main is correct. Always diff a few OTel modules against main to identify what changed during their migration.

### Module/snippet header comments

Every module and snippet has a `// Module included in the following assemblies:` (or `// Text snippet included in the following`) comment block listing assembly paths. When copying from main, these paths must be updated from main's paths (e.g., `observability/distr_tracing/distr-tracing-rn.adoc`) to standalone paths (e.g., `release-notes/distr-tracing-rn.adoc`). Also remove references to assemblies that don't exist in the standalone branch (e.g., `service_mesh/v2x/...`, `serverless/...`).

### Git workflow

- **Always fetch and rebase before copying content** from upstream: `git fetch upstream main && git fetch upstream standalone-distributed-tracing-docs-main`
- **Always fetch and rebase before pushing**: never force push without first fetching
- **Use `git commit --amend`** for fixes to the same logical change, not new commits
- **Branch naming**: branch names must match Jira ticket numbers (e.g., `OBSDOCS-3130`)
- **PR dependencies**: don't create dependent PRs until the base PR is merged. Rebase onto updated upstream after merge, then commit and push
- **Files must end with a trailing newline** (POSIX text file convention)
- **PR descriptions**: keep minimal — summary bullets only, no test plan checklists. SMEs find test checklists confusing (unclear if they're internal notes or reviewer requests)

### Verification checklist (before committing)

1. All `include::` paths resolve to existing files
2. All symlinks point to valid targets
3. Module/snippet header comments reflect standalone paths (not main paths)
4. All xrefs converted to external `link:https://docs.redhat.com/...` URLs
5. All `ifdef` conditionals reviewed — remove or update for standalone distro key
6. Topic map entries match actual files on disk
7. Vale attribute usage: use `{TempoShortName}`, `{DTShortName}`, etc. instead of plain text "Tempo", "distributed tracing"
8. Compare a sample of files against OTel standalone to confirm the migration pattern is followed correctly

### Capitalization

OPL (Official Product List) confirms lowercase "distributed tracing" is the official product name. The alias "Distributed Tracing" (capitalized) is explicitly marked as NOT approved across all columns (general use, tech docs, code/CLI). Use lowercase everywhere.

## Reference: OTel Standalone Pattern

The `standalone-otel-docs-3.8` branch strips the full repo (~14,271 files) down to ~365 files containing only OTel-relevant content.

### Key structural elements

- **`_distro_map.yml`** defines a single distro (`openshift-otel`) with product name "Red Hat build of OpenTelemetry"
- **`_topic_maps/_topic_map.yml`** contains only OTel content chapters
- **Content directories at repo root** (not nested under `observability/`), each containing:
  - The `.adoc` assembly file
  - A `docinfo.xml`
  - Symlinks to shared `_attributes`, `modules`, `images`, and `snippets` directories
- **`modules/`** at root contains only `otel-*` module files
- **`snippets/`** at root contains only relevant snippets
- **`_attributes/common-attributes.adoc`** is kept as the full file from main (not trimmed)
- **Build infrastructure** is retained (build.py, scripts/, templates, stylesheets, etc.)
- **xrefs to OCP content** are converted to external `link:https://docs.redhat.com/...` URLs
- **Module/snippet headers** are updated to reflect standalone assembly paths, not main paths

## Existing Standalone Branches on GitHub

| Product | Branch pattern |
|---|---|
| OTel | `standalone-otel-docs-{main,3.8}` |
| Logging | `standalone-logging-docs-{main,5.8,6.0-6.5}` |
| COO | `standalone-coo-docs-{main,1-latest}` |
| **Distributed Tracing** | `standalone-distributed-tracing-docs-{main,3.8}` (to be created) |

## Distributed Tracing Content in Main Branch

### Assemblies (7 files)

Located in `observability/distr_tracing/` on main:

| File | Title |
|---|---|
| `distr-tracing-tempo-architecture.adoc` | About the {DTShortName} |
| `distr-tracing-rn.adoc` | Release notes for the {TempoName} |
| `distr-tracing-tempo-installing.adoc` | Installing the {DTShortName} |
| `distr-tracing-tempo-configuring.adoc` | Configuring the {DTShortName} |
| `distr-tracing-tempo-troubleshooting.adoc` | Troubleshooting the {DTShortName} |
| `distr-tracing-tempo-updating.adoc` | Upgrading |
| `distr-tracing-tempo-removing.adoc` | Removing the {DTShortName} |

### Modules (35 files)

All prefixed with `distr-tracing-tempo-*` in `modules/`:

- `distr-tracing-tempo-about-rn.adoc`
- `distr-tracing-tempo-architecture.adoc`
- `distr-tracing-tempo-config-default.adoc`
- `distr-tracing-tempo-config-query-frontend.adoc`
- `distr-tracing-tempo-config-query-rbac.adoc`
- `distr-tracing-tempo-config-receiver-tls-for-tempomonolithic.adoc`
- `distr-tracing-tempo-config-receiver-tls-for-tempostack.adoc`
- `distr-tracing-tempo-config-spanmetrics.adoc`
- `distr-tracing-tempo-configuring-tempooperator-metrics-and-alerts.adoc`
- `distr-tracing-tempo-configuring-tempostack-metrics-and-alerts.adoc`
- `distr-tracing-tempo-coo-ui-plugin.adoc`
- `distr-tracing-tempo-features.adoc`
- `distr-tracing-tempo-install-cli.adoc`
- `distr-tracing-tempo-install-gateway-read-permissions.adoc`
- `distr-tracing-tempo-install-gateway-write-permissions.adoc`
- `distr-tracing-tempo-install-tempomonolithic-cli.adoc`
- `distr-tracing-tempo-install-tempomonolithic-web-console.adoc`
- `distr-tracing-tempo-install-tempostack-cli.adoc`
- `distr-tracing-tempo-install-tempostack-web-console.adoc`
- `distr-tracing-tempo-install-web-console.adoc`
- `distr-tracing-tempo-key-concepts-in-distributed-tracing.adoc`
- `distr-tracing-tempo-object-storage-setup-aws-sts-cco-install.adoc`
- `distr-tracing-tempo-object-storage-setup-azure-sts-install.adoc`
- `distr-tracing-tempo-object-storage-setup-gcp-sts-install.adoc`
- `distr-tracing-tempo-object-storage-setup-ibm-storage.adoc`
- `distr-tracing-tempo-remove-cli.adoc`
- `distr-tracing-tempo-remove-web-console.adoc`
- `distr-tracing-tempo-rn-bug-fixes.adoc`
- `distr-tracing-tempo-rn-deprecated-features.adoc`
- `distr-tracing-tempo-rn-enhancements.adoc`
- `distr-tracing-tempo-rn-known-issues.adoc`
- `distr-tracing-tempo-rn-removed-features.adoc`
- `distr-tracing-tempo-rn-technology-preview-features.adoc`
- `distr-tracing-tempo-storage-ref.adoc`
- `distr-tracing-tempo-troubleshoot-collecting-diagnostic-data-from-command-line.adoc`

Also uses `modules/support.adoc`.

### Snippets needed

- `distr-tracing-and-otel-disclaimer-about-docs-for-supported-features-only.adoc`
- `distr-tracing-assembly-tip-for-jaeger-replacements.adoc`
- `technology-preview.adoc`

### Key attributes (from `_attributes/common-attributes.adoc`)

```
:DTProductName: Red Hat OpenShift distributed tracing platform
:DTShortName: distributed tracing platform
:DTProductVersion: 3.1
:TempoName: Red Hat OpenShift distributed tracing platform (Tempo)
:TempoShortName: distributed tracing platform (Tempo)
:TempoOperator: Tempo Operator
:TempoVersion: 2.3.1
```

## Standalone Branch Structure

### `_distro_map.yml`

```yaml
---
openshift-distributed-tracing:
  name: Red Hat OpenShift distributed tracing platform
  author: OpenShift documentation team <openshift-docs@redhat.com>
  site: commercial
  site_name: Documentation
  site_url: https://docs.openshift.com/
  branches:
    standalone-distributed-tracing-docs-main:
      name: ''
      dir: distributed-tracing
```

### `_topic_maps/_topic_map.yml`

```yaml
---
Name: About the distributed tracing platform
Dir: about
Distros: openshift-distributed-tracing
Topics:
- Name: About the distributed tracing platform
  File: distr-tracing-tempo-architecture
---
Name: Release notes for the distributed tracing platform
Dir: release-notes
Distros: openshift-distributed-tracing
Topics:
- Name: Release notes for the distributed tracing platform
  File: distr-tracing-rn
---
Name: Installing the distributed tracing platform
Dir: installing
Distros: openshift-distributed-tracing
Topics:
- Name: Installing the distributed tracing platform
  File: distr-tracing-tempo-installing
---
Name: Configuring the distributed tracing platform
Dir: configuring
Distros: openshift-distributed-tracing
Topics:
- Name: Configuring the distributed tracing platform
  File: distr-tracing-tempo-configuring
---
Name: Troubleshooting the distributed tracing platform
Dir: troubleshooting
Distros: openshift-distributed-tracing
Topics:
- Name: Troubleshooting the distributed tracing platform
  File: distr-tracing-tempo-troubleshooting
---
Name: Upgrading the distributed tracing platform
Dir: updating
Distros: openshift-distributed-tracing
Topics:
- Name: Upgrading the distributed tracing platform
  File: distr-tracing-tempo-updating
---
Name: Removing the distributed tracing platform
Dir: removing
Distros: openshift-distributed-tracing
Topics:
- Name: Removing the distributed tracing platform
  File: distr-tracing-tempo-removing
```

### Content directory structure

```
standalone-distributed-tracing-docs-main/
├── _attributes/
│   └── common-attributes.adoc          # Full file from main
├── _distro_map.yml
├── _topic_maps/
│   └── _topic_map.yml
├── modules/
│   ├── _attributes -> ../_attributes   # symlink
│   ├── images -> ../images             # symlink
│   ├── distr-tracing-tempo-*.adoc      # 35 module files
│   └── support.adoc
├── snippets/
│   ├── _attributes -> ../_attributes   # symlink
│   ├── images -> ../images             # symlink
│   ├── modules -> ../modules           # symlink
│   ├── distr-tracing-and-otel-disclaimer-about-docs-for-supported-features-only.adoc
│   ├── distr-tracing-assembly-tip-for-jaeger-replacements.adoc
│   └── technology-preview.adoc
├── images/
│   └── kebab.png
├── about/
│   ├── _attributes -> ../_attributes   # symlink
│   ├── modules -> ../modules           # symlink
│   ├── images -> ../images             # symlink
│   ├── snippets -> ../snippets         # symlink
│   ├── docinfo.xml
│   └── distr-tracing-tempo-architecture.adoc
├── release-notes/
│   ├── _attributes -> ../_attributes
│   ├── modules -> ../modules
│   ├── images -> ../images
│   ├── snippets -> ../snippets
│   ├── docinfo.xml
│   └── distr-tracing-rn.adoc
├── installing/
│   ├── _attributes -> ../_attributes
│   ├── modules -> ../modules
│   ├── images -> ../images
│   ├── snippets -> ../snippets
│   ├── docinfo.xml
│   └── distr-tracing-tempo-installing.adoc
├── configuring/
│   ├── _attributes -> ../_attributes
│   ├── modules -> ../modules
│   ├── images -> ../images
│   ├── snippets -> ../snippets
│   ├── docinfo.xml
│   └── distr-tracing-tempo-configuring.adoc
├── troubleshooting/
│   ├── _attributes -> ../_attributes
│   ├── modules -> ../modules
│   ├── images -> ../images
│   ├── snippets -> ../snippets
│   ├── docinfo.xml
│   └── distr-tracing-tempo-troubleshooting.adoc
├── updating/
│   ├── _attributes -> ../_attributes
│   ├── modules -> ../modules
│   ├── images -> ../images
│   ├── snippets -> ../snippets
│   ├── docinfo.xml
│   └── distr-tracing-tempo-updating.adoc
├── removing/
│   ├── _attributes -> ../_attributes
│   ├── modules -> ../modules
│   ├── images -> ../images
│   ├── snippets -> ../snippets
│   ├── docinfo.xml
│   └── distr-tracing-tempo-removing.adoc
└── [build infrastructure files retained from main]
```

## xref Conversion Examples

Assemblies reference OCP content that won't exist in the standalone branch. These must be converted during migration (OBSDOCS-3130):

```asciidoc
# Before (main branch)
xref:../../storage/understanding-persistent-storage.adoc#understanding-persistent-storage[Understanding persistent storage]

# After (standalone branch)
link:https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/storage/index#understanding-persistent-storage[Understanding persistent storage]
```

```asciidoc
# Before
xref:../../operators/admin/olm-adding-operators-to-cluster.adoc#olm-installing-from-software-catalog-using-web-console_olm-adding-operators-to-a-cluster[Installing from the software catalog]

# After
link:https://docs.redhat.com/en/documentation/openshift_container_platform/latest/html-single/operators/index#olm-installing-from-software-catalog-using-web-console_olm-adding-operators-to-a-cluster[Installing from the software catalog]
```

```asciidoc
# Before
xref:../../observability/otel/otel-installing.adoc#install-otel[{OTELName}]

# After (link to OTel standalone docs or docs.redhat.com)
link:https://docs.redhat.com/en/documentation/red_hat_build_of_opentelemetry/latest/html/red_hat_build_of_opentelemetry/install-otel[{OTELName}]
```

## Completed Tasks

### OBSDOCS-3129 — Initial branch and prow build (DONE)

**What was done:**
1. Created `standalone-distributed-tracing-docs-main` branch on `openshift/openshift-docs` (based on `standalone-docs-main`)
2. Modified `_distro_map.yml`: changed distro key from `openshift-standalone` to `openshift-distributed-tracing`
3. Modified `_topic_maps/_topic_map.yml`: updated to point to DT about page
4. Removed placeholder `about/about-standalone.adoc`
5. Added `about/distr-tracing-tempo-architecture.adoc` (assembly)
6. Added `modules/distr-tracing-tempo-product-overview.adoc` (module)
7. Configured prow CI in `openshift/release` repo

**PRs:**
- openshift/openshift-docs PR #107043 — content changes (merged)
- openshift/release PR #75143 — prow config (merged)

**Branch:** `OBSDOCS-3129` on fork `gtrivedi88/openshift-docs`

### OBSDOCS-3130 — Migrate main content (DONE)

**What was done:**
1. Copied 36 DT modules and `support.adoc` from `main` to `modules/`
2. Copied 7 snippets from `main` to `snippets/`
3. Created 6 content directories (`release-notes`, `installing`, `configuring`, `troubleshooting`, `updating`, `removing`) with symlinks to `_attributes`, `modules`, `images`, `snippets`
4. Wrote 7 assembly files with xrefs converted to external `link:https://docs.redhat.com/...` URLs
5. Removed `ifdef::openshift-enterprise,openshift-dedicated[]` conditionals (content renders unconditionally in standalone)
6. Expanded `_topic_map.yml` from 1 chapter to 7 chapters
7. Copied `images/kebab.png` from `main`

**PRs:**
- openshift/openshift-docs PR #107636 — content migration (merged)

**Branch:** `OBSDOCS-3130` on fork `gtrivedi88/openshift-docs`

**SME review (max-cx) on PR #107636:**
- Vale fixes (8 `SuggestAttribute` errors): resolved by max-cx
- Module/snippet header path updates (~41 files): fixed — updated all `// *` paths to standalone paths
- CQA items (missing abstract tags, modularization): max-cx will handle separately
- Question about `distr-tracing-tempo-product-overview.adoc`: already exists from OBSDOCS-3129 in base branch

**Lesson learned:** We missed the header path updates because we verified modules matched main byte-for-byte and treated that as correct. We should have diffed OTel standalone modules against main's modules to see what within-file changes the OTel migration made (like updating header comments).

### OBSDOCS-3132 — Add docinfo.xml to 7 DT titles (DONE)

**What was done:**
1. Created `docinfo.xml` for all 7 content directories following OTel standalone pattern
2. Each file contains: `<title>`, `<productname>`, `<productnumber>`, `<subtitle>`, `<abstract>`, `<authorgroup>`, `<xi:include>` for legal notice

**Files added:**
- `about/docinfo.xml` — "About the distributed tracing platform"
- `release-notes/docinfo.xml` — "Release notes"
- `installing/docinfo.xml` — "Installing"
- `configuring/docinfo.xml` — "Configuring"
- `troubleshooting/docinfo.xml` — "Troubleshooting"
- `updating/docinfo.xml` — "Upgrading"
- `removing/docinfo.xml` — "Removing"

**PRs:**
- openshift/openshift-docs PR #107774 — docinfo.xml files (merged)

**Branch:** `OBSDOCS-3132` on fork `gtrivedi88/openshift-docs`

### OBSDOCS-3143 — Branch for current version and prow build (DONE)

**What was done:**
1. SME (mleonov) created `standalone-distributed-tracing-docs-3.9` branch from `standalone-distributed-tracing-docs-main` on upstream
2. Created prow CI config for the 3.9 branch (ci-operator config + presubmit jobs)
3. Both files verified against OTel 3.9 prow config pattern — exact match with DT-specific substitutions

**Version note:** Jira originally said `3.8` (copy-paste from OTel). SME confirmed correct version is **3.9** (from release notes module). `{DTProductVersion}` attribute is deprecated (stuck at 3.1).

**PRs:**
- openshift/release PR #75599 — prow CI config (merged)

**Branch:** `OBSDOCS-3143` on fork `gtrivedi88/release`

## Jira Epic: Tasks

| Jira | Task | Target Branch | Type | Status |
|---|---|---|---|---|
| OBSDOCS-3129 | Initial branch and prow build | Creates `standalone-distributed-tracing-docs-main` | Git/Prow | DONE |
| OBSDOCS-3130 | Migrate main content | `standalone-distributed-tracing-docs-main` | Git | DONE |
| OBSDOCS-3132 | Add docinfo.xml to 7 DT titles | `standalone-distributed-tracing-docs-main` | Git | DONE |
| OBSDOCS-3142 | Remove unused attributes as per migration requirements | `standalone-distributed-tracing-docs-main` | Git | CLOSED (CQA) |
| OBSDOCS-3131 | Sync initial branch to GitLab | N/A | GitLab (`layered-products-docs`) | DONE |
| OBSDOCS-3133 | Create branch builds on Pantheon | N/A | Pantheon | DONE |
| OBSDOCS-3134 | Create splash page with categories on Pantheon | N/A | Pantheon | DONE |
| OBSDOCS-3135 | Add release notes on main | `standalone-distributed-tracing-docs-main` | Git | DONE (covered by 3130) |
| OBSDOCS-3136 | Review standalone content against existing published content | `standalone-distributed-tracing-docs-main` | Review/Git | DONE (PR #108068) |
| OBSDOCS-3137 | Remove content in OCP and point at standalone docs | `main` | Git | |
| OBSDOCS-3138 | Get new content added to search index, get old content removed | N/A | External | |
| OBSDOCS-3139 | Get redirects added so that old links will still work | N/A | External | |
| OBSDOCS-3140 | Add Distributed Tracing to list of products on docs.redhat | N/A | External | |
| OBSDOCS-3141 | Update observability page to point at standalone docs | `main` | Git | |
| OBSDOCS-3143 | Branch for current version and prow build | Creates `standalone-distributed-tracing-docs-3.9` | Git/Prow | DONE |
| OBSDOCS-3144 | Research | N/A | Research | CLOSED |
| OBSDOCS-3145 | Final Review | N/A | Review | |

### Notes on tasks

- **OBSDOCS-3132**: Title says "14 OTEL titles" but should be "7 DT titles" (there are 7 assemblies, not 14, and this is DT not OTel)
- **OBSDOCS-3140**: Title says "Add OTEL" but should be "Add Distributed Tracing"
- **OBSDOCS-3142**: Closed — this is CQA work handled by the SME, not by us. OTel equivalent (OBSDOCS-2992) was also closed for the same reason: "This is part of CQA so not relevant here"
- **OBSDOCS-3143**: Jira originally said `3.8` (copy-paste from OTel). SME confirmed the correct version is **3.9** (from `modules/distr-tracing-tempo-about-rn.adoc`). Branch name: `standalone-distributed-tracing-docs-3.9`
- **xref conversion, ifdef updates, and attribute handling** are all part of OBSDOCS-3130 (Migrate main content)

### Version note

- **`{DTProductVersion}` is deprecated** — SME says it's set to `3.1` but is outdated and causes errors. Do not use it. The actual current product version is **3.9**, sourced from the release notes module (`modules/distr-tracing-tempo-about-rn.adoc`: `{DTShortName} 3.9 is provided through...`)

### OTel reference Jira mapping

| OTel Jira | OTel Task | Description | DT Equivalent |
|---|---|---|---|
| OBSDOCS-2910 | Sync initial branch to GitLab | "Update sync.yaml to copy from github to gitlab" | OBSDOCS-3131 |
| OBSDOCS-2912 | Create branch builds on Pantheon | "Configure branch builds on Pantheon" | OBSDOCS-3133 |
| OBSDOCS-2914 | Add release notes on main | "Create rel notes on main, CP to 3.8" (PRs #103431, #103434) | OBSDOCS-3135 |
| OBSDOCS-2935 | Review standalone content against existing published content | "Compare the generated standalone HTML with the existing published HTML on docs.redhat to see what is broken/missing" — Gabriel used Claude with article-extractor to semantically compare each chapter. Key findings: content 99-100% identical, differences were product name capitalization, metadata, `{red-hat-lightspeed}` variable not resolving | OBSDOCS-3136 |
| OBSDOCS-2987 | Cut OTEL content from main and point to standalone instead | "Remove the existing content from the OCP, and add a pointer to the standalone docs, to make sure user traffic transfers across" — PRs to main + cherry-picks to enterprise-4.12 through 4.21 | OBSDOCS-3137 |
| OBSDOCS-2988 | Get new content added to search index, get old content removed | Self-descriptive | OBSDOCS-3138 |
| OBSDOCS-2989 | Get redirects added so that old links will still work | Work tracked in CCS-7285 (chapter redirects) and CCS-7286 (title redirect) | OBSDOCS-3139 |
| OBSDOCS-2992 | Remove unused attributes as per migration requirements | Closed — "This is part of CQA so not relevant here" | OBSDOCS-3142 (closed — CQA) |
| OBSDOCS-3159 | Manual cherry-picks for OBSDOCS-2987 | Cherry-picks of the cut-content PR to enterprise branches | N/A (will need similar for DT) |

### OTel task execution order (observed)

1. Migrate content → 2. Add docinfo.xml → 3. Sync to GitLab (sync.yaml) → 4. Pantheon branch builds → 5. Splash page → 6. Release notes (parallel) → 7. Review content vs published → 8. Cut from main → 9. Search index → 10. Redirects

## GitLab Workflow

The content flows from GitHub to GitLab:

1. **GitHub** (`openshift/openshift-docs`) — standalone branch is created and maintained here as the source of truth
2. **GitLab** (`red-hat-enterprise-openshift-documentation/layered-products-docs`) — content is synced from GitHub for the CCS portal build pipeline (OBSDOCS-3131)

## ifdef Considerations

The DT assemblies use `ifdef::openshift-enterprise,openshift-dedicated[]` blocks. The new standalone distro key is `openshift-distributed-tracing`, which won't match those conditionals. During migration (OBSDOCS-3130), either:

- Add `openshift-distributed-tracing` to the ifdef lists where the content should render
- Or remove the ifdefs entirely if the content should always render in the standalone context

## OBSDOCS-3137 Research — Cut DT from Main

### OTel Reference: PR #107422 (OBSDOCS-2987)

OTel cut: 120 files changed, 8 insertions, 7,141 deletions. Merged 2026-02-25. Cherry-picked to enterprise-4.12 through 4.21 (PRs #107424-#107434) by max-cx on 2026-02-27.

Pattern: Delete all OTel modules/assemblies, keep 1 pointer page with link to standalone docs, convert xrefs from other pages to external links.

### DT Plan — Verified File Counts

**DELETE (45 items):**
- 34 modules (`modules/distr-tracing-tempo-*.adoc` that are DT-only)
- 5 snippets (`snippets/distr-tracing-*.adoc` that are DT-only)
- 6 assemblies under `observability/distr_tracing/` (all except architecture which becomes pointer)

**KEEP (4 DT-owned files shared with Service Mesh/Serverless):**
- `modules/distr-tracing-tempo-architecture.adoc` — included by `service_mesh/v2x/ossm-architecture.adoc`
- `modules/distr-tracing-tempo-features.adoc` — included by `service_mesh/v2x/ossm-architecture.adoc`
- `modules/distr-tracing-tempo-key-concepts-in-distributed-tracing.adoc` — included by `serverless/`, `service_mesh/v1x/`, `service_mesh/v2x/`
- `snippets/distr-tracing-assembly-tip-for-jaeger-replacements.adoc` — included by `service_mesh/v1x/`, `v2x/`, `modules/ossm-install-jaeger-operator.adoc`

**Also untouched (Service Mesh owned, `ossm-*` prefix):**
- `modules/ossm-configuring-distr-tracing-tempo.adoc`
- `modules/ossm-distr-tracing.adoc`
- `modules/ossm-distr-tracing-config-sampling.adoc`

**CONVERT to pointer page (1 file):**
- `observability/distr_tracing/distr-tracing-tempo-architecture.adoc` → single line linking to `https://docs.redhat.com/en/documentation/red_hat_openshift_distributed_tracing_platform/latest`

**MODIFY (4 files):**
- `_topic_maps/_topic_map.yml` — strip DT section from 7 entries to 1 (pointer page only)
- `observability/overview/index.adoc` — replace DT section xref (line 57) with external link
- `welcome/learn_more_about_openshift.adoc` — replace 2 xrefs (lines 212-213) with external links
- `service_mesh/v2x/ossm-observability.adoc` — replace 1 xref (line 44) with external link

**Cherry-picks:** enterprise-4.12 through 4.22

### 34 Modules to DELETE

```
distr-tracing-tempo-about-rn.adoc
distr-tracing-tempo-config-default.adoc
distr-tracing-tempo-config-operator.adoc
distr-tracing-tempo-config-query-frontend.adoc
distr-tracing-tempo-config-query-rbac.adoc
distr-tracing-tempo-config-receiver-tls-for-tempomonolithic.adoc
distr-tracing-tempo-config-receiver-tls-for-tempostack.adoc
distr-tracing-tempo-config-spanmetrics.adoc
distr-tracing-tempo-configuring-tempooperator-metrics-and-alerts.adoc
distr-tracing-tempo-configuring-tempostack-metrics-and-alerts.adoc
distr-tracing-tempo-coo-ui-plugin.adoc
distr-tracing-tempo-install-cli.adoc
distr-tracing-tempo-install-gateway-read-permissions.adoc
distr-tracing-tempo-install-gateway-write-permissions.adoc
distr-tracing-tempo-install-tempomonolithic-cli.adoc
distr-tracing-tempo-install-tempomonolithic-web-console.adoc
distr-tracing-tempo-install-tempostack-cli.adoc
distr-tracing-tempo-install-tempostack-web-console.adoc
distr-tracing-tempo-install-web-console.adoc
distr-tracing-tempo-network-policies.adoc
distr-tracing-tempo-object-storage-setup-aws-sts-cco-install.adoc
distr-tracing-tempo-object-storage-setup-azure-sts-install.adoc
distr-tracing-tempo-object-storage-setup-gcp-sts-install.adoc
distr-tracing-tempo-object-storage-setup-ibm-storage.adoc
distr-tracing-tempo-remove-cli.adoc
distr-tracing-tempo-remove-web-console.adoc
distr-tracing-tempo-rn-bug-fixes.adoc
distr-tracing-tempo-rn-deprecated-features.adoc
distr-tracing-tempo-rn-enhancements.adoc
distr-tracing-tempo-rn-known-issues.adoc
distr-tracing-tempo-rn-removed-features.adoc
distr-tracing-tempo-rn-technology-preview-features.adoc
distr-tracing-tempo-storage-ref.adoc
distr-tracing-tempo-troubleshoot-collecting-diagnostic-data-from-command-line.adoc
```

### 5 Snippets to DELETE

```
distr-tracing-and-otel-disclaimer-about-docs-for-supported-features-only.adoc
distr-tracing-tempo-required-secret-parameters.adoc
distr-tracing-tempo-secret-example.adoc
distr-tracing-tempo-tempomonolithic-custom-resource.adoc
distr-tracing-tempo-tempostack-custom-resource.adoc
```

### 6 Assemblies to DELETE

```
observability/distr_tracing/distr-tracing-rn.adoc
observability/distr_tracing/distr-tracing-tempo-configuring.adoc
observability/distr_tracing/distr-tracing-tempo-installing.adoc
observability/distr_tracing/distr-tracing-tempo-removing.adoc
observability/distr_tracing/distr-tracing-tempo-troubleshooting.adoc
observability/distr_tracing/distr-tracing-tempo-updating.adoc
```
