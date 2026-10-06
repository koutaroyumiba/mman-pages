<div align="center">

# mman-pages

A versioned collection of Markdown manual pages for [`mman`](https://github.com/koutaroyumiba/mman).

</div>

This repository contains **published manual content**. The `mman` repository contains the CLI implementation, while drafts and research should remain in a separate workspace (for example, an Obsidian vault).

`mman-pages` is the published edition. Once a version has been released, its contents must not change. Corrections and improvements are published as a new version and documented in the changelog.

## Quick start

Install `mman` by following its [installation instructions](https://github.com/koutaroyumiba/mman#installation), clone this repository, and configure the page directory as a documentation root:

```sh
git clone https://github.com/koutaroyumiba/mman-pages.git "$HOME/workspace/mman-pages"
export MMANPATH="$HOME/workspace/mman-pages/pages"
```

Add the `export` command to your shell configuration to make it persistent. On macOS and Linux, multiple roots are separated by `:` and searched from left to right:

```sh
export MMANPATH="$HOME/workspace/mman-pages/pages:$HOME/work/docs"
```

You can override `MMANPATH` for one invocation using the `-M` flag:

```sh
mman -M "$HOME/workspace/mman-pages/pages" --list
```

Useful commands:

```sh
mman                                    # interactively pick a topic
mman algorithms/insertion-sort          # open a topic
mman --raw algorithms/insertion-sort    # print the raw markdown
mman --where algorithms/insertion-sort  # print the source path
mman --list                             # list all topics
mman --search "binary search"           # greps through pages
```

A plain topic prints raw Markdown automatically when input or output is not an interactive terminal.

## Repository Structure

```text
mman-pages/
├── pages/                         # only published manual pages
│   ├── algorithms/
│   │   └── insertion-sort.md
│   ├── structures/
│   │   └── vector.md
│   ├── c/
│   │   └── functions/
│   │       └── printf.md
│   └── rust/
│       ├── commands/
│       │   └── cargo-test.md
│       └── concepts/
│           └── ownership.md
├── templates/                     # authoring aids; not indexed by mman
│   ├── page.md
│   ├── command.md
│   ├── function.md
│   └── algorithm.md
├── .github/
│   └── workflows/                 # optional validation and release checks
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

### Taxonomy guidance

- Add a category only when a real page needs it; Git does not retain empty directories.
- Use broad subject categories for language-neutral material, such as `algorithms/` and `data-structures/`.
- Namespace ecosystem-specific pages from the start, such as `c/functions/printf` or `rust/commands/cargo-test`.
- Keep the tree shallow unless another level removes genuine ambiguity.
- Use lowercase names with hyphens between words.
- Treat every topic path as a public interface. Choose names that are stable and unsurprising.
- Keep one primary subject per page and connect related subjects through `SEE ALSO`.

Do not reorganize merely for aesthetics: renaming or moving a published page is a breaking change.

## How files become topics

`mman` recursively discovers visible files ending in `.md`:

```text
pages/git.md                       -> git
pages/languages/rust.md            -> languages/rust
pages/concepts/ownership/index.md  -> concepts/ownership
```

Prefer direct pages (`ownership.md`) unless a topic needs adjacent supporting files. If both forms exist in the same root, the direct page wins. If a topic exists in multiple configured roots, the first root wins; `mman --select TOPIC` can select a source interactively.

Topic lookup is exact and case-sensitive. Users may include or omit `.md`. Hidden files and directories are ignored. Topic paths must not contain absolute paths, hidden components, `.`, `..`, repeated separators, or control characters.

Pages should be UTF-8 Markdown. `mman` treats Markdown as data: it does not execute code, interpret embedded HTML, fetch remote resources, or invoke external renderers.

## Page Format Standard

Every page must begin with:

```markdown
# topic-name

## NAME

`topic-name` — concise one-line description

## SYNOPSIS

The shortest useful signature, invocation, data shape, or algorithm outline.

## DESCRIPTION

The core explanation.
```

Add only sections relevant to the subject:

- `PARAMETERS`
- `RETURN VALUE`
- `EXIT STATUS`
- `OPERATIONS`
- `COMPLEXITY`
- `PROPERTIES`
- `EXAMPLES`
- `ERRORS AND PITFALLS`
- `WHEN TO USE`
- `NOTES`
- `SEE ALSO`

Additional Conventions:

- Use lowercase, stable topic paths with hyphens between words.
- Keep one primary subject per page.
- Put practical information before history and secondary detail.
- Prefer small, runnable examples to large demonstrations.
- State assumptions behind complexity bounds and behavioural claims.
- Distinguish language guarantees from implementation details.
- Name the relevant version when behavior is version-specific.
- Use `SEE ALSO` to connect related pages by topic name.
- Use ordinary relative Markdown links for navigatable relationships between published pages.
- Do not rely on raw HTML, scripts, remote images, terminal control sequences, or executable content.
- Do not add front matter until `mman` defines a metadata contract.

## Authoring templates

Copy the closest template, remove irrelevant sections, and replace all instructional text. These templates may later be copied into the proposed `templates/` directory as standalone files.

- [General Page Template](./templates/page.md)
- [Command Page Template](./templates/command.md)
- [Function Page Template](./templates/function.md)
- [Algorithm Page Template](./templates/algorithm.md)
- [Structure Page Template](./templates/structure.md)
- [Concept Page Template](./templates/concept.md)
- [Compatibility Page Template](./templates/moved-topic.md)

## Publication workflow

Use separate source-of-truth stages:

1. **Draft:** capture research and incomplete ideas outside this repository.
2. **Candidate:** rewrite the draft using a page template; verify claims and examples.
3. **Published:** add the page under `pages/` in a focused commit or pull request.
4. **Revised:** begin later changes from the published page, update the changelog, and release a new version.

After publication, do not maintain another authoritative copy. A draft note may link to a published page, but corrections should start from this repository.

Suggested commit messages name the topic and purpose:

```text
add(algorithms): document merge sort
fix(c/functions/printf): clarify return-value semantics
```

## Versioning and Compatibility

Version this collection independently from the `mman` executable using Semantic Versioning:

```text
MAJOR.MINOR.PATCH
```

- **PATCH** — factual corrections, typo fixes, clearer wording, corrected examples, or repaired links.
- **MINOR** — new pages, examples, or substantial additive sections.
- **MAJOR** — removed, renamed, or reorganized topic paths, or another incompatible editorial contract.

Release tags use a `v` prefix, such as `v1.2.0`. Tags are immutable published snapshots: never move or recreate one. Users needing reproducible content can check out a release tag and point `MMANPATH` at its `pages/` directory.

### Compatibility Rules:

- Adding a page is backward-compatible.
- Expanding a page without changing its meaning is backward-compatible.
- Correcting an error is backward-compatible but must be documented.
- Renaming, moving, or removing a topic is a breaking change.
- An old topic path must never be reused for a different subject.

When practical, leave a short replacement page at an old topic path for one major release. It should identify the new topic instead of silently presenting different material.

## Changelog Policy

Maintain one root `CHANGELOG.md`. Use an `Unreleased` section while work is in progress and create a dated section when publishing a release.

Group entries under headings such as:

- `Added`
- `Changed`
- `Fixed`
- `Deprecated`
- `Removed`
- `Migration`

Every entry should name the affected topic. Describve what a reader needs to know rather than just saying that a file changed. A page-level histoy section is usually unnecessary becasuse Git and the collection changelog already retain that information.

```markdown
# Changelog

## [Unreleased]

## [2.0.0] - 2027-01-10

### Changed

- Rename `functions/printf` to `c/functions/printf` so language-specific functions have an explicit namespace.
- Move Rust-specific command pages from `commands/` to `rust/commands/`

### Removed

- Remove the deprecated `vector` alias. Use `structures/vector`.

### Migration

- Replace `mman functions/printf` with `mman c/functions/printf`
- Replace `mman commands/cargo-test` with `mman rust/commands/cargo-test`

## [1.1.0] - 2026-03-15

### Added

- `commands/cargo-test`: document filtering, test-harness arguments, and exit behaviour
- `concepts/ownership`: introduce moves, borrowing, `Copy`, and `Clone`
- `algorithms/insertion-sort`: add guidance for nearly sorted inputs

## [1.0.1] - 2026-02-27

### Fixed

- `functions/printf`: clarify that the return value excludes the terminating null byte
- `algorithms/insertion-sort`: preserve stability in the sample comparison by moving only strictly greater elements

### Changed

- `structures/vector`: distinguish guaranteed behaviour from unspecified capacity-growth strategy

## [1.0.0] - 2026-02-20

### Added

- `algorithms/insertion-sort`: explain the invariant, complexity, stability, and a Rust implementation
- `structures/vector`: describe representation, common operations, invalidation, and complexity
- `functions/printf`: document common conversions, return values, and format string hazards
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for page standards, branch and commit
conventions, pull-request requirements, and the release workflow.

## License

Manual pages, documentation, and code examples in this repository are licensed
under the [MIT License](LICENSE), unless a file states otherwise. By
contributing, you agree that your contribution will be licensed under the same
terms; see [CONTRIBUTING.md](CONTRIBUTING.md).

## Future Automation

Could automate:

- duplicate normalized topic detection,
- broken relative-link detection,
- required-section validation,
- trailing-whitespace and Markdown formatting checks,
- verification that every changed page has a changelog entry,
