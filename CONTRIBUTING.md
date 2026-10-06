# Contributing to mman-pages

Thank you for helping improve the manual-page collection.

## What belongs here

This repository is the source of truth for published manual pages. Drafts,
research notes, and incomplete ideas should be developed elsewhere before they
are submitted here.

Good contributions include:

- new manual pages;
- factual corrections;
- clearer explanations and verified examples;
- repaired relative links;
- compatibility notices for renamed or moved topics; and
- focused improvements to the collection's documentation and validation.

Before proposing a topic rename, move, removal, or large taxonomy change, open
an issue for discussion. Published topic paths are part of the collection's
public interface, so these changes require a major release.

## Repository layout

Publishable pages belong under `pages/`. Project documentation, templates, and
automation belong outside that directory so `mman` does not index them as
manual topics.

Use lowercase topic paths with hyphens between words. Namespace pages that are
specific to a language or ecosystem, for example:

```text
pages/algorithms/insertion-sort.md
pages/c/functions/printf.md
pages/rust/commands/cargo-test.md
```

Add categories only when real pages require them, and keep the hierarchy as
shallow as practical.

## Writing a page

Start with the closest template in the [README](README.md#authoring-templates).
Every page must include:

```markdown
# topic-name

## NAME

`topic-name` — concise one-line description

## SYNOPSIS

The shortest useful form.

## DESCRIPTION

The core explanation.
```

Add only the optional sections relevant to the subject. In particular:

- keep one primary subject per page;
- put practical information first;
- prefer small, runnable examples;
- verify examples and factual claims where practical;
- state assumptions behind complexity and behavioral claims;
- distinguish guarantees from implementation details;
- identify versions when behavior is version-specific;
- use ordinary relative Markdown links rather than Obsidian syntax; and
- avoid front matter, raw HTML, scripts, remote images, and executable content.

See the [page standard](README.md#page-format-standard) for the full editorial and compatibility policies.

## Changelog

Record every reader-visible change under `Unreleased` in `CHANGELOG.md`. Name
the affected topic and explain what changed for the reader rather than merely
stating that a file changed.

Use the appropriate heading:

- `Added`
- `Changed`
- `Fixed`
- `Deprecated`
- `Removed`

See the [changelog policy](README.md#changelog-policy) for versioning examples.

## Validation

Review the requirements above, then run the smallest relevant manual checks:

```sh
mman -M pages --list
mman -M pages --raw path/to/topic
mman -M pages --where path/to/topic
mman -M pages --search "distinctive phrase"
```

Confirm that:

- the topic appears exactly once unless duplication is intentional;
- the page is valid UTF-8 and readable as plain Markdown;
- required sections are present;
- code examples behave as documented;
- relative links resolve within `pages/`; and
- existing topic paths remain valid unless the change is explicitly breaking.

## Branching

`main` is the only permanent branch, and a separate `develop` branch is not
used. A branch may contain any number of page commits. Name it after the overall
contribution rather than trying to list every affected page.

Name branches with a type and a short hyphenated description:

```text
feat/rust-ownership
feat/cargo-test
fix/insertion-sort-stability
docs/taxonomy-policy
chore/link-validation
release/v0.1.0
```

Use these prefixes:

- `feat/` for a new page or substantial new content;
- `fix/` for a factual correction or repaired example;
- `docs/` for repository documentation and templates;
- `chore/` for CI, validation, and maintenance;
- `refactor/` for page reorganization; and
- `release/` for release preparation.

Before a topic path has appeared in a release, it may be corrected normally.
After publication, renaming, moving, or removing it is a breaking change.

## Commits

Use Conventional Commit-style messages:

```text
<type>(<scope>): <imperative summary>
```

Common types are:

- `feat` for a new page or substantial additive content;
- `fix` for a factual correction, typo, broken example, or broken link;
- `docs` for repository documentation and templates;
- `chore` for CI and repository maintenance;
- `refactor` for reorganizing existing content; and
- `release` for release preparation.

Examples:

```text
feat(rust): add ownership page
feat(algorithms): document insertion sort
fix(insertion-sort): preserve stability for equal elements
docs(repo): clarify taxonomy rules
chore(ci): validate relative links
```

Use the narrowest useful scope. Every page commit must perform one action on
one primary page: add it, correct it, expand it, move it, deprecate it, or remove
it. Do not combine unrelated changes to the same page or changes to multiple
pages in one commit.

A page's `CHANGELOG.md` entry belongs in the same commit and does not count as a
second page. If an action requires updates to links or compatibility pages,
make each additional page update a separate commit. Repository-only work, such
as changing a template or CI configuration, should likewise use one focused
action per commit.

Mark a breaking topic-path change with `!` and explain the migration in the
commit body:

```text
refactor(pages)!: move printf into the C namespace

BREAKING CHANGE: `functions/printf` moved to `c/functions/printf`.
```

## Pull requests

A pull request may contain any number of pages and actions. Large collections
and multi-page contributions are welcome, provided every commit still follows
the one-page, one-action rule. This makes each part independently reviewable,
revertible, and suitable for preservation in the project history.

Give the pull request a concise title that summarizes the overall contribution;
it does not need to describe every commit. Use a draft pull request while
factual or editorial questions remain. Complete the pull-request template
before requesting review. In particular:

1. summarize the overall contribution;
2. list every affected topic path;
3. confirm that each commit changes one primary page or performs one focused
   repository action;
4. describe how examples and claims were verified;
5. identify compatibility and versioning implications;
6. update `CHANGELOG.md` for reader-visible changes; and
7. link any related issue or source material.

There is no fixed pull-request size limit. Split a contribution only when doing
so would make review, discussion, or release planning materially clearer.

## Merging

Use rebase-and-merge for every pull request so page-level commits are preserved
in a linear history. Merge commits and squash merges are not accepted. Before
merging, ensure the branch is based on the latest `main` and that every commit
independently follows the one-page, one-action rule. Delete the source branch
after merging.

Outside contributors should submit pull requests. Maintainers may push directly
to `main` when the commits already follow the one-page, one-action rule and the
relevant checks have been run. Releases and breaking topic-path changes should
still use pull requests so their full impact can be reviewed.

Do not force-push `main`. Do not merge or push changes with factual questions,
failed checks, or unresolved review comments. The `main` branch represents the
latest development edition and may contain unreleased work. Release tags,
rather than branches, identify immutable published editions.

Recommended GitHub repository and branch settings are:

- enable rebase merging and disable merge commits and squash merging;
- require configured validation checks on pull requests;
- block force pushes;
- block branch deletion; and
- allow maintainers to bypass the pull-request requirement for validated direct
  pushes.

A required approval is optional while the project has only one maintainer.

## Releases

Prepare each release in a branch such as `release/v0.1.0` and a dedicated pull
request:

1. keep a new empty `Unreleased` section in `CHANGELOG.md`;
2. move completed entries into a dated version section;
3. confirm that the version follows the collection's Semantic Versioning policy;
4. review the complete diff from the previous release tag;
5. verify discovery, lookup, listing, search, links, and examples;
6. merge the release pull request; and
7. create an annotated tag on the resulting `main` commit.

```sh
git tag -a v0.1.0 -m "mman-pages v0.1.0"
git push origin v0.1.0
```

Create the corresponding GitHub release from the changelog section. Never
move, recreate, or modify a published tag; publish another release instead.

## License

This repository is licensed under the [MIT License](LICENSE).

By submitting a contribution, you agree that your contribution will be licensed
under the same license and that you have the right to submit it. Clearly identify
material copied or adapted from another source and ensure its license is
compatible before including it.
