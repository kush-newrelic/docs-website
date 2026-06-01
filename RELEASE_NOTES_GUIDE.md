# How to Create Release Notes for docs-website

This guide explains how to add release notes to the [New Relic docs-website](https://github.com/newrelic/docs-website) repository.

---

## Directory Structure

All release notes live under:

```
src/content/docs/release-notes/
```

Each product has its own subdirectory:

```
release-notes/
├── agent-release-notes/
│   ├── java-release-notes/
│   ├── go-release-notes/
│   ├── python-release-notes/
│   └── ...
├── pipeline-control-gateway-release-notes/
├── infrastructure-release-notes/
├── new-relic-browser-release-notes/
└── ...
```

---

## File Naming Convention

```
<product-name>-<YY-MM-DD>.mdx
```

Examples:
- `pipeline-control-gateway-25-08-05.mdx` (released 2025-08-05)
- `java-agent-930.mdx` (Java agent version 9.3.0)

> **Note:** Some products use version-based naming (e.g., `java-agent-930.mdx`), others use date-based naming (e.g., `pipeline-control-gateway-25-08-05.mdx`). Match the convention already used in the target directory.

---

## File Format

Release notes are written in **MDX** (Markdown + JSX). Every file has two parts:

1. **YAML Frontmatter** (metadata)
2. **Body content** (the actual release notes in Markdown)

---

## Frontmatter (Required Fields)

```yaml
---
subject: Pipeline Control Gateway
releaseDate: '2025-08-05'
version: 1.1.0
metaDescription: Release notes for Pipeline Control gateway 1.1.0
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `subject` | Yes | Product name (e.g., `Java agent`, `Pipeline Control Gateway`) |
| `releaseDate` | Yes | Release date in `YYYY-MM-DD` format, wrapped in quotes |
| `version` | Yes | Semantic version (e.g., `1.1.0`, `9.3.0`) |
| `metaDescription` | Yes | Short description for SEO/meta tags |

### Optional Frontmatter Fields

Some products (like Java agent) use additional fields:

```yaml
downloadLink: 'https://download.newrelic.com/...'
features: ["Feature 1", "Feature 2"]
bugs: ["Bug fix 1", "Bug fix 2"]
security: []
```

---

## Body Content Structure

Use Markdown headings and lists to organize changes. Common sections:

```markdown
## New features and improvements
- Description of feature 1
- Description of feature 2

## Fixes
- Description of bug fix 1

## Security updates
- Description of security patch
```

### Formatting Guidelines

- Use `##` or `###` for section headings
- Use bullet points (`-` or `*`) for individual items
- Use backticks for code references (e.g., `` `SqlTrace` ``, `` `v0.38.0` ``)
- Link to related PRs where applicable: `[2830](https://github.com/org/repo/pull/2830)`
- Link to related docs: `[Pipeline Control docs](/docs/new-relic-control/pipeline-control/overview/)`

### Optional: Download Button (for agents)

```mdx
<ButtonGroup>
  <ButtonLink
    role="button"
    to="https://download.newrelic.com/..."
    variant="primary"
  >
    Download this agent version
  </ButtonLink>
</ButtonGroup>
```

---

## Step-by-Step: Creating a New Release Note

### 1. Identify the correct directory

Find your product's release notes folder under `src/content/docs/release-notes/`.

### 2. Create the MDX file

Create a new `.mdx` file following the naming convention in that directory.

### 3. Add frontmatter

```yaml
---
subject: Your Product Name
releaseDate: 'YYYY-MM-DD'
version: X.Y.Z
metaDescription: Release notes for Your Product Name X.Y.Z
---
```

### 4. Write the release note body

Organize by category (features, fixes, security). Keep descriptions clear and concise.

### 5. Ensure an `index.mdx` exists

Each release notes subdirectory needs an `index.mdx` with minimal frontmatter:

```yaml
---
subject: Your Product Name
---
```

If you're adding release notes for a brand-new product and no subdirectory exists yet, create both the directory and this index file.

### 6. Submit a PR

- Branch from `develop`
- Use [conventional commit](https://www.conventionalcommits.org/en/v1.0.0/) messages
- PR title example: `Release notes for Pipeline Control Gateway v1.1.0`

---

## Complete Example

**File:** `src/content/docs/release-notes/pipeline-control-gateway-release-notes/pipeline-control-gateway-25-08-05.mdx`

```mdx
---
subject: Pipeline Control Gateway
releaseDate: '2025-08-05'
version: 1.1.0
metaDescription: Release notes for Pipeline Control gateway 1.1.0
---

### Pipeline Control Gateway Release Notes - v1.1.0

#### Support for SQL trace and transaction trace data
* Implemented a new functionality to selectively drop data and attributes from `SqlTrace` and `TransactionTrace` events using drop rules.

#### Security updates

* Addressed vulnerabilities in the `golang.org/x/net` package, including a cross-site scripting issue and an HTTP Proxy bypass related to IPv6 Zone IDs.

* Upgraded to Go `1.24` and the following dependencies are updated to enhance security and incorporate recent fixes:
   * `golang.org/x/net` to `v0.38.0`
   * `golang.org/x/sys` to `v0.31.0`
   * `golang.org/x/text` to `v0.23.0`

#### Support for new functions

Added support for several new functions in NRQL drop rule queries. You can now use the following functions:

- `aparse()`
- `floor()`
- `getField()`
- `hourOf()`
- `numeric()`
- `round()`
- `string()`
- `substring()`
- `weekdayOf()`
- `mod()`
- `dimensions()`
```

---

## Checklist Before Submitting

- [ ] File is in the correct product subdirectory
- [ ] File name follows the existing naming convention
- [ ] Frontmatter includes `subject`, `releaseDate`, `version`, and `metaDescription`
- [ ] `releaseDate` is in `YYYY-MM-DD` format and quoted
- [ ] Content uses proper Markdown formatting
- [ ] Code references use backticks
- [ ] Links to related docs/PRs are included where relevant
- [ ] PR title follows conventional commits format

---

## Prompt for Claude Code / AI Assistant

Copy and paste the following prompt into a new Claude Code session (within this repo) to have it create a release note for you:

```
You are helping me create release notes for the New Relic docs-website repository.

## Setup

Read `RELEASE_NOTES_GUIDE.md` in the repo root. It contains all conventions, file naming, frontmatter format, and examples you must follow exactly.

## Workflow

### 1. Gather Information

Ask me for:
- **Product name** (e.g., Pipeline Control Gateway, Java agent, .NET agent)
- **Version number** (e.g., 1.2.0)
- **Release date** (YYYY-MM-DD format)
- **Changes** — features, bug fixes, security updates, dependency upgrades
- **Links** — related PRs, documentation pages, or download URLs (if any)

### 2. Determine File Location

- Find the correct subdirectory under `src/content/docs/release-notes/`
- Check existing files in that directory to match the naming convention
- If the subdirectory doesn't exist, create it along with an `index.mdx`

### 3. Create the Release Note File

- Use proper YAML frontmatter with `subject`, `releaseDate`, `version`, and `metaDescription`
- Organize body content into logical sections using Markdown headings:
  - `## New features and improvements` or `#### Support for X`
  - `## Fixes`
  - `## Security updates`
- Use backticks for code references (package names, versions, function names)
- Link to related PRs: `[#123](https://github.com/org/repo/pull/123)`
- Link to related docs: `[doc title](/docs/path/to/page/)`

### 4. Commit and PR

- Create a new branch (never commit directly to `develop`)
- Branch name format: `release-notes/<product>-<version>` (e.g., `release-notes/pcg-1.2.0`)
- Commit message: `docs: add release notes for <Product> v<X.Y.Z>`
- Offer to create a PR targeting the `develop` branch

## Rules

- Follow the patterns in `RELEASE_NOTES_GUIDE.md` exactly
- Match the formatting style of existing release notes in the same subdirectory
- Do not invent or assume changes — only document what the user provides
- Keep descriptions clear, concise, and technical
```
