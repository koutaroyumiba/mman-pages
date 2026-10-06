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

See the [page standard](README.md#page-standard) for the full editorial and compatibility policies.

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

If `CHANGELOG.md` has not yet been created, use the
[changelog template](README.md#changelog-template).

## Validation

Review the page checklist in the README, then run the smallest relevant manual
checks:

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

## Commits and pull requests

Keep each commit focused on one page or one coherent editorial change. Suggested
commit-message forms include:

```text
add(algorithms): document merge sort
fix(c/functions/printf): clarify return-value semantics
```

In a pull request:

1. summarize the reader-facing change;
2. list every affected topic;
3. describe how examples and claims were verified;
4. identify compatibility or versioning implications; and
5. link any related issue or source material.

## License

This repository is licensed under the [MIT License](LICENSE).

By submitting a contribution, you agree that your contribution will be licensed
under the same license and that you have the right to submit it. Clearly identify
material copied or adapted from another source and ensure its license is
compatible before including it.
