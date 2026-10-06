# Sefaria Developer Docs

This repository is a two-way mirror of the documentation published at **[developers.sefaria.org/docs](https://developers.sefaria.org/docs)**.

The docs are hosted on [ReadMe](https://readme.com). With ReadMe's [bi-directional GitHub sync](https://docs.readme.com/main/docs/bi-directional-sync) enabled, edits flow in both directions:

| You edit... | What happens |
|---|---|
| Markdown in this repo | The change is synced to developers.sefaria.org |
| A page in the ReadMe web editor | ReadMe commits the change to this repo |

This gives engineers a normal Git workflow (your editor, your branches, grep, AI coding tools) for documentation, while non-engineers can keep using the ReadMe GUI.

## 🛑 Where does my change go?

**API endpoint documentation is NOT edited in this repo.** All changes to API docs are made in the **Sefaria-Project** repo, in this file:

### 👉 [`docs/openAPI.json` in Sefaria-Project](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json)

| If you want to change... | Make the change in... |
|---|---|
| An API endpoint's description, parameters, request or response schema, or examples (anything under `reference/Sefaria API/`) | **[Sefaria-Project: `docs/openAPI.json`](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json)** |
| A guide or article (anything under `docs/`) | This repo |
| Getting Started, tutorials, or other pages under `reference/welcome/` | This repo |
| Standalone pages (`custom_pages/`) | This repo |

**Not sure which one you're looking at?** If the page is an endpoint like `GET /api/v3/texts/{tref}` (listed under *API Reference → Sefaria API* on the site), it comes from the OpenAPI spec. Edit it in Sefaria-Project. See [API endpoint docs](#api-endpoint-docs-edit-in-sefaria-project-not-here) for step-by-step instructions.

## Two ways to edit

- **In this repo:** edit the markdown directly and open a pull request. Good for anyone comfortable with Git.
- **In the ReadMe editor:** requires a ReadMe seat. Saved changes are committed to this repo automatically.

Both are fully supported. Pick whichever fits the change.

## Navigating the repo

The synced branch is **`v1.0`**. The top level has three content folders that map to the sections of the ReadMe site, plus two smaller ones:

```
.
├── docs/                 # Guides and articles ("Documentation" tab on the site)
├── reference/            # API Reference tab: endpoint pages, welcome pages, OpenAPI file
├── custom_pages/         # Standalone pages (contact page, news page, etc.)
├── custom_blocks/        # Reusable content snippets
└── README.md             # This file (not part of the synced docs)
```

### `docs/`: guides and articles

Each subfolder is a top-level section in the site's sidebar:

| Folder | What's in it |
|---|---|
| `docs/TECHNICAL DOCS/` | The main developer guides: text references, categories, the structure of a text (index schema, commentaries, terms), the Linker, topic ontology, the API wiki pages, the Sefaria MCP, accessibility, and migrating source sheets. This is the biggest section. |
| `docs/local installation/` | Instructions for running Sefaria locally (Docker Compose, CLI, technical notes on creating titles, categories, and terms) and contributing to Sefaria's repositories. |
| `docs/Dev Digest Archive/` | Archived issues of the developer newsletter. |
| `docs/SEFARIA RESEARCH/` | Research write-ups (the Disambiguator, PATOT, evals for a Jewish AI chatbot, etc.). |
| `docs/about Sefaria/` | General FAQ and the guide to contributing, including how to report a mistake. |

Things to know about how pages are organized:

- **One page = one `.md` file.** The filename (without `.md`) is the page's URL slug, e.g. `text-references.md` is `/docs/text-references`.
- **A folder with an `index.md` is a parent page.** For example, `docs/TECHNICAL DOCS/linker/index.md` is the "The Sefaria Linker" page, and the other files in that folder are its children.
- **`_order.yaml` controls sidebar order.** Every folder has one. It's a plain list of slugs (file names without `.md`, or subfolder names) in the order they should appear. `docs/_order.yaml` orders the top-level sections themselves.
- **Folder names can contain spaces and capitals** (`TECHNICAL DOCS`, `local installation`). That's how ReadMe names them; quote the paths in your shell.
- **Some pages have Hebrew filenames** (for example the Hebrew educator guides and the Hebrew Linker guide). That's expected.

### `reference/`: the API Reference tab

| Path | What's in it |
|---|---|
| `reference/Sefaria API/` | **🛑 Do not edit. Generated from the OpenAPI spec in Sefaria-Project.** One subfolder per endpoint group (`text`, `index`, `related`, `calendars`, `lexicon`, `topic`, `term`, `sheets`, `collections`, `ref`, `misc`, and `index-1`), each with one `.md` file per endpoint plus an `index.md` for the group. |
| `reference/sefaria-api.json` | **🛑 Do not edit.** The OpenAPI spec the endpoint pages are built from. The source of truth is [`docs/openAPI.json` in Sefaria-Project](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json). |
| `reference/welcome/` | Hand-written pages in the API Reference tab: Getting Started, "Others Have Asked", and a `tutorials/` folder. |
| `reference/ReadMeConfig/` | ReadMe-generated configuration pages (authentication, intro, "my requests"). You'll rarely touch these. |

The endpoint pages are thin. They contain frontmatter pointing at an operation in the OpenAPI file (for example `operationId: get-v3-texts`), and ReadMe renders the content from the spec. **That's why they are not edited here.** To change an endpoint's docs, edit [`docs/openAPI.json` in Sefaria-Project](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json). See [API endpoint docs](#api-endpoint-docs-edit-in-sefaria-project-not-here).

The pages under `reference/welcome/` *are* normal hand-written markdown and are edited like any other doc.

### `custom_pages/` and `custom_blocks/`

- `custom_pages/` holds standalone pages that sit outside the normal docs tree (e.g. `contact-us.html`, `news-from-the-sefaria-developer-team.md`). It also has an `_order.yaml`.
- `custom_blocks/` holds reusable snippets that can be embedded in multiple pages.

### Anatomy of a page

Every page starts with YAML frontmatter that ReadMe reads and writes, followed by the markdown body:

```markdown
---
title: Text References
excerpt: >-
  The core of Sefaria's system is the system of text references.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Text References (Citations)

Body of the page in markdown...
```

- `title`: the page title shown on the site and in the sidebar.
- `excerpt`: the short summary shown under the title and in previews.
- `hidden`: set to `true` to keep a page published but out of the sidebar.
- `deprecated`: marks the page as deprecated.
- Leave the other fields alone unless you know what you're changing.

## Editing docs: step by step

> **Always pull before you start.** ReadMe pushes GUI edits straight to `v1.0`, so the branch can change underneath you at any time.

### Edit an existing page

1. **Get the latest.**
   ```bash
   git checkout v1.0
   git pull
   ```
2. **Create a branch for your change.**
   ```bash
   git checkout -b docs/short-description-of-change
   ```
3. **Find the page.** Browse the folders above, or search by title. **If the page is under `reference/Sefaria API/`, stop: it's an API endpoint page, and the change belongs in [Sefaria-Project's `docs/openAPI.json`](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json) instead.**
   ```bash
   grep -rl "Text References" docs/ reference/welcome/
   ```
4. **Edit the markdown body.** Leave the frontmatter intact unless you're deliberately changing the title, excerpt, or visibility.
5. **Commit and push your branch.**
   ```bash
   git add -A
   git commit -m "Clarify rate limits on the texts endpoint"
   git push -u origin docs/short-description-of-change
   ```
6. **Open a pull request into `v1.0`** and get a teammate to review it. Nothing is published until the PR is merged.
7. **Merge, then verify.** Once merged, ReadMe picks up the change. Check the page on [developers.sefaria.org/docs](https://developers.sefaria.org/docs) to confirm it rendered correctly.

### Add a new page

1. Pull and create a branch (steps 1 and 2 above).
2. **Create the file** in the folder where the page should live. Use a lowercase, hyphenated filename; that becomes the URL slug.
   ```
   docs/TECHNICAL DOCS/my-new-page.md
   ```
3. **Add frontmatter and content**, copying the structure from an existing page:
   ```markdown
   ---
   title: My New Page
   excerpt: One-sentence summary of the page.
   deprecated: false
   hidden: false
   metadata:
     title: ''
     description: ''
     robots: index
   next:
     description: ''
   ---
   Your content here.
   ```
4. **Add the slug to the folder's `_order.yaml`** at the position where it should appear in the sidebar (slug only, no `.md`):
   ```yaml
   - text-references
   - my-new-page
   - categories
   ```
   A page that isn't listed in `_order.yaml` may not show up where you expect.
5. Commit, push, open a PR, merge, and verify (steps 5 to 7 above).

### Add a new section or sub-section

1. Create a new folder under the relevant parent.
2. Add an `index.md` inside it. This is the parent page, so give it a `title` and `excerpt`.
3. Add child pages as `.md` files in the folder.
4. Create an `_order.yaml` in the new folder listing the child slugs.
5. Add the new folder's name to the **parent folder's** `_order.yaml`.

### Reorder pages

Edit the `_order.yaml` in the folder containing the pages. Move the slug to the position you want. No other file changes are needed.

### Rename, move, or delete a page

Be careful here. The filename is the page's slug, so renaming or moving a file changes its URL and can break links from other pages, from external sites, and from the Sefaria app. Before doing this:

- Search the repo for the old slug: `grep -rn "old-slug" docs/ reference/ custom_pages/`
- Update every link and every `_order.yaml` entry that mentions it.
- When in doubt, ask in the PR whether it's safe.

To retire a page without breaking its URL, prefer setting `hidden: true` in its frontmatter.

### Edit a page in the ReadMe GUI instead

Anyone with a ReadMe seat can edit in the ReadMe editor. When they save, ReadMe commits directly to `v1.0` with a message like `Update doc <slug>`. Pull to see those changes locally:

```bash
git checkout v1.0
git pull
```

## API endpoint docs: edit in Sefaria-Project, not here

> **All API docs changes happen in the Sefaria-Project repo.**
> Source of truth: **[`docs/openAPI.json`](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json)**
>
> Do not edit `reference/sefaria-api.json` or any page under `reference/Sefaria API/` in this repo.

The endpoint pages (everything in `text/`, `topic/`, `sheets/`, `calendars/`, and so on) are generated from an OpenAPI spec. Each page's frontmatter only points at an operation in `reference/sefaria-api.json`:

```yaml
api:
  file: sefaria-api.json
  operationId: get-v3-texts
```

The descriptions, parameters, request and response schemas, and examples shown for each endpoint all come from the spec, not from the markdown file.

### How to change an endpoint's docs

1. Note the endpoint's `operationId` from the page's frontmatter in this repo (e.g. `get-v3-texts`), or just its path (e.g. `/api/v3/texts/{tref}`).
2. Go to **[Sefaria-Project](https://github.com/Sefaria/Sefaria-Project)** and create a branch off `master`.
3. Open `docs/openAPI.json` and find the entry for that endpoint (search for the `operationId` or the path).
4. Make your change: description, parameters, schema, or examples.
5. Open a pull request against `master` in Sefaria-Project and follow that repo's contribution conventions.
6. Do **not** copy the change into this repo by hand.

### Why Sefaria-Project?

The spec lives next to the API code it describes, so the two can be kept in sync in a single place. Edits made to `reference/sefaria-api.json` or to the generated endpoint pages in this repo can be overwritten and would drift away from the real source.

### What *is* edited in this repo

- The hand-written pages in `reference/welcome/` (Getting Started, tutorials, "Others Have Asked")
- The group intro page (`index.md`) in each endpoint folder
- Everything under `docs/`

## How the sync works

A few things worth knowing, because they differ from a typical repo:

- **ReadMe pushes directly, with no pull request.** When someone saves a change in the ReadMe GUI, ReadMe commits it straight to the synced branch. It does not open a PR. This is true even if you use ReadMe's own Branches and Reviews feature: merging in ReadMe still results in a direct push.
- **Sync is organized around ReadMe project versions, not around `main`/`master`.** The repo tracks ReadMe's version branches (e.g. `v1.0`, `v2.0`). The primary branch corresponds to whichever version is set as the default in the ReadMe project settings. For us that's `v1.0`.
- **Commit messages from ReadMe can't be customized.** They follow ReadMe's own naming convention based on the action and page slug, for example:
  - `Update doc <slug>`
  - `Create doc <slug>`
  - `Update reference <slug>`

  These do **not** follow Conventional Commits. Don't enforce commit-message linting on the synced branch, or ReadMe's commits will fail the check. For commits made by engineers, a short imperative summary is plenty.
- **Branch protection can break the sync.** Because ReadMe pushes directly, protection rules that require PRs or status checks on the synced branch may block it. If you change branch protection settings, verify that a GUI edit still syncs.
- **Only the synced branch publishes.** Work on other branches isn't published until it's merged into `v1.0`.

## Troubleshooting

**My merge isn't showing up on the site.**
Confirm it landed on `v1.0` and not another branch. Then check the sync status in the ReadMe dashboard and check your page's frontmatter for errors.

**My new page is missing from the sidebar.**
Check that its slug is listed in the folder's `_order.yaml`, and that `hidden` isn't `true`.

**I edited an endpoint page (or `sefaria-api.json`) and the change disappeared, or never appeared.**
API endpoint docs are generated from the OpenAPI spec, and all changes must be made in [`docs/openAPI.json` in Sefaria-Project](https://github.com/Sefaria/Sefaria-Project/blob/master/docs/openAPI.json). Edits here are not the source of truth.

**I edited a page in ReadMe and the commit isn't in the repo.**
Check that the ReadMe project's default version matches the branch you're looking at, and that branch protection isn't rejecting ReadMe's direct push.

**ReadMe's commits are failing a CI check.**
Exclude the synced branch from commit-message linting. ReadMe's commit format can't be changed.

## Questions and feedback

- Found a mistake in the docs or want to suggest an improvement? Open an issue or pull request in this repo.
- For anything else, email [developers@sefaria.org](mailto:developers@sefaria.org).
- For ReadMe's own documentation on the sync, see [Syncing with GitHub](https://docs.readme.com/main/docs/sync-with-github).
